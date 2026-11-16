# Guía de Estudio Exhaustiva: Sistemas de Tratamiento de Datos y Diseño de Bases de Datos

---

## 1. Unidad 1: Conceptos Fundamentales, Tratamiento de Datos y Niveles de Abstracción

### 1.1. Arquitectura ANSI/SPARC y Niveles de Abstracción

El diseño y la gestión de sistemas de bases de datos relacionales se fundamentan en el principio de **abstracción de datos**, cuyo propósito es aislar la complejidad de las estructuras físicas de almacenamiento de los programas de aplicación y de los usuarios finales. La arquitectura de tres niveles formalizada por el comité **ANSI/SPARC** (*American National Standards Institute / Standards Planning and Requirements Committee*) establece un marco conceptual para conseguir la independencia de datos mediante tres niveles integrados de abstracción:

* **Nivel de Visión (Externo / Vistas de Usuario):** Representación de alto nivel orientada a los usuarios finales y programas de aplicación. Describe exclusivamente la parte de la base de datos que es relevante para un grupo o perfil de usuario específico, ocultando el resto de la estructura global. Especifica visiones personalizadas mediante la definición de esquemas externos (vistas, consultas personalizadas y modelos de datos de interfaz), garantizando que los datos no autorizados permanezcan inaccesibles.
* **Nivel Lógico (Conceptual):** Descripción global, concisa e integrada de la totalidad de la base de datos, independiente de cualquier consideración de almacenamiento físico o software específico del Sistema de Gestión de Bases de Datos (DBMS). Define la estructura lógica completa: entidades, atributos, relaciones, tipos de datos, dominios y restricciones de integridad. Sirve como puente de traducción entre los esquemas externos y el esquema interno.
* **Nivel Físico (Interno):** Especificación técnica de bajo nivel que describe cómo se almacenan físicamente los datos en los medios de almacenamiento secundario (discos, bloques de memoria). Define la organización de archivos, registros de longitud fija o variable, estructuras de almacenamiento interno, métodos de acceso, asignación de bloques, punteros e índices B/B+.

#### Jerarquía de Abstracción e Independencia de Datos

```text
===================================================================
 [ NIVEL EXTERNO / VISIÓN ] (Vistas de Usuario, Consultas, DFD)
===================================================================
                               │
                 INDEPENDENCIA LÓGICA DE DATOS
                               │
                               ▼
===================================================================
 [ NIVEL LÓGICO / CONCEPTUAL ] (Modelo E-R, Esquema Relacional)
===================================================================
                               │
                 INDEPENDENCIA FÍSICA DE DATOS
                               │
                               ▼
===================================================================
 [ NIVEL FÍSICO / INTERNO ] (Archivos, Índices B+, Bloques de Disco)
===================================================================
```

* **Independencia Lógica de Datos:** Capacidad de modificar el esquema conceptual (tales como añadir o alterar entidades, atributos o relaciones) sin necesidad de reescribir los esquemas externos ni modificar los programas de aplicación de usuario existentes. Se logra al desacoplar las vistas externas de la representación lógica global.
* **Independencia Física de Datos:** Capacidad de modificar el esquema interno (reorganizar archivos, cambiar la estructuración de bloques en disco, o crear y eliminar índices de acceso) sin alterar el esquema conceptual ni los esquemas externos. Mientras que las modificaciones físicas son transparentes para las aplicaciones y usuarios, los cambios lógicos pueden requerir ocasionalmente adaptar consultas si cambian las estructuras base.

---

### 1.2. Fases del Proceso de Diseño de Bases de Datos

El diseño de una base de datos relacional es un proceso iterativo y estructurado que transforma las necesidades informativas y funcionales de una organización en un sistema robusto e implementable dentro de un DBMS comercial.

| Fase | Objetivo Principal | Entradas / Proceso | Resultado / Entregable |
| :--- | :--- | :--- | :--- |
| **1. Recopilación y Análisis de Requisitos** | Entender, especificar y documentar detalladamente las necesidades de información y las operaciones sobre los datos de los usuarios del sistema. | **Entradas:** Entrevistas con usuarios clave, expertos del dominio y documentación de procesos operacionales.<br><br>**Proceso:** Análisis del dominio de la aplicación, identificación de entidades clave y especificación de requisitos funcionales (transacciones, DFD, diagramas de secuencia, recuperaciones y actualizaciones). | Documento escrito formal de especificación de requisitos de datos y funcionales, detallado y completo. |
| **2. Diseño Conceptual** | Crear un esquema conceptual de alto nivel que represente la estructura global de datos de forma independiente de la implementación técnica y del DBMS. | **Entradas:** Documento formal de requisitos de datos.<br><br>**Proceso:** Identificación semántica de entidades, atributos, relaciones y restricciones; eliminación de redundancias y validación contra requisitos funcionales. | Esquema conceptual abstracto formal expresado mediante un **Diagrama Entidad-Relación (DER)**, de fácil interpretación para usuarios no técnicos. |
| **3. Diseño Lógico** | Traducir el esquema conceptual abstracto a un modelo de datos orientado a la arquitectura de implementación del DBMS objetivo. | **Entradas:** Diagrama Entidad-Relación (DER) conceptual.<br><br>**Proceso:** Aplicación de reglas formales de mapeo o transformación del DER al modelo relacional (estructuración en esquemas de relación, atributos, claves primarias y claves foráneas). | **Esquema Relacional de Base de Datos** (conjunto de tablas, columnas y restricciones de integridad referencial). |
| **4. Diseño Físico** | Especificar las estructuras de almacenamiento interno y los métodos de acceso para garantizar la eficiencia operativa y el rendimiento del DBMS. | **Entradas:** Esquema relacional lógico y especificaciones de estimación de carga de trabajo/transacciones.<br><br>**Proceso:** Configuración de la organización de archivos, selección de estructuras de índices, clustering de registros y parámetros de buffer de almacenamiento. | **Esquema Físico de la Base de Datos** codificado en el Lenguaje de Definición de Datos (DDL) del DBMS comercial objetivo. |

