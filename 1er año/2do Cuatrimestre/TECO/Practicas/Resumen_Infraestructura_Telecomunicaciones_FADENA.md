# 📡 Apunte Maestro de Cátedra: Infraestructura de Telecomunicaciones
**Institución:** Universidad de la Defensa Nacional (UNDEF) — Facultad de la Defensa Nacional (FADENA)  
**Cátedra:** Infraestructura de Telecomunicaciones (Material de referencia: Dr. Ing. Maximiliano C.A. Fusario)  
**Formato:** Guía Teórico-Práctica con Ejemplos Resueltos y Laboratorios en Cisco Packet Tracer  

---

> 👨‍🏫 **BIENVENIDA DEL DOCENTE:**  
> ¡Hola! Preparé este manual de estudio integrando **absolutamente todos los contenidos teóricos, fórmulas, tablas, diagramas y ejemplos de clase** presentes en las ocho presentaciones oficiales de la materia. Además de explicar el *qué*, profundizaremos en el *porqué* detrás de cada protocolo y mecanismo físico o lógico, e incluiremos **ejercicios paso a paso** y **prácticas guiadas para Cisco Packet Tracer** al cierre de cada unidad.  
>
> *💡 **Tip para exportar a PDF:** Este documento incluye saltos de página (`page-break`), tablas estructuradas y diagramas en bloques de texto monoespaciado de ancho fijo para que puedas exportarlo directamente a PDF desde **VS Code** (extensión Markdown PDF), **Typora**, **Obsidian** o **Pandoc** sin que se corten los esquemas ni las tablas.*

---

## 🧭 Guía Visual de Notas e Iconografía

Para que tu lectura y repaso previo a los exámenes sea ágil y visual, cada bloque especial está señalizado con un identificador específico:

* 📌 **CONCEPTO CLAVE:** Definición fundamental o principio teórico que debés dominar con precisión.
* 🧮 **FÓRMULA / CÁLCULO PASO A PASO:** Expresiones matemáticas, reglas binarias y desarrollos numéricos detallados.
* 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA:** Casos concretos y ejercicios numéricos desarrollados en las diapositivas de clase, explicados paso a paso.
* ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN:** Detalles técnicos sutiles, excepciones de protocolos o comparaciones críticas que suelen evaluarse en parciales y finales.
* 👨‍🏫 **NOTA DEL DOCENTE:** Aclaraciones pedagógicas y analogías del mundo real para afianzar la intuición ingenieril.
* 🛠️ **LABORATORIO EN PACKET TRACER:** Práctica guiada paso a paso con topología, direccionamiento, comandos de Cisco IOS y pruebas de simulación.

---

## 📑 Índice General del Programa

