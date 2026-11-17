# 📘 Instructivo Paso a Paso: Resolución del Laboratorio de Red Empresarial

Este instructivo te guiará **paso a paso** para configurar desde cero la topología empresarial en **Cisco Packet Tracer**, explicando **qué hace cada comando**, **por qué se ejecuta** y **cómo verificar** que cada etapa funcione antes de pasar a la siguiente.

---

## 🧭 Leyenda de Iconos de Estudio

A lo largo de la guía encontrarás estos marcadores para facilitar el estudio:
* 💡 **Analogía:** Comparaciones con la vida real para entender el concepto detrás del protocolo.
* 📌 **Ejemplo:** Casos concretos aplicados a los equipos de nuestra topología (`PC1`, `SW-CORE`, `R1`, etc.).
* 🚨 **Nota Importante:** Reglas críticas de Cisco IOS que, si se omiten, harán que la red no funcione.
* ⚠️ **Consideración:** Detalles del simulador Packet Tracer o buenas prácticas del mundo real.

---

## 🛠️ Fase 0: Armado Físico y Configuraciones Iniciales

### ¿Por qué se hace este paso?
Antes de configurar protocolos lógicos (VLANs, STP o ruteo), necesitamos identificar cada equipo con su nombre (`hostname`) y desactivar la búsqueda DNS automática de la consola (`no ip domain-lookup`) para evitar que el switch o router se congele varios segundos si escribimos mal un comando.

### Comandos en los 4 equipos (`R1`, `SW-CORE`, `SW-ACC1`, `SW-ACC2`)

```ios
enable
configure terminal
hostname SW-CORE
no ip domain-lookup
```
*(Repite cambiando `SW-CORE` por `SW-ACC1`, `SW-ACC2` y `R1` según el equipo).*

### Explicación comando por comando
* `enable`: Pasa del **modo Usuario** (`Switch>`) al **modo Privilegiado** (`Switch#`), donde puedes ver configuraciones y reiniciar el equipo.
* `configure terminal` (o `conf t`): Entra al **modo de Configuración Global** (`Switch(config)#`), donde los cambios afectan a todo el dispositivo.
* `hostname <nombre>`: Cambia el nombre del equipo en el prompt para saber siempre en qué switch o router estás trabajando.
* `no ip domain-lookup`: Evita que Cisco IOS intente traducir por DNS cualquier palabra mal escrita en la consola.

> ⚠️ **Consideración:** En un entorno real también configurarías contraseñas (`enable secret`, `line console 0`, `line vty 0 15`) y `service password-encryption`. Si quieres practicar solo conmutación y ruteo primero, puedes dejar las contraseñas para el final.

---

## 🏷️ Fase 1: Creación de VLANs en los 3 Switches

### ¿Por qué se hace este paso?
Por defecto, todos los puertos de un switch vienen en la `VLAN 1`, lo que significa que todos los dispositivos están en el **mismo dominio de broadcast** (un mensaje de difusión de una PC de Ventas le llega a los Servidores de IT). Al crear **VLANs (Virtual LANs)**, dividimos un switch físico en varios switches lógicos independientes, mejorando la **seguridad** y el **rendimiento**.

> 💡 **Analogía:** Imagina que el switch es un edificio de oficinas. Sin VLANs, es un galpón abierto donde todos se escuchan gritar entre sí. Crear VLANs es levantar **paredes acústicas** por departamento (`Ventas`, `Admin`, `IT_Servers`, `Gestión`): aunque estén en el mismo edificio físico, un departamento no escucha las conversaciones del otro a menos que pasen por recepción (el Router).

### Comandos (Ejecutar exactamente igual en `SW-CORE`, `SW-ACC1` y `SW-ACC2`)

```ios
enable
configure terminal

vlan 10
 name VENTAS
vlan 20
 name ADMIN
vlan 30
 name IT_SERVERS
vlan 99
 name MGMT_NATIVA
exit
```

### Explicación comando por comando
* `vlan <ID>`: Crea la VLAN en la base de datos interna del switch (archivo `vlan.dat`) e ingresa al submodo `config-vlan`.
* `name <NOMBRE>`: Le asigna una etiqueta legible para que al diagnosticar con `show vlan brief` sepas de qué área es cada número.
* `exit`: Regresa al modo de configuración global.

