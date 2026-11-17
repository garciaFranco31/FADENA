# Laboratorio de Red Empresarial Jerárquica (Cisco Packet Tracer)

Este documento describe una topología de **red empresarial jerárquica (Núcleo/Distribución colapsado + Acceso)** para practicar en **Cisco Packet Tracer**. Incluye **Router-on-a-Stick**, **VLANs**, **Trunks (802.1Q)**, **STP (Spanning Tree Protocol)** para resolver un ciclo en Capa 2, **DHCP en el router** y direccionamiento estático para servidores y gestión.

---

## 1. Equipos a colocar en Packet Tracer

* **Capa de Núcleo / Distribución (Colapsada):**
  * `R1`: 1 Router (ej. **Cisco 2911** o **1941**) — hará de Gateway (Router-on-a-Stick) y Servidor DHCP.
  * `SW-CORE`: 1 Switch (ej. **Cisco 2960** o **3560**) — punto central que conecta al router y a los switches de acceso.
* **Capa de Acceso:**
  * `SW-ACC1`: Switch **Cisco 2960** (Piso 1 - Ventas y Administración).
  * `SW-ACC2`: Switch **Cisco 2960** (Piso 2 - Ventas e IT/Servidores).
* **Hosts (7 dispositivos finales):**
  * `PC1`, `PC2`, `PC3`, `PC4` (PCs convencionales).
  * `Laptop1` (para el equipo de IT).
  * `SRV-WEB` y `SRV-DNS` (2 Servidores).

---

## 2. Diagrama de Topología Completa (Equipos, Puertos, VLANs e IPs)

```text
                             [       R1 (Router)       ]
                             [ Gi0/0.10: 192.168.10.1  ]
                             [ Gi0/0.20: 192.168.20.1  ]
                             [ Gi0/0.30: 192.168.30.1  ]
                             [ Gi0/0.99: 192.168.99.1  ]
                                          | Gi0/0 (Trunk 802.1Q)
                                          | Gi0/1
                             [         SW-CORE         ]
                             [ VLAN 99: 192.168.99.2   ]
                                    /           \
                      (Trunk) Fa0/1/             \Fa0/2 (Trunk)
                                  /               \
                           Fa0/1 /                 \ Fa0/1
            [       SW-ACC1       ]---------------[       SW-ACC2       ]
            [ VLAN 99: 192.168.99.3] Fa0/2   Fa0/2 [ VLAN 99: 192.168.99.4]
               /       |       \     (Trunk/Ciclo)   /      |       |      \
         Fa0/3/  Fa0/4 |  Fa0/5 \              Fa0/3/ Fa0/4 | Fa0/5 |  Fa0/6\
             /         |         \                 /        |       |        \
         [PC1]       [PC2]      [PC3]          [PC4]   [Laptop1] [SRV-WEB] [SRV-DNS]
         VL 10       VL 10      VL 20          VL 10     VL 30     VL 30     VL 30
         (DHCP)      (DHCP)     (DHCP)         (DHCP)  (Estática)(Estática)(Estática)
      192.168.     192.168.   192.168.       192.168.  192.168.  192.168.  192.168.
        10.11        10.12      20.11          10.13     30.50     30.10     30.11
```

### El Ciclo (Loop de Capa 2) y Tarea de STP
* **Enlaces que forman el ciclo:**
  * `SW-CORE` (`Fa0/1`) <---> `SW-ACC1` (`Fa0/1`)
  * `SW-CORE` (`Fa0/2`) <---> `SW-ACC2` (`Fa0/1`)
  * `SW-ACC1` (`Fa0/2`) <---> `SW-ACC2` (`Fa0/2`) *(Enlace redundante que genera el ciclo)*
