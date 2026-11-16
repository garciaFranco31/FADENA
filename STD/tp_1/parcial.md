# Guía de Repaso y Banco de Preguntas de Examen / Parcial
### Sistemas de Tratamiento de Datos y Bases de Datos (FADENA)

---

## 📌 Índice de Preguntas

1. [Pregunta 1: Propósito de la Fase de Retención / Archivo](#pregunta-1)
2. [Pregunta 2: Actividades de la Fase de Procesamiento / Transformación](#pregunta-2)
3. [Pregunta 3: Garantías de las Propiedades ACID](#pregunta-3)
4. [Pregunta 4: Concurrencia en Sistemas de Archivos Tradicionales](#pregunta-4)
5. [Pregunta 5: Definición y Beneficio de un Modelo de Datos](#pregunta-5)
6. [Pregunta 6: Mejora del Modelo de Red sobre el Jerárquico](#pregunta-6)
7. [Pregunta 7: Operaciones pop() vs. peek() en Pilas](#pregunta-7)
8. [Pregunta 8: Regla Esencial del Árbol Binario de Búsqueda (BST)](#pregunta-8)
9. [Pregunta 10: Restricción de Integridad de Dominio](#pregunta-10)
10. [Pregunta 11: Restricción de Integridad de Entidad sobre la Clave Primaria](#pregunta-11)
11. [Pregunta 12: Consulta en Cálculo Relacional Orientado a Tuplas (TRC)](#pregunta-12)
12. [Pregunta 14: Mapeo de Relación Muchos a Muchos (N:M) al Modelo Relacional](#pregunta-14)
13. [Pregunta 16: Priorización de Normalización (3FN) vs. Desnormalización](#pregunta-16)
14. [Pregunta 18: Patrones de Búsqueda con LIKE en SQL](#pregunta-18)
15. [Pregunta 19: Sintaxis de Actualización Condicional con UPDATE](#pregunta-19)
16. [Pregunta 20: Función de Agregación MAX en SQL](#pregunta-20)
17. [Pregunta 21: Altura de un Árbol Vacío](#pregunta-21)
18. [Pregunta 22: Disciplina FIFO en Streaming y Buffering](#pregunta-22)
19. [Pregunta 23: Fecha más Antigua en la Tabla Incidentes](#pregunta-23)
20. [Pregunta 24: Conteo de Incidentes en Estado Destruido](#pregunta-24)
21. [Pregunta 25: Conteo de Incidentes por País Operador (Russia)](#pregunta-25)

---

### Pregunta 1
**¿Cuál es el propósito principal de la fase de Retención / Archivo en el ciclo de vida del dato?**

* a) **Mover datos inactivos o históricos a almacenamiento de menor costo y acceso menos frecuente para optimizar costos y cumplir con normativas.** *(Correcta)*
* b) Capturar clics de sitios web en tiempo real.
* c) Ejecutar el procesador sintáctico del motor de base de datos.
* d) Eliminar físicamente los datos de forma permanente para que sean irrecuperables.

> **Respuesta Correcta: `a`**  
> **Justificación:** La fase de *Archivo/Retención* traslada datos históricos o "fríos" (que ya no se usan en la operativa diaria) a medios de menor costo, manteniéndolos disponibles para auditorías, análisis a largo plazo o regulaciones legales.

---

### Pregunta 2
**En el ciclo de vida del dato, ¿qué actividades caracterizan a la fase de Procesamiento / Transformación?**

* a) La captura automática mediante sensores IoT y la creación manual en formularios.
* b) La definición de políticas legales de conservación según la normativa GDPR.
* c) **La limpieza de errores/duplicados, estandarización, integración de múltiples fuentes y enriquecimiento.** *(Correcta)*
* d) El borrado seguro de discos duros mediante desmagnetización.

> **Respuesta Correcta: `c`**  
> **Justificación:** Corresponde a las tareas de **ETL / Preparación de Datos** (*Data Wrangling*), donde se limpian valores inconsistentes, se eliminan duplicados, se homologan esquemas y se integran múltiples orígenes para dar valor a la información.

---

### Pregunta 3
**¿Qué garantizan las propiedades ACID en un Sistema de Gestión de Bases de Datos?**

* a) Que el hardware no sufra desgaste físico por el uso continuo.
* b) Que todos los datos se almacenen en formato XML.
* c) **La correcta ejecución de transacciones asegurando Atomicidad, Consistencia, Aislamiento y Durabilidad.** *(Correcta)*
* d) Que los usuarios no necesiten contraseñas para autenticarse.

> **Respuesta Correcta: `c`**  
> **Justificación:**
> * **Atomicidad (*Atomicity*):** Se completa toda la transacción o no se aplica nada (todo o nada).
> * **Consistencia (*Consistency*):** Preserva todas las reglas e invariantes de integridad de la base de datos.
> * **Aislamiento (*Isolation*):** Transacciones concurrentes se ejecutan sin interferencias mutuas.
> * **Durabilidad (*Durability*):** Tras el *commit*, los cambios persisten de manera definitiva ante cualquier fallo.

---

### Pregunta 4
**¿Qué ocurre cuando múltiples usuarios intentan modificar simultáneamente un mismo archivo en un sistema de archivos tradicional?**

* a) El sistema operativo aplica automáticamente bloqueos granulares a nivel de fila.
* b) Se crea una nueva tabla relacional para almacenar las dos versiones.
* c) **Se generan problemas de concurrencia, como sobrescritura accidental de datos o corrupción del archivo.** *(Correcta)*
* d) Se ejecuta un rollback automático mediante el catálogo del sistema.

> **Respuesta Correcta: `c`**  
> **Justificación:** Los sistemas de archivos clásicos carecen de un motor transaccional con control de concurrencia granular (a nivel de registro), lo que provoca pérdidas de actualización (*lost updates*) e inconsistencias cuando múltiples procesos intentan escribir al mismo tiempo.

---

### Pregunta 5
**¿Qué es un Modelo de Datos y cuál es su beneficio principal?**

* a) **Una representación visual y abstracta de los elementos de la realidad y sus conexiones, que actúa como puente entre las necesidades del negocio y la implementación técnica.** *(Correcta)*
* b) Un circuito integrado de memoria caché de alta velocidad.
* c) Un conjunto de archivos planos sin formato definido.
* d) Una plantilla de diseño gráfico para pantallas web.

> **Respuesta Correcta: `a`**  
> **Justificación:** Un modelo de datos provee herramientas conceptuales y formales para estructurar los datos y sus restricciones semánticas, facilitando el entendimiento común entre analistas de negocio y los diseñadores/desarrolladores de bases de datos.

---

### Pregunta 6
**¿Qué mejora introdujo el Modelo de Red respecto al Modelo Jerárquico?**

* a) **Permitió representar relaciones muchos a muchos (M:N) al hacer posible que un nodo hijo tenga múltiples nodos padres.** *(Correcta)*
* b) Eliminó la necesidad de contar con almacenamiento físico en disco.
* c) Reemplazó los punteros por lenguaje SQL.
* d) Introdujo la escalabilidad horizontal en clústeres distribuidos.