> 🚨 **Nota Importante:** ¡Debes crear las 4 VLANs (`10`, `20`, `30` y `99`) también en **`SW-CORE`** y **`SW-ACC1`** aunque `SW-CORE` no tenga PCs conectadas y `SW-ACC1` no tenga servidores de la `VLAN 30`! Si un switch no tiene creada una VLAN en su base de datos, **no reenviará tráfico de esa VLAN por sus enlaces Trunk ni ejecutará STP para ella**.

### ✅ Método de Comprobación (Fase 1)
Ejecuta en cada switch:
```ios
show vlan brief
```
* **¿Qué debes ver?** Las VLANs `10 (VENTAS)`, `20 (ADMIN)`, `30 (IT_SERVERS)` y `99 (MGMT_NATIVA)` deben figurar con estado `active`.

---

## 🔌 Fase 2: Asignación de Puertos de Acceso (Hacia los Hosts)

### ¿Por qué se hace este paso?
Crear las VLANs solo las registra en el switch, pero los puertos siguen perteneciendo a la `VLAN 1`. Ahora debemos indicar **qué enchufe físico (interfaz)** pertenece a **qué VLAN**, y fijarlo en modo **Acceso (`access`)** para que nunca negocie enlaces troncales con una PC (mitigando ataques de *VLAN Hopping*).

> 📌 **Ejemplo:** En `SW-ACC1`, las computadoras `PC1` y `PC2` son del área de Ventas, por lo que sus puertos (`Fa0/3` y `Fa0/4`) se asignan a la `VLAN 10`. En cambio, `PC3` es de Administración, así que su puerto (`Fa0/5`) va a la `VLAN 20`.

### A. Comandos en `SW-ACC1`

```ios
configure terminal

! Puertos de Ventas (PC1 y PC2)
interface range fastEthernet 0/3 - 4
 description Hosts_VENTAS
 switchport mode access
 switchport access vlan 10
 exit

! Puerto de Administración (PC3)
interface fastEthernet 0/5
 description Host_ADMIN
 switchport mode access
 switchport access vlan 20
 exit
```

### B. Comandos en `SW-ACC2`

```ios
configure terminal

! Puerto de Ventas (PC4)
interface fastEthernet 0/3
 description Host_VENTAS_Piso2
 switchport mode access
 switchport access vlan 10
 exit

! Puertos de IT y Servidores (Laptop1, SRV-WEB y SRV-DNS)
interface range fastEthernet 0/4 - 6
 description Hosts_IT_SERVERS
 switchport mode access
 switchport access vlan 30
 exit
```

### Explicación comando por comando
* `interface range fastEthernet 0/3 - 4`: Selecciona varios puertos al mismo tiempo para no repetir los mismos comandos puerto por puerto.
* `description <texto>`: Documenta en la configuración qué hay conectado en ese cable.
* `switchport mode access`: Fuerza al puerto a funcionar exclusivamente como puerto para un dispositivo final (envía y recibe tramas Ethernet **sin etiquetar**).
* `switchport access vlan <ID>`: Asocia ese puerto a la VLAN indicada.

### ✅ Método de Comprobación (Fase 2)
Ejecuta en `SW-ACC1` y `SW-ACC2`:
```ios
show vlan brief
```
* **¿Qué debes ver?** A la derecha de cada VLAN deben aparecer listados los puertos correspondientes (ej. `Fa0/3, Fa0/4` al lado de la `VLAN 10` en `SW-ACC1`, y `Fa0/4, Fa0/5, Fa0/6` al lado de la `VLAN 30` en `SW-ACC2`).

---

## 🛣️ Fase 3: Configuración de Enlaces Troncales (Trunks 802.1Q)

### ¿Por qué se hace este paso?
Si quisiéramos pasar 4 VLANs entre dos switches usando puertos de acceso, necesitaríamos **4 cables físicos** (uno por cada VLAN). Un enlace **Trunk (Troncal)** usa el protocolo **IEEE 802.1Q** para transportar el tráfico de **todas las VLANs por un único cable**, agregando una pequeña etiqueta (*tag*) de 4 bytes a cada trama Ethernet antes de enviarla al otro switch o al router.

> 💡 **Analogía:** Un puerto `access` es un **carril exclusivo** para autos de un solo color. Un enlace `trunk` es una **autopista multipropósito** donde viajan autos de todos los colores, pero cada auto lleva un **sticker en el parabrisas (etiqueta 802.1Q)** que dice a qué VLAN pertenece. La **VLAN Nativa (99)** es la única excepción: sus autos viajan sin sticker.