* **Configuración STP:**
  * Todos los enlaces entre switches deben configurarse en modo **Trunk** (`switchport mode trunk`).
  * Configurar **Rapid PVST+** (`spanning-tree mode rapid-pvst`).
  * Forzar a **`SW-CORE` como Root Bridge Principal** (`spanning-tree vlan 10,20,30,99 root primary`) y a **`SW-ACC1` como Root Secundario** (`spanning-tree vlan 10,20,30,99 root secondary`). Así verás cómo STP bloquea el puerto `Fa0/2` en `SW-ACC2`.
  * En todos los puertos de acceso (hacia PCs, Laptop o Servidores), activar **PortFast** (`spanning-tree portfast`) y **BPDU Guard** (`spanning-tree bpduguard enable`).

---

## 3. Tabla de Subredes y VLANs

| VLAN ID | Nombre | Subred | Máscara | Gateway (Subinterfaz en R1) | Modo de asignación IP |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | `VENTAS` | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` (`Gi0/0.10`) | **DHCP** (Excluir `.1` a `.10`) |
| **VLAN 20** | `ADMIN` | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` (`Gi0/0.20`) | **DHCP** (Excluir `.1` a `.10`) |
| **VLAN 30** | `IT_SERVERS` | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` (`Gi0/0.30`) | **Estática** (Servidores y Laptop IT) |
| **VLAN 99** | `MGMT_NATIVA` | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` (`Gi0/0.99`) | **Estática** (SVI Switches + Nativa) |

---

## 4. Tabla Completa de Direccionamiento IP por Dispositivo

| Dispositivo | Interfaz / Puerto | Conectado a | VLAN | Dirección IP | Máscara | Default Gateway | DNS / Modo |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **R1** (Router) | `Gi0/0.10` | `SW-CORE (Gi0/1)` | 10 (`VENTAS`) | `192.168.10.1` | `255.255.255.0` | *N/A (Es el GW)* | Estática (`dot1Q 10`) |
| **R1** (Router) | `Gi0/0.20` | `SW-CORE (Gi0/1)` | 20 (`ADMIN`) | `192.168.20.1` | `255.255.255.0` | *N/A (Es el GW)* | Estática (`dot1Q 20`) |
| **R1** (Router) | `Gi0/0.30` | `SW-CORE (Gi0/1)` | 30 (`IT_SERVERS`) | `192.168.30.1` | `255.255.255.0` | *N/A (Es el GW)* | Estática (`dot1Q 30`) |
| **R1** (Router) | `Gi0/0.99` | `SW-CORE (Gi0/1)` | 99 (`MGMT_NATIVA`)| `192.168.99.1` | `255.255.255.0` | *N/A (Es el GW)* | Estática (`dot1Q 99 native`) |
| **SW-CORE** | `Interface VLAN 99` | Gestión interna | 99 (`MGMT_NATIVA`)| `192.168.99.2` | `255.255.255.0` | `192.168.99.1` | Estática (SVI) |
| **SW-ACC1** | `Interface VLAN 99` | Gestión interna | 99 (`MGMT_NATIVA`)| `192.168.99.3` | `255.255.255.0` | `192.168.99.1` | Estática (SVI) |
| **SW-ACC2** | `Interface VLAN 99` | Gestión interna | 99 (`MGMT_NATIVA`)| `192.168.99.4` | `255.255.255.0` | `192.168.99.1` | Estática (SVI) |
| **PC1** | `FastEthernet0` | `SW-ACC1 (Fa0/3)` | 10 (`VENTAS`) | `192.168.10.11` | `255.255.255.0` | `192.168.10.1` | **DHCP** (DNS `192.168.30.11`) |
| **PC2** | `FastEthernet0` | `SW-ACC1 (Fa0/4)` | 10 (`VENTAS`) | `192.168.10.12` | `255.255.255.0` | `192.168.10.1` | **DHCP** (DNS `192.168.30.11`) |
| **PC3** | `FastEthernet0` | `SW-ACC1 (Fa0/5)` | 20 (`ADMIN`) | `192.168.20.11` | `255.255.255.0` | `192.168.20.1` | **DHCP** (DNS `192.168.30.11`) |
| **PC4** | `FastEthernet0` | `SW-ACC2 (Fa0/3)` | 10 (`VENTAS`) | `192.168.10.13` | `255.255.255.0` | `192.168.10.1` | **DHCP** (DNS `192.168.30.11`) |
| **Laptop1** | `FastEthernet0` | `SW-ACC2 (Fa0/4)` | 30 (`IT_SERVERS`) | `192.168.30.50` | `255.255.255.0` | `192.168.30.1` | **Estática** (DNS `192.168.30.11`) |
| **SRV-WEB** | `FastEthernet0` | `SW-ACC2 (Fa0/5)` | 30 (`IT_SERVERS`) | `192.168.30.10` | `255.255.255.0` | `192.168.30.1` | **Estática** (DNS `192.168.30.11`) |
| **SRV-DNS** | `FastEthernet0` | `SW-ACC2 (Fa0/6)` | 30 (`IT_SERVERS`) | `192.168.30.11` | `255.255.255.0` | `192.168.30.1` | **Estática** (DNS `192.168.30.11`) |