> **Respuesta Correcta: `a`**  
> **Justificación:** Mientras que el Modelo Jerárquico impone una estructura de árbol estricta donde cada registro hijo solo puede tener un único padre ($1:N$), el Modelo de Red (grafo / CODASYL) permite que un registro hijo tenga múltiples propietarios, soportando relaciones de tipo $N:M$.

---

### Pregunta 7
**En una Pila, ¿cuál es la diferencia entre la operación pop() y la operación peek() (o top())?**

* a) Ambas operaciones realizan exactamente la misma acción.
* b) pop() inserta un elemento en el fondo; peek() elimina todos los elementos.
* c) **pop() devuelve el elemento superior y lo elimina de la pila; peek() consulta el elemento superior sin eliminarlo.** *(Correcta)*
* d) pop() vacía la memoria caché; peek() verifica si hay un bucle infinito.

> **Respuesta Correcta: `c`**  
> **Justificación:** En una Pila (*Stack* - LIFO):
> * `pop()` extrae y elimina el elemento en el tope ($O(1)$).
> * `peek()` / `top()` inspecciona el elemento en el tope sin modificar la estructura ($O(1)$).

---

### Pregunta 8
**¿Cuál es la regla esencial que define a un Árbol Binario de Búsqueda (BST)?**

* a) Los elementos se eliminan bajo el principio de último en entrar, primero en salir.
* b) El nodo raíz se ubica en el nivel de profundidad más bajo.
* c) Todos los nodos deben tener exactamente dos hijos.
* d) **Para cualquier nodo $T$, los valores del subárbol izquierdo son menores o iguales al valor de $T$, y los del subárbol derecho son mayores o iguales.** *(Correcta)*