### Comandos en los 3 Switches

#### En `SW-CORE` (Tiene 3 enlaces Trunk: `Gi0/1` hacia `R1`, `Fa0/1` hacia `SW-ACC1`, `Fa0/2` hacia `SW-ACC2`):
```ios
configure terminal
interface range fastEthernet 0/1 - 2, gigabitEthernet 0/1
 description Enlaces_Trunk
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 exit
```

#### En `SW-ACC1` (Tiene 2 enlaces Trunk: `Fa0/1` hacia `SW-CORE`, `Fa0/2` hacia `SW-ACC2`):
```ios
configure terminal
interface range fastEthernet 0/1 - 2
 description Enlaces_Trunk
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99
 exit
```

#### En `SW-ACC2` (Tiene 2 enlaces Trunk: `Fa0/1` hacia `SW-CORE`, `Fa0/2` hacia `SW-ACC1`):
```ios
configure terminal
interface range fastEthernet 0/1 - 2
 description Enlaces_Trunk
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
 switchport trunk native vlan 99
 exit
```

### Explicación comando por comando
* `switchport mode trunk`: Configura la interfaz como enlace troncal 802.1Q permanente.
* `switchport trunk native vlan 99`: Cambia la VLAN nativa por defecto (`VLAN 1`) a la `VLAN 99` por seguridad. Todo tráfico sin etiquetar que circule por el trunk se asignará a la `VLAN 99`.
* `switchport trunk allowed vlan 10,20,30,99`: Restringe el trunk para que solo deje pasar las VLANs que realmente usamos en la empresa (evita tráfico innecesario).

> 🚨 **Nota Importante:** La `native vlan 99` **debe coincidir exactamente en ambos extremos del cable**. Mientras estés configurando un switch y todavía no hayas configurado el de enfrente, verás mensajes de alerta de CDP (`%CDP-4-NATIVE_VLAN_MISMATCH`). Es normal y desaparecerán apenas configures el otro extremo.

> ⚠️ **Consideración (Switches Capa 3 como el 3560):** Si para `SW-CORE` elegiste un switch **2960**, el comando `switchport mode trunk` funciona directo. Si elegiste un **3560**, primero debes escribir `switchport trunk encapsulation dot1q` antes de `switchport mode trunk`.

### ✅ Método de Comprobación (Fase 3)
Ejecuta en los 3 switches:
```ios
show interfaces trunk
```
* **¿Qué debes ver?**
  * Columna `Mode`: `on`
  * Columna `Encapsulation`: `802.1q`
  * Columna `Status`: `trunking`
  * Columna `Native vlan`: `99`
  * Nota: En `SW-CORE`, el puerto `Gi0/1` (que va hacia el router `R1`) recién aparecerá en esta lista cuando encendamos la interfaz `Gi0/0` del router en la Fase 5.

---

## 🔄 Fase 4: Spanning Tree Protocol (STP) para romper el Ciclo

### ¿Por qué se hace este paso?
Observa el triángulo entre `SW-CORE`, `SW-ACC1` y `SW-ACC2`:
* Tener un cable extra entre `SW-ACC1 (Fa0/2)` y `SW-ACC2 (Fa0/2)` nos da **alta disponibilidad (redundancia)**: si alguien corta el cable que sube de `SW-ACC2` a `SW-CORE`, el Piso 2 sigue conectado a través de `SW-ACC1`.
* **El problema:** Las tramas Ethernet de Capa 2 **no tienen campo TTL (Time-To-Live)**. Si una PC envía un broadcast (ej. un ARP preguntando quién tiene cierta IP), los 3 switches se lo reenviarán en círculo infinitamente, causando una **tormenta de broadcast** que colapsa toda la red en segundos.
* **La solución (STP):** El protocolo **Spanning Tree** detecta el ciclo y **bloquea lógicamente un puerto** (`BLK` / luz ámbar en Packet Tracer), dejándolo en reserva. Si un enlace principal cae, STP desbloquea ese puerto automáticamente.
* **¿Por qué elegir nosotros el Root Bridge?** Si dejamos STP en automático, gana el switch con la dirección MAC más baja (suele ser el switch más viejo de la oficina). Nosotros debemos forzar a que **`SW-CORE` sea el Root Bridge Principal**, porque está conectado al Router `R1` y queremos que todo el tráfico fluya por el camino más corto hacia él.