---

### 1.3. Clasificación y Cualidades de los Modelos de Datos Conceptuales

Los modelos conceptuales constituyen herramientas de modelado de alto nivel que abstraen el dominio del problema. Para cumplir adecuadamente con sus objetivos docentes y técnicos, todo modelo conceptual de datos (como el Modelo Entidad-Relación) debe reunir cuatro cualidades fundamentales:

* **Expresividad:** Debe poseer la capacidad semántica y el número suficiente de conceptos e instrumentos de modelado para expresar de forma exacta, rica y completa todos los elementos de la realidad y sus restricciones asociadas.
* **Simplicidad:** Debe ofrecer construcciones claras, intuitivas y concisas de modo que los esquemas resultantes sean fácilmente comprensibles tanto por analistas de sistemas como por usuarios no técnicos del dominio.
* **Minimalidad:** Debe garantizar que cada concepto dentro del lenguaje de modelado tenga un significado distintivo, único e irremplazable, evitando duplicidades teóricas o construcciones sintácticas redundantes.
* **Formalidad:** Debe asegurar que todos sus conceptos dispongan de una interpretación matemática única, precisa y sintácticamente bien definida, libre de ambigüedades o contradicciones formales.

---

## 2. Unidad 2: Estructuras de Datos y Recursividad

### 2.1. Estructuras de Datos Fundamentales

Las estructuras de datos fundamentan la gestión operativa en memoria y representan la base algorítmica sobre la cual operan los motores de procesamiento de datos y los índices de bases de datos.

| Estructura | Mecanismo de Acceso | Operaciones Principales | Caso de Uso Típico |
| :--- | :--- | :--- | :--- |
| **Arreglos (Arrays)** | Acceso directo e indexado por posición mediante cálculo de desplazamiento (*offset*) ($O(1)$). Memoria contigua. | `Lectura(i)`, `Escritura(i)`, `Recorrido secuencial` ($O(n)$). | Tablas estáticas de tamaño fijo, vectores de atributos en tuplas y búferes contiguos de memoria secundaria. |
| **Pilas (Stack)** | LIFO (*Last In, First Out*). Acceso exclusivo al elemento situado en la cima o tope (*top*). | `push(x)` ($O(1)$), `pop()` ($O(1)$), `peek()` ($O(1)$). | Control de la pila de llamadas de funciones (*call stack*), evaluación de expresiones algebraicas y *parsing* de consultas SQL. |
| **Colas (Queue)** | FIFO (*First In, First Out*). Inserción por el extremo posterior (*rear*) y extracción por el anterior (*front*). | `enqueue(x)` ($O(1)$), `dequeue()` ($O(1)$), `front()` ($O(1)$). | Gestión de colas de planificación de transacciones en DBMS, procesamiento por lotes (*batch*) y búferes de entrada/salida. |
| **Árboles (Trees)** | Acceso jerárquico no lineal navegando a través de punteros desde la raíz hacia nodos hijos y hojas. | `Búsqueda` ($O(\log n)$), `Inserción` ($O(\log n)$), `Eliminación`, `Recorridos` (Inorden, Preorden, Postorden). | Estructuras de indexación en DBMS (Árboles B/B+), representación jerárquica de planes de ejecución de consultas. |

---

### 2.2. Fundamentos de Recursividad y Mecanismos de Ejecución

La recursividad es un paradigma de diseño algorítmico y matemático donde una función se define expresamente en términos de sí misma. Permite resolver problemas complejos dividiéndolos en subproblemas idénticos pero de menor magnitud.

#### Componentes Críticos

1. **Caso Base:** Condición explícita de parada que evalúa un valor directo y detiene la generación de nuevas llamadas recursivas. Su omisión o incorrecta definición induce una recursión infinita.
2. **Paso Recursivo:** Sección algorítmica donde se descompone la instancia del problema y se efectúa la auto-invocación con un parámetro reducido que converge numéricamente hacia el caso base.
3. **Pila de Llamadas (Call Stack):** Estructura de datos LIFO en memoria asignada por el entorno de ejecución para gestionar el contexto de ejecución. Cada llamada recursiva genera un *stack frame* (marco de pila) que almacena los parámetros de entrada, las variables locales y la dirección de retorno en el programa.
4. **Prevención de Stack Overflow:** El agotamiento de la memoria de la pila se desencadena cuando las llamadas recursivas sobrepasan la capacidad asignada al *call stack*. Se mitiga mediante optimización de llamada final (*tail-call optimization*), transformación a algoritmos iterativos equivalentes o aplicando técnicas de memoización.

#### Esquema de Ejecución en la Pila de Llamadas

```text
EVALUACIÓN RECURSIVA (Apilado de Marcos)     DESAPILADO Y RETORNO DE VALORES
┌──────────────────────────────────────┐     ┌──────────────────────────────────────┐
│ Frame n:   [Param N, Vars, Retorno]  │     │ Frame n:   [Libera memoria y retorna]│
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│ Frame 1:   [Param 1, Vars, Retorno]  │     │ Frame 1:   [Recibe valor de Frame n] │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│ Frame Base:[Caso Base Alcanzado]     │     │ Frame Base:[Retorna resultado final] │
└──────────────────────────────────────┘     └──────────────────────────────────────┘
```

---

### 2.3. Recursividad vs. Iteración y Ejemplos Algorítmicos

#### Implementación Algorítmica (Pseudocódigo en Sintaxis Tipo Python)