1. [**Módulo 1 (Unidad 7):** Tratamiento de Errores en los Canales de Comunicación de Datos](#módulo-1-unidad-7-tratamiento-de-errores-en-los-canales-de-comunicación-de-datos)
2. [**Módulo 2 (Unidad 8):** Modulación Analógica y Digital, Digitalización PCM y Jerarquía PDH](#módulo-2-unidad-8-modulación-analógica-y-digital-digitalización-pcm-y-jerarquía-pdh)
3. [**Módulo 3 (Unidad 9 - Parte A):** Introducción a las Redes, Topologías, Confiabilidad y Seguridad](#módulo-3-unidad-9---parte-a-introducción-a-las-redes-topologías-confiabilidad-y-seguridad)
4. [**Módulo 4 (Unidad 9 - Parte B):** Modelos de Referencia (OSI y TCP/IP), Protocolos, PDU y Acceso a Datos](#módulo-4-unidad-9---parte-b-modelos-de-referencia-osi-y-tcpip-protocolos-pdu-y-acceso-a-datos)
5. [**Módulo 5 (Unidad 10):** Redes LAN, Trama Ethernet, Direccionamiento MAC y Switching de Capa 2](#módulo-5-unidad-10-redes-lan-trama-ethernet-direccionamiento-mac-y-switching-de-capa-2)
6. [**Módulo 6 (Capa 3):** Capa de Red, Paquetes IPv4 e IPv6, Enrutamiento Estático y Dinámico (RIP, OSPF, EIGRP)](#módulo-6-capa-3-capa-de-red-paquetes-ipv4-e-ipv6-enrutamiento-estático-y-dinámico-rip-ospf-eigrp)
7. [**Módulo 7 (Subredes y VLSM):** Direccionamiento IPv4, Clases, NAT, Subnetting Fijo (FLSM) y Variable (VLSM)](#módulo-7-subredes-y-vlsm-direccionamiento-ipv4-clases-nat-subnetting-fijo-flsm-y-variable-vlsm)
8. [**Módulo 8 (Capa 4):** Capa de Transporte, TCP vs. UDP, Puertos, Sockets, Handshake y Control de Flujo](#módulo-8-capa-4-capa-de-transporte-tcp-vs-udp-puertos-sockets-handshake-y-control-de-flujo)
9. [**Anexo Práctico Integrador:** Guía de Uso del Archivo `FADENA.pkt` y Resumen de Comandos Cisco IOS](#anexo-práctico-integrador-guía-de-comandos-cisco-ios-y-práctica-integral)

<div style="page-break-after: always;"></div>

---

# Módulo 1 (Unidad 7): Tratamiento de Errores en los Canales de Comunicación de Datos

## 1.1. Causas de los Errores en las Comunicaciones

Cuando transmitimos señales a través de cualquier medio físico (cobre, fibra óptica o el espectro radioeléctrico), la señal recibida nunca es idéntica a la transmitida. Los errores de bit aparecen cuando el receptor interpreta un `1` lógico donde se envió un `0`, o viceversa.

> 📌 **CONCEPTO CLAVE — Las 4 causas físicas de errores en un canal:**
> 1. **Ruido:** Señales eléctricas o electromagnéticas no deseadas que se suman a la señal útil en el canal (ruido térmico, ruido impulsivo, diafonía o *crosstalk*, interferencia externa).
> 2. **Atenuación:** Pérdida progresiva de potencia (amplitud) de la señal a medida que se propaga por el medio. Si la señal llega demasiado débil al receptor, se confunde con el piso de ruido.
> 3. **Distorsión:** Deformación de la forma de onda original debido a que las distintas componentes de frecuencia de la señal viajan a diferentes velocidades de propagación o sufren diferente atenuación (distorsión por retardo o de amplitud).
> 4. **Ancho de banda ($\Delta f$) insuficiente y en consecuencia $T > C$:** Si el ancho de banda físico disponible en el canal ($\Delta f$) es insuficiente para la velocidad de transmisión o tasa de transferencia pretendida ($T$), se supera la **Capacidad máxima del canal ($C$)** definida por las leyes de Nyquist y Shannon ($T > C$), provocando solapamiento entre símbolos (interferencia intersimbólica) y errores inevitables.

---

## 1.2. Medición de los Errores: BER (*Bit Error Rate*)

Para cuantificar la calidad de un enlace digital utilizamos la **Tasa de Error de Bits** o **BER** (*Bit Error Rate*), que expresa la probabilidad empírica de que un bit transmitido llegue alterado.

> 🧮 **FÓRMULA — Tasa de Error de Bits (BER):**
> $$\text{BER} = \frac{\text{Nro. de bits erróneos recibidos}}{\text{Cantidad total de Bits transmitidos (Tx)}}$$
> *Nota:* Es una magnitud adimensional y suele expresarse en potencias negativas de diez (por ejemplo, $10^{-4}$, $10^{-6}$, $10^{-9}$). **Cuanto más negativo es el exponente, mejor es la calidad del enlace.**

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Comparación de enlaces E1 y E2:**
> **Enunciado:** Se dispone de dos enlaces de telecomunicaciones, **E1** y **E2**, cuyos valores de BER son respectivamente:
> * $\text{BER}_{E1} = 10^{-4}$
> * $\text{BER}_{E2} = 10^{-9}$
>
> **Pregunta 1: ¿Qué enlace tiene una tasa de error menor (es decir, mayor calidad)?**
> * **Enlace E1 ($10^{-4} = \frac{1}{10.000}$):** Tiene en promedio **1 bit erróneo por cada $10.000\text{ bits}$ recibidos** (típico de enlaces de radio o líneas telefónicas antiguas ruidosas).
> * **Enlace E2 ($10^{-9} = \frac{1}{1.000.000.000}$):** Tiene en promedio **1 bit erróneo por cada $1.000.000.000\text{ bits}$ recibidos** (típico de enlaces de fibra óptica o redes LAN cableadas modernas).
> * **Conclusión:** El enlace **E2** tiene una tasa de error muchísimo menor (es $100.000$ veces más confiable que E1).
>
> **Pregunta 2: ¿Qué sucede si transmitimos paquetes todos iguales de longitud $L = 5.000\text{ bits}$ por ambos enlaces?**
> * En el enlace **E1** ($\text{BER} = 10^{-4}$), como ocurre $1$ error cada $10.000\text{ bits}$ y cada paquete mide $5.000\text{ bits}$, estadísticamente **1 de cada 2 paquetes transmitidos ($50\%$ de los paquetes) sufrirá un error** y será descartado, haciendo caer drásticamente el rendimiento útil (*throughput*) de la red.
> * En cambio, en el enlace **E2** ($\text{BER} = 10^{-9}$), se pueden transmitir $200.000$ paquetes consecutivos de $5.000\text{ bits}$ ($200.000 \times 5.000 = 10^9\text{ bits}$) sufriendo estadísticamente **un solo paquete con error cada $200.000$ paquetes**.

> 👨‍🏫 **NOTA DEL DOCENTE:**  
> Esta pregunta de clase explica por qué en enlaces muy ruidosos (como enlaces tácticos de radio HF/VHF en defensa) conviene usar **tramas más cortas**: si el paquete es muy largo y el BER es alto, la probabilidad de que al menos un bit falle dentro del paquete se acerca al $100\%$, obligando a retransmitir todo el bloque constantemente.

---

## 1.3. Políticas Empleadas en el Tratamiento de Errores en las Redes

Cuando un nodo o dispositivo de red detecta que una unidad de datos (trama, paquete o segmento) llegó con errores, los protocolos de comunicaciones aplican una de las **tres políticas fundamentales**:

| Política | Acción del Protocolo | Ejemplo Típico | Capa OSI |
| :--- | :--- | :--- | :---: |
| **Política 1** | **Detectar errores y eliminar el paquete** (descarte silencioso, sin avisar ni pedir retransmisión). | **Ethernet** (IEEE 802.3) | Capa 2 (Enlace) |
| **Política 2** | **Detectar errores + eliminar el paquete + avisar al extremo emisor** (mediante un protocolo de control, pero sin retransmitir). | **Protocolo IP** (avisa mediante **ICMP**) | Capa 3 (Red) |
| **Política 3** | **Detectar errores + eliminar el paquete + pedir la retransmisión** al extremo transmisor. | **TCP**, **HDLC**, **X.25**, **ATM** | Capa 4 / Capa 2 orientada a conexión |

### Detalle de cada política según las diapositivas de cátedra:

#### 1️⃣ Política 1: Detectar errores y eliminar paquete (Ej.: Ethernet)
En una trama Ethernet, el último campo es la **Secuencia de Verificación de Trama (FCS - *Frame Check Sequence*)** de **4 bytes (32 bits)** que utiliza un algoritmo **CRC**.

```text
+-----------+--------------------+--------------+--------------+------+-----------+-------------+
| Preámbulo | Inicio delimitador |  Dirección   |  Dirección   | Tipo |   Datos   |  Secuencia  |
|  sincron. |   de trama (SFD)   |   Destino    |    Origen    |      |           | verificac.  |
| (7 Bytes) |     (1 Byte)       |  (6 Bytes)   |  (6 Bytes)   | (2B) |(46-1500 B)| FCS (4B CRC)|
+-----------+--------------------+--------------+--------------+------+-----------+-------------+
                                 |<------------ Cubiertos por el CRC ------------>|
```

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN:**  
> El campo FCS de **4 bytes** (con protocolo CRC) en Ethernet verifica errores únicamente sobre los campos de **Dirección Destino, Dirección Origen, Tipo y Datos**. **NO incluye los campos de sincronismo iniciales** (Preámbulo de 7 bytes ni el Delimitador de Inicio de Trama de 1 byte). Si el CRC calculado por la tarjeta de red (NIC) o el switch receptor no coincide con el FCS, la trama se elimina inmediatamente sin avisar a nadie.

#### 2️⃣ Política 2: Detectar errores + eliminar paquete + avisar (Ej.: Protocolo IP + ICMP)
El protocolo **IP** opera bajo la filosofía **Best Effort** (*Mejor Esfuerzo*):
* No tiene calidad de servicio (QoS) intrínseca.
* No corrige errores de datos de usuario ni problemas de congestionamiento.
* Incluye un campo de **control de errores (Header Checksum) que protege ÚNICAMENTE a la cabecera IP (NO a los datos)**.
* Cuando un router o host desecha un datagrama IP (por ejemplo, por expiración de TTL, destino inalcanzable o problema de parámetros), el protocolo se apoya en **ICMP (*Internet Control Message Protocol*)**, el cual encapsula la **Cabecera IP** del datagrama descartado dentro de un mensaje ICMP para **avisar al emisor** del descarte.

#### 3️⃣ Política 3: Detectar + eliminar + pedir retransmisión (Ej.: TCP, HDLC, X.25, ATM)
En protocolos confiables (como **HDLC** en enlace o **TCP** en transporte), además de un campo de detección de errores (como **CRC** o **Checksum**), el encabezado incluye un **Campo de Control** (con números de secuencia y acuses de recibo):

```text
Estructura de Trama orientada a conexión (ej. HDLC):
+--------+--------+-----------------------+---------------+--------+--------+
|  FLAG  |  DIR   |        CONTROL        |     DATOS     |  CRC   |  FLAG  |
+--------+--------+-----------------------+---------------+--------+--------+
                  |  [ N(S) | N(R) | P/F ]|               |   ^
                  +-----------------------+               |   +-- Si hay errores se tira la
                  Permite individualizar la trama         |       trama, pero se pide
                  y pedir la retransmisión al Tx.         |       retransmisión usando CONTROL.
```

---

## 1.4. Métodos para Detectar Errores

La cátedra desarrolla tres métodos matemáticos fundamentales para la detección de errores: **BCC**, **Suma de Verificación (Checksum)** y **CRC**.

---

### A) Método BCC (*Block Check Character* — Carácter de Verificación por Bloque)

> 📌 **CONCEPTO CLAVE — ¿Cómo funciona el BCC (Paridad Bidimensional)?**  
> Combina dos controles de paridad simultáneos sobre un conjunto de caracteres:
> 1. **1° Control de Paridad (Paridad por carácter / VRC):** A cada carácter de datos se le añade al final un bit de paridad (por ejemplo, **Paridad PAR**, de modo que la cantidad total de unos `1` en el carácter sea un número par).
> 2. **2° Control con Bloque (Bloque de Control / LRC):** Se toman todos los caracteres transmitidos y se calcula la paridad entre los bits que ocupan la **misma posición** (el 1.er bit de cada carácter entre sí, el 2.º bit de cada carácter entre sí, etc., incluyendo los bits de paridad del primer control). El carácter resultante se envía al final como **Bloque de Control (BCC)**.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Cálculo de BCC con Paridad PAR:**
> Supongamos que queremos transmitir 4 bloques de datos de 4 bits cada uno: `1001`, `0001`, `1100` y `0101` utilizando **Paridad PAR**.
>
> **Paso 1 (1° Control de Paridad por carácter):** Agregamos una 5.ª columna (`PAR`) para que cada fila tenga una cantidad par de unos (`1`):
> * Bloque 1: `1 0 0 1` (tiene dos `1`, que ya es par) $\rightarrow$ bit de paridad = **`0`** $\rightarrow$ Se transmite: `1 0 0 1` **`0`**
> * Bloque 2: `0 0 0 1` (tiene un solo `1`, impar) $\rightarrow$ bit de paridad = **`1`** $\rightarrow$ Se transmite: `0 0 0 1` **`1`**
> * Bloque 3: `1 1 0 0` (tiene dos `1`, par) $\rightarrow$ bit de paridad = **`0`** $\rightarrow$ Se transmite: `1 1 0 0` **`0`**
> * Bloque 4: `0 1 0 1` (tiene dos `1`, par) $\rightarrow$ bit de paridad = **`0`** $\rightarrow$ Se transmite: `0 1 0 1` **`0`**
>
> **Paso 2 (2° Control con bloque - Cálculo del BCC por columnas/posiciones):**
> Alineamos los 4 caracteres de 5 bits y calculamos la paridad par vertical columna por columna:
>
> | Carácter | Bit 1 | Bit 2 | Bit 3 | Bit 4 | Bit 5 (PAR) |
> | :--- | :---: | :---: | :---: | :---: | :---: |
> | Carácter 1 | `1` | `0` | `0` | `1` | **`0`** |
> | Carácter 2 | `0` | `0` | `0` | `1` | **`1`** |
> | Carácter 3 | `1` | `1` | `0` | `0` | **`0`** |
> | Carácter 4 | `0` | `1` | `0` | `1` | **`0`** |
> | *Cantidad de unos en la columna* | *(dos `1`)* | *(dos `1`)* | *(cero `1`)* | *(tres `1`)* | *(un `1`)* |
> | **Bloque de Control (BCC)** | **`0`** | **`0`** | **`0`** | **`1`** | **`1`** |
>
> **Secuencia completa transmitida en línea:**  
> `[10010] [00011] [11000] [01010] [00011 (BCC)]`
>
> *Ventaja:* Si un bit se altera durante el viaje, fallará tanto la paridad de su carácter como la paridad de la columna correspondiente en el BCC, permitiendo detectar (e incluso localizar en la coordenada fila-columna) errores simples y detectar la gran mayoría de errores dobles.

---

### B) Método Suma de Verificación (*Checksum*)

> 📌 **CONCEPTO CLAVE — El Checksum en la familia TCP/IP:**
> Es el método estándar empleado por toda la suite de protocolos **TCP/IP**, pero cada protocolo lo aplica con un alcance distinto:
> * **IP (IPv4):** Solo lo aplica a la **cabecera IP**, **NO** a los datos (porque la cabecera cambia en cada router al decrementarse el campo TTL). *(Nota: En IPv6 se eliminó el Checksum de capa 3 para agilizar el enrutamiento).*
> * **UDP:** Lo aplica a **todo el datagrama UDP** (cabecera + datos + pseudo-cabecera), pero en IPv4 **no es obligatorio** (puede ir en cero).
> * **TCP:** Lo aplica a **todo el segmento TCP** (cabecera + datos + pseudo-cabecera) y **es obligatorio**.
> * **Regla de oro:** En el campo Checksum del paquete **solo se transmite el complemento a 1** de la suma obtenida.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Cálculo y Verificación del Checksum (4 bits):**
> **Enunciado:** Se desean transmitir las siguientes 4 palabras binarias de 4 bits:
> * $A = 0011$ (3 en decimal)
> * $B = 1000$ (8 en decimal)
> * $C = 0010$ (2 en decimal)
> * $D = 0101$ (5 en decimal)
>
> **1. Cálculo en el Emisor (Suma en complemento a 1 con acarreo circular):**
> ```text
>     A :   0 0 1 1
>   + B :   1 0 0 0
>   ---------------
>           1 0 1 1   (Suma parcial A + B)
>   + C :   0 0 1 0
>   ---------------
>           1 1 0 1   (Suma parcial A + B + C)
>   + D :   0 1 0 1
>   ---------------
>       (1) 0 0 1 0   <-- Se genera un acarreo (carry) de '1' fuera de los 4 bits.
>   +             1   <-- El acarreo se suma al bit menos significativo (acarreo circular)
>   ===============
>   Suma:   0 0 1 1   <-- Resultado de la Suma en el transmisor
> ```
>
> **2. Obtención del Checksum a transmitir (Complemento a 1 de la Suma):**
> Invertimos todos los bits de la `Suma` (`0011`), cambiando `0` por `1` y `1` por `0`:
> $$\text{Checksum} = \overline{0011} = \mathbf{1100}$$
> *(Este valor `1100` es el que viaja dentro del campo Checksum del paquete).*
>
> **3. Verificación en el Receptor:**
> El receptor suma todas las palabras recibidas ($A + B + C + D$) obteniendo nuevamente `0011`, y le suma el **Checksum** recibido (`1100`):
> ```text
>   Suma recibida :   0 0 1 1
>   + Checksum Rx :   1 1 0 0
>   =========================
>   Resultado     :   1 1 1 1  (Todos los bits en '1' -> ¡Transmisión SIN ERRORES!)
> ```
> *(Si algún bit es `0` en el resultado final, el receptor sabe que hubo un error y descarta el paquete/segmento).*

---

### C) Método CRC (*Código de Redundancia Cíclica*)

Es el método más robusto de detección de errores en tramas de enlace de datos (Ethernet, Wi-Fi, HDLC). Representa las cadenas de bits como coeficientes de **polinomios** y opera mediante **aritmética de módulo 2**.

> 📌 **CONCEPTO CLAVE — Aritmética Módulo 2 y Compuerta XOR:**
> En la aritmética de **módulo 2**:
> * **No existen acarreos (*carry*) en la suma, ni préstamos (*borrow*) en la resta.**
> * La suma y la resta binaria en módulo 2 son idénticas y equivalen exactamente a la operación lógica **OR Exclusiva (XOR, símbolo $\oplus$)**:
>   $$X = A \oplus B$$
>
> | Entrada $A$ | Entrada $B$ | Salida $X = A \oplus B$ | Regla práctica |
> | :---: | :---: | :---: | :--- |
> | `0` | `0` | **`0`** | Bits iguales dan `0` |
> | `0` | `1` | **`1`** | Bits distintos dan `1` |
> | `1` | `0` | **`1`** | Bits distintos dan `1` |
> | `1` | `1` | **`0`** | Bits iguales dan `0` |

> 🧮 **FÓRMULA — Algoritmo Matemático del CRC:**
> Definimos los cuatro polinomios que intervienen:
> * $M(x)$: Polinomio del mensaje original a transmitir.
> * $G(x)$: Polinomio generador (conocido previamente por emisor y receptor, de grado $r$).
> * $C(x)$: Polinomio cociente de la división.
> * $R(x)$: Polinomio resto de la división (de $r$ bits, que actuará como CRC).
>
> **Paso 1 (En el Transmisor):** Se multiplica $M(x)$ por $x^r$ (lo que equivale a **agregar $r$ ceros a la derecha** del mensaje) y se divide en módulo 2 por $G(x)$:
> $$\frac{x^r \cdot M(x)}{G(x)} = C(x) + \frac{R(x)}{G(x)}$$
> **Paso 2 (Formación de la trama a transmitir $\text{Tx}$):** Se reemplazan los $r$ ceros agregados por el resto obtenido $R(x)$:
> $$\text{Tx}(x) = x^r \cdot M(x) + R(x)$$
> **Paso 3 (En el Receptor):** El receptor divide toda la trama recibida $\text{Tx}(x)$ por el mismo polinomio generador $G(x)$. Si no hubo errores durante la transmisión, **el resto de la división es exactamente cero ($0$)**:
> $$\frac{x^r \cdot M(x) + R(x)}{G(x)} = C(x) + \mathbf{0}$$

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — División Polinómica CRC Paso a Paso:**
> **Datos de la diapositiva:**
> * Mensaje a transmitir: $M(x) = \mathbf{10010101}$ (8 bits)
> * Polinomio generador: $G(x) = x^3 + x^2 + 1 = 1\cdot x^3 + 1\cdot x^2 + 0\cdot x^1 + 1\cdot x^0 = \mathbf{1101}$ (4 bits, grado $r = 3$).
>
> **1. Cálculo del Resto $R(x)$ en el Transmisor:**  
> Como el grado de $G(x)$ es $r = 3$, agregamos **3 ceros** (`000`) al final de $M(x)$, formando el dividendo `10010101000`, y dividimos mediante XOR por `1101`:
>
> ```text
>   Mx = 10010101  |  Xr = 000
>   ---------------+----------
>   1 0 0 1 0 1 0 1   0 0 0   <-- Dividendo inicial (Mx con 3 ceros agregados)
>   1 1 0 1                   <-- XOR con G(x) = 1101
>   -------
>   0 1 0 0 0                 <-- Resultado XOR (0100) y baja el siguiente bit (0)
>     1 1 0 1                 <-- XOR con G(x)
>     -------
>     0 1 0 1 1               <-- Resultado XOR (0101) y baja el siguiente bit (1)
>       1 1 0 1               <-- XOR con G(x)
>       -------
>       0 1 1 0 0             <-- Resultado XOR (0110) y baja el siguiente bit (0)
>         1 1 0 1             <-- XOR con G(x)
>         -------
>         0 0 0 1 1 0 0       <-- Queda 0001; bajan '1', '0' y '0' hasta formar 4 bits (1100)
>               1 1 0 1       <-- XOR con G(x)
>               -------
>               0 0 0 1 0     <-- Queda 0001 y baja el último '0' -> Resto de 3 bits: 010
> ```
> * **Resto obtenido ($R(x)$):** **`010`**
> * **Trama transmitida al canal ($\text{Tx}$):** `10010101` + `010` = **`10010101010`**
>
> **2. Comprobación en el Receptor:**  
> El receptor recibe `10010101010` y lo divide en módulo 2 por $G(x) = 1101$:
> ```text
>   Mx = 10010101  | Resto = 010
>   ---------------+------------
>   1 0 0 1 0 1 0 1   0 1 0
>   1 1 0 1
>   -------
>   0 1 0 0 0
>     1 1 0 1
>     -------
>     0 1 0 1 1
>       1 1 0 1
>       -------
>       0 1 1 0 0
>         1 1 0 1
>         -------
>         0 0 0 1 1 0 1       <-- Al bajar los bits del resto (1 y 0) queda 1101
>               1 1 0 1       <-- XOR con G(x) = 1101
>               -------
>               0 0 0 0 0     <-- ¡RESTO = 000! Trama recibida SIN ERRORES.
> ```

---

## 1.5. Métodos para Corregir Errores de Transmisión

Una vez detectado un error, existen tres enfoques para corregirlo:

1. **Códigos Autocorrectores (ej. Códigos de Hamming, Hagelbarger, etc.):**
   * Permiten al receptor no solo detectar qué bit falló, sino corregirlo por sí mismo sin pedir retransmisión.
   * ⚠️ **Limitación clave:** **No se utilizan en aplicaciones comerciales generales** (sino en casos especiales como enlaces satelitales de espacio profundo o memorias ECC), porque **requieren muy alta redundancia** de bits de control y, en consecuencia, **bajan drásticamente la eficiencia de la transmisión**.
2. **Técnicas Especiales de Retransmisión (FEC y ARQ):**
   * Empleadas en radioenlaces punto a multipunto o radioteletipo donde no es viable el diálogo convencional.
3. **Técnica de Retransmisión de Paquetes:**
   * Es la técnica **utilizada universalmente en los protocolos de comunicaciones comerciales**: **TCP, ATM, X.25, HDLC**, etc. Como los canales modernos (fibra/cobre) tienen un BER muy bajo ($10^{-9}$), resulta mucho más eficiente enviar poca redundancia (un simple CRC o Checksum) y pedir la retransmisión únicamente en las raras ocasiones en que un paquete falla.

### Estudio Detallado del Sistema FEC (*Forward Error Correction*)

> 📌 **CONCEPTO CLAVE — Corrección de Errores hacia Adelante (FEC) por Diversidad de Tiempo:**
> * Se emplea cuando **hay más de una estación receptora** (difusión o *broadcast* por radio) y **no se necesitan (o no se pueden enviar) réplicas/acuses de recibo** por parte de las receptoras.
> * Permite operar en canales **Simplex** (unidireccionales).
> * Opera en modo **diversidad de tiempo**: cada mensaje se envía **dos veces**, intercalando los caracteres de la **Serie Directa ($D_x$)** con los de la **Serie Repetida ($R_x$)** desplazados varios instantes de tiempo (para que una ráfaga corta de ruido no destruya ambas copias del mismo carácter).

```text
Secuencia de transmisión FEC con diversidad de tiempo (intercalado Dx y Rx):
        Mensaje Dx (Emisión directa en posiciones impares)
            |       |       |       |       |       |       |       |       |
            v       v       v       v       v       v       v       v       v
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
| * | * | 1 | * | 2 | * | 3 | 1 | 4 | 2 | 5 | 3 | 6 | 4 | 7 | 5 | 8 | 6 | 9 |etc|
+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
  ^       ^       ^       ^       ^       ^       ^       ^       ^
  |_______|_______|_______|_______|_______|_______|_______|_______|
        Mensaje Rx (Repetición diferida en posiciones pares)
Nota: (*) Representa los caracteres anteriores a la serie transmitida en el ejemplo.
```

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Tabla de Operaciones del Receptor FEC:**
> Como la estación receptora tiene **dos oportunidades** de recibir cada carácter (una en la serie $D_x$ y otra instantes después en la serie $R_x$), toma su decisión según esta tabla:
>
> | Estado en Serie Directa ($D_x$) | Estado en Serie Repetida ($R_x$) | Operación que ejecuta el receptor FEC |
> | :---: | :---: | :--- |
> | **Sin error** | **Sin error** | Se ordena imprimir el carácter. |
> | **Con error** | **Sin error** | Se ordena imprimir el carácter recibido en la **serie $R_x$**. |
> | **Sin error** | **Con error** | Se ordena imprimir el carácter recibido en la **serie $D_x$**. |
> | **Con error** | **Con error** | Se ordena imprimir un **carácter especial** (ej. `*` o espacio) que indica error irrecuperable en la recepción. |

---

> 🛠️ **LABORATORIO EN PACKET TRACER #1 (Unidad 7): Inspección de FCS, Checksum e ICMP**
>
> **Objetivo:** Observar en tiempo real cómo actúan las 3 políticas de tratamiento de errores (FCS de Ethernet en Capa 2, Header Checksum + ICMP en Capa 3 y Checksum + Retransmisión TCP en Capa 4).
>
> > ⚠️ **ACLARACIÓN TÉCNICA CLAVE — ¿Hace falta configurar el Switch para que el paquete llegue al Router?**  
> > **¡NO!** Un Switch de Capa 2 (como el `Cisco 2960`) es *plug-and-play*: no necesita dirección IP ni ejecutar `no shutdown` porque todos sus puertos vienen encendidos por defecto en la `VLAN 1` y conmuta tramas únicamente leyendo direcciones MAC.  
> > Si al armar el laboratorio notás que **el paquete no llega al router o falla en el switch**, se debe a uno de estos **3 detalles clásicos de Packet Tracer**:
> > 1. 🟠 **Puertos del Switch en Naranja (Convergencia STP):** Al conectar los cables o encender la interfaz del router (`no shutdown`), los puertos del switch quedan en color **naranja/ámbar** durante **30 segundos** (mientras ejecuta *Spanning Tree Protocol*). Si pasás al **Modo Simulación** mientras el punto está naranja, el reloj se detiene y el switch bloquea el tráfico. **Solución:** En modo **Realtime**, hacé clic en el botón **`Fast Forward Time` (`>>` abajo a la izquierda, o `Alt + D`)** hasta que todos los triángulos estén en **verde**.
> > 2. 🔌 **Cable entre `Router0` y `Server0`:** Como el Router y el Servidor operan con la misma disposición de pines (MDI), para unir `Router0 (Gig0/1)` directamente con `Server0` sin un switch intermedio debés usar un **Cable Cruzado (*Copper Cross-Over*, línea negra punteada)** o el ícono del **rayo automático** (si usás cable directo entre Router y Server sin switch, el enlace queda en rojo).
> > 3. 📢 **Tabla ARP vacía en el primer envío:** En el primer intento de comunicación, `PC0` todavía no conoce la dirección MAC del router (`192.168.1.1`), por lo que retiene el paquete HTTP y envía primero un **ARP Broadcast**. **Solución:** Hacé primero un `ping 192.168.2.10` en modo **Realtime** para que se completen las tablas ARP y MAC, y luego pasá al **Modo Simulación**.
>
> **Paso a paso corregido y verificado en Cisco Packet Tracer:**
> 1. Colocá en el área de trabajo **1 PC (`PC0`)**, **1 Switch (`2960`)**, **1 Router (`1941` o `2911`)** y **1 Servidor (`Server0`)**.
> 2. Conectá los equipos:
>    * Con **Cable Directo (*Copper Straight-Through*, línea negra continua):** `PC0 (Fa0)` $\rightarrow$ `Switch0 (Fa0/1)`, y `Switch0 (Fa0/2)` $\rightarrow$ `Router0 (Gig0/0)`.
>    * Con **Cable Cruzado (*Copper Cross-Over*, línea negra punteada):** `Router0 (Gig0/1)` $\rightarrow$ `Server0 (Fa0)`.
> 3. Configurá el direccionamiento IP:
>    * **`PC0`** *(Desktop $\rightarrow$ IP Configuration)*: IP `192.168.1.10`, Máscara `255.255.255.0`, **Default Gateway `192.168.1.1`**.
>    * **`Server0`** *(Desktop $\rightarrow$ IP Configuration)*: IP `192.168.2.10`, Máscara `255.255.255.0`, **Default Gateway `192.168.2.1`**.
>    * **`Router0`** *(pestaña CLI)*:
>      ```ios
>      enable
>      configure terminal
>      interface GigabitEthernet0/0
>       ip address 192.168.1.1 255.255.255.0
>       no shutdown
>      interface GigabitEthernet0/1
>       ip address 192.168.2.1 255.255.255.0
>       no shutdown
>      end
>      ```
> 4. **Sincronización previa (en modo Realtime):** Presioná **`Fast Forward Time` (`>>`)** dos veces hasta que **todos los enlaces estén en verde**. Abrí el `Command Prompt` de `PC0` y ejecutá `ping 192.168.2.10` hasta ver respuestas (`Reply from 192.168.2.10...`) para que el switch y los dispositivos aprendan las direcciones MAC por ARP.
> 5. Activá el **Modo Simulación** (esquina inferior derecha) y desde el `Web Browser` de `PC0` ingresá a `http://192.168.2.10` (o lanzá un nuevo `ping 192.168.2.10`).
> 6. Hacé clic en el sobre (PDU) cuando pasa por `PC0` y `Router0`, y abrí la pestaña **Outbound PDU Details**:
>    * En la sección **Ethernet II**, ubicá el campo **`FCS`** de 4 bytes al final de la trama (*Política 1: detectar y descartar*).
>    * En la sección **IP**, ubicá el campo **`HEADER CHECKSUM`** (*Política 2: protege solo la cabecera IP*). Observá qué pasa con ese valor cuando el paquete atraviesa el `Router0`: ¡cambia porque el router resta 1 al campo `TTL` y recalcula el Checksum!
>    * Finalmente, desde el `Command Prompt` de `PC0` hacé `ping 192.168.2.99` (una IP que no existe). Al avanzar con *Capture/Forward*, luego de que el router intente buscar por ARP a `.99` sin éxito, verás cómo descarta el paquete y devuelve un mensaje **ICMP** avisando al emisor (*Política 2 en acción*).

<div style="page-break-after: always;"></div>

---

# Módulo 2 (Unidad 8): Modulación Analógica y Digital, Digitalización PCM y Jerarquía PDH

## 2.1. Evolución Histórica: Codificador Morse y Radiotelegrafía

La transmisión de información codificada mediante pulsos eléctricos nace con el **Telégrafo y el Código Morse**, donde cada letra (A–Z) y número (0–9) se representa mediante combinaciones de pulsos cortos (**puntos `.`**) y pulsos largos (**rayas `-`**), como por ejemplo la famosa señal de socorro `S` (`...`) `O` (`---`) `S` (`...`).  
Con el desarrollo de la radiotelegrafía inalámbrica de Guglielmo Marconi, este esquema digital primitivo se montó sobre ondas de radiofrecuencia, cuyo hito histórico más resonante fue el rescate de los sobrevivientes del **RMS Titanic** (1912) gracias a los operadores de radio a bordo.

---

## 2.2. Esquema Funcional de un Modulador y Efecto del Ruido

¿Por qué necesitamos **modular** una señal para transmitirla a distancia? Porque las señales originales de información (como la voz humana, de baja frecuencia entre $300\text{ Hz}$ y $3400\text{ Hz}$) requerirían antenas de kilómetros de longitud y se atenuarían rápidamente si se irradiaran de forma directa.

> 📌 **CONCEPTO CLAVE — Elementos del Proceso de Modulación:**
> * **Señal Moduladora (Audiofrecuencia / Banda Base):** Es la señal de baja frecuencia que **contiene la información** útil (por ejemplo, la onda eléctrica proveniente de un micrófono luego de ser amplificada).
> * **Señal Portadora (Radiofrecuencia - RF):** Es una onda sinusoidal de **alta frecuencia** y parámetros constantes generada por un **Oscilador** local y un amplificador. Por sí sola no transporta información, sino que sirve de "vehículo".
> * **Modulador:** Circuito que hace variar alguno de los parámetros de la portadora (su **amplitud**, su **frecuencia** o su **fase**) en proporción directa al valor instantáneo de la señal moduladora.
> * **Señal Modulada (Señal Compuesta):** Señal resultante a la salida del modulador que se inyecta en la **antena emisora** para ser irradiada como ondas electromagnéticas al canal.

```text
Esquema en bloques de un Transmisor Modulador:

[ Oscilador ] ---> [ Amplificador ] ----(Señal Portadora de RF)----+
                                                                   v
                                                            +-------------+      |  /  Ondas
                                                            |  MODULADOR  | ---> | /  Emitidas
                                                            +-------------+      |/  (Antena)
                                                                   ^
[ Micrófono ] ---> [ Amplificador ] ---(Señal Moduladora de AF)----+
```

---

## 2.3. Modulación Analógica: AM y FM

Cuando la señal moduladora es **analógica continua** (como la voz o la música), distinguimos:

1. **Modulación en Amplitud (AM - *Amplitude Modulation*):**
   * La **frecuencia** de la onda portadora se mantiene constante, pero su **amplitud (envolvente)** sube y baja copiando la forma de onda de la señal moduladora.
   * ⚠️ **Vulnerabilidad al ruido:** Como el ruido eléctrico del canal se **suma a la amplitud** de la señal ($\text{Señal} + \text{Ruido}$), la modulación AM es muy sensible al ruido atmosférico e industrial.
   * **Esquema del Receptor AM Superheterodino (visto en clase):**
     $$\text{Antena} \longrightarrow [\text{Etapa RF}] \longrightarrow [\text{Mezclador}] \longrightarrow [\text{Etapa IF (Frec. Intermedia)}] \longrightarrow [\text{Detector AM}] \longrightarrow [\text{Amplif. Audio}] \longrightarrow \text{Altavoz}$$

2. **Modulación en Frecuencia (FM - *Frequency Modulation*):**
   * La **amplitud** de la portadora se mantiene constante, pero su **frecuencia instantánea** aumenta cuando la señal moduladora es positiva (ciclos más comprimidos) y disminuye cuando la moduladora es negativa (ciclos más espaciados).
   * *Ventaja:* Al no llevar la información en la amplitud, el receptor puede recortar las variaciones de amplitud causadas por el ruido, logrando mucha mayor fidelidad e inmunidad al ruido que AM.

---

## 2.4. Modulación Digital de Portadora Continua: ASK, FSK y PSK

Cuando la señal moduladora es **digital** (una secuencia de bits `1` y `0` provenientes de una computadora o codificador), la modulación se denomina *Shift Keying* (modulación por desplazamiento):

| Tipo de Modulación | Parámetro que varía | Comportamiento para `1` y `0` | Características principales |
| :--- | :--- | :--- | :--- |
| **ASK** (*Amplitude Shift Keying*) | **Amplitud** | Bit `1`: presencia de portadora con amplitud $A$.<br>Bit `0`: amplitud nula (ausencia de portadora). | Muy simple y económica, pero **muy vulnerable al ruido** y a la atenuación. |
| **FSK** (*Frequency Shift Keying*) | **Frecuencia** | Bit `1`: se transmite una frecuencia alta $f_1$.<br>Bit `0`: se transmite una frecuencia baja $f_0$. | Amplitud constante; más inmune al ruido que ASK, pero consume mayor ancho de banda. |
| **PSK / 2PSK** (*Phase Shift Keying*) | **Fase** | Bit `1` y Bit `0` se diferencian por un **salto o inversión de fase de $180^\circ$** en cada cambio de nivel lógico. | Amplitud y frecuencia constantes; excelente desempeño frente al ruido. |

---

## 2.5. Modulaciones Multinivel y en Cuadratura: 4PSK (QPSK), 8QAM y 16QAM

Para transmitir más bits por segundo sin aumentar la velocidad de símbolos (baudios) ni el ancho de banda requerido, agrupamos los bits en conjuntos de 2, 3 o 4 bits por cada símbolo transmitido:

### 1️⃣ Modulación 4PSK o QPSK (*Quadrature Phase Shift Keying*) — Para Dibits (2 bits/símbolo)
Agrupa los bits de entrada de a pares (**dibits**: `00`, `01`, `10`, `11`). Como hay $2^2 = 4$ combinaciones posibles, utiliza **4 fases distintas** separadas por $90^\circ$, manteniendo la **amplitud constante** (un único círculo en el diagrama polar/constelación).

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Tablas de Fase y Constelación 4PSK:**
> En las diapositivas se presentan dos convenciones de ángulos para los dibits en 4PSK (según se tomen sobre los ejes a $0^\circ, \pm 90^\circ, 180^\circ$ o en el centro de cada cuadrante $I/Q$ a $45^\circ, 135^\circ, 225^\circ, 315^\circ$):
>
> | Dibit (Par de bits) | Nivel de tensión equivalente | Fase sobre ejes (Esquema 1) | Ángulo del vector en Constelación $I/Q$ (Esquema 2) | Cuadrante $(I, Q)$ |
> | :---: | :---: | :---: | :---: | :---: |
> | **`00`** | $0\text{ Volts}$ | $0^\circ$ | **$225^\circ$** | 3.er Cuadrante |
> | **`01`** | $1\text{ Volt}$ | $-90^\circ$ ($270^\circ$) | **$135^\circ$** | 2.º Cuadrante |
> | **`10`** | $2\text{ Volts}$ | $+90^\circ$ | **$315^\circ$** | 4.º Cuadrante |
> | **`11`** | $3\text{ Volts}$ | $180^\circ$ | **$45^\circ$** | 1.er Cuadrante |

### 2️⃣ Modulación 8QAM (*Quadrature Amplitude Modulation*) — Para Tribits (3 bits/símbolo)
Agrupa los bits de a tres (**tribits**: de `000` a `111`). Para representar $2^3 = \mathbf{8\text{ símbolos}}$ sin acercar demasiado las fases entre sí, combina simultáneamente **Modulación de Amplitud y de Fase**:
* Utiliza **4 fases** ($0^\circ, 90^\circ, 180^\circ, 270^\circ$) y **2 niveles de amplitud** (dos círculos concéntricos en la constelación):
  * **Círculo exterior (Amplitud máxima):** Tribits **`000`** ($0^\circ$), **`001`** ($90^\circ$), **`010`** ($180^\circ$) y **`011`** ($270^\circ$).
  * **Círculo interior (Amplitud reducida):** Tribits **`100`** ($0^\circ$), **`101`** ($90^\circ$), **`110`** ($180^\circ$) y **`111`** ($270^\circ$).

### 3️⃣ Modulación 16QAM — Para Cuadribits (4 bits/símbolo)
Agrupa los bits de a cuatro (**cuadribits**: de `0000` a `1111`), conformando una grilla cuadrada de **$4 \times 4 = 16\text{ puntos}$** en el diagrama de constelación $(I, Q)$:

```text
Diagrama de Constelación 16-QAM (16 símbolos de 4 bits):
                      Eje Q (Cuadratura)
                              ^
            1011     1001     |     0010     0011
                              |
            1010     1000     |     0000     0001
        ----------------------+----------------------> Eje I (En Fase)
            1101    [1100]    |     0100     0110      (Escalas: 25%, 50%, 75%)
                      /       |
            1111     1110     |     0101     0111
                              v
Ejemplo destacado en diapositiva: Símbolo [1100] -> Amplitud = 25%, Fase = 225°
```

---

## 2.6. Modulación por Pulso Analógica: PAM, PDM y PPM

Antes de llegar a la digitalización completa (PCM), una señal analógica moduladora puede modular un **tren periódico de pulsos rectangulares** (portadora de impulsos) de tres formas:

1. **PAM (*Pulse Amplitude Modulation* — Impulsos Modulados en Amplitud):** Los pulsos mantienen su ancho y posición fijos, pero la **altura (amplitud)** de cada pulso varía siguiendo el valor instantáneo de la señal analógica moduladora. *(Es el paso previo indispensable para el sistema digital PCM).*
2. **PDM o PWM (*Pulse Duration / Width Modulation* — Impulsos Modulados en Anchura):** Los pulsos tienen amplitud constante, pero el **ancho o duración** de cada pulso es proporcional al valor de la moduladora en ese instante.
3. **PPM (*Pulse Position Modulation* — Impulsos Modulados en Posición):** Los pulsos tienen amplitud y anchura constantes, pero se **desplazan temporalmente** respecto a su instante nominal según el valor de la señal moduladora.

---

## 2.7. Modulación Digital PCM (*Pulse Code Modulation*)

El sistema **PCM** (*Modulación por Impulsos Codificados*) es el estándar universal para convertir una señal analógica continua (como la voz telefónica) en un flujo digital binario puro (`1` y `0`).

> 📌 **CONCEPTO CLAVE — Las 3 etapas del conversor Analógico-Digital PCM:**
> $$\text{Señal Analógica } S(t) \longrightarrow \boxed{\text{1. Muestreo (PAM)}} \longrightarrow \boxed{\text{2. Cuantificación}} \longrightarrow \boxed{\text{3. Codificación Binaria}} \longrightarrow \text{Flujo Digital}$$

### Etapa 1: Muestreo (*Sampling*) y Teorema de Nyquist
Un interruptor electrónico se cierra periódicamente cada $T_s$ segundos (a una frecuencia de muestreo $f_s = 1/T_s$), tomando "fotografías" instantáneas de la amplitud de la señal analógica $S(t)$ para obtener una señal **PAM** $S_s(t)$.

> 🧮 **FÓRMULA — Criterio de Nyquist (Teorema del Muestreo):**
> Para poder reconstruir de forma exacta la señal analógica original en el receptor sin pérdida de información ni solapamiento espectral (*aliasing*), la **frecuencia de muestreo ($f_s$)** debe ser mayor o igual al **doble de la frecuencia máxima ($f_m$)** contenida en la señal moduladora:
> $$f_s > 2 \cdot f_m$$
> * **Aplicación en Telefonía Digital Estándar:** El canal vocal telefónico se filtra limitando su frecuencia máxima a **$f_m = 4\text{ kHz}$** ($300\text{ Hz}$ a $3400\text{ Hz}$ más banda de guarda). Por lo tanto, la frecuencia de muestreo normalizada por la UIT-T es:
>   $$f_s = 2 \times 4.000\text{ Hz} = \mathbf{8.000\text{ muestras/segundo}} \implies T_s = \frac{1}{8000\text{ Hz}} = \mathbf{125\,\mu\text{s}}$$

### Etapa 2: Cuantificación (*Quantization*)
Las muestras PAM tienen valores reales continuos (infinitos decimales posibles, como $6,5\text{ V}$ o $3,3\text{ V}$). La **cuantificación** consiste en redondear cada muestra al nivel discreto más cercano dentro de una escala finita de $N = 2^b$ niveles (donde $b$ es la cantidad de bits por muestra).
* En telefonía PCM estándar se utilizan **$b = 8\text{ bits por muestra}$**, lo que brinda $2^8 = \mathbf{256\text{ niveles de cuantificación}}$ (por ejemplo, de $-127$ a $+127$ pasando por $000$).

### Etapa 3: Codificación (*Binary Encoding*)
A cada nivel entero cuantificado se le asigna su código binario de $b$ bits correspondiente, generando la trama de bits serie que viajará por el canal digital.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Muestreo, Cuantificación y Codificación PCM (3 bits / 8 niveles):**
> En la diapositiva 22 (tomada de *L. Couch, "Sistemas de Comunicaciones"*), se muestra una señal analógica muestreada en 9 instantes de tiempo utilizando un cuantificador de **8 niveles (del `0` al `7`)**, lo que requiere **$b = 3\text{ bits por muestra}$** ($2^3 = 8$):
>
> | Instante de Muestreo | 1. Valor Muestreado PAM (*Sampling*) | 2. Nivel Cuantificado (*Quantizing*: redondeo a entero 0–7) | 3. Palabra Binaria de 3 bits (*Encoding*) |
> | :---: | :---: | :---: | :---: |
> | $t_1$ | $6,5$ | **`7`** | **`111`** |
> | $t_2$ | $3,3$ | **`3`** | **`011`** |
> | $t_3$ | $2,0$ | **`2`** | **`010`** |
> | $t_4$ | $2,5$ | **`3`** | **`011`** |
> | $t_5$ | $4,4$ | **`4`** | **`100`** |
> | $t_6$ | $5,0$ | **`5`** | **`101`** |
> | $t_7$ | $3,7$ | **`4`** | **`100`** |
> | $t_8$ | $2,2$ | **`2`** | **`010`** |
> | $t_9$ | $2,0$ | **`2`** | **`010`** |
>
> **Secuencia digital resultante transmitida al canal:** `111 011 010 011 100 101 100 010 010`  
> *(Además, en la diapositiva 18 se muestra el ejemplo con 8 bits donde la muestra cuantificada en el nivel `+024` se codifica como `00011000` y la muestra `+038` se codifica como `00100110`).*

---

## 2.8. Jerarquía Digital Plesiócrona (PDH) — Norma Europea y Argentina

Una vez que digitalizamos **1 canal telefónico** mediante PCM ($8.000\text{ muestras/s} \times 8\text{ bits/muestra}$), obtenemos el canal digital elemental (**DS0 / E0**) cuya velocidad es:
$$V_{\text{canal PCM}} = 8.000\,\frac{\text{muestras}}{\text{s}} \times 8\,\frac{\text{bits}}{\text{muestra}} = \mathbf{64.000\text{ bps}} = \mathbf{64\text{ Kbps}}$$

Para transportar miles de llamadas simultáneas por un único vínculo troncal de fibra óptica o radioenlace de microondas, se utiliza la **Jerarquía Digital Plesiócrona (PDH - *Plesiochronous Digital Hierarchy*)**, que multiplexa por división de tiempo (TDM) múltiples canales de $64\text{ Kbps}$ en órdenes superiores.

### Formación del Primer Orden Digital: Trama E1 (Sistema PCM 30)

> 📌 **CONCEPTO CLAVE — Estructura de la Trama E1 (Norma Europea y Argentina):**
> El primer nivel de multiplexación PDH se denomina **E1 (o PCM 30)** y agrupa **32 intervalos de tiempo (*Time Slots*, numerados del `0` al `31`)**:
> * **Intervalo `0` (`ch 0`):** Reservado para **1 canal de sincronismo** (alineación de trama).
> * **Intervalos `1` al `15` y `17` al `31`:** **30 canales de voz/datos de usuario** (de $64\text{ Kbps}$ cada uno).
> * **Intervalo `16` (`ch 16`):** Reservado para **1 canal de señalización** telefónica (establecimiento y corte de llamadas de los 30 canales).

```text
Estructura de 1 Trama E1 (Duración total = 125 microsegundos = 256 bits):
+---------+---------+-----+---------+---------+---------+-----+---------+
| Canal 0 | Canal 1 | ... |Canal 15 |Canal 16 |Canal 17 | ... |Canal 31 |
| Sincron.|   Voz   | ... |   Voz   | Señaliz.|   Voz   | ... |   Voz   |
+---------+---------+-----+---------+---------+---------+-----+---------+
|<-8 bits>|<-8 bits>|     |<-8 bits>|<-8 bits>|<-8 bits>|     |<-8 bits>|
|<-- 15 canales de voz -->|         |<-- 15 canales de voz -->|
Total = 32 canales (Time Slots) de 8 bits c/u: 1 Sincronismo + 1 Señalización + 30 Voz
```

> 🧮 **FÓRMULA — Todos los cálculos exactos de la Trama E1 (¡Frecuente en exámenes!):**
> 1. **Cantidad de bits por trama E1:**
>    $$32\text{ intervalos} \times 8\text{ bits/intervalo} = \mathbf{256\text{ bits por trama}}$$
> 2. **Duración de una trama E1 completa:**  
>    Como cada canal de voz necesita enviar 1 muestra cada $125\,\mu\text{s}$ ($8.000\text{ veces por segundo}$), toda la trama E1 debe completarse en:
>    $$T_{\text{trama}} = \frac{1}{8.000\text{ tramas/s}} = \mathbf{125\,\mu\text{s}}$$
> 3. **Duración de cada intervalo de canal (*Time Slot* de 8 bits):**
>    $$t_{\text{canal}} = \frac{125\,\mu\text{s}}{32\text{ canales}} = \mathbf{3,90625\,\mu\text{s}} \approx \mathbf{3,9\,\mu\text{s}}$$
> 4. **Duración de 1 bit individual dentro de E1:**
>    $$t_{\text{bit}} = \frac{3,90625\,\mu\text{s}}{8\text{ bits}} = \mathbf{0,48828\,\mu\text{s}} \approx \mathbf{488\text{ ns}}$$
> 5. **Velocidad de transmisión final del flujo E1:**
>    $$V_{E1} = 256\,\frac{\text{bits}}{\text{trama}} \times 8.000\,\frac{\text{tramas}}{\text{s}} = \mathbf{2.048.000\text{ bps}} = \mathbf{2.048\text{ Kbps}} = \mathbf{2,048\text{ Mbps}}$$
>    *(O equivalentemente: $32\text{ canales} \times 64\text{ Kbps} = 2.048\text{ Kbps}$).*

### Órdenes Superiores de la Jerarquía PDH (E1 a E5)

A partir del flujo primario **E1**, cada etapa superior de multiplexación agrupa **4 tributarios del orden inmediato anterior** ($\times 4$ en cantidad de canales de voz), agregando bits adicionales de sincronismo y justificación plesiócrona:

```text
Árbol de Multiplexación PDH (Norma UIT / Europea y Argentina):

30 canales VF ---> [ Mux Primario ] ===> 1° Orden (E1: 2,048 Mbps | 30 canales)
   (64 Kbps)                                  | (x 4 líneas de 1° orden)
                                              v
                                      [ Mux Secundario ] ===> 2° Orden (E2: 8,448 Mbps | 120 canales)
                                                                   | (x 4 líneas de 2° orden)
                                                                   v
                                                           [ Mux Terciario ] ===> 3° Orden (E3: 34,368 Mbps | 480 canales)
                                                                                       | (x 4 líneas de 3° orden)
                                                                                       v
                                                                               [ Mux Cuaternario ] ===> 4° Orden (E4: 139,264 Mbps | 1920 canales)
                                                                                                             | (x 4 líneas de 4° orden)
                                                                                                             v
                                                                                                     [ Mux Quinario ] ===> 5° Orden (E5: 564,992 Mbps | 7680 canales)
```

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — Tabla Oficial de la Jerarquía Digital PDH (Norma Europea/Argentina):**
>
> | Orden (Nivel PDH) | Nombre del Multiplexor | Velocidad de Transmisión | Cantidad de bits por trama | Duración de la trama ($\mu\text{s}$) | N° de Canales Telefónicos (de $64\text{ Kbps}$) |
> | :---: | :---: | :---: | :---: | :---: | :---: |
> | **1° Orden (E1)** | Primario | **2,048 Mbps** | 256 | $125,00\,\mu\text{s}$ | **30** |
> | **2° Orden (E2)** | Secundario ($4 \times \text{E1}$) | **8,448 Mbps** | 848 | $100,38\,\mu\text{s}$ | **120** ($30 \times 4$) |
> | **3° Orden (E3)** | Terciario ($4 \times \text{E2}$) | **34,368 Mbps** | 1536 | $44,69\,\mu\text{s}$ | **480** ($120 \times 4$) |
> | **4° Orden (E4)** | Cuaternario ($4 \times \text{E3}$) | **139,264 Mbps** | 2904 | $20,85\,\mu\text{s}$ | **1.920** ($480 \times 4$) |
> | **5° Orden (E5)** | Quinario ($4 \times \text{E4}$) | **564,992 Mbps** | 2688 | $4,70\,\mu\text{s}$ | **7.680** ($1920 \times 4$) |

---

> 🛠️ **LABORATORIO EN PACKET TRACER #2 (Unidad 8): Enlace Serial Sincrónico con Velocidad de Reloj E1 ($2.048.000\text{ bps}$)**
>
> **Objetivo:** Configurar un vínculo WAN punto a punto entre dos routers emulando una línea digital dedicada de primer orden **E1 ($2,048\text{ Mbps}$)** mediante el comando `clock rate` en una interfaz serial DCE.
>
> **Paso a paso en Cisco Packet Tracer:**
> 1. Colocá dos routers **`1941`** (`R1` y `R2`).
> 2. Hacé clic en cada router, apagalo desde el interruptor físico en la pestaña **Physical**, arrastrá el módulo **`HWIC-2T`** (placa serial de 2 puertos) a una ranura vacía y volvé a encender ambos routers.
> 3. En la sección de cables (*Connections*), elegí el cable **Serial DCE** (ícono de rayo rojo con un relojito). Hacé clic primero en `R1` (`Serial0/0/0` — será el **DCE** que provee el reloj de sincronismo de línea) y luego en `R2` (`Serial0/0/0` — será el **DTE**).
> 4. Abrí el CLI de `R1` y configurá el enlace serial con la velocidad aproximada de un **E1 ($2.000.000\text{ bps}$)**:
>    ```ios
>    enable
>    configure terminal
>    interface Serial0/0/0
>     ip address 10.0.0.1 255.255.255.252
>     clock rate 2000000
>     bandwidth 2048
>     no shutdown
>    ```
> 5. En el CLI de `R2`, configurá el otro extremo:
>    ```ios
>    enable
>    configure terminal
>    interface Serial0/0/0
>     ip address 10.0.0.2 255.255.255.252
>     bandwidth 2048
>     no shutdown
>    ```
> 6. Ejecutá `show controllers Serial0/0/0` y `show interfaces Serial0/0/0` en `R1` para verificar que actúa como **DCE** proveyendo el reloj de sincronismo digital y que el ancho de banda declarado coincide con los **$2048\text{ Kbps}$ de una trama E1**.

<div style="page-break-after: always;"></div>

---

# Módulo 3 (Unidad 9 - Parte A): Introducción a las Redes, Topologías, Confiabilidad y Seguridad

## 3.1. Componentes de una Red de Datos

Toda infraestructura de red moderna se compone de tres grandes categorías de elementos: **Dispositivos Finales (*End Devices*)**, **Dispositivos Intermediarios (*Intermediary Devices*)** y **Medios de Red (*Network Media*)**.

### 1️⃣ Dispositivos Finales (*Hosts*): Clientes y Servidores
> 📌 **CONCEPTO CLAVE — Host, Cliente y Servidor:**
> * **Host (Dispositivo Final):** Es cualquier computadora o dispositivo conectado a la red desde donde **se origina un mensaje o donde se recibe**. Incluye PCs de escritorio, laptops, impresoras de red, teléfonos IP, tablets inalámbricas y terminales de TelePresencia.
> * **Clientes:** Son dispositivos finales que cuentan con software instalado para **enviar solicitudes** a los servidores y mostrar la información recuperada al usuario (ej. un navegador web o un cliente de correo).
> * **Servidores:** Son computadoras con software especializado que **proporcionan información y servicios** al recibir solicitudes de los clientes:
>   * **Servidor de Correo (*Email Server*):** Ejecuta software de servidor de correo (ej. SMTP/POP3/IMAP); los clientes acceden con un cliente de email.
>   * **Servidor Web (*Web Server*):** Ejecuta software de servidor web (HTTP/HTTPS); los clientes acceden usando un navegador web.
>   * **Servidor de Archivos (*File Server*):** Almacena archivos corporativos y de usuarios en una ubicación central a la que acceden los dispositivos clientes.

### Redes Punto a Punto (*Peer-to-Peer* — P2P)
En redes pequeñas o domésticas, es posible que un mismo dispositivo cumpla simultáneamente el rol de **cliente y servidor** (por ejemplo, dos PCs conectadas donde una comparte una impresora —*Print Sharing*— y la otra comparte una carpeta de archivos —*File Sharing*—).

| ✅ Ventajas de las Redes Peer-to-Peer (P2P) | ❌ Desventajas de las Redes Peer-to-Peer (P2P) |
| :--- | :--- |
| • Fáciles de instalar y configurar. | • **No hay administración centralizada.** |
| • Menos complejas (no requieren servidor dedicado). | • **No son tan seguras** (cada usuario administra sus propios permisos). |
| • Menor costo de implementación. | • **No son escalables** (al crecer el número de equipos se vuelven inmanejables). |
| • Ideales para tareas simples de transferencia de archivos e impresoras compartidas. | • **Rendimiento más lento** (los hosts actúan como cliente y servidor a la vez). |

---

### 2️⃣ Dispositivos Intermediarios de Red (*Intermediary Network Devices*)
Son los equipos encargados de **interconectar los dispositivos finales** y dirigir el flujo de datos a través de la red.
* **Ejemplos principales:**
  * **LAN Switch** (Conmutador de Capa 2) y **Multilayer Switch** (Switch Multicapa de Capa 3).
  * **Router** (Enrutador de Capa 3) y **Wireless Router / Access Point** (Punto de acceso inalámbrico).
  * **Firewall Appliance** (Dispositivo de seguridad / filtrado de tráfico).

> 📌 **CONCEPTO CLAVE — Las 3 funciones esenciales de los Dispositivos Intermediarios:**
> A medida que los datos fluyen por la red, los dispositivos intermediarios realizan todas o algunas de estas funciones:
> 1. **Regenerar y retransmitir** las señales de datos.
> 2. **Mantener información sobre qué vías (rutas) existen** a través de la red y de la interred (*internetwork*).
> 3. **Notificar a otros dispositivos sobre errores** y fallas de comunicación (y dirigir los datos por rutas alternativas si falla un enlace).

---

### 3️⃣ Medios de Red (*Network Media*) y Conectividad Física
Para que el mensaje viaje del origen al destino se necesita un **canal o medio físico**. En la representación de redes debemos distinguir:
* **NIC (*Network Interface Card* — Tarjeta de Interfaz de Red):** Adaptador o placa de red que conecta físicamente el dispositivo final al medio de la red.
* **Puerto físico / Interfaz:** Conector o receptáculo físico en un dispositivo de red donde se enchufa el medio. *(A menudo los términos **puerto** e **interfaz** se usan indistintamente).*

| Tipo de Medio de Red | Principio Físico de Transmisión | Representación Gráfica en Topologías |
| :--- | :--- | :--- |
| **1. Cables con hilos metálicos (Cobre / *Copper*)** | Utilizan **impulsos eléctricos** (ej. par trenzado UTP/STP con conectores RJ-45, cable coaxial). | **LAN Media:** Línea recta negra continua.<br>**WAN Media (Serial):** Línea roja quebrada en forma de rayo (`Z`). |
| **2. Fibras de vidrio o plástico (Fibra Óptica / *Fiber-optic*)** | Utilizan **pulsos de luz** (láser o LED). Inmunes a interferencias electromagnéticas y de gran alcance. | Línea continua (suele indicarse en naranja/amarillo o línea recta entre equipos ópticos). |
| **3. Transmisión Inalámbrica (*Wireless*)** | Utilizan la **modulación de frecuencias específicas de ondas electromagnéticas** por el aire (RF, microondas, láser). | **Wireless Media:** Onda sinusoidal / línea punteada celeste. |

---

## 3.2. Representaciones y Diagramas de Topología: Física vs. Lógica

Los diagramas de red (o **diagramas de topología**) utilizan símbolos estandarizados para representar los dispositivos y sus conexiones. Existen dos tipos complementarios:

> 📌 **CONCEPTO CLAVE — Diferencia entre Topología Física y Topología Lógica:**
> 1. **Diagrama de Topología Física:** Ilustra la **ubicación física real** de los dispositivos intermedios, las salas/oficinas, los racks/estantes (*shelves*) y la disposición de la instalación del cableado.
>    * *Ejemplo de las diapositivas:* Muestra en qué habitación y en qué estante del rack está cada equipo: `Server Room Rm 2158` (`R1` en Rack 1 Shelf 1, `S1` en Rack 1 Shelf 2, `Web Server` en Rack 2 Shelf 1), `IT Office Rm 2159`, y las aulas `Class 1 Rm 2125`, `Class 2 Rm 2126`, `Class 3 Rm 2127`.
> 2. **Diagrama de Topología Lógica:** Ilustra cómo fluyen los datos en la red mostrando **los dispositivos, los nombres de las interfaces/puertos y el esquema de direccionamiento IP (subredes)**.
>    * *Ejemplo de las diapositivas:* Muestra los mismos equipos agrupados por dominios de red IP: la red de servidores `192.168.10.0` conectada a `S1` (puertos `F0/1`, `F0/2`, `F0/3`, `G0/1`) hacia la interfaz `G0/1` del router `R1`; la oficina IT `192.168.11.0` en `G0/2`; y las aulas `192.168.100.0`, `192.168.101.0` y `192.168.102.0` en `G1/0`, `G1/1` y `G1/2`.

---

## 3.3. Clasificación de Redes por Tamaño y Alcance: LAN, WAN, Internet, Intranet y Extranet

### Clasificación según el tamaño:
* **Redes Domésticas Pequeñas (*Small Home*):** Conectan unas pocas computadoras entre sí y a Internet.
* **SOHO (*Small Office / Home Office*):** Permiten que equipos en una oficina pequeña o en el hogar se conecten de forma remota a una red corporativa central.
* **Redes Medianas a Grandes (*Medium/Large*):** Abarcan múltiples edificios o ubicaciones con cientos o miles de hosts interconectados.
* **Redes de Alcance Mundial (*World Wide Networks*):** Conectan cientos de millones de computadoras en todo el planeta (el ejemplo por excelencia es **Internet**).

### Comparativa Técnica: LAN vs. WAN
Las infraestructuras de red varían según el tamaño del área que cubren, la cantidad de usuarios conectados, el número/tipo de servicios disponibles y el área de responsabilidad:

| Criterio de Comparación | Red de Área Local (LAN - *Local Area Network*) | Red de Área Amplia (WAN - *Wide Area Network*) |
| :--- | :--- | :--- |
| **Área Geográfica y Función** | Infraestructura que abarca un **área geográfica pequeña** (hogar, edificio, campus) e interconecta dispositivos finales locales. | Infraestructura que abarca una **amplia área geográfica** (ciudades, países) para **interconectar redes LAN entre sí**. |
| **Propiedad y Administración** | Administrada por **una sola organización** o individuo. | Generalmente administrada por **uno o más proveedores de servicios de telecomunicaciones (ISP/Telcos)**. |
| **Velocidad / Ancho de Banda** | Proporciona **ancho de banda de muy alta velocidad** (ej. 100 Mbps, 1 Gbps, 10 Gbps) a los dispositivos internos. | Generalmente proporciona **enlaces de menor velocidad** relativa entre las LAN (debido al costo de las largas distancias). |

### Internet, Intranet y Extranet
* **Internet:** Es una colección mundial de redes LAN y WAN interconectadas entre sí (usando cobre, fibra óptica y enlaces inalámbricos). Su estructura y estandarización es mantenida por organizaciones como **IETF** (*Internet Engineering Task Force*), **ICANN** (*Internet Corporation for Assigned Names and Numbers*) e **IAB** (*Internet Architecture Board*).
* **Intranet vs. Extranet:**

```text
Esquema de Círculos Concéntricos de Acceso:
+-------------------------------------------------------------------+
|  THE INTERNET (El Mundo Público - Acceso Global)                  |
|   +-----------------------------------------------------------+   |
|   |  EXTRANET (Proveedores, Clientes, Colaboradores externos  |   |
|   |            con acceso seguro autorizado a datos)          |   |
|   |   +---------------------------------------------------+   |   |
|   |   |  INTRANET (Colección privada de LAN/WAN internas: |   |   |
|   |   |            Solo miembros de la Organización)      |   |   |
|   |   +---------------------------------------------------+   |   |
|   +-----------------------------------------------------------+   |
+-------------------------------------------------------------------+
```

---

## 3.4. Arquitectura de Redes Confiables (*Reliable Networks*)

La **arquitectura de red** se refiere a las tecnologías que dan soporte a la infraestructura y a los servicios y reglas que trasladan los datos.

> 📌 **CONCEPTO CLAVE — Los 4 Pilares de una Red Confiable (¡Pregunta fija de examen!):**
> Para que una red se considere confiable y cumpla con las expectativas de las organizaciones modernas, su arquitectura subyacente debe garantizar **cuatro características básicas**:
> 1. **Tolerancia a Fallos (*Fault Tolerance*)**
> 2. **Escalabilidad (*Scalability*)**
> 3. **Calidad de Servicio (*Quality of Service* — QoS)**
> 4. **Seguridad (*Security*)**

### 1️⃣ Tolerancia a Fallos (*Fault Tolerance*)
* Una red tolerante a fallos **limita el impacto de una falla** de hardware o software reduciendo la cantidad de dispositivos afectados, y permite una rápida recuperación.
* Depende de contar con **múltiples rutas físicas (redundancia)** entre el origen y el destino.
* **Conmutación por Paquetes vs. Conmutación por Circuitos:**
  * En una **red conmutada por circuitos** (*circuit-switched network*, como la telefonía tradicional), se establece un circuito físico dedicado entre origen y destino; si ese enlace se corta, la llamada se cae y no puede redirigirse sola.
  * En cambio, las redes de datos modernas son **redes conmutadas por paquetes** (*packet-switched networks*): el mensaje se divide en pequeños **paquetes** independientes, y cada paquete puede tomar una ruta alternativa diferente si un router o enlace intermedio falla.

### 2️⃣ Escalabilidad (*Scalability*)
* Una red escalable puede **expandirse rápida y fácilmente** para admitir nuevos usuarios, redes completas y nuevas aplicaciones **sin degradar el rendimiento** de los servicios que ya están utilizando los usuarios actuales.
* Se logra gracias a que los diseñadores siguen **estándares y protocolos abiertos aceptados** por la industria.

### 3️⃣ Calidad de Servicio (*Quality of Service* — QoS)
* Cuando la demanda de tráfico supera el ancho de banda disponible en un enlace, los paquetes se encolan en la memoria del router y sufren demoras (*latencia*, *jitter*) o descartes.
* Aplicaciones en tiempo real como **transmisiones de voz (VoIP) y video en vivo** son extremadamente sensibles a las demoras: si no hay QoS configurada, el video sufre pausas o cortes constantes.
* **QoS** es el mecanismo principal configurado en los routers y switches para **establecer prioridades en las colas de tráfico**, garantizando que la voz y el video crítico pasen antes que el tráfico menos sensible al retardo (como descargas de archivos o correo electrónico).

### 4️⃣ Seguridad de Red (*Network Security*) y la Tríada CIA
La seguridad de red se divide en dos áreas:
1. **Seguridad de la Infraestructura de Red:** Asegurar físicamente los dispositivos de red (salas bajo llave) y prevenir el acceso lógico no autorizado a su consola o gestión (contraseñas, SSH).
2. **Seguridad de la Información:** Proteger los datos empresariales mientras viajan por la red o están almacenados, cumpliendo con los **3 Objetivos de la Seguridad (Tríada CIA)**:

> 📌 **CONCEPTO CLAVE — La Tríada CIA de Seguridad de la Información:**
> * 🔒 **Confidencialidad (*Confidentiality*):** Garantiza que **únicamente los destinatarios previstos y autorizados** puedan acceder y leer los datos (se implementa mediante autenticación fuerte y cifrado).
> * 🛡️ **Integridad (*Integrity*):** Garantiza que la información **no haya sido alterada, manipulada ni corrompida** de forma accidental o intencional durante la transmisión del origen al destino (se implementa mediante hashes criptográficos y firmas digitales).
> * ⚡ **Disponibilidad (*Availability*):** Garantiza que los usuarios autorizados tengan **acceso oportuno, continuo y confiable** a los servicios y datos de red cuando los necesiten (se implementa mediante redundancia, firewalls y mitigación de ataques DoS).

---

## 3.5. Amenazas y Soluciones de Seguridad en Capas

Los **vectores de amenaza** contra una red pueden provenir desde el exterior o desde el propio interior de la organización:

| 🌍 Amenazas Externas (*External Threats*) | 🏢 Amenazas Internas (*Internal Threats*) |
| :--- | :--- |
| • **Virus, gusanos (*worms*) y caballos de Troya (*Trojan horses*)**.<br>• **Spyware y adware**.<br>• **Ataques de día cero (*Zero-day attacks*)** (ocurren el primer día que se descubre una vulnerabilidad).<br>• **Ataques de actores de amenazas (*Threat actor attacks*)**.<br>• **Ataques de Denegación de Servicio (*DoS / DDoS*)**.<br>• **Intercepción y robo de datos** / **Robo de identidad**. | • Dispositivos corporativos **perdidos o robados** (laptops, smartphones).<br>• **Uso indebido accidental** por parte de empleados (conectar pendrives infectados, abrir correos de phishing).<br>• **Empleados malintencionados** (*insiders* que roban información o sabotean equipos desde adentro). |

### Defensa en Profundidad (Soluciones por tamaño de red):
* **Componentes básicos para Redes Domésticas o SOHO:**
  * Instalación de software **Antivirus y Antispyware** en todos los dispositivos finales.
  * **Filtrado de Firewall** básico para bloquear el tráfico entrante no autorizado desde Internet.
* **Requisitos adicionales para Redes Empresariales / Grandes:**
  * **Sistemas de Firewall dedicados** (*Dedicated firewall appliances*).
  * **Listas de Control de Acceso (ACL - *Access Control Lists*)** configuradas en los routers para filtrar paquetes por IP y puerto.
  * **Sistemas de Prevención de Intrusiones (IPS - *Intrusion Prevention Systems*)** que inspeccionan el tráfico en tiempo real buscando firmas de ataques.
  * **Redes Privadas Virtuales (VPN - *Virtual Private Networks*)** para cifrar el tráfico de trabajadores remotos y sucursales a través de Internet.

<div style="page-break-after: always;"></div>

---

# Módulo 4 (Unidad 9 - Parte B): Modelos de Referencia (OSI y TCP/IP), Protocolos, PDU y Acceso a Datos

## 4.1. Fundamentos de la Comunicación y las Reglas (Protocolos)

Toda comunicación —sea una conversación hablada entre dos personas o una transferencia de datos entre dos computadoras— posee siempre **tres elementos esenciales**:
1. **Fuente del Mensaje (Emisor / *Transmitter*):** Persona o dispositivo que origina la información.
2. **Destino del Mensaje (Receptor / *Receiver*):** Persona o dispositivo al que va dirigido el mensaje.
3. **Canal (Medio de Transmisión / *Transmission Medium*):** Proporciona la ruta física por la cual viaja la señal desde el emisor hasta el receptor.

Para que el emisor y el receptor se entiendan, deben ponerse de acuerdo previamente en **"cómo" comunicarse** mediante un conjunto de reglas llamadas **Protocolos**.
* **Reglas básicas en toda conversación:** Identificar remitente y receptor, usar un idioma y gramática común, acordar la velocidad y el momento de entrega, y definir si se requiere confirmación o acuse de recibo.

> 📌 **CONCEPTO CLAVE — Los 5 Requerimientos de los Protocolos de Red:**
> 1. **Codificación del mensaje (*Message Encoding*):** Proceso de convertir la información en otra forma aceptable para la transmisión (y **decodificación** en el receptor para revertir el proceso). Además, los bits se codifican en patrones de luz, impulsos eléctricos o ondas de radio en el canal.
> 2. **Formato y encapsulación del mensaje (*Message Formatting and Encapsulation*):** Todo mensaje debe seguir una estructura específica de campos (igual que una carta se mete dentro de un sobre con dirección de remitente y destinatario, los datos se encapsulan dentro de un paquete IPv4 o IPv6 con sus campos de cabecera).
> 3. **Tamaño del mensaje (*Message Size*):** Los mensajes grandes no pueden enviarse como una única ráfaga gigante por el cable porque monopolizarían el canal y cualquier error obligaría a reenviar todo; por eso se dividen en fragmentos más pequeños (**segmentación**).
> 4. **Temporización del mensaje (*Message Timing*):** Incluye tres sub-reglas:
>    * **Control de flujo (*Flow Control*):** Administra la velocidad de envío para que un emisor rápido no sature a un receptor más lento.
>    * **Tiempo de espera de respuesta (*Response Timeout*):** Define cuánto tiempo espera un host el acuse de recibo antes de asumir que el paquete se perdió y tomar una acción (retransmitir).
>    * **Método de acceso (*Access Method*):** Determina en qué momento un dispositivo puede inyectar datos en el canal compartido para evitar **colisiones**.
> 5. **Opciones de entrega del mensaje (*Message Delivery Options*):** Define a cuántos destinatarios va dirigido el mensaje.

### Opciones de Entrega de Mensajes: Unicast, Multicast, Broadcast y Anycast

| Opción de Entrega | Relación Emisor $\rightarrow$ Receptor | Descripción Técnica | Soporte en IPv4 vs. IPv6 |
| :--- | :---: | :--- | :--- |
| **Unicast** | **Uno a Uno** ($1 \rightarrow 1$) | Un único origen envía un mensaje a un único destino específico. | Soportado en **IPv4 e IPv6**. |
| **Multicast** | **Uno a Muchos** ($1 \rightarrow M$, grupo) | Un origen envía un mensaje simultáneamente a un **grupo específico** de hosts suscriptos (no a todos). | Soportado en **IPv4 e IPv6**. |
| **Broadcast** | **Uno a Todos** ($1 \rightarrow \text{Todos}$) | Un origen envía un mensaje a **todos los dispositivos** dentro de la misma red local (dominio de broadcast). | Soportado **SOLO en IPv4**.<br>⚠️ **NO existe Broadcast en IPv6** (fue reemplazado por Multicast). |
| **Anycast** | **Uno al más cercano de un grupo** | Un origen envía un paquete a una dirección asignada a varios servidores y se entrega al nodo **más cercano** según el enrutamiento. | Característica nativa incorporada en **IPv6**. |

---

## 4.2. Funciones e Interacción de los Protocolos y Suites de Protocolos

Cada protocolo de red (implementado en software, hardware o ambos) cumple una o varias **funciones específicas**:
* **Direccionamiento (*Addressing*):** Identifica al emisor y al receptor.
* **Confiabilidad (*Reliability*):** Proporciona entrega garantizada en caso de pérdidas.
* **Control de flujo (*Flow Control*):** Asegura que los datos fluyan a una tasa eficiente.
* **Secuenciamiento (*Sequencing*):** Etiqueta cada segmento transmitido para poder reordenarlos si llegan por caminos distintos.
* **Detección de errores (*Error Detection*):** Determina si los datos se corrompieron en tránsito.
* **Interfaz de aplicación (*Application Interface*):** Gestiona la comunicación proceso a proceso entre programas en distintos hosts.

### Ejemplo de Interacción de una Suite de Protocolos (Petición Web)
Ningún protocolo trabaja aislado; forman una **Suite de Protocolos** organizada en capas donde las capas inferiores prestan servicios de traslado de datos a las capas superiores:

| Capa TCP/IP | Protocolo que interviene | Función específica en una consulta Web |
| :---: | :--- | :--- |
| **4. Aplicación** | **HTTP** (*Hypertext Transfer Protocol*) | Rige la forma en que interactúan el servidor web y el navegador cliente; define el contenido y formato de las solicitudes y respuestas. |
| **3. Transporte** | **TCP** (*Transmission Control Protocol*) | Gestiona las conversaciones individuales entre servidor y cliente, segmenta los datos, garantiza la entrega y administra el control de flujo. |
| **2. Internet** | **IP** (*Internet Protocol*) | Toma los segmentos de TCP, los encapsula en paquetes, les asigna direcciones lógicas y los entrega globalmente desde el origen hasta el destino. |
| **1. Acceso a la Red** | **Ethernet** | Toma los paquetes IP, los encapsula en tramas físicas y los entrega desde una tarjeta de red (NIC) a otra NIC en la misma red local (LAN). |

### Evolución Histórica de las Suites de Protocolos y Mapa Completo de TCP/IP
A lo largo de la historia compitieron cuatro suites de protocolos (todas compartiendo en la capa inferior los estándares físicos/enlace como Ethernet o WLAN):
1. **TCP/IP (*Internet Protocol Suite*):** Estándar abierto mantenido por el **IETF**, hoy universal en Internet.
2. **ISO (OSI):** Suite desarrollada por ISO e ITU (usaba protocolos como CLNP/CONP en red y TP0–TP4 en transporte).
3. **AppleTalk:** Suite propietaria antigua de Apple Inc.
4. **Novell NetWare (IPX/SPX):** Suite propietaria antigua de Novell Inc.

```text
MAPA COMPLETO DE PROTOCOLOS DE LA SUITE TCP/IP (Según Diapositiva 19):
+-----------------------------------------------------------------------------------+
| CAPA DE APLICACIÓN (Application Layer)                                            |
| • Sistema de Nombres:  DNS                                                        |
| • Config. de Hosts:    DHCPv4, DHCPv6, SLAAC                                      |
| • Correo Electrónico:  SMTP, POP3, IMAP                                           |
| • Transf. de Archivos: FTP, SFTP, TFTP                                            |
| • Web y Servicios Web: HTTP, HTTPS, REST                                          |
+-----------------------------------------------------------------------------------+
| CAPA DE TRANSPORTE (Transport Layer)                                              |
| • Orientado a la Conexión: TCP          • Sin Conexión (Connectionless): UDP      |
+-----------------------------------------------------------------------------------+
| CAPA DE INTERNET (Internet Layer)                                                 |
| • Protocolo Internet: IPv4, IPv6, NAT   • Mensajería: ICMPv4, ICMPv6, ICMPv6 ND   |
| • Protocolos de Enrutamiento (Routing): OSPF, EIGRP, BGP                          |
+-----------------------------------------------------------------------------------+
| CAPA DE ACCESO A LA RED (Network Access Layer)                                    |
| • Resolución de Direcciones: ARP        • Enlace de Datos: Ethernet, WLAN         |
+-----------------------------------------------------------------------------------+
```

### Organizaciones de Estandarización Abierta
Los estándares abiertos son creados por organizaciones sin fines de lucro y neutrales para fomentar la **interoperabilidad**, la **competencia** y la **innovación**:
* **IETF (*Internet Engineering Task Force*):** Desarrolla y actualiza los protocolos de Internet y TCP/IP mediante documentos **RFC** (*Request for Comments*).
* **ICANN (*Internet Corporation for Assigned Names and Numbers*):** Coordina globalmente la asignación de direcciones IP y la gestión del sistema de nombres de dominio (DNS).
* **IANA (*Internet Assigned Numbers Authority*):** Opera bajo ICANN supervisando y administrando la distribución de bloques de direcciones IP, nombres de dominio raíz y los **números de puerto TCP/UDP**.
* **IEEE (*Institute of Electrical and Electronics Engineers*):** Crea estándares de redes locales e inalámbricas, destacando el comité **IEEE 802** (`802.3` Ethernet y `802.11` Wi-Fi).
* **ITU (*International Telecommunication Union*):** Organismo de la ONU que regula telecomunicaciones globales, espectro radioeléctrico y normas de transmisión como PDH/SDH.

---

## 4.3. Modelos de Referencia: Modelo OSI vs. Modelo TCP/IP

> 📌 **CONCEPTO CLAVE — ¿Cuáles son los 4 Beneficios de usar un Modelo en Capas?**
> 1. **Ayuda en el diseño de protocolos:** Cada protocolo opera en una capa bien definida con funciones específicas e interfaces claras hacia la capa superior e inferior.
> 2. **Fomenta la competencia:** Productos de distintos fabricantes pueden trabajar juntos sin problemas (**interoperabilidad**).
> 3. **Aísla los cambios tecnológicos:** Una mejora o cambio en una capa (por ejemplo, cambiar cable de cobre por fibra óptica o Wi-Fi en Capa 1/2) no obliga a modificar el protocolo IP (Capa 3) ni el navegador web (Capa 7).
> 4. **Proporciona un lenguaje común:** Facilita describir y diagnosticar funciones de red universalmente.

### Tabla Comparativa Maestra: Modelo OSI (7 Capas) vs. Modelo TCP/IP (4 Capas)

| N° Capa OSI | Capa del Modelo de Referencia OSI | Capa Equivalente en Modelo TCP/IP | Protocolos Ejemplares | Unidad de Datos de Protocolo (PDU) | Direccionamiento / Identificador que utiliza |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **Capa 7** | **Aplicación** (*Application*) | **Aplicación** | HTTP, DNS, DHCP, FTP, SMTP | **Datos** (*Data* / `A-PDU`) | Datos codificados de usuario / URL / Nombres |
| **Capa 6** | **Presentación** (*Presentation*) | **Aplicación** | Formatos, cifrado, compresión | **Datos** (*Data* / `P-PDU`) | Formato de representación de datos |
| **Capa 5** | **Sesión** (*Session*) | **Aplicación** | Control de diálogos | **Datos** (*Data* / `S-PDU`) | Identificadores de sesión |
| **Capa 4** | **Transporte** (*Transport*) | **Transporte** | **TCP, UDP** | **Segmento** (TCP) / **Datagrama** (UDP) (`T-PDU`) | **Números de Puerto** de origen y destino (Procesos) |
| **Capa 3** | **Red** (*Network*) | **Internet** | **IPv4, IPv6, ICMPv4, ICMPv6**, OSPF, EIGRP | **Paquete** (*Packet* / `N-PDU`) | **Direcciones Lógicas (IP)** de origen y destino (Entrega global extremo a extremo) |
| **Capa 2** | **Enlace de Datos** (*Data Link*) | **Acceso a la Red** | **Ethernet, WLAN (802.11), ARP**, PPP, HDLC | **Trama** (*Frame* / `DL-PDU`) | **Direcciones Físicas (MAC)** de origen y destino (Entrega local de NIC a NIC en el mismo enlace) |
| **Capa 1** | **Física** (*Physical*) | **Acceso a la Red** | Cobre (UTP), Fibra (SONET/SDH), Radio | **Bits** (*Bit Stream*) | Bits de temporización y sincronización (Señales físicas) |

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — Diferencias clave entre el Modelo OSI y el Modelo TCP/IP:**
> * El modelo **OSI** desglosa la capa de **Aplicación** de TCP/IP en **tres capas** (Aplicación, Presentación y Sesión) y divide la capa de **Acceso a la Red** de TCP/IP en **dos capas** (Enlace de Datos y Física).
> * Las capas de **Transporte (Capa 4)** y de **Red / Internet (Capa 3)** tienen equivalencia directa $1\text{ a }1$ entre ambos modelos.
> * La suite de protocolos TCP/IP **no especifica por sí misma qué protocolos usar para transmitir sobre un medio físico**, sino que se apoya en los estándares de Capa 1 y Capa 2 descritos en el modelo OSI (como Ethernet IEEE 802.3).

---

## 4.4. Encapsulación, Desencapsulación y Unidades de Datos de Protocolo (PDU)

* **Encapsulación (en el Emisor — de arriba hacia abajo):** A medida que los datos de la aplicación descienden por la pila de protocolos, cada capa les agrega su propio **encabezado (*Header*)** —y en el caso de la Capa 2 de Enlace de Datos, también un **tráiler (*Trailer*)** al final—.
* **Desencapsulación (en el Receptor — de abajo hacia arriba):** Cuando el flujo de bits llega al destino, sube por la pila: cada capa lee y verifica su encabezado correspondiente, lo **elimina** y entrega la carga útil a la capa inmediatamente superior hasta llegar a la aplicación.

```text
PROCESO DE ENCAPSULACIÓN (De Capa 7 a Capa 1):

[ Capa Aplicación ]   | Datos de Usuario (Data) |                       ---> PDU: DATOS
                                  |
                                  v
[ Capa Transporte ]   | TH (TCP) | Datos |                              ---> PDU: SEGMENTO
                                  |
                                  v
[ Capa de Red / IP]   | NH (IP) | TH (TCP) | Datos |                    ---> PDU: PAQUETE
                                  |
                                  v
[ Capa de Enlace  ]   | LH (Ethernet) | NH (IP) | TH (TCP) | Datos | LT (FCS) | -> PDU: TRAMA (FRAME)
                                  |
                                  v
[ Capa Física     ]   1 1 0 0 0 1 0 1 0 1 0 0 0 1 0 1 1 0 0 1 0 1 ...   ---> PDU: BITS
```

*(Nota del enfoque OSI detallado de la Diapositiva 30: En OSI estricto, cada capa $X$ agrega su encabezado `XH` a la PDU de la capa superior formando una `X-PDU`: `AH + Datos = A-PDU` $\rightarrow$ `PH + A-PDU = P-PDU` $\rightarrow$ `SH + P-PDU = S-PDU` $\rightarrow$ `TH + S-PDU = T-PDU` $\rightarrow$ `NH + T-PDU = N-PDU` $\rightarrow$ `LH + N-PDU + LT = DL-PDU`).*

---

## 4.5. Acceso a los Datos: Lógica de Direccionamiento de Capa 2 vs. Capa 3

Para entregar un mensaje, la red combina el trabajo de **dos tipos de direcciones** que cumplen roles completamente distintos:

> 📌 **CONCEPTO CLAVE — La Regla de Oro del Direccionamiento en Redes (¡Pregunta Clave de Examen!):**
> * **Direcciones de Capa 3 (Direcciones Lógicas IP):** Se encargan de llevar el paquete desde la **fuente original hasta el destino final**, ya sea que estén en la misma red o en el otro extremo del planeta. **Las direcciones IP de origen y destino NUNCA cambian de salto en salto** a medida que el paquete atraviesa routers (salvo que intervenga NAT).
> * **Direcciones de Capa 2 (Direcciones Físicas MAC):** Tienen alcance **estrictamente local** y se encargan de entregar la trama únicamente **desde una tarjeta de red (NIC) hasta la siguiente NIC dentro del mismo enlace físico**. Por lo tanto, **las direcciones MAC de origen y destino CAMBIAN en cada salto (en cada router)**.

Veamos los dos escenarios analizados en las diapositivas de cátedra:

### Escenario 1: Comunicación entre Dispositivos en la MISMA Red IP
* **Origen (PC1):** IP `192.168.1.110` (Porción de red: `192.168.1`, Host: `110`), MAC `AA-AA-AA-AA-AA-AA`.
* **Destino (FTP Server):** IP `192.168.1.9` (Porción de red: `192.168.1`, Host: `9`), MAC `CC-CC-CC-CC-CC-CC`.
* **Análisis:** Al comparar las porciones de red, `PC1` comprueba que ambos pertenecen a la misma red `192.168.1.0/24`. Por lo tanto, no necesita usar el router: averigua directamente la MAC del servidor FTP (mediante ARP) y arma la trama:

| Campo de Trama Ethernet (Capa 2) | Campo de Paquete IP (Capa 3) |
| :--- | :--- |
| **MAC Destino:** `CC-CC-CC-CC-CC-CC` (NIC del FTP Server)<br>**MAC Origen:** `AA-AA-AA-AA-AA-AA` (NIC de PC1) | **IP Origen:** `192.168.1.110` (PC1)<br>**IP Destino:** `192.168.1.9` (FTP Server) |

---

### Escenario 2: Comunicación con un Dispositivo en una RED REMOTA (Uso del Default Gateway)
Ahora `PC1` (`192.168.1.110`) quiere comunicarse con un **Servidor Web** cuya IP es `172.16.1.99` (ubicado a través de dos routers, `R1` y `R2`).

```text
Topología del Escenario en Red Remota:
[ PC1 ] ----------------> [ Router R1 ] ----------------> [ Router R2 ] ----------------> [ Web Server ]
IP:  192.168.1.110        IP LAN: 192.168.1.1 (DGW)       IP LAN: 172.16.1.1              IP:  172.16.1.99
MAC: AA-AA-AA-AA-AA-AA    MAC LAN: 11-11-11-11-11-11      MAC LAN: 22-22-22-22-22-22      MAC: AB-CD-EF-12-34-56
       (Salto 1: LAN Origen)            (Salto 2: Enlace WAN)           (Salto 3: LAN Destino)
```

* **¿Cómo actúa `PC1`?** Al ver que la porción de red del destino (`172.16.1`) es **diferente** a la suya (`192.168.1`), sabe que el destino es remoto. Como `PC1` no puede poner en la trama Ethernet la MAC de un equipo que está fuera de su LAN, le entrega la trama a su **Gateway Predeterminado (*Default Gateway* — DGW)**, que es la interfaz local del router `R1` (`192.168.1.1`, cuya MAC es `11-11-11-11-11-11`).

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Seguimiento Salto por Salto (Qué cambia y qué se mantiene):**
> Observá en la siguiente tabla cómo viaja el paquete desde `PC1` hasta el `Web Server` a través de los tres enlaces:
>
> | Tramo / Salto del Recorrido | MAC de Origen (Capa 2) | MAC de Destino (Capa 2) | IP de Origen (Capa 3) | IP de Destino (Capa 3) |
> | :--- | :--- | :--- | :---: | :---: |
> | **1.er Salto:** `PC1` $\rightarrow$ `Router R1` | `AA-AA-AA-AA-AA-AA` *(NIC de PC1)* | `11-11-11-11-11-11` *(NIC LAN de R1 - DGW)* | **`192.168.1.110`** *(No cambia)* | **`172.16.1.99`** *(No cambia)* |
> | **2.º Salto:** `Router R1` $\rightarrow$ `Router R2` | *MAC de interfaz de salida WAN de `R1`* | *MAC de interfaz de entrada WAN de `R2`* | **`192.168.1.110`** *(No cambia)* | **`172.16.1.99`** *(No cambia)* |
> | **3.er Salto:** `Router R2` $\rightarrow$ `Web Server` | `22-22-22-22-22-22` *(NIC LAN de salida de R2)* | `AB-CD-EF-12-34-56` *(NIC del Web Server)* | **`192.168.1.110`** *(No cambia)* | **`172.16.1.99`** *(No cambia)* |
>
> ⚠️ **Conclusión fundamental:** Si un host no tiene configurada la dirección de su **Default Gateway** (o tiene una dirección errónea), podrá comunicarse perfectamente con los equipos de su propia LAN, pero **su tráfico jamás podrá salir hacia redes remotas ni hacia Internet**.

<div style="page-break-after: always;"></div>

---

# Módulo 5 (Unidad 10): Redes LAN, Trama Ethernet, Direccionamiento MAC y Switching de Capa 2

## 5.1. Propósito de la Capa de Enlace de Datos y Topologías LAN

La **Capa de Enlace de Datos (Capa 2 del modelo OSI)** es la responsable directa de las comunicaciones entre las tarjetas de interfaz de red (**NIC**) de los dispositivos dentro de un mismo enlace físico:
* Permite que las capas superiores (como IPv4 e IPv6 en Capa 3) accedan a los medios físicos.
* **Encapsula** los paquetes de Capa 3 dentro de **Tramas (*Frames*) de Capa 2**.
* Realiza **detección de errores** mediante el tráiler FCS y **rechaza/descarta las tramas corruptas**.

### Topologías Físicas de Redes LAN
* **Topologías actuales:**
  * **Estrella (*Star Topology*):** Todos los dispositivos finales se conectan a un dispositivo intermediario central (un **Switch Ethernet**).
  * **Estrella Extendida (*Extended Star Topology*):** Varios switches en estrella se interconectan entre sí formando una jerarquía.
  * *Ventajas:* Son **fáciles de instalar**, **altamente escalables** (agregar un host es solo conectar un cable al switch) y **fáciles de diagnosticar** (si se corta el cable de una PC, solo cae esa PC y el resto de la red sigue operando).
* **Topologías heredadas (*Legacy* — primeras redes Ethernet coaxiales y Token Ring):**
  * **Bus (*Bus Topology*):** Todos los hosts están encadenados a un mismo cable coaxial troncal con **terminadores** en ambos extremos para absorber la señal. Si el cable se corta en cualquier punto, cae toda la red.
  * **Anillo (*Ring Topology*):** Cada host se conecta con sus dos vecinos formando un lazo cerrado (ej. Token Ring / FDDI).

---

## 5.2. Comunicación Half-Duplex vs. Full-Duplex y Método CSMA/CD

| Modo de Comunicación | Principio de Operación | Dónde se utiliza | ¿Pueden ocurrir colisiones? | Requires CSMA/CD o CSMA/CA |
| :--- | :--- | :--- | :---: | :---: |
| **Half-Duplex** (*Semidúplex*) | Solo permite que un dispositivo **envíe O reciba a la vez** en un medio compartido (como un *walkie-talkie*). | • Redes inalámbricas (**WLAN**).<br>• Topologías en bus heredadas y concentradores (**Hubs** Ethernet). | **SÍ** (Dominio de colisión compartido). | **SÍ** (CSMA/CD en cable; CSMA/CA en Wi-Fi). |
| **Full-Duplex** (*Dúplex Completo*) | Permite que ambos extremos **transmitan Y reciban simultáneamente** por pares de hilos separados. | • **Switches Ethernet modernos** conectados a NICs o a otros switches. | **NO** (Microsegmentación: 0 colisiones). | **NO** (No es necesario usar CSMA/CD). |

> 📌 **CONCEPTO CLAVE — ¿Cómo funciona CSMA/CD (*Carrier Sense Multiple Access with Collision Detection*)?**
> Utilizado en las redes Ethernet heredadas en modo **half-duplex** (con hubs o cable coaxial):
> 1. **Escucha de portadora (*Carrier Sense*):** Antes de transmitir, la NIC escucha el cable para verificar si está libre.
> 2. **Acceso múltiple (*Multiple Access*):** Como todos comparten el mismo cable, dos estaciones pueden escuchar silencio al mismo tiempo y transmitir simultáneamente, provocando una **colisión** (las señales eléctricas se superponen y se destruyen).
> 3. **Detección de colisión (*Collision Detection*) y Retroceso Exponencial Binario:** Los dispositivos detectan el aumento de voltaje de la colisión, envían una señal de refuerzo de colisión (*jam signal*) para que todos se enteren, abortan el envío y ejecutan el **algoritmo de retroceso exponencial binario (*binary exponential backoff*)**, que calcula un **tiempo de espera aleatorio y creciente** antes de volver a intentar la transmisión.

---

## 5.3. Estructura de la Trama, Subcapas IEEE 802 (LLC y MAC) y Estándares Ethernet

Toda trama de Capa 2 (sea en redes LAN o WAN como **Ethernet**, **802.11 Wi-Fi**, **PPP**, **HDLC** o **Frame Relay**) consta de tres secciones:
1. **Encabezado (*Header*):** Contiene los campos `Frame Start` (Inicio de trama), `Addressing` (Direcciones MAC destino y origen), `Type` (Protocolo de Capa 3 encapsulado) y `Control`.
2. **Datos (*Data / Payload*):** Contiene el paquete IP de Capa 3 encapsulado.
3. **Tráiler (*Trailer*):** Contiene el campo `Error Detection` (FCS/CRC) y `Frame Stop` (Fin de trama).

### Las Dos Subcapas de la Capa de Enlace de Datos (Arquitectura IEEE 802)
El estándar IEEE divide la Capa 2 (Enlace de Datos) en **dos subcapas independientes** para aislar a la Capa 3 del hardware físico:

```text
+-------------------------------------------------------------------+
| CAPA DE RED (Capa 3 OSI) -> IPv4, IPv6                            |
+-------------------------------------------------------------------+
| CAPA DE ENLACE DE DATOS (Capa 2 OSI)                              |
|  +-------------------------------------------------------------+  |
|  | 1. Subcapa LLC (Logical Link Control) - Estándar IEEE 802.2 |  |
|  |    • Se comunica con la Capa 3 (identifica qué protocolo de |  |
|  |      capa de red viaja dentro de la trama).                 |  |
|  +-------------------------------------------------------------+  |
|  | 2. Subcapa MAC (Media Access Control)                       |  |
|  |    • Responsable de encapsular la trama (estructura, MAC    |  |
|  |      origen/destino, FCS) y del control de acceso al medio. |  |
|  +-------------------+--------------------+--------------------+  |
|  | Ethernet          | WLAN (Wi-Fi)       | WPAN (Bluetooth)   |  |
|  | IEEE 802.3        | IEEE 802.11        | IEEE 802.15        |  |
+--+-------------------+--------------------+--------------------+--+
| CAPA FÍSICA (Capa 1 OSI) -> Cobre, Fibra Óptica, Radiofrecuencia  |
+-------------------------------------------------------------------+
```

* **Variantes principales del estándar Ethernet IEEE 802.3 mencionadas en cátedra:**
  * **IEEE 802.3u:** *Fast Ethernet* ($100\text{ Mbps}$).
  * **IEEE 802.3z:** *Gigabit Ethernet sobre Fibra Óptica* ($1000\text{ Mbps}$).
  * **IEEE 802.3ab:** *Gigabit Ethernet sobre Cobre* ($1000\text{ Mbps}$ en par trenzado UTP).
  * **IEEE 802.3ae:** *10 Gigabit Ethernet sobre Fibra Óptica* ($10\text{ Gbps}$).

### Formato y Tamaño de la Trama Ethernet (Ethernet II vs. IEEE 802.3)

```text
a) Trama Ethernet II (DIX - la utilizada hoy en redes TCP/IP):
+-----------+-------+--------------+--------------+--------+-------------+---------+-------+
| Preámbulo |  SOF  | Dir. Destino | Dir. Fuente  |  Tipo  |    Datos    | Relleno |  CRC  |
|  (7 B)    | (1 B) |    (6 B)     |    (6 B)     | (2 B)  | (0-1500 B)  | (0-46B) | (4 B) |
+-----------+-------+--------------+--------------+--------+-------------+---------+-------+

b) Trama IEEE 802.3 original:
+-------------------+--------------+--------------+--------+-------------+---------+-------+
| Preámbulo (inc SOF| Dir. Destino | Dir. Fuente  | Largo  |    Datos    | Relleno |  CRC  |
|       (8 B)       |    (6 B)     |    (6 B)     | (2 B)  | (0-1500 B)  | (0-46B) | (4 B) |
+-------------------+--------------+--------------+--------+-------------+---------+-------+
                    |<----------- Tamaño de la Trama: Mínimo 64 B / Máximo 1518 B -------->|
```

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — Tamaño Mínimo y Máximo de la Trama Ethernet:**
> * El tamaño válido de una trama Ethernet va desde un **mínimo de 64 bytes** hasta un **máximo de 1518 bytes** ($6 + 6 + 2 + 1500 + 4 = 1518\text{ bytes}$).
> * **El campo de Preámbulo (y SOF, 8 bytes en total) NO se incluye** al contar el tamaño de la trama.
> * Si el paquete IP tiene menos de $46\text{ bytes}$ de datos, Ethernet agrega bytes de **Relleno (*Pad*)** hasta alcanzar los $46\text{ bytes}$ de carga útil para que la trama total llegue al mínimo de **$64\text{ bytes}$** ($6 + 6 + 2 + 46 + 4 = 64\text{ bytes}$).
> * Cualquier trama recibida que sea **menor a 64 bytes** (fragmento de colisión o *runt frame*) o **mayor a 1518 bytes** (*giant frame*) se considera **inválida y es descartada automáticamente** por el dispositivo receptor.

---

## 5.4. Direcciones MAC de Ethernet: Estructura y Tipos (Unicast, Broadcast y Multicast)

> 📌 **CONCEPTO CLAVE — Estructura de una Dirección MAC de 48 bits:**
> * Toda dirección MAC Ethernet mide **48 bits (6 bytes)** y se escribe como **12 dígitos hexadecimales** (donde cada dígito hexadecimal equivale a 4 bits y cada par de dígitos hexadecimales forma 1 byte).
> * Está grabada físicamente en la tarjeta de red (NIC) y se divide exactamente en dos mitades de 24 bits:
>   1. **Primeros 24 bits (3 bytes / 6 dígitos hexadecimales) — OUI (*Organizationally Unique Identifier*):** Código único asignado por el **IEEE** a cada fabricante registrado (ej. Cisco, Intel, Realtek).
>   2. **Últimos 24 bits (3 bytes / 6 dígitos hexadecimales) — Asignado por el Fabricante (*Vendor Assigned*):** Número de serie único asignado por el propio fabricante a cada interfaz producida.

```text
Estructura de una Dirección MAC (48 bits = 6 Bytes = 12 Dígitos Hexadecimales):
+-----------------------------------+-----------------------------------+
|        OUI del Fabricante         |     Asignado por el Fabricante    |
|  24 bits = 3 Bytes = 6 Díg. Hex   |  24 bits = 3 Bytes = 6 Díg. Hex   |
|         Ej: 00 - 07 - E9          |         Ej: 63 - CE - 53          |
+-----------------------------------+-----------------------------------+
```

### Procesamiento de Tramas en la NIC y los 3 Tipos de Direcciones MAC
Cuando una tarjeta de red (NIC) recibe una trama del cable, lee la **Dirección MAC de Destino**: si coincide con su propia MAC física (o es una MAC Broadcast o una MAC Multicast a la que está suscripta), acepta la trama y la pasa a las capas superiores; **si no coincide, la descarta inmediatamente**.

| Tipo de Dirección MAC | Formato / Prefijo Hexadecimal | Relación con Capa 3 (IPv4 / IPv6) | Comportamiento en Switch y Router |
| :--- | :--- | :--- | :--- |
| **1. MAC Unicast** | Dirección única de la NIC del host o router (ej. `00-07-E9-42-AC-28`). | Se obtiene dinámicamente mediante **ARP** (en IPv4) o **ND - Neighbor Discovery** (en IPv6). | • **Switch:** La envía solo por el puerto de destino (si está en su tabla MAC).<br>• ⚠️ **Regla:** **La MAC de origen SIEMPRE debe ser Unicast.** |
| **2. MAC Broadcast** | **`FF-FF-FF-FF-FF-FF`** *(48 unos en binario)*. | Encapsula paquetes IPv4 de broadcast (aquellos con todos `1` en la porción de host, ej. `192.168.1.255`). | • **Switch:** La reenvía por **todos los puertos excepto el de entrada**.<br>• **Router:** **NO la reenvía** (los routers detienen los broadcasts). |
| **3. MAC Multicast** | • En **IPv4:** comienza con **`01-00-5E`** (ej. `01-00-5E-00-00-C8`).<br>• En **IPv6:** comienza con **`33-33`**. | Encapsula paquetes IP multicast (ej. en IPv4 el rango `224.0.0.0` a `239.255.255.255`; la IP `224.0.0.200` mapea sus últimos bits `.200` = `C8` hex a `01-00-5E-00-00-C8`). | • **Switch:** La inunda por todos los puertos salvo el de entrada (a menos que tenga *IGMP snooping*).<br>• **Router:** No la reenvía salvo que tenga ruteo multicast.<br>• ⚠️ **Regla:** **Solo puede usarse como MAC de Destino** (nunca como origen). |

---

## 5.5. Funcionamiento del Switch Ethernet de Capa 2: Aprendizaje, Filtrado e Inundación

Un **Switch Ethernet de Capa 2** opera exclusivamente mirando las **direcciones MAC** de la trama Ethernet; **no conoce ni inspecciona qué protocolo viaja adentro** (no le importa si adentro hay un paquete IPv4, IPv6 o ARP). Para decidir por qué puerto reenviar cada trama utiliza una memoria de alta velocidad llamada **Tabla de Direcciones MAC (o tabla CAM)**.

> 📌 **CONCEPTO CLAVE — Las 2 Etapas del Switch: Aprendizaje (*Learning*) y Reenvío/Filtrado (*Forwarding/Filtering*):**
>
> **Etapa 1: APRENDIZAJE (Examina siempre la Dirección MAC de ORIGEN)**
> * Cuando el switch se enciende, su tabla MAC está **vacía**.
> * Cada vez que entra una trama por un puerto, el switch mira la **Dirección MAC de Origen** y el **número de puerto por donde entró**:
>   * Si esa MAC de origen no estaba en la tabla, **la agrega asociada a ese puerto** (y activa un temporizador, típicamente de 5 minutos).
>   * Si ya estaba, actualiza el temporizador.
>
> **Etapa 2: REENVÍO Y FILTRADO (Examina siempre la Dirección MAC de DESTINO)**
> * Luego de registrar el origen, el switch mira la **Dirección MAC de Destino** y toma una de estas tres decisiones:
>   1. **Unicast Conocido (Filtrado):** Si la MAC de destino es unicast y **está en la tabla MAC**, el switch **filtra** la trama enviándola **únicamente por el puerto específico** donde está conectado ese destino.
>   2. **Unicast Desconocido (*Flooding* / Inundación):** Si la MAC de destino es unicast pero **NO figura todavía en la tabla MAC**, el switch reenvía una copia de la trama por **todos los puertos activos excepto por el puerto por donde ingresó**.
>   3. **Broadcast o Multicast:** Si la MAC de destino es `FF-FF-FF-FF-FF-FF` (Broadcast) o Multicast, el switch también la envía por **todos los puertos excepto el de entrada**.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Filtrado de Tramas en un Switch de 4 Puertos:**
> Supongamos un switch con 4 hosts conectados:
> * `Puerto 1`: Host **A** (`MAC 00-0A`)
> * `Puerto 2`: Host **B** (`MAC 00-0B`)
> * `Puerto 3`: Host **C** (`MAC 00-0C`)
> * `Puerto 4`: Host **D** (`MAC 00-0D`)
>
> En su tabla MAC actual ya figuran aprendidas las entradas `Puerto 1 -> 00-0A` y `Puerto 4 -> 00-0D`.  
> Cuando el **Host A** envía una trama dirigida al **Host D** (`[ Dest MAC: 00-0D | Source MAC: 00-0A | Type | Data | FCS ]`):
> 1. La trama entra por el **Puerto 1**. El switch verifica que `00-0A` ya está asociado al Puerto 1.
> 2. Busca la MAC de destino `00-0D` en su tabla y encuentra que corresponde al **Puerto 4**.
> 3. **Filtra** la trama: la conmuta directamente hacia el **Puerto 4**, sin molestar ni ocupar ancho de banda en los puertos 2 y 3 (a diferencia de un antiguo Hub que habría repetido la señal por los puertos 2, 3 y 4).

---

> 🛠️ **LABORATORIO EN PACKET TRACER #3 (Unidades 9 y 10): Aprendizaje de Tabla MAC, ARP y Filtrado del Switch**
>
> **Objetivo:** Comprobar experimentalmente cómo un switch aprende direcciones MAC en su tabla CAM, cómo opera el protocolo ARP usando broadcast y cómo el switch pasa de inundar (*flooding*) a filtrar (*filtering*).
>
> **Paso a paso en Cisco Packet Tracer:**
> 1. Colocá **1 Switch `2960`** (`S1`) y **3 PCs** (`PC1`, `PC2`, `PC3`) conectadas a los puertos `Fa0/1`, `Fa0/2` y `Fa0/3` respectivamente.
> 2. Asigná las siguientes direcciones IP en la misma red:
>    * `PC1`: `192.168.10.1` / `255.255.255.0`
>    * `PC2`: `192.168.10.2` / `255.255.255.0`
>    * `PC3`: `192.168.10.3` / `255.255.255.0`
> 3. Abrí el CLI del switch `S1` y ejecutá:
>    ```ios
>    S1# show mac-address-table
>    ```
>    *(Observá que la tabla de direcciones MAC dinámicas está vacía).*
> 4. Pasá al **Modo Simulación** y desde `PC1` abrí el `Command Prompt` y ejecutá `ping 192.168.10.3` (`PC3`).
> 5. Avanzá paso a paso con el botón **Capture / Forward**:
>    * **Primer sobre (ARP Request):** Como `PC1` no conoce la MAC de `PC3`, envía una trama **ARP** con **MAC Destino `FFFF.FFFF.FFFF` (Broadcast)**. Cuando llega a `S1`, el switch **aprende la MAC de `PC1` en `Fa0/1`** y reenvía el broadcast tanto a `PC2` como a `PC3`.
>    * `PC2` descarta el ARP (aparece una `X` roja porque no es su IP), mientras que `PC3` acepta el ARP y responde con un **ARP Reply (Unicast)** dirigido a la MAC de `PC1`.
>    * Cuando el ARP Reply de `PC3` entra a `S1` por `Fa0/3`, el switch **aprende la MAC de `PC3` en `Fa0/3`** y, como ya conoce dónde está `PC1`, **filtra** la trama enviándola **solo a `PC1`** (`Fa0/1`), sin enviarla a `PC2`.
> 6. Volvé al CLI del switch y ejecutá nuevamente `show mac-address-table` para ver las dos direcciones MAC registradas como `DYNAMIC` en `Fa0/1` y `Fa0/3`.

<div style="page-break-after: always;"></div>

---

# Módulo 6 (Capa 3): Capa de Red, Paquetes IPv4 e IPv6, Enrutamiento Estático y Dinámico (RIP, OSPF, EIGRP)

## 6.1. Operaciones Básicas de la Capa de Red y Características del Protocolo IP

La **Capa de Red (Capa 3 del modelo OSI / Capa de Internet en TCP/IP)** provee los servicios que permiten a los dispositivos finales intercambiar datos a través de distintas redes interconectadas. Sus dos protocolos principales son **IP versión 4 (IPv4)** e **IP versión 6 (IPv6)**.

> 📌 **CONCEPTO CLAVE — Las 4 Operaciones Básicas de la Capa de Red:**
> 1. **Direccionamiento de los dispositivos finales (*Addressing*):** Asigna una identificación lógica única (IP) a cada host.
> 2. **Encapsulación (*Encapsulation*):** Toma la PDU de la capa de transporte (el **Segmento** TCP o Datagrama UDP) y le agrega un **Encabezado IP** (IPv4 o IPv6) para formar un **Paquete IP**.
> 3. **Enrutamiento (*Routing*):** Los routers a lo largo del camino examinan la dirección IP de destino en el encabezado del paquete y determinan la mejor ruta (*best path*) hacia la red de destino.
> 4. **Desencapsulación (*De-encapsulation*):** Cuando el paquete llega al host de destino final, este verifica que la IP de destino sea la suya, quita el encabezado IP y entrega el segmento a la Capa 4.

### Las 3 Características Fundamentales del Protocolo IP
El protocolo IP fue diseñado intencionalmente para ser rápido y tener **bajo consumo de recursos (*low overhead*)**, delegando las tareas pesadas de confiabilidad a la Capa 4 (TCP). Sus tres características son:

| Característica de IP | Significado Técnico Detallado |
| :--- | :--- |
| **1. Sin Conexión (*Connectionless*)** | • **No establece una conexión ni diálogo previo** con el destino antes de enviar los paquetes (igual que dejar una carta en el buzón de correo postal sin llamar antes al destinatario).<br>• No intercambia información de control previa ni sincronización.<br>• Si se requiere orientación a la conexión, se encarga **TCP en Capa 4**. |
| **2. De Mejor Esfuerzo (*Best Effort* / No Confiable — *Unreliable*)** | • **No garantiza la entrega** del paquete.<br>• **No espera confirmaciones (*ACK*)** ni sabe si el receptor está encendido o si el paquete llegó.<br>• **No retransmite** paquetes perdidos o dañados, ni **reordena** paquetes que lleguen fuera de secuencia. |
| **3. Independiente del Medio (*Media Independent*)** | • Funciona de manera transparente sobre **cualquier medio físico** (cobre Ethernet, serial de cobre, fibra óptica, radio inalámbrica Wi-Fi) sin importar el tipo de trama de Capa 2.<br>• La única característica del medio que la Capa 3 sí tiene en cuenta es la **MTU (*Maximum Transmission Unit* — Unidad Máxima de Transmisión)** que le informa la Capa 2. |

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — MTU y Fragmentación en IPv4 vs. IPv6:**
> * La **MTU** es el tamaño máximo de paquete (en bytes) que puede transportar una trama en un enlace específico (en Ethernet es por defecto **$1500\text{ bytes}$**).
> * **En IPv4:** Si un router recibe un paquete por una interfaz Ethernet y debe enviarlo por un enlace WAN más lento cuya MTU es menor, **el router fragmenta (divide) el paquete IPv4 en fragmentos más pequeños** (lo cual **genera latencia** y sobrecarga).
> * **En IPv6:** **Los routers NUNCA fragmentan paquetes IPv6.** (Si es necesario fragmentar, solo puede hacerlo el host de origen mediante un Encabezado de Extensión o ajustando su tamaño mediante *Path MTU Discovery*).

---

## 6.2. El Paquete IPv4 y sus Limitaciones

### Estructura Completa de la Cabecera IPv4 (20 Bytes base)
El encabezado IPv4 se organiza en filas de **32 bits (4 bytes)** leídas de izquierda a derecha, sumando un tamaño mínimo estándar de **20 Bytes** (sin opciones):

```text
ESTRUCTURA DEL ENCABEZADO IPv4 (20 Bytes = 5 filas de 4 Bytes / 32 bits):
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |   DS (DSCP/ECN)|         Total Length         |  <- Bytes 1 a 4
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|     Fragment Offset     |  <- Bytes 5 a 8 (Fragmentación)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |        Header Checksum        |  <- Bytes 9 a 12
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |  <- Bytes 13 a 16 (32 bits)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |  <- Bytes 17 a 20 (32 bits)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

* **Campos clave de la cabecera IPv4:**
  * **`Version` (4 bits):** Identifica la versión del protocolo; para IPv4 vale siempre `0100` ($4$ en binario).
  * **`IHL` (*Internet Header Length*, 4 bits):** Indica la longitud del encabezado IPv4.
  * **`DS` (*Differentiated Services*, 8 bits / 1 byte):** Utilizado para **Calidad de Servicio (QoS)**. Se divide en **6 bits de `DSCP`** (*DiffServ Code Point*, prioridad del paquete) y **2 bits de `ECN`** (*Explicit Congestion Notification*).
  * **`Total Length` (16 bits / 2 bytes):** Tamaño total del paquete IPv4 (cabecera + datos).
  * **`Identification` (16 bits), `Flag` (3 bits) y `Fragment Offset` (13 bits):** Tres campos utilizados exclusivamente para controlar y reensamblar paquetes cuando sufren **fragmentación**.
  * **`Time-to-Live (TTL)` (8 bits / 1 byte):** Tiempo de vida expresado en saltos. Cada router que atraviesa el paquete le resta $1$; si llega a $0$, el router descarta el paquete y envía un mensaje *ICMP Time Exceeded* (evitando bucles infinitos de enrutamiento).
  * **`Protocol` (8 bits / 1 byte):** Indica qué protocolo de capa superior viaja encapsulado en los datos (ej. `1` = ICMP, `6` = TCP, `17` = UDP).
  * **`Header Checksum` (16 bits / 2 bytes):** Suma de verificación que controla errores **solo de la cabecera IPv4**.
  * **`Source IP Address` (32 bits / 4 bytes) y `Destination IP Address` (32 bits / 4 bytes):** Los **dos campos más importantes**, con la IP del emisor original y del receptor final.

### Las 3 Grandes Limitaciones de IPv4
1. **Agotamiento de direcciones IPv4 (*IPv4 Address Depletion*):** Con 32 bits solo existen $2^{32} \approx 4.294\text{ millones}$ de direcciones teóricas, y las direcciones públicas disponibles ya se agotaron.
2. **Falta de conectividad de extremo a extremo (*Lack of End-to-End Connectivity*):** Para estirar la vida de IPv4 se crearon las direcciones privadas y el **NAT (*Network Address Translation*)**, lo que rompió la comunicación directa extremo a extremo entre hosts con direcciones públicas propias.
3. **Aumento de la complejidad de la red:** El NAT manipula las direcciones y puertos en cada paquete al cruzar el router de borde, lo que **agrega latencia y dificulta la resolución de problemas** y el uso de ciertos protocolos de seguridad.

---

## 6.3. El Protocolo IPv6: Mejoras y Estructura de su Cabecera

Desarrollado por el **IETF** para reemplazar a IPv4, **IPv6** introduce tres grandes mejoras:
1. **Espacio de direcciones gigantesco:** Utiliza direcciones de **128 bits** en lugar de 32 bits, pasando de $\sim 4\text{ mil millones}$ ($4 \times 10^9$) en IPv4 a **$2^{128} \approx 340\text{ undecillones}$** ($3,4 \times 10^{38}$) de direcciones en IPv6.
2. **Manejo mejorado de paquetes:** Su encabezado está **simplificado** con menos campos (8 campos en lugar de 12) y tiene un **tamaño fijo**, lo que acelera el procesamiento en los routers.
3. **Elimina la necesidad de NAT:** Al sobrar direcciones públicas globales para cada dispositivo del planeta, se recupera la conectividad real de extremo a extremo sin usar NAT.

### Estructura del Encabezado IPv6 (40 Bytes Fijos)

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN:**  
> El encabezado IPv6 es **más simple** (tiene menos campos), pero **NO es más pequeño en bytes**: mide un tamaño **fijo de 40 bytes** (frente a los 20 bytes de IPv4) debido a que las direcciones de origen y destino ocupan 16 bytes (128 bits) cada una.

```text
ESTRUCTURA DEL ENCABEZADO IPv6 (40 Bytes Fijos = 10 filas de 4 Bytes):
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |              Flow Label               |  <- Fila 1 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |  <- Fila 2 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                  Source IPv6 Address (128 bits)               +  <- Filas 3 a 6 (16 Bytes)
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+               Destination IPv6 Address (128 bits)             +  <- Filas 7 a 10 (16 Bytes)
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

| Campo en Cabecera IPv6 | Tamaño | Función y Equivalencia con IPv4 |
| :--- | :---: | :--- |
| **`Version`** | 4 bits | Fijado en **`0110`** ($6$ en binario, en lugar de `0100`). |
| **`Traffic Class`** | 8 bits | Usado para **QoS** (clasificación de tráfico); equivale al campo **`DS` (DiffServ)** de IPv4. |
| **`Flow Label`** | 20 bits | Campo nuevo que etiqueta paquetes de un mismo flujo en tiempo real para que los routers los traten de igual forma sin abrir la Capa 4. |
| **`Payload Length`** | 16 bits (2 B) | Indica la longitud de la porción de datos (carga útil) del paquete IPv6. |
| **`Next Header`** | 8 bits (1 B) | Identifica el protocolo de capa superior (**ICMPv6, TCP, UDP**) o un **Encabezado de Extensión (EH)**. Equivale al campo **`Protocol`** de IPv4. |
| **`Hop Limit`** | 8 bits (1 B) | Reemplaza al campo **`TTL`** de IPv4 para contar saltos de Capa 3. |
| **`Source IPv6 Address`** | 128 bits (16 B) | Dirección IPv6 del host emisor original. |
| **`Destination IPv6 Address`** | 128 bits (16 B) | Dirección IPv6 del host receptor final. |

* **¿Qué campos de IPv4 se ELIMINARON de la cabecera base de IPv6 para ganar velocidad?**
  1. Los 3 campos de fragmentación (**`Identification`**, **`Flags`** y **`Fragment Offset`**), porque los routers IPv6 no fragmentan.
  2. El campo **`Header Checksum`**, porque tanto la Capa 2 (FCS Ethernet) como la Capa 4 (TCP/UDP) ya verifican errores, ahorrándole a cada router tener que recalcular el Checksum IP en cada salto.
  3. El campo **`IHL`**, porque el encabezado IPv6 mide siempre **40 bytes fijos**.
* **Encabezados de Extensión (*Extension Headers* — EH):** Si se necesita información opcional de Capa 3 (como fragmentación hecha por el host origen, seguridad IPsec o movilidad), IPv6 inserta encabezados opcionales **entre la cabecera de 40 bytes y el payload**.

---

## 6.4. Enrutamiento del Host y Tablas de Enrutamiento Locales

Antes de que un paquete llegue siquiera a un router, **el propio dispositivo final (PC/Servidor) debe tomar una decisión de reenvío** consultando su propia tabla de enrutamiento interna.

> 📌 **CONCEPTO CLAVE — Los 3 Destinos Posibles desde un Host:**
> 1. **A sí mismo (*Itself / Loopback*):** Cuando hace ping o conecta a **`127.0.0.1`** (en IPv4) o **`::1`** (en IPv6) para probar su propia pila TCP/IP interna sin salir por la placa de red.
> 2. **Host Local (*Local Host*):** El destino está en la **misma red LAN**. El host lo descubre aplicando la operación lógica **AND** entre su IP, su máscara de subred y la IP de destino (en IPv6, comparando con el prefijo anunciado por el router). En este caso, el tráfico va directo al switch de Capa 2 usando ARP/ND sin pasar por el router.
> 3. **Host Remoto (*Remote Host*):** El destino está en **otra red distinta**. El host reenvía la trama directamente a la dirección MAC de su **Gateway Predeterminado (*Default Gateway* — DGW)**.

### Cómo aprende el Host su Default Gateway y cómo ver su Tabla en Windows
* **En IPv4:** El host recibe el DGW de forma dinámica por **DHCP** o se configura manualmente de forma **estática**.
* **En IPv6:** El router lo anuncia automáticamente mediante mensajes **RS / RA (*Router Solicitation / Router Advertisement*)** o se configura manualmente.
* En **Windows**, ejecutando **`route print`** o **`netstat -r`** en la consola se visualizan las **3 secciones** de ruteo del host:
  1. **`Interface List`:** Lista todas las NICs del equipo y sus direcciones MAC.
  2. **`IPv4 Routing Table`:** Donde la primera línea (`Network Destination: 0.0.0.0`, `Netmask: 0.0.0.0`, `Gateway: 192.168.10.1`) representa la **Ruta por Defecto (*Default Route*)** hacia el router, y las rutas `192.168.10.0` aparecen como **`On-link`** (conectadas directamente a la LAN).
  3. **`IPv6 Routing Table`:** Tabla equivalente para prefijos IPv6.

---

## 6.5. Enrutamiento en el Router: Proceso, Tipos de Rutas y `show ip route`

### Los 3 Pasos del Router al Recibir un Paquete
1. **Paso 1 (Desencapsulación L2):** El paquete entra por una interfaz del router (ej. `G0/0/0`). El router verifica que la MAC de destino sea la suya y **elimina el encabezado y el tráiler Ethernet de Capa 2**.
2. **Paso 2 (Inspección L3 y *Best Match*):** El router lee la **Dirección IP de Destino** del paquete y busca la coincidencia más específica (**mejor coincidencia o *longest prefix match***) en su **Tabla de Enrutamiento**.
3. **Paso 3 (Re-encapsulación L2 y Reenvío):** El router decrementa el `TTL` en 1, recalcula el Checksum IPv4, **vuelve a encapsular el paquete en una nueva trama de Capa 2** (con la MAC de su interfaz de salida como origen y la MAC del router de siguiente salto *next-hop* o del host final como destino) y lo envía por la interfaz de salida.

### Los 3 Tipos de Rutas en la Tabla de un Router y Códigos de `show ip route`

| Categoría de Ruta | Código en `show ip route` | Cómo se agrega a la tabla | Ejemplo en la salida CLI de Cisco IOS |
| :--- | :---: | :--- | :--- |
| **1. Directamente Conectadas** (*Directly Connected*) | **`C`** y **`L`** | **Automáticamente** cuando se configura una IP en una interfaz y se levanta con `no shutdown` (`up/up`). | `C 192.168.10.0/24 is directly connected, Gig0/0/0`<br>`L 192.168.10.1/32 is directly connected, Gig0/0/0` *(IP exacta de la interfaz `/32`)* |
| **2. Rutas Remotas** (*Remote Networks*) | **`S`** (Estática)<br>**`O`** (OSPF)<br>**`D`** (EIGRP)<br>**`R`** (RIP) | • **Manualmente** por el administrador (Ruta Estática `S`).<br>• **Dinámicamente** mediante un protocolo de ruteo (`O`, `D`, `R`). | `O 10.1.1.0/24 [110/2] via 209.165.200.226, 00:02:45, Gig0/0/1`<br>*(Donde `[110/2]` es `[Distancia Admin. / Métrica]`)* |
| **3. Ruta por Defecto** (*Default Route / Last Resort*) | **`S*`** | Ruta de último recurso usada cuando un paquete **no coincide con ninguna otra ruta** más específica de la tabla (ej. salida a Internet). | `S* 0.0.0.0/0 [1/0] via 209.165.200.226, Gig0/0/1` |

---

## 6.6. Enrutamiento Estático vs. Enrutamiento Dinámico

| Característica | Enrutamiento Estático (`S`) | Enrutamiento Dinámico (`O`, `D`, `R`, `B`) |
| :--- | :--- | :--- |
| **Configuración** | Configurado **manualmente** ruta por ruta por el administrador. | Los routers intercambian mensajes automáticamente para **descubrir redes remotas**. |
| **Reacción ante fallas en la red** | **NO se adapta solo:** si se cae un enlace, el administrador debe reconfigurar la ruta manualmente. | **Convergencia automática:** si cae un enlace, el protocolo recalcula y elige una **nueva mejor ruta** alternativa automáticamente. |
| **Caso de uso ideal** | Redes pequeñas sin caminos redundantes, enlaces punto a punto (*stub*) y configuración de **rutas por defecto (`0.0.0.0/0`)**. | Redes medianas a grandes con múltiples caminos redundantes y cambios frecuentes de topología. |
| **Comando Cisco IOS** | `R1(config)# ip route <Red_Remota> <Máscara> <IP_Next_Hop>`<br>Ej.: `ip route 10.1.1.0 255.255.255.0 209.165.200.226` | Se habilita el proceso del protocolo (ej. `router ospf 1`, `router eigrp 100`, `router rip`) y se declaran las redes con `network`. |

---

## 6.7. Clasificación de Protocolos de Enrutamiento Dinámico, Métrica y Distancia Administrativa

### Sistema Autónomo (AS — *Autonomous System*) y Árbol de Clasificación
> 📌 **CONCEPTO CLAVE — Sistema Autónomo (AS), IGP y EGP:**
> Un **Sistema Autónomo (AS)** es un conjunto de redes y routers que se encuentran bajo la administración de **una misma autoridad técnica o corporativa** (por ejemplo, la red de una universidad, un banco o un proveedor ISP).
> * **IGP (*Interior Gateway Protocols* — Protocolos de Enrutamiento Interior):** Operan **DENTRO** de un mismo Sistema Autónomo (**RIP, IGRP, EIGRP, OSPF, IS-IS**).
> * **EGP (*Exterior Gateway Protocols* — Protocolos de Enrutamiento Exterior):** Operan **ENTRE distintos** Sistemas Autónomos conectando todo Internet (**BGPv4** — *Border Gateway Protocol*).

```text
ÁRBOL DE CLASIFICACIÓN DE LOS PROTOCOLOS DE ENRUTAMIENTO DINÁMICO:

                      [ Protocolos de Enrutamiento Dinámico ]
                                         |
            +----------------------------+----------------------------+
            |                                                         |
  [ Enrutamiento Interior (IGP) ]                           [ Enrutamiento Exterior (EGP) ]
  (Dentro de un mismo Sistema Autónomo)                     (Entre Sistemas Autónomos)
            |                                                         |
     +------+-----------------------+                         [ Vector Ruta ]
     |                              |                                 |
[ Vector Distancia ]       [ Estado de Enlace ]                    • BGPv4
  • RIP -> RIPv2             • OSPF
  • IGRP -> EIGRP (Híbrido)  • IS-IS
```

### Diferencia Crítica: Métrica vs. Distancia Administrativa (AD)

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — ¿Cuándo usa el router la Distancia Administrativa y cuándo usa la Métrica?**
> Supongamos que en un examen te preguntan: *"Si un router aprende dos caminos hacia la misma red `10.1.1.0/24`, ¿cómo elige cuál instalar en su tabla de enrutamiento?"*  
> La respuesta depende de si los caminos provienen del **mismo protocolo** o de **protocolos diferentes**:
>
> 1. **Distancia Administrativa (AD — *Administrative Distance*):**
>    * **¿Qué mide?** Mide la **confiabilidad o preferencia del protocolo (origen)** que informó la ruta.
>    * **¿Cuándo se usa?** Cuando el router aprende la misma red destino a través de **DIFERENTES protocolos de enrutamiento** (por ejemplo, una ruta por OSPF y otra por RIP, o una Estática y otra por EIGRP).
>    * **Regla:** **Menor número = Mayor prioridad** (es más confiable).
>
> 2. **Métrica (*Metric*):**
>    * **¿Qué mide?** Mide el **costo de llegar al destino** calculado internamente por un protocolo.
>    * **¿Cuándo se usa?** Cuando el router tiene múltiples caminos hacia el mismo destino aprendidos por **EL MISMO protocolo de enrutamiento** (por ejemplo, dos rutas distintas dentro de OSPF).
>    * **Regla:** Cada protocolo calcula su métrica con una fórmula diferente (no se pueden comparar métricas de protocolos distintos entre sí); **se elige el camino de menor métrica**.

### Tabla Oficial de Distancia Administrativa (AD) y Métricas por Protocolo

| Origen de la Ruta / Protocolo | Valor de Distancia Administrativa (AD) | Métrica que utiliza el Protocolo para elegir el mejor camino |
| :--- | :---: | :--- |
| **Red Directamente Conectada (`C`)** | **`0`** *(Máxima prioridad)* | No aplica (conectada directamente al cable de la interfaz). |
| **Ruta Estática (`S`)** | **`1`** | Métrica `0` (configurada manualmente). |
| **Ruta Sumarizada de EIGRP** | **`5`** | Compuesta. |
| **BGP Externo (eBGP)** | **`20`** | Atributos de ruta / Vector de Sistemas Autónomos (*AS-Path*). |
| **EIGRP Interno (`D`)** | **`90`** | **Métrica Compuesta** (**Ancho de banda y Retardo** por defecto; más **Confiabilidad y Carga** opcionales). |
| **IGRP** | **`100`** | **Métrica Compuesta** (Ancho de banda, Retardo, Confiabilidad, Carga). |
| **OSPF (`O`)** | **`110`** | **Costo basado en el Ancho de Banda** (*Bandwidth*). |
| **RIP (`R`)** | **`120`** *(Menor prioridad)* | **Conteo de Saltos (*Hops*)** (cantidad de routers atravesados; máximo **15 saltos**). |

*(En la captura de `show ip route`, cuando ves `O 10.1.1.0/24 [110/2]`, el primer número entre corchetes `110` es siempre la **Distancia Administrativa** y el segundo número `2` es la **Métrica**).*

---

## 6.8. Estudio Detallado de RIP (v1 y v2) y EIGRP

### Comparativa: Protocolo RIPv1 vs. RIPv2

| Característica | RIP versión 1 (RIPv1) | RIP versión 2 (RIPv2) |
| :--- | :--- | :--- |
| **Tipo de Direccionamiento** | **Classful (Con clase):** NO envía la máscara de subred en sus actualizaciones. | **Classless (Sin clase):** **SÍ envía la máscara de subred**, por lo que **soporta VLSM y CIDR**. |
| **Métrica y Límite** | Número de saltos (**máximo 15 saltos**; 16 saltos = inalcanzable). | Número de saltos (**máximo 15 saltos**). |
| **Envío de Actualizaciones** | Envía toda su tabla cada **30 segundos** por **Broadcast (`255.255.255.255`)**. | Envía actualizaciones por **Multicast (`224.0.0.9`)**. |
| **Redes no contiguas y Sumarización** | **No soporta redes no contiguas**. Sumariza automáticamente en el borde de clase. | Usa **Split Horizon** (Horizonte Dividido) y permite desactivar la auto-sumarización (**`no auto-summary`**) y hacer sumarización manual. |

---

### Estudio Profundo de EIGRP (*Enhanced Interior Gateway Routing Protocol*)

> 📌 **CONCEPTO CLAVE — Características Esenciales de EIGRP:**
> * **¿Qué es?** Es un protocolo de enrutamiento dinámico interior (**IGP**) desarrollado por **Cisco** y catalogado como **Híbrido** (o vector distancia avanzado), porque combina lo mejor del vector distancia con ventajas del estado de enlace.
> * **Algoritmo DUAL (*Diffusing Update Algorithm*):** Es el motor matemático de EIGRP. Garantiza topologías **100% libres de bucles** y **convergencia ultrarrápida** porque mantiene en su tabla de topología tanto la mejor ruta principal (*Successor*) como una ruta de respaldo precalculada (*Feasible Successor*).
> * **Distancia Administrativa:** **`90`** para rutas internas (y `5` para rutas sumarizadas). En `show ip route` se identifica con la letra **`D`** (por el algoritmo **D**UAL).
> * **Comunicación Multicast y Eficiencia de Ancho de Banda:** Se comunica con sus routers vecinos enviando paquetes a la dirección multicast **`224.0.0.10`**. A diferencia de RIP, **NO envía actualizaciones periódicas completas cada 30 segundos**, sino que **solo envía actualizaciones parciales cuando ocurre un cambio** en la red.
> * **Soporte Classless (VLSM y CIDR):** Envía siempre la máscara de subred en sus mensajes.
> * **Cálculo de la Métrica Compuesta:** EIGRP evalúa hasta 4 parámetros de la interfaz:
>   1. **Ancho de Banda (*Bandwidth* — BW)** *(Habilitado por defecto — ¡Importante!)*
>   2. **Retardo (*Delay*)** *(Habilitado por defecto — ¡Importante!)*
>   3. **Confiabilidad (*Reliability*)** *(Opcional)*
>   4. **Carga (*Load*)** *(Opcional)*
> * **Balanceo de Carga Desigual:** Es el **único protocolo IGP capaz de balancear tráfico entre múltiples rutas aunque tengan diferente métrica (costo desigual)** (mediante el comando `variance`).

### Configuración y Verificación de EIGRP en Cisco IOS (Según Diapositiva 38)

```ios
! --- CONFIGURACIÓN DE EIGRP EN EL ROUTER ---
R1(config)# router eigrp 100
R1(config-router)# network 192.168.1.0 0.0.0.255
R1(config-router)# network 10.0.0.0 0.0.0.255
R1(config-router)# no auto-summary
```
* **Explicación línea por línea:**
  * `router eigrp 100`: Inicia el proceso EIGRP para el **Sistema Autónomo (AS) número `100`** (todos los routers vecinos deben tener el **mismo número de AS** para formar adyacencia).
  * `network 192.168.1.0 0.0.0.255`: Anuncia en EIGRP las interfaces pertenecientes a esa red utilizando su **máscara wildcard** (`0.0.0.255`, que es la inversa de `255.255.255.0`).
  * `no auto-summary`: Desactiva la sumarización automática con clase para soportar subredes no contiguas y VLSM sin conflictos.

* **Los 3 Comandos Esenciales de Diagnóstico de EIGRP:**
  1. **`show ip route`**: Muestra la tabla de enrutamiento IPv4 (las rutas aprendidas por EIGRP aparecen con el código **`D`** y distancia administrativa **`90`**).
  2. **`show ip eigrp neighbors`**: Muestra la tabla de vecinos EIGRP directamente conectados con los que se estableció adyacencia.
  3. **`show ip protocols`**: Muestra los protocolos de enrutamiento activos en el router, el número de AS, las redes anunciadas y los valores K de la métrica.

<div style="page-break-after: always;"></div>

---

# Módulo 7 (Subredes y VLSM): Direccionamiento IPv4, Clases, NAT, Subnetting Fijo (FLSM) y Variable (VLSM)

## 7.1. Estructura de una Dirección IPv4, Máscara de Subred, Prefijo y Operación ANDing

> 📌 **CONCEPTO CLAVE — Estructura Jerárquica de 32 bits y Operación ANDing:**
> * Una dirección IPv4 es un número binario jerárquico de **32 bits** dividido en **4 octetos (4 bytes)** de 8 bits cada uno, representados en decimal separados por puntos (ej. `192.168.10.10`).
> * Se compone de dos partes:
>   1. **Porción de Red (*Network Portion*):** Bits de la izquierda que identifican a qué red o subred pertenece el equipo (todos los hosts de la misma LAN comparten exactamente la misma porción de red).
>   2. **Porción de Host (*Host Portion*):** Bits de la derecha que identifican de forma única a un dispositivo específico dentro de esa red.
> * **Máscara de Subred y Longitud de Prefijo (`/n`):** La máscara de subred es otro número de 32 bits que tiene **todos unos (`1`) consecutivos en la porción de red** y **todos ceros (`0`) en la porción de host**. La **longitud de prefijo** (notación *slash*, ej. `/24`) indica simplemente **cuántos bits en `1` tiene la máscara de subred**.

### Cómo determina el Host la Dirección de Red: Operación Lógica AND (*ANDing*)
Para obtener la **Dirección de Red** a partir de la dirección IP de un host, el dispositivo realiza una operación booleana **AND bit a bit** entre la IP del host y la máscara de subred.
* **Regla de la compuerta lógica AND:** El resultado es **`1` únicamente cuando ambos bits de entrada son `1`** (`1 AND 1 = 1`); en cualquier otro caso (`1 AND 0`, `0 AND 1`, `0 AND 0`) el resultado es **`0`**. Por lo tanto, donde la máscara tiene `1` se copia el bit de la IP, y donde la máscara tiene `0` el resultado queda forzado a `0`.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Operación ANDing Paso a Paso:**
> **Datos:** Host con IP `192.168.10.10` y máscara de subred `255.255.255.0` (`/24`).
>
> | Elemento | Notación Decimal | Representación Binaria (32 bits en 4 octetos) |
> | :--- | :---: | :--- |
> | **Dirección IPv4 del Host** | `192.168.10.10` | `11000000 . 10101000 . 00001010 .` **`00001010`** |
> | **AND Máscara de Subred (`/24`)** | `255.255.255.0` | `11111111 . 11111111 . 11111111 .` **`00000000`** |
> | **= Dirección de Red resultante** | **`192.168.10.0`** | `11000000 . 10101000 . 00001010 .` **`00000000`** |

---

## 7.2. Direccionamiento Classful (RFC 790) vs. Classless (CIDR)

Originalmente, el **RFC 790 (1981)** dividió el espacio de direcciones IPv4 en **5 Clases fijas** (*Classful Addressing*):

| Clase | Rango del Primer Octeto (y Rango de Red) | Máscara y Prefijo Natural | Bits Red / Bits Host | % del Espacio Total IPv4 | Cantidad Total de Redes | Cantidad de Hosts Utilizables por Red ($2^h - 2$) |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **Clase A** | `0.0.0.0/8` a `127.0.0.0/8` | `255.0.0.0` (`/8`) | **8 Red** / 24 Host | **50%** | $128$ | **$16.777.214$** ($2^{24}-2$) |
| **Clase B** | `128.0.0.0/16` a `191.255.0.0/16` | `255.255.0.0` (`/16`) | **16 Red** / 16 Host | **25%** | $16.384$ | **$65.534$** ($2^{16}-2$) |
| **Clase C** | `192.0.0.0/24` a `223.255.255.0/24` | `255.255.255.0` (`/24`) | **24 Red** / 8 Host | **12,5%** | $2.097.152$ | **$254$** ($2^8-2$) |
| **Clase D** | `224.0.0.0` a `239.0.0.0` | *(Sin máscara)* | Bloque **Multicast** | **6,25%** *(12,5% D+E)* | — | Reservado para grupos **Multicast**. |
| **Clase E** | `240.0.0.0` a `255.0.0.0` | *(Sin máscara)* | Experimental | **6,25%** | — | Reservado para investigación / futuro. |

* **¿Por qué se abandonó el direccionamiento *Classful*?** Porque **desperdiciaba millones de direcciones IPv4** (una empresa con 300 PCs no cabía en una Clase C de 254 hosts y si recibía una Clase B de $65.534$ hosts desperdiciaba más de $65.200$ direcciones). Fue reemplazado por el direccionamiento **Classless (Sin Clase / CIDR)**, donde la máscara puede tener cualquier longitud de prefijo (`/8` a `/30`) independientemente de la clase histórica.

---

## 7.3. Los 3 Tipos de Direcciones Dentro de Cada Red (Red, Hosts y Broadcast)

Dentro de cualquier red o subred IPv4 existen siempre **tres tipos de direcciones**, razón por la cual la fórmula de hosts utilizables resta siempre **`2`** ($2^h - 2$):

| Tipo de Dirección en la Subred | Regla en los Bits de la Porción de Host | ¿Se puede asignar a una PC o interfaz de Router? | Ejemplo en la red `192.168.10.0/24` |
| :--- | :--- | :---: | :---: |
| **1. Dirección de Red** (*Network Address*) | **Todos los bits de host en `0`** (*All 0s*, ej. `.00000000`). | ❌ **NO** *(Identifica a la red en las tablas de ruteo)*. | **`192.168.10.0`** |
| **2. Direcciones de Host** (*Host Addresses*) | Cualquier combinación entre:<br>• **Primera IP utilizable:** Todos `0` y un `1` al final (`.00000001`).<br>• **Última IP utilizable:** Todos `1` y un `0` al final (`.11111110`). | ✅ **SÍ** *(Se asignan a PCs, servidores, impresoras y al Default Gateway)*. | • Primera: **`192.168.10.1`**<br>• Última: **`192.168.10.254`**<br>*(Total: 254 hosts)* |
| **3. Dirección de Broadcast** (*Broadcast Address*) | **Todos los bits de host en `1`** (*All 1s*, ej. `.11111111`). | ❌ **NO** *(Se usa como destino para hablarle a todos los hosts de esa subred)*. | **`192.168.10.255`** |

---

## 7.4. Direcciones Públicas, Privadas (RFC 1918), Especiales, NAT y Jerarquía IANA/RIR

### Direcciones IPv4 Públicas vs. Privadas (RFC 1918)
* **Direcciones Públicas:** Son únicas en todo el mundo y **son enrutables globalmente** a través de los routers de los proveedores de Internet (**ISP**).
* **Direcciones Privadas (definidas en el RFC 1918):** Son tres bloques gratuitos reservados para usar dentro de las redes internas (**Intranets**) de hogares, empresas y universidades. **NO son únicas** (cualquier organización puede repetirlas adentro de su red) y **NO son enrutables en Internet** (los routers de los ISP descartan cualquier paquete con IP privada).

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — Los 3 Bloques de Direcciones Privadas (RFC 1918):**
>
> | Bloque de Red y Prefijo | Rango Completo de Direcciones Privadas (RFC 1918) | Equivalencia en Clases |
> | :---: | :---: | :--- |
> | **`10.0.0.0 /8`** | **`10.0.0.0` — `10.255.255.255`** | 1 red Clase A completa |
> | **`172.16.0.0 /12`** | **`172.16.0.0` — `172.31.255.255`** | 16 redes Clase B contiguas |
> | **`192.168.0.0 /16`** | **`192.168.0.0` — `192.168.255.255`** | 256 redes Clase C contiguas |

### Traducción de Direcciones de Red: NAT y PAT
Para que un host con IP privada pueda salir a Internet, el **router de borde** de la organización ejecuta **NAT (*Network Address Translation*)**, traduciendo la IP privada de origen por una IP pública global. En cátedra se distinguen **tres variantes de NAT**:

| Variante de NAT | Cómo funciona | Caso de uso típico |
| :--- | :--- | :--- |
| **1. Static NAT** (NAT Estático) | Mapeo fijo **$1\text{ IP privada} \leftrightarrow 1\text{ IP pública}$**. | Publicar un servidor web interno (o en una **DMZ**) para que sea accesible desde afuera en Internet. |
| **2. Dynamic NAT** (NAT Dinámico) | Mapea direcciones privadas hacia un **pool (conjunto) de varias direcciones públicas** disponibles por orden de llegada. | Corporaciones que contrataron un bloque de varias IPs públicas. |
| **3. PAT** (*Port Address Translation* / NAT Overload) | Traduce **muchas IPs privadas simultáneas a UNA SOLA IP pública**, diferenciando cada conexión mediante **números de puerto TCP/UDP**. | **El más común** en todos los hogares y empresas hoy en día. |

### Direcciones IPv4 de Uso Especial y Asignación Global (IANA / RIR)
* **Direcciones de Loopback (`127.0.0.0/8`, de `127.0.0.1` a `127.255.255.254`):** Identificada habitualmente como **`127.0.0.1`**. El host la usa para enviarse paquetes a sí mismo (`ping 127.0.0.1`) y verificar que el protocolo TCP/IP esté operativo en su sistema operativo.
* **Direcciones Link-Local o APIPA (`169.254.0.0/16`, de `169.254.0.1` a `169.254.255.254`):** *Automatic Private IP Addressing*. Cuando una PC con Windows está configurada para obtener IP por **DHCP**, pero **no encuentra ningún servidor DHCP disponible** en la red, se autoasigna una dirección `169.254.x.x` para poder comunicarse al menos en el enlace local.
* **Jerarquía de Asignación de IPs Públicas:** La **IANA** distribuye los grandes bloques de direcciones IP a **5 Registros Regionales de Internet (RIR)** en el mundo, quienes a su vez las asignan a los **ISP**:
  1. **LACNIC:** América Latina y el Caribe *(nuestra región en Argentina)*.
  2. **ARIN:** América del Norte y parte del Caribe.
  3. **RIPE NCC:** Europa, Medio Oriente y Asia Central.
  4. **APNIC:** Asia-Pacífico y Oceanía.
  5. **AfriNIC:** África.

---

## 7.5. Dominios de Broadcast y Razones para Segmentar (Subnetting)

* Un **Dominio de Broadcast** abarca todos los dispositivos que reciben una trama de broadcast enviada por cualquier host dentro de ese segmento.
* **Los switches propagan los broadcasts** por todos sus puertos (salvo el de entrada). **El único dispositivo que detiene (no propaga) los broadcasts es un Router**: cada interfaz de un router define un dominio de broadcast separado.
* Si un dominio de broadcast es demasiado grande (por ejemplo, una red `172.16.0.0/16` con 400 usuarios conectados mediante switches), el tráfico constante de broadcasts (ARP, DHCP Discover) degrada el rendimiento de todos los equipos.
* **Solución — Subnetting (División en subredes):** Dividir una red grande en varias subredes más pequeñas separadas por un router (por ejemplo, `LAN 1: 172.16.0.0/24` y `LAN 2: 172.16.1.0/24` de 200 usuarios cada una).
* **Beneficios y Criterios de Segmentación:** Reduce el tráfico general de broadcast, mejora el rendimiento y permite aplicar **políticas de seguridad** (ACL/Firewalls) entre subredes, agrupando por:
  1. **Ubicación (*Location*):** Una subred para cada piso de un edificio (`Piso 1: 10.0.1.0/24`, `Piso 2: 10.0.2.0/24`, etc.).
  2. **Grupo o Función (*Group or Function*):** Una subred para Administración (`10.0.1.0/24`), Alumnos (`10.0.2.0/24`), Recursos Humanos (`10.0.3.0/24`), Contabilidad (`10.0.4.0/24`).
  3. **Tipo de Dispositivo (*Device Type*):** Separar una subred para Hosts (`10.0.1.0/24`), otra para Impresoras (`10.0.2.0/24`) y otra para Servidores (`10.0.3.0/24`), o separar la **Intranet** (con IPs privadas) de la **DMZ — Zona Desmilitarizada** (donde se alojan los servidores expuestos a Internet con IPs públicas).

---

## 7.6. Cálculo de Subredes de Longitud Fija (FLSM / Subnetting Tradicional)

El **Subnetting** consiste en **"pedir prestados" bits de la porción de host** (de izquierda a derecha) y convertirlos en bits de red (`1` en la máscara) para crear múltiples subredes.

> 🧮 **FÓRMULA — Las 3 Ecuaciones Maestras del Subnetting:**
> Si pedimos prestados **$n$ bits** para subred y nos quedan **$h$ bits** libres en cero para host:
> 1. **Cantidad de Subredes creadas:**
>    $$\text{Nro. de Subredes} = 2^n$$
> 2. **Cantidad de Hosts utilizables por cada subred:**
>    $$\text{Nro. de Hosts utilizables} = 2^h - 2$$
>    *(Se restan siempre 2 direcciones: la **Dirección de Subred** y la **Dirección de Broadcast**).*
> 3. **Salto entre subredes (*Block Size* o incremento en el octeto donde cae el corte):**
>    $$\text{Salto} = 256 - \text{Valor decimal del octeto de la nueva máscara}$$

### Tabla Maestra 1: División de una Red `/24` en el 4.º Octeto (De `/25` a `/30`)

| Prefijo | Máscara de Subred Decimal | Último Octeto en Binario (`n`=subred, `h`=host) | Bits prestados ($n$) / Bits host ($h$) | # de Subredes ($2^n$) | # de Hosts Utilizables ($2^h - 2$) | Salto entre Subredes ($256 - \text{Másc}$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`/24`** | `255.255.255.0` | `hhhhhhhh` (`00000000`) | $n=0$ / $h=8$ | $1$ | **$254`** | $256$ |
| **`/25`** | `255.255.255.128` | `nhhhhhhh` (`10000000`) | $n=1$ / $h=7$ | **$2`** | **$126`** | $128$ (`.0`, `.128`) |
| **`/26`** | `255.255.255.192` | `nnhhhhhh` (`11000000`) | $n=2$ / $h=6$ | **$4`** | **$62`** | $64$ (`.0`, `.64`, `.128`, `.192`) |
| **`/27`** | `255.255.255.224` | `nnnhhhhh` (`11100000`) | $n=3$ / $h=5$ | **$8`** | **$30`** | $32$ (`.0`, `.32`, `.64`, `.96`...) |
| **`/28`** | `255.255.255.240` | `nnnnhhhh` (`11110000`) | $n=4$ / $h=4$ | **$16`** | **$14`** | $16$ (`.0`, `.16`, `.32`, `.48`...) |
| **`/29`** | `255.255.255.248` | `nnnnnhhh` (`11111000`) | $n=5$ / $h=3$ | **$32`** | **$6`** | $8$ (`.0`, `.8`, `.16`, `.24`...) |
| **`/30`** | `255.255.255.252` | `nnnnnnhh` (`11111100`) | $n=6$ / $h=2$ | **$64`** | **$2`** *(Ideal enlaces WAN)* | $4$ (`.0`, `.4`, `.8`, `.12`...) |

### Tabla Maestra 2: División de una Red `/16` (De `/17` a `/30` — Diapositiva 21)

| Prefijo | Máscara de Subred | Bits prestados ($n$) de `/16` |Bits de Host ($h$) | # de Subredes ($2^n$) | # de Hosts por Subred ($2^h - 2$) |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **`/17`** | `255.255.128.0` | $1$ | $15$ | $2$ | $32.766$ |
| **`/18`** | `255.255.192.0` | $2$ | $14$ | $4$ | $16.382$ |
| **`/19`** | `255.255.224.0` | $3$ | $13$ | $8$ | $8.190$ |
| **`/20`** | `255.255.240.0` | $4$ | $12$ | $16$ | $4.094$ |
| **`/21`** | `255.255.248.0` | $5$ | $11$ | $32$ | $2.046$ |
| **`/22`** | `255.255.252.0` | $6$ | $10$ | $64$ | $1.022$ |
| **`/23`** | `255.255.254.0` | $7$ | $9$ | $128$ | $510$ |
| **`/24`** | `255.255.255.0` | $8$ | $8$ | $256$ | $254$ |
| **`/25` a `/30`** | `.128` a `.252` | $9\text{ a }14$ | $7\text{ a }2$ | $512\text{ a }16.384$ | $126\text{ a }2$ |

---

## 7.7. VLSM (*Variable Length Subnet Masking* — Máscara de Subred de Longitud Variable)

En el subnetting tradicional (FLSM), todas las subredes tienen exactamente el mismo tamaño (la misma máscara). Si tenemos una LAN de 100 usuarios y un enlace punto a punto entre dos routers que solo necesita **2 direcciones IP**, darle una subred de 126 hosts (`/25`) al enlace entre routers **desperdicia 124 direcciones**.

> 📌 **CONCEPTO CLAVE — ¿Qué es VLSM y cuál es su Regla de Oro?**
> * **VLSM** es la técnica de **subnetear una subred**, aplicando **máscaras de distinta longitud** dentro del mismo bloque de direccionamiento para adaptar cada subred exactamente a la cantidad de hosts que necesita cada sector.
> * ⚠️ **REGLA DE ORO DE VLSM:** Siempre se debe **ordenar y comenzar asignando desde la subred más grande (mayor cantidad de hosts requeridos) hasta la subred más pequeña** (dejando los enlaces WAN punto a punto `/30` de 2 hosts para el final). De esta forma nunca se solapan los rangos de direcciones.

> 💡 **EJEMPLO PRÁCTICO PASO A PASO — Resolución Completa de un Ejercicio de Examen con VLSM:**
> **Enunciado:** Se dispone del bloque de red **`192.168.1.0/24`** y se pide diseñarlo mediante **VLSM** para cubrir:
> * **LAN A (Alumnos):** Requiere **50 hosts**.
> * **LAN B (Docentes):** Requiere **25 hosts**.
> * **LAN C (Administración):** Requiere **10 hosts**.
> * **Enlace WAN R1–R2:** Enlace punto a punto entre dos routers (requiere **2 hosts**).
>
> **Paso 1: Ordenar de MAYOR a MENOR necesidad de hosts y elegir el prefijo/máscara para cada una:**
> 1. **LAN A (50 hosts):** Buscamos $2^h - 2 \ge 50 \implies h = 6\text{ bits}$ ($2^6 - 2 = \mathbf{62\text{ hosts}}$). Como de los 32 bits dejamos 6 para host, el prefijo es $32 - 6 = \mathbf{/26}$ (Máscara `255.255.255.192`, Salto = $256 - 192 = \mathbf{64}$).
> 2. **LAN B (25 hosts):** Buscamos $2^h - 2 \ge 25 \implies h = 5\text{ bits}$ ($2^5 - 2 = \mathbf{30\text{ hosts}}$). Prefijo: $32 - 5 = \mathbf{/27}$ (Máscara `255.255.255.224`, Salto = $256 - 224 = \mathbf{32}$).
> 3. **LAN C (10 hosts):** Buscamos $2^h - 2 \ge 10 \implies h = 4\text{ bits}$ ($2^4 - 2 = \mathbf{14\text{ hosts}}$). Prefijo: $32 - 4 = \mathbf{/28}$ (Máscara `255.255.255.240`, Salto = $256 - 240 = \mathbf{16}$).
> 4. **Enlace WAN R1–R2 (2 hosts):** Buscamos $2^h - 2 \ge 2 \implies h = 2\text{ bits}$ ($2^2 - 2 = \mathbf{2\text{ hosts}}$). Prefijo: $32 - 2 = \mathbf{/30}$ (Máscara `255.255.255.252`, Salto = $256 - 252 = \mathbf{4}$).
>
> **Paso 2: Tabla de Asignación VLSM Contigua (sin solapamientos):**
>
> | Subred | Hosts Pedidos / Disponibles | Prefijo y Máscara | Dirección de Subred | Primera IP Utilizable (ej. DGW) | Última IP Utilizable | Dirección de Broadcast |
> | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
> | **1. LAN A** *(Salto 64)* | $50$ / **$62$** | **`/26`** (`255.255.255.192`) | **`192.168.1.0`** | `192.168.1.1` | `192.168.1.62` | `192.168.1.63` |
> | **2. LAN B** *(Salto 32)* | $25$ / **$30$** | **`/27`** (`255.255.255.224`) | **`192.168.1.64`** | `192.168.1.65` | `192.168.1.94` | `192.168.1.95` |
> | **3. LAN C** *(Salto 16)* | $10$ / **$14$** | **`/28`** (`255.255.255.240`) | **`192.168.1.96`** | `192.168.1.97` | `192.168.1.110` | `192.168.1.111` |
> | **4. WAN R1–R2** *(Salto 4)* | $2$ / **$2$** | **`/30`** (`255.255.255.252`) | **`192.168.1.112`** | `192.168.1.113` | `192.168.1.114` | `192.168.1.115` |
>
> *(Observá cómo cada subred arranca exactamente en el número siguiente al Broadcast de la subred anterior: termina en `.63` $\rightarrow$ arranca en `.64`; termina en `.95` $\rightarrow$ arranca en `.96`; termina en `.111` $\rightarrow$ arranca en `.112`, dejando libre desde `192.168.1.116` hasta `.255` para futuras expansiones).*

---

> 🛠️ **LABORATORIO EN PACKET TRACER #4 (Capa 3 y VLSM): Implementación de VLSM y Enrutamiento con EIGRP**
>
> **Objetivo:** Construir en Packet Tracer la red recién calculada con **VLSM** y comunicar todas las subredes configurando el protocolo de enrutamiento dinámico **EIGRP** (classless).
>
> **Paso a paso en Cisco Packet Tracer:**
> 1. Colocá **2 Routers `1941`** (`R1` y `R2`), **3 Switches `2960`** (`S1`, `S2`, `S3`) y **3 PCs** (`PC-A`, `PC-B`, `PC-C`).
> 2. Conectá:
>    * `PC-A` $\rightarrow$ `S1` $\rightarrow$ `R1 (Gig0/0)` (**LAN A**: `192.168.1.0/26`)
>    * `PC-B` $\rightarrow$ `S2` $\rightarrow$ `R1 (Gig0/1)` (**LAN B**: `192.168.1.64/27`)
>    * `PC-C` $\rightarrow$ `S3` $\rightarrow$ `R2 (Gig0/0)` (**LAN C**: `192.168.1.96/28`)
>    * `R1 (Serial0/0/0)` $\leftrightarrow$ `R2 (Serial0/0/0)` (**Enlace WAN**: `192.168.1.112/30`)
> 3. Configurá las interfaces y el enrutamiento **EIGRP AS 100** en **`R1`**:
>    ```ios
>    enable
>    configure terminal
>    hostname R1
>    interface GigabitEthernet0/0
>     ip address 192.168.1.1 255.255.255.192
>     no shutdown
>    interface GigabitEthernet0/1
>     ip address 192.168.1.65 255.255.255.224
>     no shutdown
>    interface Serial0/0/0
>     ip address 192.168.1.113 255.255.255.252
>     clock rate 64000
>     no shutdown
>    router eigrp 100
>     network 192.168.1.0 0.0.0.255
>     no auto-summary
>    end
>    ```
> 4. Configurá las interfaces y **EIGRP AS 100** en **`R2`**:
>    ```ios
>    enable
>    configure terminal
>    hostname R2
>    interface GigabitEthernet0/0
>     ip address 192.168.1.97 255.255.255.240
>     no shutdown
>    interface Serial0/0/0
>     ip address 192.168.1.114 255.255.255.252
>     no shutdown
>    router eigrp 100
>     network 192.168.1.0 0.0.0.255
>     no auto-summary
>    end
>    ```
> 5. Configurá las PCs con la última IP utilizable de cada subred y su respectivo Default Gateway:
>    * **`PC-A`:** IP `192.168.1.62`, Máscara `255.255.255.192`, Gateway `192.168.1.1`
>    * **`PC-B`:** IP `192.168.1.94`, Máscara `255.255.255.224`, Gateway `192.168.1.65`
>    * **`PC-C`:** IP `192.168.1.110`, Máscara `255.255.255.240`, Gateway `192.168.1.97`
> 6. Verificá en `R1` ejecutando `show ip route` y `show ip eigrp neighbors`: verás la ruta `D 192.168.1.96/28 [90/...]` aprendida con su máscara exacta `/28` gracias a que EIGRP es *classless*, y comprobá conectividad con `ping 192.168.1.110` desde `PC-A`.

<div style="page-break-after: always;"></div>

---

# Módulo 8 (Capa 4): Capa de Transporte, TCP vs. UDP, Puertos, Sockets, Handshake y Control de Flujo

## 8.1. Rol y Responsabilidades de la Capa de Transporte

Mientras que la Capa de Red (IP) se ocupa de llevar paquetes entre un **host de origen y un host de destino**, la **Capa de Transporte (Capa 4 del modelo OSI)** es la responsable de establecer **comunicaciones lógicas directamente entre los procesos de aplicación** que se ejecutan dentro de esos hosts, actuando como el puente entre la Capa de Aplicación y las capas inferiores.

> 📌 **CONCEPTO CLAVE — Las 5 Responsabilidades de la Capa de Transporte:**
> 1. **Seguimiento de conversaciones individuales:** Rastrea cada comunicación activa entre aplicaciones de origen y destino.
> 2. **Segmentación de datos y reensamblaje de segmentos:** Divide los grandes bloques de datos de aplicación en piezas manejables (**Segmentos** en TCP o **Datagramas** en UDP) en el origen, y los vuelve a unir en el destino.
> 3. **Agregar información de encabezado (Capa 4):** Añade los números de puerto de origen/destino y datos de control.
> 4. **Identificar, separar y administrar múltiples conversaciones:** Mediante números de puerto evita que los datos de una página web se mezclen con los de un correo electrónico o un chat de video.
> 5. **Multiplexación de conversaciones (*Conversation Multiplexing*):** Gracias a la segmentación, permite que múltiples aplicaciones (Email, Mensajería Instantánea, múltiples pestañas Web, Video Chat y Streaming) **entremezclen sus segmentos y utilicen el mismo enlace físico de red al mismo tiempo**.

---

## 8.2. Los Protocolos de la Capa de Transporte: Comparativa Maestra TCP vs. UDP

Como el protocolo **IP** de Capa 3 es *Best Effort* y no garantiza entrega, la Capa 4 ofrece dos protocolos con filosofías opuestas según lo que necesite cada aplicación de Capa 7:

| Criterio Técnico | **TCP (*Transmission Control Protocol*)** | **UDP (*User Datagram Protocol*)** |
| :--- | :--- | :--- |
| **Nombre de la PDU** | **Segmento (*Segment*)** | **Datagrama (*Datagram*)** |
| **Orientación a la Conexión** | **Orientado a la conexión (*Connection-oriented* / *Stateful*):** Establece una sesión previa (*Three-Way Handshake*) antes de enviar datos. | **Sin conexión (*Connectionless* / *Stateless*):** Envía datagramas de inmediato sin establecer sesión previa. |
| **Confiabilidad y Entrega** | **Entrega Confiable (*Reliable*):** Usa acuses de recibo (**ACK**) y **retransmite** cualquier segmento perdido o dañado. | **Mejor Esfuerzo (*Best-Effort* / No confiable):** **NO envía ACKs** y **NO retransmite** datos perdidos. |
| **Orden de Entrega (Secuencia)** | **Entrega en el mismo orden:** Numera cada byte (*Sequence Number*) y **reordena** los segmentos si llegan por caminos distintos. | **Sin reordenamiento:** Reconstruye y entrega los datos a la aplicación **en el mismo orden en que van llegando**. |
| **Control de Flujo y Congestión** | **SÍ soporta control de flujo:** Ajusta la velocidad de envío según la ventana (*Window*) del receptor para no saturar sus recursos. | **NO tiene control de flujo:** El emisor no es informado sobre la disponibilidad de recursos del receptor. |
| **Sobrecarga (*Overhead*) y Velocidad** | **Mayor sobrecarga** (Encabezado de **20 Bytes** base) y mayor retardo por confirmaciones. | **Muy baja sobrecarga (*Low overhead*) y muy rápido** (Encabezado de solo **8 Bytes**). |
| **Aplicaciones Típicas (Según Diapositivas 5, 8, 11 y 14)** | • **HTTP / HTTPS** (Web)<br>• **FTP** (Transferencia de archivos)<br>• **SMTP / POP3 / IMAP** (Email)<br>• **SSH / Telnet** (Consola remota)<br>• **DNS** *(para transferencias de zona o respuestas $>512\text{ bytes}$)* | 1. **Video en vivo y multimedia** (toleran pérdidas pero no demoras): **VoIP**, **Video Conferencing / Streaming**.<br>2. **Solicitud y respuesta simples:** **DNS** (consultas), **DHCP**.<br>3. **Confiabilidad propia en Capa 7:** **TFTP**, **SNMP**, **RADIUS**. |

---

## 8.3. Estructura de los Encabezados TCP (20 Bytes) y UDP (8 Bytes)

### 1️⃣ Encabezado del Segmento TCP (20 Bytes = 160 bits base)

```text
ESTRUCTURA DEL ENCABEZADO TCP (20 Bytes base = 5 filas de 32 bits):
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port (16)     |       Destination Port (16)   |  <- Fila 1 (Bytes 1-4)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number (32)                   |  <- Fila 2 (Bytes 5-8)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgement Number (32)                |  <- Fila 3 (Bytes 9-12)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Header |           |U|A|P|R|S|F|                             |
| Length  | Reserved  |R|C|S|S|Y|I|            Window           |  <- Fila 4 (Bytes 13-16)
|   (4)   |    (6)    |G|K|H|T|N|N|             (16)            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum (16)       |         Urgent Pointer (16)   |  <- Fila 5 (Bytes 17-20)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (0 or 32 bits if any)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                Application Layer Data (Size Varies)           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

> ⚠️ **ATENCIÓN / PREGUNTA DE EXAMEN — Las 6 Banderas de Control (*Control Bits*, 6 bits) de TCP:**
> Ubicadas en la 4.ª fila del encabezado TCP, estas 6 banderas de 1 bit gobiernan el estado de la conexión:
> 1. **`URG` (*Urgent*):** Indica que el campo *Urgent Pointer* (puntero urgente) es significativo.
> 2. **`ACK` (*Acknowledgment*):** Bandera de reconocimiento; indica que el campo *Acknowledgement Number* contiene un número válido (usada en el *handshake*, en la transferencia de datos y en el cierre de sesión).
> 3. **`PSH` (*Push*):** Función de empuje para que los datos se entreguen de inmediato a la aplicación sin esperar a llenar el búfer.
> 4. **`RST` (*Reset*):** Reinicia o aborta abruptamente la conexión cuando ocurre un error grave, rechazo de puerto o *timeout*.
> 5. **`SYN` (*Synchronize*):** Sincroniza los números de secuencia iniciales durante el establecimiento de una conexión (*Three-Way Handshake*).
> 6. **`FIN` (*Finish*):** Indica que el emisor no tiene más datos para enviar; se utiliza para terminar ordenadamente la sesión.

---

### 2️⃣ Encabezado del Datagrama UDP (8 Bytes = 64 bits fijos)
El encabezado UDP es sumamente liviano porque solo posee **4 campos de 16 bits (2 bytes) cada uno**:

```text
ESTRUCTURA DEL ENCABEZADO UDP (8 Bytes = 2 filas de 32 bits):
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port (16)     |       Destination Port (16)   |  <- Fila 1 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length (16)        |          Checksum (16)        |  <- Fila 2 (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                Application Layer Data (Size Varies)           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

## 8.4. Números de Puerto, Pares de Sockets y el Comando `netstat`

### ¿Qué es un Socket y un Par de Sockets?
> 📌 **CONCEPTO CLAVE — Definición de Socket:**
> Un **Socket** es la combinación de una **dirección IP y un número de puerto** separados por dos puntos (`IP:Puerto`).
> * Cada conversación a través de la red queda identificada de forma única por un **Par de Sockets** (Socket de Origen + Socket de Destino).
> * **Regla de los Puertos Cliente/Servidor:**
>   * El **Puerto de Origen (*Source Port*)** en una petición lo elige **dinámicamente** el host cliente (de entre los puertos altos/dinámicos) para identificar su propio proceso local.
>   * El **Puerto de Destino (*Destination Port*)** en una petición es el **puerto bien conocido** en el que está escuchando el servicio en el servidor (ej. `80` para HTTP o `21` para FTP).
>   * ⚠️ **Cuando el servidor responde al cliente, los puertos se invierten:** el puerto bien conocido del servidor pasa a ser el **Puerto de Origen** y el puerto dinámico del cliente pasa a ser el **Puerto de Destino**.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Pares de Sockets Simultáneos (Diapositiva 16):**
> Un mismo **Cliente** (`192.168.1.5`, MAC `00-07-E9-63-CE-53`) abre al mismo tiempo un cliente **FTP** (puerto origen `1305`) y un navegador **Web** (puerto origen `1099`) hacia un mismo **Servidor** (`192.168.1.7`, MAC `00-07-E9-42-AC-28`):
> * **Conexión FTP:**
>   * Socket Origen: `192.168.1.5:1305` $\longrightarrow$ Socket Destino: `192.168.1.7:21`
> * **Conexión Web (HTTP):**
>   * Socket Origen: `192.168.1.5:1099` $\longrightarrow$ Socket Destino: `192.168.1.7:80`
> *(Así, aunque ambas tramas tienen exactamente las mismas direcciones MAC y las mismas direcciones IP, la Capa 4 las separa sin confusión gracias a los números de puerto).*

---

### Los 3 Rangos de Números de Puerto (Clasificación IANA de 16 bits: `0` a `65.535`)

| Grupo de Puertos (IANA) | Rango Numérico | Propósito y Uso |
| :--- | :---: | :--- |
| **1. Puertos Bien Conocidos** (*Well-Known Ports*) | **`0` a `1.023`** | Reservados por IANA para servicios y protocolos estándar de Internet (HTTP, SSH, FTP, SMTP, DNS, DHCP). Se usan como **puertos de destino** en solicitudes a servidores. |
| **2. Puertos Registrados** (*Registered Ports*) | **`1.024` a `49.151`** | Asignados por IANA a una entidad que los solicita para aplicaciones específicas de usuario o servidores propietarios (ej. RADIUS en puerto `1812`, bases de datos, mensajería). |
| **3. Puertos Privados y/o Dinámicos** (*Private and/or Dynamic Ports*) | **`49.152` a `65.535`** | Conocidos como *puertos efímeros*. El sistema operativo del **cliente los asigna dinámicamente como Puerto de Origen** cada vez que inicia una conexión (ej. `49152`, `51152`). |

### Tabla Oficial de Puertos Bien Conocidos de la Cátedra (Diapositiva 18)

| Número de Puerto | Protocolo Capa 4 | Aplicación / Servicio |
| :---: | :---: | :--- |
| **20** | **TCP** | **FTP (*File Transfer Protocol*) — Datos** (Transferencia de archivos) |
| **21** | **TCP** | **FTP (*File Transfer Protocol*) — Control** (Comandos de sesión) |
| **22** | **TCP** | **SSH (*Secure Shell*)** (Administración remota cifrada y segura) |
| **23** | **TCP** | **Telnet** (Administración remota en texto plano — *no segura*) |
| **25** | **TCP** | **SMTP (*Simple Mail Transfer Protocol*)** (Envío/transferencia de correos electrónicos) |
| **53** | **UDP y TCP** | **DNS (*Domain Name Service*)** (Resolución de nombres de dominio a IP) |
| **67** | **UDP** | **DHCP (*Dynamic Host Configuration Protocol*) — Servidor** |
| **68** | **UDP** | **DHCP (*Dynamic Host Configuration Protocol*) — Cliente** |
| **69** | **UDP** | **TFTP (*Trivial File Transfer Protocol*)** (Transferencia simple sin conexión) |
| **80** | **TCP** | **HTTP (*Hypertext Transfer Protocol*)** (Páginas web sin cifrar) |
| **110** | **TCP** | **POP3 (*Post Office Protocol version 3*)** (Descarga de correo electrónico) |
| **143** | **TCP** | **IMAP (*Internet Message Access Protocol*)** (Acceso a correo electrónico) |
| **161** | **UDP** | **SNMP (*Simple Network Management Protocol*)** (Monitoreo y gestión de red) |
| **443** | **TCP** | **HTTPS (*Hypertext Transfer Protocol Secure*)** (Páginas web cifradas con TLS/SSL) |

### Verificación de Conexiones Activas con `netstat`
Dado que las conexiones TCP abiertas no explicadas pueden indicar la presencia de *malware* o intrusiones de seguridad, el comando **`netstat`** ejecutado en la consola del sistema operativo permite auditar todas las sesiones activas mostrando:
* **`Proto`:** Protocolo de transporte (`TCP` o `UDP`).
* **`Local Address`:** IP local y puerto de origen (ej. `192.168.1.124:3158`).
* **`Foreign Address`:** IP o nombre remoto y puerto de destino (ej. `www.cisco.com:http`).
* **`State`:** Estado de la sesión TCP (ej. `ESTABLISHED`, `LISTENING`, `TIME_WAIT`, `CLOSE_WAIT`).

---

## 8.5. Gestión de Sesiones TCP: Establecimiento (*Three-Way Handshake*) y Cierre (4 Pasos)

### 1️⃣ Establecimiento de Conexión TCP en 3 Pasos (*Three-Way Handshake*)
Antes de enviar un solo byte de datos de aplicación, TCP establece la sesión entre el **Host A (Cliente)** y el **Host B (Servidor)** cumpliendo tres objetivos: verificar que el destino está presente en la red, comprobar que tiene el puerto de servicio abierto y **sincronizar los números de secuencia iniciales (ISN)** en ambas direcciones.

```text
PROCESO DE ESTABLECIMIENTO DE SESIÓN TCP (THREE-WAY HANDSHAKE):

  Host A (Cliente)                                        Host B (Servidor)
  ----------------                                        -----------------
         |                                                        |
         |--- Paso 1: Send SYN (SEQ = 100, CTL = SYN) ----------->|  (1. Solicita sesión A -> B)
         |                                                        |  SYN received
         |                                                        |
         |<-- Paso 2: Send SYN, ACK (SEQ=300, ACK=101, CTL=SYN,ACK)|  (2. Confirma A->B con ACK=100+1
  SYN,ACK|                                                        |      y solicita sesión B->A con SEQ=300)
 received|                                                        |
         |--- Paso 3: Send ACK (SEQ = 101, ACK = 301, CTL = ACK)->|  (3. Confirma B->A con ACK=300+1)
         |                                                        |
  ================== [ SESIÓN TCP ESTABLISHED ] ==================
```

---

### 2️⃣ Terminación / Fin de Sesión TCP en 4 Pasos
Como una conexión TCP es *full-duplex* (consta de dos flujos independientes: de Cliente a Servidor y de Servidor a Cliente), para cerrarla por completo se requiere un **intercambio de 4 segmentos (dos pares `FIN` + `ACK`)**:

```text
PROCESO DE CIERRE DE SESIÓN TCP (4 PASOS):

  Host A (Cliente)                                        Host B (Servidor)
  ----------------                                        -----------------
         |                                                        |
         |--- Paso 1: Send FIN (CTL = FIN) ---------------------->|  FIN received (A ya no enviará más datos)
         |                                                        |
         |<-- Paso 2: Send ACK (CTL = ACK) -----------------------|  (B confirma el cierre del sentido A -> B)
     ACK |                                                        |
 received|<-- Paso 3: Send FIN (CTL = FIN) -----------------------|  (B termina de enviar y pide cerrar B -> A)
     FIN |                                                        |
 received|--- Paso 4: Send ACK (CTL = ACK) ---------------------->|  ACK received (A confirma; sesión cerrada)
         |                                                        |
```

---

## 8.6. Confiabilidad, Control de Flujo, Ventana Deslizante y MSS en TCP

### 1️⃣ Retransmisión ante Pérdida de Datos: Modo Clásico vs. SACK (*Selective Acknowledgment*)
Cuando viajan múltiples segmentos (por ejemplo, `Segmento 1` al `Segmento 10`) y en el camino se pierden el **`Segmento 3` y el `Segmento 4`**, pero los segmentos **`1, 2` y `5 al 10`** sí llegan al receptor:

* **Comportamiento Clásico (Sin SACK):**
  * El receptor (Host B) responde con un acuse acumulativo **`ACK 3`** (porque el `3` es el siguiente segmento consecutivo que le falta).
  * Como el emisor (Host A) no sabe si los segmentos `5` al `10` llegaron o también se perdieron, se ve obligado a **retransmitir todo desde el `Segmento 3` hasta el `Segmento 10`**, generando **segmentos duplicados** innecesarios del `5` al `10`. Luego B confirma con `ACK 11`.
* **Comportamiento Moderno con SACK (*Selective Acknowledgment*):**
  * Negociado durante el *Three-Way Handshake*, **SACK** permite al receptor informar huecos discontinuos: Host B responde **`ACK 3, SACK 5-10`** (*"Me falta desde el 3, pero ya recibí bien del 5 al 10"*).
  * Así, Host A **retransmite ÚNICAMENTE el `Segmento 3` y el `Segmento 4`** y continúa enviando segmentos nuevos (`11` y `12`), ahorrando ancho de banda. Finalmente Host B confirma todo con **`ACK 13`**.

---

### 2️⃣ Cálculo del Tamaño Máximo de Segmento (MSS — *Maximum Segment Size*)

> 🧮 **FÓRMULA — Relación entre MTU de Ethernet (Capa 2) y MSS de TCP (Capa 4):**
> * La **MTU (*Maximum Transmission Unit*)** de Ethernet por defecto es de **$1.500\text{ bytes}$** (es el tamaño máximo del paquete IP completo que cabe en el campo Datos de una trama Ethernet).
> * El **MSS (*Maximum Segment Size*)** es la **cantidad máxima de datos puros de aplicación (*Payload*)** que puede llevar un único segmento TCP, **sin contar las cabeceras IP ni TCP**.
> * Como la cabecera IPv4 estándar mide **$20\text{ bytes}$** y la cabecera TCP estándar mide **$20\text{ bytes}$**:
>   $$\text{MSS} = \text{MTU}_{\text{Ethernet}} - \text{Cabecera IPv4} - \text{Cabecera TCP}$$
>   $$\text{MSS} = 1.500\text{ bytes} - 20\text{ bytes} - 20\text{ bytes} = \mathbf{1.460\text{ bytes}}$$

```text
Desglose de Bytes dentro de la Trama Ethernet (MTU vs. MSS):
+-------------+------------+------------+------------------------------+-----------+
| Encab. Eth  | Cabec. IPv4| Cabec. TCP |   Payload (Datos de Aplic.)  |  FCS Eth  |
|  (14 Bytes) | (20 Bytes) | (20 Bytes) |        (1.460 Bytes)         | (4 Bytes) |
+-------------+------------+------------+------------------------------+-----------+
                                        |<------ TCP MSS = 1460 B ---->|
              |<---------------- IP / Ethernet MTU = 1500 B ---------->|
|<------------------------ Trama Ethernet = 1518 Bytes --------------------------->|
```

---

### 3️⃣ Control de Flujo mediante Ventana Deslizante (*Sliding Window*) y Control de Congestión

El campo **`Window` (Tamaño de la Ventana)** en la cabecera TCP indica **cuántos bytes puede enviar el emisor de corrido antes de detenerse a esperar un acuse de recibo (`ACK`)** del receptor.

> 💡 **EJEMPLO PRÁCTICO DE CÁTEDRA — Seguimiento Numérico de la Ventana Deslizante (Diapositiva 27):**
> **Parámetros iniciales:** Durante el *handshake*, el receptor **PC B** le informa al emisor **PC A** que su **`Window size = 10.000 bytes`** y el **`MSS = 1.460 bytes`**.
>
> 1. **Envío 1 (A $\rightarrow$ B):** `PC A` envía un primer segmento con **`SEQ = 1`** y $1.460\text{ bytes}$ de datos (`Bytes 1 al 1.460`).
> 2. **Envío 2 (A $\rightarrow$ B):** Sin esperar confirmación todavía (porque su ventana es de $10.000\text{ bytes}$), `PC A` envía enseguida un segundo segmento con **`SEQ = 1.461`** y otros $1.460\text{ bytes}$ (`Bytes 1.461 al 2.920`).
> 3. **Acuse Acumulativo 1 (B $\rightarrow$ A):** `PC B` recibe ambos segmentos y envía un único acuse acumulativo **`ACK = 2.921`** (*"Recibí todo bien hasta el byte $2.920$, espero el byte $2.921$"*) manteniendo `Window size = 10.000`.
>    * **Deslizamiento de la ventana en A:** Al recibir el `ACK 2.921`, la ventana de envío de `PC A` se desliza hacia adelante permitiendo enviar hasta el byte $2.920 + 10.000 = \mathbf{12.920}$.
> 4. **Envío 3 (A $\rightarrow$ B):** `PC A` envía el tercer segmento con **`SEQ = 2.921`** y $1.460\text{ bytes}$ (`Bytes 2.921 al 4.380`).
> 5. **Acuse Acumulativo 2 (B $\rightarrow$ A):** `PC B` confirma con **`ACK = 4.381`** y `Window size = 10.000`, deslizando la ventana de `PC A` hasta el byte $4.380 + 10.000 = \mathbf{14.380}$.
>
> **¿Qué ocurre si hay Congestión en la Red?**  
> Si un router intermedio se sobrecarga y empieza a descartar paquetes, `PC A` deja de recibir los `ACK` esperados a tiempo. Entonces, los algoritmos de **prevención de congestión de TCP** hacen que `PC A` **reduzca automáticamente la cantidad de bytes que envía antes de recibir un ACK** (achicando su ventana de congestión) y retransmita los segmentos perdidos para aliviar al router sobrecargado.

---

> 🛠️ **LABORATORIO EN PACKET TRACER #5 (Capa de Transporte): Comparación de Tráfico TCP (HTTP/FTP) vs. UDP (DNS/TFTP)**
>
> **Objetivo:** Capturar y comparar en el Modo Simulación de Packet Tracer el establecimiento de sesión **TCP Three-Way Handshake** en una consulta Web frente al envío directo sin conexión de **UDP** en una consulta DNS.
>
> **Paso a paso en Cisco Packet Tracer:**
> 1. Colocá **1 PC (`PC-Cliente`)**, **1 Switch `2960`** y **1 Servidor (`Server-Multiservicio`)** conectados con cable directo.
> 2. Configurá las IPs:
>    * **`Server-Multiservicio`:** IP `192.168.1.100`, Máscara `255.255.255.0`. En la pestaña **Services** $\rightarrow$ **DNS**, encendé el servicio DNS (`On`) y agregá un registro tipo `A Record` con Nombre `www.fadena.edu.ar` y Address `192.168.1.100` (clic en *Add*). Verificá que el servicio **HTTP** también esté en `On`.
>    * **`PC-Cliente`:** IP `192.168.1.10`, Máscara `255.255.255.0`, **DNS Server: `192.168.1.100`**.
> 3. Activá el **Modo Simulación**, hacé clic en **Edit Filters** y dejá tildados únicamente **`DNS`**, **`HTTP`** y **`TCP`**.
> 4. En `PC-Cliente`, abrí el **Web Browser**, escribí `http://www.fadena.edu.ar` y presioná **Go**.
> 5. Presioná **Capture / Forward** paso a paso y observá la diferencia exacta entre UDP y TCP:
>    * **Fase 1 (DNS sobre UDP — Sin Handshake):** Verás que `PC-Cliente` envía **directamente** un único paquete **DNS** usando **UDP** (Puerto Origen dinámico ej. `1025` $\rightarrow$ **Puerto Destino `53`**) sin pedir permiso ni hacer handshake previo. El servidor le responde inmediatamente desde el **Puerto Origen `53`** hacia el `1025` con la IP `192.168.1.100`.
>    * **Fase 2 (Three-Way Handshake de TCP antes de HTTP):** Recién cuando `PC-Cliente` conoce la IP, antes de poder enviar la petición HTTP, verás viajar **3 segmentos TCP puros**:
>      1. `Cliente -> Servidor`: Bandera **`SYN`** hacia el **Puerto Destino `80`**.
>      2. `Servidor -> Cliente`: Banderas **`SYN + ACK`** desde el **Puerto Origen `80`**.
>      3. `Cliente -> Servidor`: Bandera **`ACK`**.
>    * **Fase 3 (Transferencia HTTP y Cierre FIN/ACK):** Recién después del 3.er paso del handshake sale el sobre **HTTP** con la página web, y al terminar verás el intercambio de cierre con las banderas **`FIN` y `ACK`**.

<div style="page-break-after: always;"></div>

---

# Anexo Práctico Integrador: Guía de Comandos Cisco IOS y Práctica Integral

## A.1. Guía Rápida de Comandos Esenciales de Cisco IOS (Cheat Sheet para Laboratorios y Exámenes)

| Modo / Objetivo | Comando Exacto en Cisco IOS | Descripción y Función |
| :--- | :--- | :--- |
| **Paso a modo privilegiado** | `Router> enable` | Ingresa al modo EXEC privilegiado (`Router#`). |
| **Paso a configuración global** | `Router# configure terminal` | Ingresa al modo de configuración global (`Router(config)#`). |
| **Configurar una interfaz LAN** | `R1(config)# interface gigabitEthernet 0/0`<br>`R1(config-if)# ip address 192.168.10.1 255.255.255.0`<br>`R1(config-if)# no shutdown` | Asigna dirección IPv4 y máscara a la interfaz y la enciende físicamente (genera las rutas **`C`** y **`L`** en la tabla). |
| **Configurar una Ruta Estática** | `R1(config)# ip route 10.1.1.0 255.255.255.0 209.165.200.226` | Crea una ruta estática (`S`, AD=1) hacia la red remota `10.1.1.0/24` enviando los paquetes al siguiente salto `209.165.200.226`. |
| **Configurar Ruta por Defecto** | `R1(config)# ip route 0.0.0.0 0.0.0.0 209.165.200.226` | Crea una ruta estática por defecto (`S*`, *Gateway of last resort*) para enviar todo tráfico desconocido hacia Internet/ISP. |
| **Configurar EIGRP (AS 100)** | `R1(config)# router eigrp 100`<br>`R1(config-router)# network 192.168.1.0 0.0.0.255`<br>`R1(config-router)# network 10.0.0.0 0.0.0.255`<br>`R1(config-router)# no auto-summary` | Activa el protocolo dinámico híbrido **EIGRP** en el Sistema Autónomo `100`, declara las redes con máscara wildcard y desactiva la auto-sumarización. |
| **Ver Tabla de Enrutamiento** | `R1# show ip route` | Muestra todas las rutas activas (`C`, `L`, `S`, `S*`, `D`, `O`, `R`), su `[Distancia Administrativa / Métrica]` y su interfaz de salida. |
| **Ver Vecinos EIGRP** | `R1# show ip eigrp neighbors` | Verifica qué routers vecinos han formado adyacencia EIGRP (vía multicast `224.0.0.10`). |
| **Ver Protocolos Activos** | `R1# show ip protocols` | Muestra los protocolos de ruteo configurados, el número de AS, redes anunciadas, valores K y distancias administrativas. |
| **Ver Tabla MAC en Switch** | `Switch# show mac-address-table` | Muestra las direcciones MAC aprendidas dinámicamente por el switch y a qué puerto físico (`Fa0/x`) está asociada cada una. |
| **Comandos en PC (Windows)** | `C:\> ipconfig /all`<br>`C:\> ping <IP>` / `tracert <IP>`<br>`C:\> arp -a`<br>`C:\> route print` o `netstat -r`<br>`C:\> netstat` | • `ipconfig /all`: Ver IP, máscara, DGW, DNS y MAC.<br>• `arp -a`: Ver caché ARP (mapeo IP $\leftrightarrow$ MAC).<br>• `route print` / `netstat -r`: Ver tabla de ruteo del host.<br>• `netstat`: Ver sesiones y puertos TCP activos. |

---

## A.2. Práctica Integradora en el Archivo `FADENA.pkt` de tu Carpeta

En tu carpeta de trabajo contás con el instalador **`CiscoPacketTracer_901_win_64bit.exe`** y el archivo de topología **`FADENA.pkt`**. Te propongo la siguiente **hoja de ruta de repaso práctico** sobre `FADENA.pkt` para integrar todas las unidades antes del examen:

1. **Inspección Topológica (Unidad 9 - Parte A):**
   * Abrí `FADENA.pkt` y alterná entre las pestañas **Logical** (arriba a la izquierda, donde ves las subredes, routers y switches) y **Physical** (donde podés inspeccionar los racks, cableado estructurado y salas de servidores).
2. **Auditoría de Direccionamiento y Subredes (Unidad 7 de Subredes/VLSM):**
   * Posá el cursor sobre cada router y PC (o ejecutá `show ip interface brief` en los routers) y anotá qué máscara de subred tiene cada LAN y cada enlace entre routers. Comprobá aplicando **ANDing** cuál es la **Dirección de Subred**, el **Primer Host**, el **Último Host** y el **Broadcast** de cada segmento.
3. **Verificación de Capa 2 y ARP (Unidad 10):**
   * Ejecutá `arp -d` en una PC para limpiar su caché ARP y `clear mac-address-table dynamic` en su switch. En **Modo Simulación**, lanzá un `ping` hacia otra PC o hacia el Default Gateway y observá cómo viaja primero el **Broadcast ARP (`FFFF.FFFF.FFFF`)**, cómo el switch aprende las MACs de origen y cómo el router detiene el broadcast.
4. **Verificación de Capa 3 y Tablas de Ruteo (Unidad de Capa 3):**
   * En cada router ejecutá `show ip route` y `show ip protocols`. Identificá cuáles son las rutas directamente conectadas (`C` y `L`) y cuáles son las rutas remotas aprendidas de forma estática (`S`) o dinámica (`D` / `O` / `R`), verificando sus corchetes `[AD / Métrica]`.
   * Seguí un paquete ICMP de extremo a extremo a través de los routers y comprobá en la pestaña **Inbound / Outbound PDU Details** la regla de oro de la cátedra: **las direcciones MAC de Capa 2 cambian en cada router, pero las direcciones IP de Capa 3 permanecen inalteradas de principio a fin**.