> 💡 **Analogía:** Imagina 3 estaciones de tren conectadas en triángulo y una regla que dice que ningún tren puede dar vueltas en círculo. STP elige una **Estación Central (`Root Bridge` = `SW-CORE`)** y coloca una **barrera baja (`Puerto Bloqueado`)** en la vía lateral entre las dos estaciones secundarias (`SW-ACC1` y `SW-ACC2`). Si se rompe la vía principal hacia la Estación Central, el guarda levanta la barrera lateral al instante.

### A. Comandos en `SW-CORE` (Root Bridge Principal)
```ios
configure terminal
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,99 root primary
exit
```

### B. Comandos en `SW-ACC1` (Root Bridge Secundario + PortFast/BPDU Guard en accesos)
```ios
configure terminal
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,99 root secondary

interface range fastEthernet 0/3 - 5
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit
```

### C. Comandos en `SW-ACC2` (PortFast/BPDU Guard en accesos)
```ios
configure terminal
spanning-tree mode rapid-pvst

interface range fastEthernet 0/3 - 6
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit
```

### Explicación comando por comando
* `spanning-tree mode rapid-pvst`: Activa **Rapid PVST+** (IEEE 802.1w por VLAN), que converge en ~1 segundo ante fallos en vez de los 30–50 segundos del STP clásico (802.1D).
* `spanning-tree vlan 10,20,30,99 root primary`: Baja automáticamente la prioridad del switch (a `24576` o `4096`) para garantizar que `SW-CORE` sea elegido **Root Bridge** en esas VLANs.
* `spanning-tree vlan 10,20,30,99 root secondary`: Fija la prioridad en `28672` (menor que el `32768` por defecto, pero mayor que el primario) para que `SW-ACC1` asuma el rol de Root si `SW-CORE` se apaga.
* `spanning-tree portfast`: Hace que los puertos donde hay PCs o servidores pasen inmediatamente al estado de reenvío (`FWD`) al enchufar el cable, sin esperar 30 segundos. **Solo se usa en puertos de acceso hacia hosts, jamás en Trunks.**
* `spanning-tree bpduguard enable`: Protege los puertos con PortFast: si alguien conecta por error o malicia otro switch en el enchufe de una PC (el cual enviará tramas BPDU de STP), el puerto se apaga automáticamente (`err-disabled`) para evitar nuevos ciclos.

### ✅ Método de Comprobación (Fase 4)
1. **En `SW-CORE`**, ejecuta:
   ```ios
   show spanning-tree vlan 10
   ```
   * **¿Qué debes ver?** El texto **`This bridge is the root`** debe aparecer en la sección `Root ID`. Todos sus puertos (`Fa0/1`, `Fa0/2`) deben decir `Desg FWD` (Designated Forwarding).
2. **En `SW-ACC2`**, ejecuta:
   ```ios
   show spanning-tree vlan 10
   ```
   * **¿Qué debes ver?**
     * `Fa0/1` (hacia `SW-CORE`) tendrá rol **`Root`** y estado **`FWD`** (es su mejor camino hacia el Root Bridge).
     * `Fa0/2` (hacia `SW-ACC1`) tendrá rol **`Altn` (Alternate)** y estado **`BLK` (Blocked)**. ¡En Packet Tracer verás el circulito naranja/ámbar sobre `Fa0/2` de `SW-ACC2`!

> 📌 **Ejemplo de prueba de fallo:** Si en Packet Tracer borras el cable entre `SW-CORE (Fa0/2)` y `SW-ACC2 (Fa0/1)`, y vuelves a ejecutar `show spanning-tree vlan 10` en `SW-ACC2`, verás que `Fa0/2` cambia automáticamente de `Altn BLK` a `Root FWD` sin perder conectividad. (Luego vuelve a conectar el cable).

---

## 🌐 Fase 5: Ruteo Inter-VLAN (Router-on-a-Stick en `R1`)

### ¿Por qué se hace este paso?
Hasta ahora, `PC1` (`SW-ACC1`) y `PC4` (`SW-ACC2`) pueden comunicarse entre sí porque ambas están en la `VLAN 10`. Sin embargo, **`PC1` (`VLAN 10`) no puede hablar con `PC3` (`VLAN 20`) ni con los servidores (`VLAN 30`)**, porque están en subredes IP distintas.
Para comunicar VLANs diferentes se necesita un dispositivo de **Capa 3 (el Router `R1`)**. Con la técnica **Router-on-a-Stick**, dividimos la única interfaz física `Gi0/0` del router en **4 subinterfaces lógicas** (`.10`, `.20`, `.30` y `.99`), donde cada subinterfaz actúa como el **Default Gateway (Puerta de Enlace)** de su respectiva VLAN.