> **Respuesta Correcta: `d`**  
> **Justificación:** La propiedad fundamental de ordenamiento del BST establece que para cualquier nodo $T$, todos los elementos de su rama izquierda son $\le T$ y los de su rama derecha son $\ge T$, permitiendo búsquedas en tiempo promedio $O(\log n)$.

---

### Pregunta 10
**En una base de datos del modelo relacional para la gestión de incidentes, el atributo “Severidad” de un incidente solo puede tomar valores en el rango [CRÍTICO, ALTO, MEDIO, BAJO]. ¿Qué tipo de restricción de integridad asegura la validez de este valor?:**

* a) Restricción de Unicidad
* b) **Integridad de Dominio** *(Correcta)*
* c) Integridad Referencial
* d) Integridad de Entidad

> **Respuesta Correcta: `b`**  
> **Justificación:** La **Integridad de Dominio** delimita los valores permitidos para un atributo (tipo, formato, longitud y conjuntos discretos de valores válidos, comúnmente implementada mediante restricciones `CHECK (Severidad IN ('CRÍTICO', 'ALTO', 'MEDIO', 'BAJO'))`).

---

### Pregunta 11
**¿Cuál es la condición más crítica impuesta a una Clave Primaria por la Restricción de Integridad de Entidad?:**

* a) Debe ser única, pero puede contener valores nulos (NULL).
* b) Debe ser una Clave Alternativa que no haya sido seleccionada como Clave Foránea.
* c) Debe ser irreducible, y sus valores pueden modificarse con frecuencia.
* d) **Debe ser un conjunto de atributos único y No Nulo (Not Null) en toda la tabla.** *(Correcta)*

> **Respuesta Correcta: `d`**  
> **Justificación:** La regla de Integridad de Entidad exige que ningún atributo de la Clave Primaria ($\text{PK}$) pueda tomar valor nulo (`NOT NULL`) y que sus valores identifiquen de manera unívoca a cada tupla en la relación.

---

### Pregunta 12
**Dada la tabla SERVIDORES (t), ¿qué expresión de cálculo relacional de tupla recupera los nombres de los Servidores con Criticidad ‘Alta’?**

* a) `{t.Nombre | t.Criticidad(SERVIDORES) = ’Alta’}`
* b) **`{t.Nombre | SERVIDORES(t) AND t.Criticidad = ‘Alta’}`** *(Correcta)*
* c) `{t | SERVIDORES(t) OR t.Criticidad = ‘Alta’}`
* d) `{t | EMPLEADO(t) EXPLOIT Nombre AND t.Criticidad = ‘Alta’}`

> **Respuesta Correcta: `b`**  
> **Justificación:** En Cálculo Relacional de Tuplas (TRC), la notación formal $\{t.\text{Atributo} \mid \text{Tabla}(t) \land \text{Condición}(t)\}$ define la proyección de `t.Nombre` restringiendo la variable $t$ al conjunto `SERVIDORES` que satisfacen `t.Criticidad = 'Alta'`.