```python
# --- FACTORIAL RECURSIVO ---
def factorial_recursivo(n: int) -> int:
    if n <= 1:
        return 1  # Caso Base
    else:
        return n * factorial_recursivo(n - 1)  # Paso Recursivo

# --- FACTORIAL ITERATIVO ---
def factorial_iterativo(n: int) -> int:
    resultado = 1
    for i in range(2, n + 1):
        resultado = resultado * i
    return resultado

# --- FIBONACCI RECURSIVO ---
def fibonacci_recursivo(n: int) -> int:
    if n <= 0:
        return 0  # Caso Base 1
    if n == 1:
        return 1  # Caso Base 2
    return fibonacci_recursivo(n - 1) + fibonacci_recursivo(n - 2)  # Paso Recursivo Doble

# --- FIBONACCI ITERATIVO ---
def fibonacci_iterativo(n: int) -> int:
    if n <= 0:
        return 0
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b
```

#### Cuadro Comparativo de Criterios

| Criterio | Enfoque Recursivo | Enfoque Iterativo |
| :--- | :--- | :--- |
| **Estructura de Control** | Flujo condicional y auto-invocación funcional continua. | Estructuras repetitivas explícitas (`mientras`, `para`). |
| **Uso de Memoria** | Alto consumo en memoria proporcional a la profundidad ($O(n)$ marcos en pila). | Eficiente, consume memoria fija constante ($O(1)$) sobre variables locales. |
| **Mantenibilidad y Claridad** | Expresión declarativa concisa, ideal para estructuras jerárquicas y árboles. | Código orientado a estados explícitos que requiere seguimiento de variables mutables. |
| **Riesgo de Ejecución** | Desbordamiento de la pila (*Stack Overflow*) si la profundidad es excesiva. | Bucles infinitos si las condiciones de parada sufren errores lógicos. |

---

## 3. Unidad 3: Modelo Relacional y Diseño de Bases de Datos

### 3.1. Fundamentos Teóricos del Modelo Relacional

Formalizado por Edgar F. Codd en 1970, el modelo relacional fundamenta la gestión de datos en un soporte matemático riguroso basado en la teoría de conjuntos y la lógica de predicados de primer orden.

* **Relación:** Estructura bidimensional basada en un subconjunto del producto cartesiano de una lista de dominios:
  $$R \subseteq D_1 \times D_2 \times \dots \times D_n$$
  En un DBMS, se representa formalmente mediante una **tabla**.
* **Tupla:** Elemento o fila dentro de una relación que representa un registro individual ($t \in R$). Numéricamente corresponde a un elemento ordenado de $n$ valores.
* **Dominio ($D$):** Conjunto finito o infinito de valores escalares atómicos y homogéneos válidos que puede adoptar un atributo:
  $$D_{\text{edad}} = \{x \in \mathbb{N} \mid 0 \le x \le 120\}$$
* **Grado:** Número total de atributos o columnas que definen la estructura formal del esquema de una relación (una relación de $n$ atributos se denomina $n$-aria o de grado $n$).
* **Cardinalidad:** Número total de tuplas o filas almacenadas dinámicamente en una instancia concreta de la relación ($|R|$).

---

### 3.2. Teoría de Claves e Integridad Referencial

Las claves imponen restricciones estructurales sobre el modelo para garantizar la unicidad de las tuplas y mantener la coherencia relacional entre tablas.

#### Jerarquía Formal de Claves

$$\text{Superclaves} \supset \text{Claves Candidatas} \supset \text{Clave Primaria (PK)}$$

```text
========================================================================================
 [ SUPERCLAVE ]          Conjunto de atributos que identifican unívocamente una tupla.
       │
       ▼ (Aplicación del principio de minimalidad)
 [ CLAVE CANDIDATA ]     Superclave mínima sin atributos redundantes.
       │
       ▼ (Elegida formalmente por el diseñador)
 [ CLAVE PRIMARIA (PK) ] Identificador único formal de la relación.
       │
       │ (Referenciada desde otra tabla)
       ▼
 [ CLAVE FORÁNEA (FK) ]  Atributo en tabla relacionada para mantener integridad referencial.
========================================================================================
```

* **Superclave:** Conjunto de uno o más atributos que, tomados colectivamente, permiten identificar de forma única e unívoca a una tupla dentro de una relación.
* **Clave Candidata:** Superclave mínima. Es aquel conjunto de atributos identificadores del cual no se puede eliminar ningún atributo sin perder la capacidad de identificación única.
* **Clave Primaria (PK - Primary Key):** Clave candidata seleccionada explícitamente por el diseñador para identificar unívocamente las tuplas de una relación. No admite valores nulos (`NOT NULL`).
* **Clave Foránea (FK - Foreign Key):** Atributo o conjunto de atributos en un esquema relacional cuyos valores coinciden obligatoriamente con la clave primaria de otra relación (o de la misma, en relaciones reflexivas).
* **Regla de Integridad Referencial:** Dictamina que si una tupla de una relación contiene una clave foránea, el valor de esta debe existir previamente como valor de clave primaria en la relación referenciada, o bien ser completamente nulo (`NULL`) si la participación es parcial. Impide la generación de referencias huérfanas.

#### Esquema de Ejemplo (Dominio Bancario)

Considerando la entidad fuerte `PRÉSTAMO` y la entidad débil `PAGO`:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ TABLA: PAGO                                                                            │
├───────────────────────────────────┬──────────────────┬───────────────┬─────────────────┤
│ número_préstamo (FK)              │ número_pago (PK) │ fecha_pago    │ importe_pago    │
├───────────────────────────────────┴──────────────────┴───────────────┴─────────────────┤
│ └─────── PK Compuesta del Pago ────────────────────┘                                   │
│ (número_préstamo actúa como FK hacia PRÉSTAMO, y número_pago es el Discriminante)     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### 3.3. Elementos del Modelo Entidad-Relación (Básico y Extendido)

#### Entidades Fuertes vs. Débiles

* **Entidades Fuertes (Regulares):** Tienen existencia propia e independiente en el dominio. Disponen de un atributo clave que identifica de manera única sus instancias.
* **Entidades Débiles:** Su existencia está supeditada a una entidad fuerte (denominada entidad propietaria). Poseen una restricción de participación total (dependencia de existencia). No tienen suficiente número de atributos propios para conformar una clave primaria, por lo que emplean un **atributo discriminante (clave parcial)** que se combina con la clave primaria de la entidad fuerte para constituir su PK.