> 💡 **Analogía:** Si cada VLAN es un país con su propio idioma, el enlace Trunk entre `SW-CORE` y `R1` es un puente internacional, y las subinterfaces de `R1` (`Gi0/0.10`, `Gi0/0.20`, etc.) son las **ventanillas de aduana** que reciben paquetes de una VLAN, miran la IP de destino y los reenvían etiquetados hacia la otra VLAN.

### Comandos en `R1`

```ios
enable
configure terminal

! 1. Subinterfaz para VLAN 10 (VENTAS)
interface gigabitEthernet 0/0.10
 description Gateway_VLAN10_VENTAS
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 exit

! 2. Subinterfaz para VLAN 20 (ADMIN)
interface gigabitEthernet 0/0.20
 description Gateway_VLAN20_ADMIN
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 exit

! 3. Subinterfaz para VLAN 30 (IT_SERVERS)
interface gigabitEthernet 0/0.30
 description Gateway_VLAN30_IT_SERVERS
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 exit

! 4. Subinterfaz para VLAN 99 (MGMT_NATIVA)
interface gigabitEthernet 0/0.99
 description Gateway_VLAN99_MGMT_NATIVA
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0
 exit

! 5. Encender la interfaz física principal
interface gigabitEthernet 0/0
 no shutdown
 exit
```

### Explicación comando por comando
* `interface gigabitEthernet 0/0.<ID>`: Crea una subinterfaz virtual asociada a la interfaz física `Gi0/0`.
* `encapsulation dot1Q <VLAN_ID>`: Le indica al router qué etiqueta VLAN 802.1Q debe leer y colocar en esa subinterfaz. **Es obligatorio ejecutar este comando antes de `ip address`** (si intentas poner la IP primero, el router dará error).
* `encapsulation dot1Q 99 native`: Le avisa al router que la `VLAN 99` es la **VLAN Nativa** del trunk, por lo que sus tramas viajarán sin etiqueta 802.1Q, coincidiendo con el comando `switchport trunk native vlan 99` que pusimos en `SW-CORE`.
* `ip address <IP> <Máscara>`: Asigna la dirección IP que usarán los hosts de esa VLAN como su `Default Gateway`.
* `interface gigabitEthernet 0/0` seguido de `no shutdown`: En los routers Cisco, las interfaces físicas vienen **apagadas (`administratively down`) por defecto**. Al encender la interfaz física `Gi0/0`, se levantan automáticamente todas sus subinterfaces.

> 🚨 **Nota Importante:** Jamás le asignes una dirección IP a la interfaz física `Gi0/0` cuando uses Router-on-a-Stick; las direcciones IP van únicamente en las subinterfaces (`Gi0/0.10`, `.20`, `.30`, `.99`).

### ✅ Método de Comprobación (Fase 5)
Ejecuta en `R1`:
```ios
show ip interface brief
```
* **¿Qué debes ver?** `GigabitEthernet0/0` y sus 4 subinterfaces (`.10`, `.20`, `.30`, `.99`) con sus respectivas IPs y con `Status: up` y `Protocol: up`.
* También puedes ejecutar `show ip route`: verás las 4 redes (`192.168.10.0/24`, `20.0/24`, `30.0/24` y `99.0/24`) marcadas con la letra **`C`** (*Connected*).

---

## 📨 Fase 6: Configuración del Servidor DHCP en `R1` y Hosts

### ¿Por qué se hace este paso?
En una empresa real, configurar IPs a mano en cientos de PCs de usuarios (`VENTAS` y `ADMIN`) es propenso a errores (IPs duplicadas, máscaras mal puestas). Por eso configuramos **DHCP (Dynamic Host Configuration Protocol)** en `R1` para que entregue automáticamente IP, máscara, puerta de enlace y servidor DNS a `PC1`, `PC2`, `PC3` y `PC4`.
En cambio, los **Servidores (`SRV-WEB`, `SRV-DNS`) y equipos de IT (`Laptop1`)** en la `VLAN 30` llevan **IP Estática**, porque los servidores necesitan tener siempre la misma dirección fija para que los clientes los encuentren.