---

### Pregunta 14
**¿Cuál es la forma correcta de mapear una relación Muchos a Muchos (N:M), entre “Médicos” y “Pacientes” al modelo relacional?**

* a) Añadir la clave del Paciente en la tabla Médicos.
* b) **Crear una nueva tabla intermedia (ej. “Citas”) con las claves primarias de “Médicos” y “Pacientes” como claves foráneas, y los atributos propios que la relación tuviera.** *(Correcta)*
* c) Añadir la clave del Médico en la tabla Pacientes.
* d) Duplicar los registros de médicos por cada paciente que atienden.

> **Respuesta Correcta: `b`**  
> **Justificación:** Una relación $N:M$ requiere una **tabla asociativa/intermedia** cuya clave primaria se forma por la concatenación de las claves primarias de ambas entidades (que son claves foráneas $\text{FK}$ hacia sus tablas base), agregando los atributos específicos del vínculo.

---

### Pregunta 16
**¿En qué escenario se debe priorizar la normalización máxima (3FN o superior) sobre la desnormalización?**

* a) **En una base de datos transaccional de gestión de inventario de activos.** *(Correcta)*
* b) Cuando las uniones son extremadamente costosas.
* c) En un sistema de Business Intelligence (BI) para reportes históricos.
* d) En un sistema con requisitos de rendimiento en tiempo real por debajo del segundo.

> **Respuesta Correcta: `a`**  
> **Justificación:** En entornos transaccionales (**OLTP**), la normalización (3FN/BCNF) es indispensable para garantizar la integridad de datos, atomicidad y evitar anomalías de inserción, actualización y borrado ante continuas operaciones concurrentes.

---

### Pregunta 18
**Considere la siguiente tabla: INCIDENTES (ID_Inc, Hostname, Fecha, Tipo_Amenaza, Impacto_Score). Un analista quiere encontrar todos los incidentes cuyo Tipo_Amenaza comienza con la palabra "Malware". ¿Cuál de las siguientes sentencias sirven para esto?:**

* a) **`SELECT * FROM INCIDENTES WHERE Tipo_Amenaza LIKE 'Malware%';`** *(Correcta)*
* b) `SELECT * FROM INCIDENTES WHERE Tipo_Amenaza LIKE '_Malware';`
* c) `SELECT * FROM INCIDENTES WHERE Tipo_Amenaza = 'Malware';`
* d) `SELECT * FROM INCIDENTES WHERE Tipo_Amenaza LIKE 'Malware_';`

> **Respuesta Correcta: `a`**  
> **Justificación:** El comodín `%` en SQL representa cero o más caracteres arbitrarios, permitiendo capturar cualquier valor que comience con la palabra `'Malware'` (ej. `'Malware'`, `'Malware.Trojan'`, `'Malware_Spyware'`).

---

### Pregunta 19
**¿Cuál es la sintaxis correcta para actualizar el precio de un producto a 1500 solo si el "ID_Producto" es 101 y la "Marca" es 'Dell'?**

* a) **`UPDATE Productos SET Precio = 1500 WHERE ID_Producto = 101 AND Marca = 'Dell';`** *(Correcta)*
* b) `UPDATE Productos SET Precio = 1500 WHERE ID_Producto = 101 OR Marca = 'Dell';`
* c) `UPDATE Productos SET Precio = 1500 AND ID_Producto = 101 AND Marca = 'Dell';`
* d) `UPDATE Productos WHERE ID_Producto = 101 AND Marca = 'Dell' SET Precio = 1500;`

> **Respuesta Correcta: `a`**  
> **Justificación:** La estructura estándar es `UPDATE <tabla> SET <campo> = <valor> WHERE <condición>;`. Al requerir que ambas condiciones se satisfagan simultáneamente, se utiliza el operador booleano `AND`.

---

### Pregunta 20
**¿Cuál es la consulta SQL para obtener el valor más alto del atributo Impacto_Score en la tabla INCIDENTES?**