#### Clasificación de Atributos

* **Simples (Atómicos):** Atributos indivisibles que contienen un único elemento de información.
* **Compuestos:** Atributos estructurados jerárquicamente que pueden dividirse en partes atómicas componentes (ejemplo: `Dirección` que se desglosa en `Calle`, `Altura`, `Piso`).
* **Monovalor:** Almacenan un único valor por cada instancia de entidad (ejemplo: `DNI`).
* **Multivalor:** Pueden almacenar un conjunto de múltiples valores para una misma instancia (ejemplo: `Teléfono`).
* **Almacenados:** Datos guardados de manera explícita e inmune en el sistema (ejemplo: `FechaNacimiento`).
* **Derivados:** Valores calculados en tiempo de ejecución a partir de otros atributos o relaciones (ejemplo: `Edad` derivada de `FechaNacimiento`).
* **Valores Nulos (NULL):** Indican la ausencia de valor. Representan situaciones de "no aplicable" o "desconocido" (faltante o no constatado).

#### Relaciones y Restricciones

* **Cardinalidad (Límite Máximo):**
  * **Uno a Uno (1:1):** Una instancia de A se vincula a lo sumo con una de B, y viceversa.
  * **Uno a Varios (1:N):** Una instancia de A se vincula con múltiples instancias de B, pero una de B se relaciona a lo sumo con una de A.
  * **Varios a Varios (N:M):** Una instancia de A se vincula con múltiples de B, y viceversa.
* **Participación (Límite Mínimo):**
  * **Total (Dependencia de existencia):** Toda instancia de la entidad debe participar obligatoriamente en al menos una relación (representada con línea doble).
  * **Parcial:** Algunas instancias de la entidad pueden no estar involucradas en la relación.
* **Grado y Roles:** El grado es el número de entidades participantes en la relación (Binario = 2, Ternario = 3). El rol especifica la función desempeñada por cada entidad en la asociación, siendo indispensable en relaciones reflexivas.

#### Modelo E-R Extendido

* **Especialización:** Proceso *top-down* donde una superclase se divide en subclases diferenciadas según características particulares.
* **Generalización:** Proceso *bottom-up* donde se extraen propiedades comunes de varias entidades para constituir una superclase generalizada.
* **Restricciones de Jerarquía:** Disjunta vs. Superpuesta (evalúan si una instancia pertenece a una o múltiples subclases) y Total vs. Parcial (evalúan si toda instancia de la superclase debe pertenecer obligatoriamente a una subclase).
* **Agregación:** Abstracción que permite asociar una relación completa (junto con sus entidades asociadas) con otra entidad del sistema, superando la limitación del modelo E-R estándar.

#### Simbología Gráfica Formal del DER

```text
[ Entidad ]         [[ Entidad Débil ]]        < Relación >        << Rel. Identificativa >>
 ┌─────────┐            ╔═════════╗                /\                       /\
 │         │            ║         ║               /  \                     //\\
 └─────────┘            ╚═════════╝              /    \                   //  \\
                                                 \    /                   \\  //
                                                  \  /                     \\//
                                                   \/                       \/

( Atributo )       ( Atributo Clave )     (( Atrib. Multivalor ))    ( Atrib. Derivado )
   ┌──────┐             ┌──────┐                 ╔══════╗                  - - - -
  /        \           / ______ \               //      \\                /       \
 (          )         (  id_emp  )             ((        ))              (         )
  \        /           \        /               \\      //                \       /
   └──────┘             └──────┘                 ╚══════╝                  - - - -

(- Atrib. Discriminante -)             Jerarquía de Atributos Compuestos
          - - - -                                   (Dirección)
         /       \                                    /     \
        ( núm_pago)                             (Calle)     (Altura)
         \       /
          - - - -

Restricciones de Participación                 Restricciones Estructurales (mín, máx)
Total:   [E1] =================== <R>          (1, N)
Parcial: [E1] ------------------- <R>          [E1] ------------------- <R>

Indicadores de Rol en Relaciones Reflexivas
            ┌──────────────┐
            │   EMPLEADO   │
            └──────┬───────┬┘
                   │       │ [supervisor]
     [supervisado] │       │
                   └─<CONTROL>─┘
```

---

### 3.4. Reglas de Mapeo / Transformación de DER a Modelo Relacional

La transformación del esquema conceptual (DER) al esquema relacional lógico se ejecuta aplicando un algoritmo sistemático estructurado en 7 pasos formales:

1. **Mapeo de Entidades Fuertes:**
   Para cada entidad fuerte $E$, crear una relación $R$ que contenga todos sus atributos simples. Los atributos compuestos se desglosan en sus componentes atómicas individuales. Seleccionar la clave de $E$ como clave primaria ($\text{PK}$) de $R$.
   * *Ejemplo:* $\text{EMPLEADO}(\underline{\text{id\_empleado}}, \text{nombre}, \text{calle}, \text{altura}, \text{ciudad})$

2. **Mapeo de Entidades Débiles:**
   Para cada entidad débil $W$ dependiente de la entidad propietaria $P$, crear una relación $R_W$. Incluir todos sus atributos simples e integrar la clave primaria de $P$ como clave foránea ($\text{FK}$) en $R_W$. La clave primaria ($\text{PK}$) de $R_W$ se conforma mediante la combinación compuesta de la $\text{PK}$ de $P$ y el atributo discriminante de $W$.
   * *Ejemplo:* $\text{PAGO}(\underline{\text{número\_préstamo}^{(\text{FK})}, \text{número\_pago}}, \text{fecha\_pago}, \text{importe})$

3. **Mapeo de Relaciones Binarias 1:1 y 1:N:**
   * **Para 1:N:** No se crea una nueva tabla. Propagar la $\text{PK}$ del conjunto de entidades del lado "1" a la relación del lado "N" en calidad de clave foránea ($\text{FK}$). Los atributos propios de la relación se integran en la tabla del lado "N".
   * **Para 1:1:** Propagar la $\text{PK}$ de una de las tablas como $\text{FK}$ a la relación de la otra, priorizando aquella entidad que posea una participación total en la relación.