---

## 5. Detalle de Puertos y VLANs por Switch

* **`SW-CORE`** (IP SVI VLAN 99: `192.168.99.2/24`, Default-Gateway: `192.168.99.1`):
  * **VLANs:** `10`, `20`, `30`, `99`.
  * **Puertos Trunk (Nativa 99):** `Gi0/1` (a `R1`), `Fa0/1` (a `SW-ACC1`), `Fa0/2` (a `SW-ACC2`).
  * **Hosts conectados:** Ninguno.
* **`SW-ACC1`** (IP SVI VLAN 99: `192.168.99.3/24`, Default-Gateway: `192.168.99.1`):
  * **VLANs:** `10`, `20`, `30`, `99`.
  * **Puertos Trunk (Nativa 99):** `Fa0/1` (a `SW-CORE`), `Fa0/2` (a `SW-ACC2`).
  * **Puertos de Acceso:** `Fa0/3` -> `PC1` (VLAN 10), `Fa0/4` -> `PC2` (VLAN 10), `Fa0/5` -> `PC3` (VLAN 20).
* **`SW-ACC2`** (IP SVI VLAN 99: `192.168.99.4/24`, Default-Gateway: `192.168.99.1`):
  * **VLANs:** `10`, `20`, `30`, `99`.
  * **Puertos Trunk (Nativa 99):** `Fa0/1` (a `SW-CORE`), `Fa0/2` (a `SW-ACC1`).
  * **Puertos de Acceso:** `Fa0/3` -> `PC4` (VLAN 10), `Fa0/4` -> `Laptop1` (VLAN 30), `Fa0/5` -> `SRV-WEB` (VLAN 30), `Fa0/6` -> `SRV-DNS` (VLAN 30).

---

## 6. Checklist de objetivos para resolver el laboratorio

- [ ] **1. Capa 2 - VLANs y Accesos:** Crear las VLANs `10`, `20`, `30` y `99` en los 3 switches y asignar los puertos de acceso a cada host.
- [ ] **2. Capa 2 - Trunks:** Configurar los enlaces entre switches y hacia el router en modo Trunk con VLAN nativa `99`.
- [ ] **3. Capa 2 - STP:** Activar `rapid-pvst`, definir `SW-CORE` como Root Primary, `SW-ACC1` como Root Secondary, y verificar con `show spanning-tree` qué interfaz quedó en estado `BLK` (Alternate/Blocked).
- [ ] **4. Capa 3 - Inter-VLAN Routing:** Configurar las subinterfaces `Gi0/0.10`, `Gi0/0.20`, `Gi0/0.30` y `Gi0/0.99` en `R1` y encender la interfaz física `Gi0/0`.
- [ ] **5. Servicios - DHCP:** Crear dos pools DHCP en `R1` (`POOL_VENTAS` y `POOL_ADMIN`) con sus `default-router`, `dns-server 192.168.30.11` y exclusiones (`ip dhcp excluded-address`).
- [ ] **6. Gestión (SVI):** Configurar la `interface vlan 99` y el `ip default-gateway 192.168.99.1` en los 3 switches, y probar hacer `ping` desde `Laptop1` (`192.168.30.50`) a las IPs de gestión de los switches (`192.168.99.2`, `.3`, `.4`).