* a) `SELECT Impacto_Score FROM INCIDENTES HAVING MAX(Impacto_Score);`
* b) `SELECT TOP(Impacto_Score) FROM INCIDENTES;`
* c) **`SELECT MAX(Impacto_Score) FROM INCIDENTES;`** *(Correcta)*
* d) `SELECT Impacto_Score FROM INCIDENTES ORDER BY Impacto_Score;`

> **Respuesta Correcta: `c`**  
> **Justificación:** `MAX()` es la función de agregación estándar de SQL para obtener el valor escalar más alto de una columna.

---

### Pregunta 21
**De acuerdo con las definiciones técnicas del material de estudio, ¿cuál es la altura de un árbol que no contiene ningún nodo (árbol vacío)?**

* a) 1
* b) Indefinido
* c) **-1** *(Correcta)*
* d) 0

> **Respuesta Correcta: `c`**  
> **Justificación:** Por definición formal en ciencias de la computación, un árbol de un solo nodo (raíz) tiene altura $0$. Para que la relación de recurrencia $\text{altura}(T) = 1 + \max(\text{altura}(T.izq), \text{altura}(T.der))$ sea consistente en nodos hoja, un árbol vacío se define con altura **$-1$**.

---

### Pregunta 22
**Un sistema de streaming de video utiliza una cola para el buffering de paquetes de datos. ¿Qué sucede si los paquetes se procesaran bajo un principio distinto al FIFO?**

* a) **Los paquetes de datos llegarían al reproductor en una secuencia incorrecta, causando saltos temporales.** *(Correcta)*
* b) La memoria se liberaría más rápido.
* c) El sistema se transformaría automáticamente en un árbol de búsqueda.
* d) El video se vería en orden ascendente de tamaño.

> **Respuesta Correcta: `a`**  
> **Justificación:** La transmisión y decodificación de video requiere que las tramas se procesen estrictamente en orden cronológico (*FIFO: First In, First Out*). Cualquier otro orden (como LIFO) provocaría saltos o inversión de la secuencia temporal de reproducción.

---

### Pregunta 23
**¿Cuál es la fecha más antigua registrada en la tabla incidentes?** *(Base de datos `defensa_fadena.sqlite`)*

* a) 2022-02-24
* b) 2022-04-01
* c) 2022-03-26
* d) **2022-03-19** *(Correcta)*

> **Respuesta Correcta: `d`**  
> **Validación en Base de Datos:**
> ```sql
> SELECT MIN(fecha) FROM incidentes;
> -- Resultado: '2022-03-19'
> ```

---

### Pregunta 24
**¿Cuántos incidentes poseen el estado Destruido?** *(Base de datos `defensa_fadena.sqlite`)*

* a) 5.432
* b) 3.413
* c) **26.624** *(Correcta)*
* d) 2.493

> **Respuesta Correcta: `c`**  
> **Validación en Base de Datos:**
> ```sql
> SELECT COUNT(*) 
> FROM incidentes i 
> JOIN estados e ON i.id_estado = e.id_estado 
> WHERE e.codigo = 'destroyed';
> -- Resultado: 26624
> ```
> *(Desglose de estados en la BD: Destruido = 26.624, Capturado = 5.432, Dañado = 3.413, Abandonado = 2.493).*

---

### Pregunta 25
**¿Cuántos incidentes tienen a Russia como país operador?** *(Base de datos `defensa_fadena.sqlite`)*

* a) 16.724
* b) 14.646
* c) **26.851** *(Correcta)*
* d) 11.111

> **Respuesta Correcta: `c`**  
> **Validación en Base de Datos:**
> ```sql
> SELECT COUNT(*) 
> FROM incidentes i 
> JOIN paises p ON i.id_pais_operador = p.id_pais 
> WHERE p.nombre = 'Russia';
> -- Resultado: 26851
> ```
> *(Distribución por operador: Russia = 26.851, Ukraine = 11.111).*