> 💡 **Analogía:** El servidor DHCP es como la **recepción de un hotel**: cuando llega un huésped temporal (`PC1` a `PC4`), le asigna cualquier habitación libre del piso 10 o 20. En cambio, las oficinas de dirección y mantenimiento (`SRV-WEB`, `SRV-DNS`, `R1`) tienen el número de puerta reservado de forma permanente (`ip dhcp excluded-address` e IPs estáticas).

### A. Comandos de DHCP en `R1`

```ios
configure terminal

! 1. Excluir las primeras 10 IPs de cada red (para que DHCP no entregue la .1 del router ni futuras IPs fijas)
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10

! 2. Crear el Pool DHCP para VLAN 10 (VENTAS)
ip dhcp pool POOL_VENTAS
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.30.11
 exit

! 3. Crear el Pool DHCP para VLAN 20 (ADMIN)
ip dhcp pool POOL_ADMIN
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.30.11
 exit
```

### Explicación comando por comando
* `ip dhcp excluded-address <IP_inicial> <IP_final>`: Reserva ese rango de IPs para que el router **nunca** las asigne a un cliente DHCP. Así las PCs empezarán a recibir desde la `.11` en adelante (`192.168.10.11`, `.12`, etc.).
* `ip dhcp pool <NOMBRE>`: Crea un ámbito de direcciones DHCP.
* `network <Red> <Máscara>`: El router compara la IP de la subinterfaz por donde entró la petición DHCP (ej. `Gi0/0.10` = `192.168.10.1`) con este comando `network` para saber de qué pool debe darle IP a esa PC.
* `default-router <IP>`: La puerta de enlace que se le configurará automáticamente a la PC.
* `dns-server 192.168.30.11`: La IP de nuestro `SRV-DNS` (en la `VLAN 30`) que recibirán las PCs.

### B. Configuración en la Interfaz Gráfica de los 7 Hosts (Packet Tracer)

1. **En `PC1`, `PC2`, `PC3` y `PC4` (DHCP):**
   * Haz clic en la PC $\rightarrow$ pestaña **Desktop** $\rightarrow$ **IP Configuration** $\rightarrow$ selecciona **DHCP**.
   * Verás `DHCP request successful` y las IPs asignadas:
     * `PC1`: `192.168.10.11` | `PC2`: `192.168.10.12` | `PC4`: `192.168.10.13`
     * `PC3`: `192.168.20.11`
2. **En `Laptop1`, `SRV-WEB` y `SRV-DNS` (Estática):**
   * Haz clic en cada equipo $\rightarrow$ **Desktop** $\rightarrow$ **IP Configuration** $\rightarrow$ deja **Static** y completa:
     * **`Laptop1`**: IP `192.168.30.50` | Máscara `255.255.255.0` | Gateway `192.168.30.1` | DNS `192.168.30.11`
     * **`SRV-WEB`**: IP `192.168.30.10` | Máscara `255.255.255.0` | Gateway `192.168.30.1` | DNS `192.168.30.11`
     * **`SRV-DNS`**: IP `192.168.30.11` | Máscara `255.255.255.0` | Gateway `192.168.30.1` | DNS `192.168.30.11`
3. **(Opcional recomendado) Activar el registro DNS en `SRV-DNS`:**
   * En `SRV-DNS` $\rightarrow$ pestaña **Services** $\rightarrow$ **DNS** $\rightarrow$ pon `DNS Service: On`.
   * En `Name` escribe `empresa.local`, en `Address` escribe `192.168.30.10` (la IP de `SRV-WEB`) y haz clic en **Add**.

### ✅ Método de Comprobación (Fase 6)
Ejecuta en `R1`:
```ios
show ip dhcp binding
```
* **¿Qué debes ver?** Una tabla con las 4 direcciones IP (`192.168.10.11`, `.10.12`, `.10.13` y `192.168.20.11`) asociadas a las direcciones MAC de `PC1`, `PC2`, `PC4` y `PC3`.

---

## 🖥️ Fase 7: Gestión de los Switches (SVI en VLAN 99)