4. **Mapeo de Relaciones M:N y N-arias:**
   Crear una nueva tabla intermedia $R_{MN}$. Incluir en $R_{MN}$ las claves primarias de todas las entidades participantes como claves foráneas ($\text{FKs}$). La clave primaria ($\text{PK}$) de $R_{MN}$ será la combinación de las claves primarias de las entidades participantes. Agregar los atributos descriptivos de la relación.
   * *Ejemplo:* $\text{TRABAJA\_EN}(\underline{\text{id\_empleado}^{(\text{FK})}, \text{id\_proyecto}^{(\text{FK})}}, \text{horas})$

5. **Mapeo de Atributos Multivalor:**
   Por cada atributo multivalor de una entidad $E$, generar una relación independiente $R_M$. Incluir una columna para el valor del atributo multivalor y la $\text{PK}$ de $E$ como clave foránea ($\text{FK}$). La clave primaria ($\text{PK}$) de $R_M$ estará constituida por la unión de ambos campos.
   * *Ejemplo:* $\text{TELÉFONO\_EMPLEADO}(\underline{\text{id\_empleado}^{(\text{FK})}, \text{número\_teléfono}})$

6. **Mapeo de Especialización / Generalización (Criterios y Compromisos):**
   * **Estrategia 1 (Tabla Única por Jerarquía):** Crear una sola tabla que contenga la totalidad de los atributos de la superclase y de todas las subclases, añadiendo una columna discriminadora de tipo de subclase.
     * *Criterio de Aplicación:* Jerarquías con múltiples subclases que poseen pocos atributos específicos o diferenciados.
     * *Compromiso (Trade-off):* Genera una elevada proporción de valores nulos (`NULL`) y provoca cierta pérdida de expresividad en las relaciones particulares de las subclases.
   * **Estrategia 2 (Tabla para Superclase y Subclases):** Crear una tabla para la superclase y una tabla separada por cada subclase. La tabla de la superclase almacena la $\text{PK}$ y atributos comunes; cada tabla de subclase contiene sus atributos propios y la $\text{PK}$ de la superclase (actuando como $\text{PK}$ y $\text{FK}$ simultáneamente).
     * *Criterio de Aplicación:* Subclases con un volumen elevado de atributos específicos y restricciones de completitud total.
     * *Compromiso (Trade-off):* Requiere operaciones de combinación (`JOIN`) costosas computacionalmente para reconstruir las entidades completas de las subclases.
   * **Estrategia 3 (Tablas Solo para Subclases):** Crear tablas exclusivamente para las subclases, omitiendo la tabla de la superclase. Cada tabla de subclase almacena todos los atributos heredados de la superclase más sus atributos propios.
     * *Criterio de Aplicación:* Exclusivo para superclases abstractas con restricciones de jerarquía Disjunta y Total.
     * *Compromiso (Trade-off):* Introduce redundancia estructural y complica las consultas globales formuladas sobre el conjunto general de la superclase.

7. **Mapeo de Agregación (Algoritmo en 2 Pasos):**
   * **Paso 1:** Mapear la relación base subyacente entre la Entidad A y la Entidad B generando una tabla intermedia $T_{AB}$ según las reglas de cardinalidad (usualmente $T_{AB}(\underline{\text{PK}_A, \text{PK}_B})$).
   * **Paso 2:** Mapear la relación secundaria que vincula la agregación $T_{AB}$ con la Entidad C, propagando la clave primaria de $T_{AB}$ $(\text{PK}_A, \text{PK}_B)$ a la tabla de C como clave foránea ($\text{FK}$) o creando una tabla terciaria según corresponda por cardinalidad.
     * *Ejemplo:* $\text{ASIGNACIÓN\_MÁQUINA}(\underline{\text{id\_empleado}^{(\text{FK})}, \text{id\_proyecto}^{(\text{FK})}, \text{id\_máquina}^{(\text{FK})}}, \text{fecha})$ donde los dos primeros provienen de la tabla intermedia de la relación `TRABAJA_EN`.

---

## 4. Unidad 4: Lenguajes Formales de Consulta

### 4.1. Álgebra Relacional (Procedimental)

El álgebra relacional es un lenguaje procedimental de consulta formal donde el usuario especifica la secuencia matemática de operaciones que debe seguir el sistema para construir la relación resultante.

#### Operadores Fundamentales y Derivados

* **Selección ($\sigma_{\text{predicado}}(R)$):** Operador unario que filtra el conjunto de tuplas de $R$ que satisfacen el predicado de condición lógica.
  $$\sigma_{P}(R) = \{t \in R \mid P(t) = \text{Verdadero}\}$$
  * *Consulta de Ejemplo:*
    $$\sigma_{\text{sueldo} > 50000 \land \text{depto} = \text{'SISTEMAS'}}(\text{EMPLEADO})$$

* **Proyección ($\pi_{\text{atributos}}(R)$):** Operador unario que extrae las columnas especificadas de la relación $R$ y elimina las tuplas duplicadas resultantes para mantener la definición de conjunto.
  $$\pi_{A_1, \dots, A_k}(R) = \{t[A_1, \dots, A_k] \mid t \in R\}$$
  * *Consulta de Ejemplo:*
    $$\pi_{\text{nombre, sueldo}}(\text{EMPLEADO})$$

* **Unión ($R \cup S$):** Operador binario de conjuntos que combina las tuplas de dos relaciones que deben ser estrictamente compatibles en esquema (mismo grado y dominios equivalentes).
  $$R \cup S = \{t \mid t \in R \lor t \in S\}$$
  * *Consulta de Ejemplo:*
    $$\pi_{\text{ciudad}}(\text{CLIENTE}) \cup \pi_{\text{ciudad}}(\text{PROVEEDOR})$$

