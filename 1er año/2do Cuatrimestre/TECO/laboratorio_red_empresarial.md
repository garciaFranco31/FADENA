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

## 2. Diagrama de Conexiones y Ciclo para STP

Conecta los equipos usando las siguientes interfaces para mantener el orden de configuración:

```text
                    [ R1 (Router) ]
                          | Gi0/0
                          | (Trunk 802.1Q)
                          | Gi0/1
                    [ SW-CORE ]
                     /       \
       (Trunk) Fa0/1/         \Fa0/2 (Trunk)
                   /           \
           Fa0/1  /             \ Fa0/1
          [ SW-ACC1 ]--------- [ SW-ACC2 ]
                 Fa0/2 (Trunk) Fa0/2
```

### El Ciclo (Loop de Capa 2)
* `SW-CORE` (`Fa0/1`) <---> `SW-ACC1` (`Fa0/1`)
* `SW-CORE` (`Fa0/2`) <---> `SW-ACC2` (`Fa0/1`)
* `SW-ACC1` (`Fa0/2`) <---> `SW-ACC2` (`Fa0/2`) *(Enlace redundante que genera el ciclo)*

### Tarea de STP
* Todos los enlaces entre switches deben configurarse en modo **Trunk** (`switchport mode trunk`).
* Configurar **Rapid PVST+** (`spanning-tree mode rapid-pvst`).
* Forzar a que **`SW-CORE` sea el Root Bridge Principal** para todas las VLANs (`spanning-tree vlan 10,20,30,99 root primary`) y que **`SW-ACC1` sea el Root Secundario** (`spanning-tree vlan 10,20,30,99 root secondary`). Así verás cómo STP bloquea el puerto en `SW-ACC2` (`Fa0/2`).
* En todos los puertos de acceso (hacia PCs, Laptop o Servidores), activar **PortFast** (`spanning-tree portfast`) y **BPDU Guard** (`spanning-tree bpduguard enable`).

---

## 3. Tabla de VLANs y Direccionamiento IP

| VLAN ID | Nombre | Subred | Máscara | Gateway (Subinterfaz en R1) | ¿Modo de asignación IP? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | `VENTAS` | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` (`Gi0/0.10`) | **DHCP** (Excluir de la `.1` a la `.10`) |
| **VLAN 20** | `ADMIN` | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` (`Gi0/0.20`) | **DHCP** (Excluir de la `.1` a la `.10`) |
| **VLAN 30** | `IT_SERVERS` | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` (`Gi0/0.30`) | **Estática** (Servidores y Laptop de IT) |
| **VLAN 99** | `MGMT_NATIVA` | `192.168.99.0/24` | `255.255.255.0` | `192.168.99.1` (`Gi0/0.99`) | **Estática** (SVI de los Switches) |

> **Nota extra para practicar:** Configura la **VLAN 99** también como **VLAN Nativa** (`switchport trunk native vlan 99` en los switches y `encapsulation dot1Q 99 native` en el router) en todos los enlaces troncales.

---

## 4. Distribución de VLANs y Hosts por Switch

### A. `SW-CORE`
* **VLANs en la base de datos:** `10`, `20`, `30`, `99` (todas, porque pasan por él hacia el router).
* **Puertos Trunk:** `Gi0/1` (hacia `R1`), `Fa0/1` (hacia `SW-ACC1`), `Fa0/2` (hacia `SW-ACC2`).
* **Hosts conectados:** Ninguno (buena práctica de diseño jerárquico: no conectar usuarios finales al núcleo).
* **IP de Gestión (Interface VLAN 99):** `192.168.99.2 /24` (Default Gateway: `192.168.99.1`).

### B. `SW-ACC1` (Piso 1)
* **VLANs en el switch:** `10`, `20`, `30`, `99`.
* **Puertos Trunk:** `Fa0/1` (hacia `SW-CORE`), `Fa0/2` (hacia `SW-ACC2`).
* **IP de Gestión (Interface VLAN 99):** `192.168.99.3 /24` (Default Gateway: `192.168.99.1`).
* **Hosts conectados (Puertos de Acceso):**
  * `Fa0/3` ---> **`PC1`** (VLAN 10 - `VENTAS`) ---> Obtiene IP por **DHCP** (ej. `192.168.10.11`).
  * `Fa0/4` ---> **`PC2`** (VLAN 10 - `VENTAS`) ---> Obtiene IP por **DHCP** (ej. `192.168.10.12`).
  * `Fa0/5` ---> **`PC3`** (VLAN 20 - `ADMIN`) ---> Obtiene IP por **DHCP** (ej. `192.168.20.11`).

### C. `SW-ACC2` (Piso 2)
* **VLANs en el switch:** `10`, `20`, `30`, `99`.
* **Puertos Trunk:** `Fa0/1` (hacia `SW-CORE`), `Fa0/2` (hacia `SW-ACC1`).
* **IP de Gestión (Interface VLAN 99):** `192.168.99.4 /24` (Default Gateway: `192.168.99.1`).
* **Hosts conectados (Puertos de Acceso):**
  * `Fa0/3` ---> **`PC4`** (VLAN 10 - `VENTAS`) ---> Obtiene IP por **DHCP** (ej. `192.168.10.13`). *(Útil para probar conectividad dentro de la misma VLAN 10 entre distintos switches).*
  * `Fa0/4` ---> **`Laptop1`** (VLAN 30 - `IT_SERVERS`) ---> IP **Estática**: `192.168.30.50 /24`, Gateway `192.168.30.1`, DNS `192.168.30.11`.
  * `Fa0/5` ---> **`SRV-WEB`** (VLAN 30 - `IT_SERVERS`) ---> IP **Estática**: `192.168.30.10 /24`, Gateway `192.168.30.1`, DNS `192.168.30.11`.
  * `Fa0/6` ---> **`SRV-DNS`** (VLAN 30 - `IT_SERVERS`) ---> IP **Estática**: `192.168.30.11 /24`, Gateway `192.168.30.1`. *(Activar el servicio DNS en Packet Tracer apuntando `empresa.local` a `192.168.30.10` e incluir `dns-server 192.168.30.11` en los pools DHCP de `R1`).*

---

## 5. Checklist de objetivos para resolver el laboratorio

- [ ] **1. Capa 2 - VLANs y Accesos:** Crear las VLANs `10`, `20`, `30` y `99` en los 3 switches y asignar los puertos de acceso a cada host.
- [ ] **2. Capa 2 - Trunks:** Configurar los enlaces entre switches y hacia el router en modo Trunk con VLAN nativa `99`.
- [ ] **3. Capa 2 - STP:** Activar `rapid-pvst`, definir `SW-CORE` como Root Primary, `SW-ACC1` como Root Secondary, y verificar con `show spanning-tree` qué interfaz quedó en estado `BLK` (Alternate/Blocked).
- [ ] **4. Capa 3 - Inter-VLAN Routing:** Configurar las subinterfaces `Gi0/0.10`, `Gi0/0.20`, `Gi0/0.30` y `Gi0/0.99` en `R1` y encender la interfaz física `Gi0/0`.
- [ ] **5. Servicios - DHCP:** Crear dos pools DHCP en `R1` (`POOL_VENTAS` y `POOL_ADMIN`) con sus `default-router`, `dns-server 192.168.30.11` y exclusiones (`ip dhcp excluded-address`).
- [ ] **6. Gestión (SVI):** Configurar la `interface vlan 99` y el `ip default-gateway 192.168.99.1` en los 3 switches, y probar hacer `ping` desde `Laptop1` (`192.168.30.50`) a las IPs de gestión de los switches (`192.168.99.2`, `.3`, `.4`).