### ¿Por qué se hace este paso?
Un switch de Capa 2 no necesita una dirección IP para conmutar tramas entre PCs. Sin embargo, como administradores de red en la `Laptop1` (`VLAN 30`), queremos poder hacerle `ping` (o conectarnos por SSH/Telnet) a los 3 switches de forma remota sin tener que ir con un cable consola piso por piso.
Para eso creamos una **SVI (Switch Virtual Interface)** en la `VLAN 99` de cada switch y le configuramos su **`ip default-gateway`** (`192.168.99.1`), para que el switch sepa cómo responder a paquetes que vienen desde otra subred (como la `192.168.30.0/24` de IT).

### A. Comandos en `SW-CORE`
```ios
configure terminal
interface vlan 99
 description SVI_Gestion_SW-CORE
 ip address 192.168.99.2 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.99.1
```

### B. Comandos en `SW-ACC1`
```ios
configure terminal
interface vlan 99
 description SVI_Gestion_SW-ACC1
 ip address 192.168.99.3 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.99.1
```

### C. Comandos en `SW-ACC2`
```ios
configure terminal
interface vlan 99
 description SVI_Gestion_SW-ACC2
 ip address 192.168.99.4 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.99.1
```

### Explicación comando por comando
* `interface vlan 99`: Entra a la interfaz virtual de administración del switch asociada a la `VLAN 99`.
* `ip address 192.168.99.X 255.255.255.0`: Le asigna la dirección IP de gestión al switch.
* `no shutdown`: Enciende la interfaz virtual (en los switches las interfaces VLAN vienen apagadas por defecto).
* `ip default-gateway 192.168.99.1`: Como un switch de Capa 2 no tiene tabla de ruteo, este comando le dice: *"Si te habla una IP que no sea de la red `192.168.99.0/24` (por ejemplo, `Laptop1` desde `192.168.30.50`), envíale tu respuesta a `192.168.99.1` (`R1`)"*.

> 🚨 **Nota Importante:** Si olvidas el comando `ip default-gateway 192.168.99.1` en un switch, podrás hacerle ping desde el router `R1`, ¡pero **fallará** cuando intentes hacerle ping desde `Laptop1` (`192.168.30.50`) o cualquier otra VLAN!

---

## 🧪 Fase 8: Guardado y Batería de Pruebas Finales

### 1. Guardar la configuración en todos los equipos (`R1`, `SW-CORE`, `SW-ACC1`, `SW-ACC2`)
En el modo privilegiado (`#`) de cada equipo, ejecuta:
```ios
copy running-config startup-config
```
*(O su abreviatura `write memory` / `wr`). Esto copia la configuración de la memoria RAM volátil a la memoria NVRAM para que no se borre al reiniciar.*

### 2. Pruebas de Conectividad de Extremo a Extremo

Abre el **Command Prompt** de **`Laptop1` (`192.168.30.50`)** o de **`PC1` (`192.168.10.11`)** y realiza estas 4 pruebas:

| Prueba | Comando desde `Laptop1` (`192.168.30.50`) | ¿Qué valida? |
| :--- | :--- | :--- |
| **1. Ping al Gateway propio** | `ping 192.168.30.1` | Que el acceso en `SW-ACC2`, el Trunk hacia `SW-CORE` y la subinterfaz `Gi0/0.30` de `R1` funcionan. |
| **2. Ping Inter-VLAN a Hosts DHCP** | `ping 192.168.10.11` (`PC1`) y `ping 192.168.20.11` (`PC3`) | Que `R1` está ruteando paquetes entre `VLAN 30`, `VLAN 10` y `VLAN 20`. |
| **3. Ping a la Gestión de Switches** | `ping 192.168.99.2`, `ping 192.168.99.3` y `ping 192.168.99.4` | Que la `VLAN 99` nativa, las SVIs y el `ip default-gateway` de los 3 switches están bien configurados. |
| **4. Prueba Web y DNS** | Abrir **Web Browser** en `PC1` y entrar a `http://empresa.local` | Que el servidor DHCP entregó bien el DNS (`192.168.30.11`), el DNS resolvió `192.168.30.10` y `SRV-WEB` respondió por HTTP. |

> ⚠️ **Consideración sobre el primer Ping (ARP):** En Packet Tracer, es totalmente normal que de los 4 paquetes de un `ping` nuevo, **el primero o los dos primeros den `Request timed out.` y los siguientes respondan `Reply from...` (`Success rate 75%`)**. Esto ocurre porque los equipos primero deben descubrir las direcciones MAC mediante el protocolo **ARP** en cada salto. Si repites el mismo `ping` inmediatamente después, dará **4 de 4 recibidos (0% perdidos)**.