* **Diferencia ($-$) e Intersección ($\cap$):**
  * $R - S$ retorna tuplas presentes en $R$ pero ausentes en $S$:
    $$R - S = \{t \mid t \in R \land t \notin S\}$$
  * $R \cap S$ retorna tuplas simultáneamente presentes en ambas relaciones:
    $$R \cap S = \{t \mid t \in R \land t \in S\}$$
  * *Consulta de Ejemplo:*
    $$\pi_{\text{id\_emp}}(\text{EMPLEADO}) - \pi_{\text{id\_emp}}(\text{TRABAJA\_EN})$$

* **Producto Cartesiano ($R \times S$):** Operador binario que combina cada tupla de $R$ con todas y cada una de las tuplas de $S$. Si $R$ tiene grado $n$ y $S$ grado $m$, el resultado tiene grado $n+m$.
  $$R \times S = \{t \cdot q \mid t \in R \land q \in S\}$$

* **Theta Join ($\bowtie_{\theta}$) y Natural Join ($\bowtie$):**
  * El **Theta Join** combina un producto cartesiano con una condición explícita de selección:
    $$R \bowtie_{\theta} S = \sigma_{\theta}(R \times S)$$
  * El **Natural Join** ($\bowtie$) une dos relaciones igualando automáticamente los valores de todos los atributos que poseen el mismo nombre en ambas tablas y elimina las columnas redundantes resultantes:
    $$R \bowtie S = \pi_{\text{Atributos Únicos}}(\sigma_{R.A_1 = S.A_1 \land \dots \land R.A_k = S.A_k}(R \times S))$$
  * *Consulta de Ejemplo:*
    $$\text{EMPLEADO} \bowtie_{\text{EMPLEADO.id\_depto} = \text{DEPARTAMENTO.id\_depto}} \text{DEPARTAMENTO}$$

* **División ($\div$):** Operador derivado empleado para expresar consultas que involucran un cuantificador universal (*"obtener las tuplas de $R$ que están vinculadas con todas las tuplas de $S$"*).
  $$R(X, Y) \div S(Y) = \pi_{X}(R) - \pi_{X}((\pi_{X}(R) \times S) - R)$$
  * *Consulta de Ejemplo:*
    $$\pi_{\text{id\_emp, id\_proy}}(\text{TRABAJA\_EN}) \div \pi_{\text{id\_proy}}(\sigma_{\text{estado}=\text{'ACTIVO'}}(\text{PROYECTO}))$$
    *(Devuelve los empleados asignados a todos los proyectos activos).*

* **Funciones de Agregación (${}_{G}\mathcal{F}_{F(A)}(R)$):** Agrupa tuplas según el conjunto de atributos $G$ y aplica funciones de resumen $F$ (`SUM`, `AVG`, `COUNT`, `MIN`, `MAX`) sobre el atributo $A$.
  * *Consulta de Ejemplo:*
    $${}_{\text{id\_depto}}\mathcal{F}_{\text{AVG}(\text{sueldo}), \text{COUNT}(\text{id\_emp})}(\text{EMPLEADO})$$

---

### 4.2. Cálculo Relacional (No Procedimental)

El cálculo relacional es un lenguaje no procedimental declarativo basado en la lógica de predicados de primer orden, donde se especifica qué información se requiere sin detallar el algoritmo para recuperarla.

* **Cálculo Relacional Orientado a Tuplas (TRC):** Expresado mediante la sintaxis de conjunto $\{t \mid P(t)\}$, donde $t$ es una variable de tupla y $P(t)$ es una fórmula lógica bien formada.
* **Cálculo Relacional Orientado a Dominios (DRC):** Expresado mediante $\{\langle x_1, x_2, \dots, x_n \rangle \mid P(x_1, x_2, \dots, x_n)\}$, donde $x_i$ son variables que toman valores del dominio de atributos específicos.

#### Ejemplo Comparativo de Expresividad Equivalente

> **Requerimiento de Negocio:** *"Obtener el nombre y sueldo de los empleados que trabajan en el departamento de 'SISTEMAS'".*

* **Álgebra Relacional:**
  $$\pi_{\text{nombre, sueldo}}(\sigma_{\text{nombre\_depto} = \text{'SISTEMAS'}}(\text{EMPLEADO} \bowtie \text{DEPARTAMENTO}))$$

* **Cálculo Relacional Orientado a Tuplas (TRC):**
  $$\{t \mid \exists e \in \text{EMPLEADO}, \exists d \in \text{DEPARTAMENTO} \, (e[\text{id\_depto}] = d[\text{id\_depto}] \land d[\text{nombre\_depto}] = \text{'SISTEMAS'} \land t[\text{nombre}] = e[\text{nombre}] \land t[\text{sueldo}] = e[\text{sueldo}])\}$$

* **Cálculo Relacional Orientado a Dominios (DRC):**
  $$\{\langle n, s \rangle \mid \exists e, d, \text{dept\_nom} \, (\langle e, n, s, d \rangle \in \text{EMPLEADO} \land \langle d, \text{dept\_nom} \rangle \in \text{DEPARTAMENTO} \land \text{dept\_nom} = \text{'SISTEMAS'})\}\quad \text{(asumiendo esquemas reducidos)}$$

---

### 4.3. Equivalencia Expresiva

El **Teorema de Completeness de Codd** demuestra formalmente la equivalencia expresiva entre el Álgebra Relacional y el Cálculo Relacional restringido a fórmulas seguras (que no generan conjuntos infinitos). Un lenguaje de consulta relacional que posee una capacidad de expresión al menos equivalente a la del álgebra o cálculo relacional se clasifica formalmente como **Relacionalmente Completo**.

---

## 5. Unidad 5: Lenguaje SQL Básico y Procesamiento de Consultas

### 5.1. Estructura Declarativa de SQL

El lenguaje SQL (*Structured Query Language*) es el estándar de sintaxis declarativa para la definición, manipulación y consulta de datos en los DBMS relacionales.

```sql
-- Plantilla Estructurada de Consulta SQL
SELECT e.nombre, d.nombre_depto, AVG(e.sueldo) AS sueldo_promedio -- Proyección y Agregación
FROM empleado e                                                   -- Origen de datos (Tablas base)
INNER JOIN departamento d ON e.id_depto = d.id_depto              -- Especificación de combinación
WHERE e.estado = 'ACTIVO'                                         -- Predicado de filtrado por fila
GROUP BY e.nombre, d.nombre_depto                                 -- Agrupamiento operacional
HAVING AVG(e.sueldo) > 3000                                       -- Filtrado condicional sobre grupos
ORDER BY sueldo_promedio DESC;                                    -- Criterio de ordenamiento final
```

---

### 5.2. Orden de Procesamiento Lógico del Motor de SQL y Mecánica de Memoria

A diferencia del orden de escritura sintáctica, el motor de ejecución de la base de datos procesa las cláusulas SQL en una secuencia lógica estricta a nivel del buffer en memoria RAM:

```text
1. FROM ──> 2. ON/JOIN ──> 3. WHERE ──> 4. GROUP BY ──> 5. HAVING ──> 6. SELECT ──> 7. DISTINCT ──> 8. ORDER BY ──> 9. LIMIT
```

1. **FROM:** Inicializa el espacio de trabajo en el buffer de memoria. Identifica las tablas origen y genera la estructura de trabajo virtual inicial (si hay múltiples tablas, realiza el producto cartesiano preliminar en memoria).
2. **ON / JOIN:** Evalúa la condición lógica de combinación indicada en `ON`. Conserva las tuplas que cumplen el predicado e integra registros nulos en las operaciones de tipo `OUTER JOIN` sobre el buffer de trabajo.
3. **WHERE:** Aplica los filtros condicionales evaluando fila por fila. Descarta del buffer de memoria todas aquellas tuplas individuales que no satisfacen los predicados lógicos especificados antes de cualquier agregación.
4. **GROUP BY:** Clasifica y reorganiza las tuplas restantes en el buffer asignándolas a estructuras tipo *hash table* o baldes de memoria agrupados según los valores comunes de las claves de agrupamiento.
5. **HAVING:** Evalúa las condiciones lógicas agregadas sobre las estructuras agrupadas en memoria. Elimina del buffer los grupos enteros que no cumplen la condición condicional.
6. **SELECT:** Realiza la proyección formal de los atributos requeridos, calcula expresiones numéricas o transformaciones de cadenas e incrementa los campos alias resultantes.
7. **DISTINCT:** Ejecuta un ordenamiento temporal en memoria o una verificación por tabla hash para identificar y purgar filas idénticas duplicadas en el conjunto proyectado.
8. **ORDER BY:** Aplica un algoritmo de ordenamiento ($O(n \log n)$) sobre el conjunto final de tuplas presentes en el buffer, ordenándolas en base a las columnas especificadas.
9. **TOP / LIMIT:** Aplica un truncamiento final sobre la pila de resultados en memoria, retornando únicamente el número prefijado de tuplas al proceso cliente solicitante.

---

### 5.3. Operaciones de Combinación (JOINs)

| Tipo de JOIN | Tuplas Preservadas | Tratamiento de No-Coincidencias | Caso de Uso Típico |
| :--- | :--- | :--- | :--- |
| **INNER JOIN** | Exclusivamente aquellas tuplas que poseen coincidencia exacta de claves en ambas tablas. | Las tuplas sin pareja en la clave de unión se descartan completamente del resultado. | Consultar únicamente relaciones validadas (ej. empleados que tienen un departamento asignado). |
| **LEFT OUTER JOIN** | Preserva la totalidad de tuplas presentes en la tabla de la izquierda (`FROM`). | Asigna valores `NULL` a todos los atributos de la tabla derecha cuando no hay coincidencia. | Generar reportes generales garantizando la inclusión de la entidad principal (ej. clientes sin compras). |
| **RIGHT OUTER JOIN** | Preserva la totalidad de tuplas presentes en la tabla de la derecha (`JOIN`). | Asigna valores `NULL` a los atributos de la tabla izquierda en ausencia de coincidencia. | Recuperar todos los registros de una tabla secundaria o catálogo asegurando su inclusión total. |
| **FULL OUTER JOIN** | Preserva la totalidad de las tuplas de ambas tablas sin excepción. | Asigna valores `NULL` en los lados correspondientes donde las claves no se cruzaron. | Procesos de auditoría integral de datos y reconciliación de esquemas dispares. |
| **SELF-JOIN** | Preserva tuplas según se configure como INNER o OUTER Join al asociar una tabla consigo misma. | Maneja las faltas de coincidencia rellenando con `NULL` si se invoca como OUTER Join. | Consultar estructuras jerárquicas o reflexivas (ej. tabla `EMPLEADO` vinculada a su `JEFE`). |

---

## 6. Unidad 6: Normalización y Desnormalización de Bases de Datos

### 6.1. Dependencias Funcionales

La teoría de normalización se articula sobre el concepto explícito de **Dependencia Funcional ($\text{DF}$)**, una restricción semántica que vincula conjuntos de atributos dentro de una relación. Se denota formalmente como:

$$X \rightarrow Y \quad \text{($X$ determina funcionalmente a $Y$)}$$

Esto exige matemáticamente que para cualquier par de tuplas $t_1$ y $t_2$ pertenecientes a la relación $R$:

$$\text{si } t_1[X] = t_2[X] \implies t_1[Y] = t_2[Y]$$

#### Clases Formales de Dependencias Funcionales

* **Dependencia Funcional Total:** Un atributo $Y$ posee una dependencia funcional total de $X$ ($X \rightarrow Y$) si depende de la totalidad de $X$ y no depende de ningún subconjunto propio de $X$.
* **Dependencia Funcional Parcial:** Ocurre cuando un atributo $Y$ no clave depende funcionalmente de un subconjunto propio de una clave primaria compuesta $X$.
* **Dependencia Funcional Transitiva:** Ocurre cuando se identifican dependencias de la forma $X \rightarrow Y$ y $Y \rightarrow Z$, implicando indirectamente $X \rightarrow Z$, siendo $Y$ un atributo no clave.

#### Ejemplos de Esquema Real

Dada la relación:
$$\text{EMPLEADO\_PROYECTO}(\underline{\text{id\_emp}, \text{id\_proy}}, \text{horas}, \text{nombre\_emp}, \text{id\_depto}, \text{nombre\_depto})$$

* **Dependencia Parcial:** $\{\text{id\_emp}, \text{id\_proy}\} \rightarrow \text{nombre\_emp}$ es Parcial porque el atributo $\text{nombre\_emp}$ depende únicamente del subconjunto $\text{id\_emp}$, ignorando $\text{id\_proy}$.
* **Dependencia Total:** $\{\text{id\_emp}, \text{id\_proy}\} \rightarrow \text{horas}$ es Total porque requiere de la combinación de ambos campos identificadores para determinar el volumen de horas aportado.
* **Dependencia Transitiva:** $\text{id\_emp} \rightarrow \text{id\_depto}$ e $\text{id\_depto} \rightarrow \text{nombre\_depto}$ constituyen una Dependencia Transitiva, determinando que $\text{id\_emp} \rightarrow \text{nombre\_depto}$ a través de $\text{id\_depto}$.

---

### 6.2. Formas Normales y Anomalías de Modificación

El proceso de normalización descompone esquemas relacionales mediante pasos formales secuenciales para erradicar la redundancia y prevenir las **Anomalías de Modificación**:

* **Anomalía de Inserción:** Imposibilidad de registrar datos válidos de una entidad sin la obligación de ingresar datos no relacionados o inexistentes de otra.
* **Anomalía de Actualización:** Duplicación de datos que exige modificar la misma información en múltiples filas, arriesgando inconsistencias lógicas si el proceso se interrumpe.
* **Anomalía de Borrado:** Pérdida involuntaria de información valiosa de una entidad secundaria al eliminar la tupla de una entidad primaria.

| Forma Normal | Requisito Previo | Regla / Condición a Cumplir | Anomalía Eliminada |
| :--- | :--- | :--- | :--- |
| **1FN** (Primera Forma Normal) | Esquema relacional base. | Todos los atributos deben contener exclusivamente valores escalares atómicos e indivisibles. Se eliminan grupos repetitivos y atributos multivalor. | Inconsistencia de formato en celdas, valores compuestos y atribución multivaluada. |
| **2FN** (Segunda Forma Normal) | Estar en 1FN. | Eliminación de dependencias parciales. Todo atributo no clave debe depender funcionalmente de la clave primaria completa ($\text{PK}$). | Redundancia de atributos que dependen solo de una sección de una clave primaria compuesta. |
| **3FN** (Tercera Forma Normal) | Estar en 2FN. | Eliminación de dependencias transitivas. Ningún atributo no clave puede depender funcionalmente de otro atributo no clave. | Anomalías de actualización y eliminación no deseada de entidades secundarias subordinadas. |
| **BCNF** (Forma Normal de Boyce-Codd) | Estar en 3FN. | Para toda dependencia funcional $X \rightarrow Y$ no trivial activa, $X$ debe ser estrictamente una superclave de la relación. | Redundancia subsistente provocada por claves candidatas solapadas o compuestas superpuestas. |

---

### 6.3. Desnormalización Controlada

La desnormalización es una técnica arquitectónica de optimización aplicada intencionalmente en las fases finales del diseño relacional. Consiste en introducir redundancia estructurada sobre un esquema normalizado para minimizar los tiempos de respuesta en entornos con alta tasa de operaciones de lectura masiva.

#### Matriz de Evaluación Técnica: Esquemas Normalizados (OLTP) vs. Desnormalizados (OLAP / Lectura)

| Criterio de Evaluación | Esquema Normalizado (3FN / BCNF - OLTP) | Esquema Desnormalizado (OLAP / Read-Heavy) | Impacto Técnico / Arquitectónico |
| :--- | :--- | :--- | :--- |
| **Rendimiento de Lectura** | Menor rendimiento en agregaciones complejas debido al costo computacional de múltiples JOINs. | Rendimiento Superior ($O(1)$) al leer datos consolidados previamente reunidos en una única tabla. | Reduce sustancialmente el I/O en disco y el uso de CPU en motores de consulta analíticos. |
| **Rendimiento de Escritura** | Óptimo y Veloz. Operaciones `INSERT`, `UPDATE` y `DELETE` afectan una única tupla de forma atómica. | Degradado. Requiere actualizar la información duplicada de forma redundante en múltiples tablas. | Incrementa los tiempos de bloqueo (*locking*) y la carga de procesamiento I/O en escrituras. |
| **Consumo de Almacenamiento** | Mínimo y optimizado. Inmunidad contra duplicidades de datos. | Elevado. La duplicación intencional de columnas incrementa el tamaño en GB/TB. | Requiere mayor aprovisionamiento de capacidad en disco secundario. |
| **Mecanismos de Integridad** | Garantizada de forma nativa por el DBMS mediante restricciones de `FOREIGN KEY` e integridad declarativa. | Complejo. Exige implementar controladores en la aplicación o disparadores (`TRIGGERS`). | Riesgo latente de inconsistencias lógicas si falla la sincronización atómica de datos redundantes. |

---

## Conclusión Pedagógica

La construcción de bases de datos relacionales exige un conocimiento riguroso que abarca desde la representación abstracta del dominio mediante el modelo conceptual, pasando por la aplicación de las reglas formales de transformación lógica y el dominio del álgebra relacional y SQL, hasta las decisiones arquitectónicas de normalización y desnormalización controlada. Esta aproximación garantiza sistemas con un óptimo rendimiento, alta mantenibilidad y una integridad de datos superior.
