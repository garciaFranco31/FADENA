# Resumen Completo: SQL Avanzado y Base de Datos `defensa_fadena.sqlite`

**Materia:** Sistemas de Tratamiento de Datos — Licenciatura en Ciberdefensa (FADENA - UNDEF)  
**Año:** 2026

---

## Parte 1: Esquema y Estructura de la Base de Datos (`defensa_fadena.sqlite`)

Antes de abordar los conceptos de SQL Avanzado, se detalla la estructura de la base de datos con la que se resolverá el **TP2**, la cual registra pérdidas y eventos de equipamiento militar en conflicto (basado en datos de Oryx) junto con indicadores de países.

### Diagrama Entidad-Relación

```mermaid
erDiagram
    paises ||--o{ incidentes : "id_pais_operador"
    paises ||--o{ incidentes : "id_pais_origen"
    paises ||--o{ resumen_diario : "id_pais"
    tipos_equipo ||--o{ sistemas : "id_tipo"
    tipos_equipo ||--o{ resumen_diario : "id_tipo"
    sistemas ||--o{ incidentes : "id_sistema"
    estados ||--o{ incidentes : "id_estado"
    fuentes ||--o{ incidentes : "id_fuente"

    paises {
        INTEGER id_pais PK
        VARCHAR nombre UNIQUE
        VARCHAR continente
        VARCHAR bloque_alianza
        INTEGER poblacion
        INTEGER pib_millones_usd
        INTEGER defensa_millones_usd
    }
    tipos_equipo {
        INTEGER id_tipo PK
        VARCHAR nombre UNIQUE
    }
    sistemas {
        INTEGER id_sistema PK
        VARCHAR nombre UNIQUE
        INTEGER id_tipo FK
    }
    estados {
        INTEGER id_estado PK
        VARCHAR codigo UNIQUE
        VARCHAR nombre UNIQUE
    }
    fuentes {
        INTEGER id_fuente PK
        VARCHAR dominio UNIQUE
    }
    incidentes {
        INTEGER id_incidente PK
        INTEGER id_sistema FK
        INTEGER id_pais_operador FK
        INTEGER id_pais_origen FK
        INTEGER id_estado FK
        INTEGER id_fuente FK
        DATE fecha
        VARCHAR url
        INTEGER oryx_sys_id
        INTEGER oryx_image_id
        INTEGER oryx_status_id
        VARCHAR oryx_mat_id UNIQUE
    }
    resumen_diario {
        INTEGER id_resumen PK
        INTEGER id_pais FK
        INTEGER id_tipo FK
        DATE fecha
        INTEGER destruidos
        INTEGER abandonados
        INTEGER capturados
        INTEGER danados
        INTEGER total
        INTEGER var_destruidos
        INTEGER var_abandonados
        INTEGER var_capturados
        INTEGER var_danados
        INTEGER var_total
    }
```

### Detalle de Tablas e Índices Existentes

| Tabla | Filas | Descripción y Columnas Clave |
| :--- | ---: | :--- |
| **`paises`** | 38 | Países involucrados o proveedores (`id_pais`, `nombre`, `continente`, `bloque_alianza`, `poblacion`, `pib_millones_usd`, `defensa_millones_usd`). |
| **`tipos_equipo`** | 29 | Categorías de armamento (`id_tipo`, `nombre`, ej. *Aircraft*, *Tanks*, *Armoured Fighting Vehicles*). |
| **`sistemas`** | 1.186 | Modelos específicos de armamento (`id_sistema`, `nombre`, `id_tipo`). |
| **`estados`** | 4 | Estado del equipo en el incidente (`1`: Destruido, `2`: Capturado, `3`: Dañado, `4`: Abandonado/otro). |
| **`fuentes`** | 17 | Dominios web de la evidencia fotográfica (`id_fuente`, `dominio`). |
| **`incidentes`** | 37.962 | Registro individual de cada pérdida/incidente (`id_incidente`, `id_sistema`, `id_pais_operador`, `id_pais_origen`, `id_estado`, `id_fuente`, `fecha`, `url`, `oryx_*`). Nota: `id_pais_origen` e `id_fuente` admiten valores `NULL`. |
| **`resumen_diario`** | 1.091 | Serie temporal acumulada y variación diaria por país y tipo de equipo (`id_resumen`, `id_pais`, `id_tipo`, `fecha`, `destruidos`, `abandonados`, `capturados`, `danados`, `total`, `var_*`). |

**Índices ya creados en `defensa_fadena.sqlite`:**
- En `incidentes`: `idx_incidentes_estado (id_estado)`, `idx_incidentes_fecha (fecha)`, `idx_incidentes_operador (id_pais_operador)`, `idx_incidentes_origen (id_pais_origen)`, `idx_incidentes_sistema (id_sistema)`.
- En `resumen_diario`: `idx_resumen_fecha (fecha)`, `idx_resumen_pais_tipo (id_pais, id_tipo)` *(índice compuesto)*.

---

## Parte 2: Sentencias de Manipulación de Datos (DML) — Subconsultas

Una **subconsulta** (o consulta anidada) es una sentencia `SELECT` dentro de otra sentencia SQL (`SELECT`, `INSERT`, `UPDATE` o `DELETE`).
- Se escribe encerrada entre **paréntesis** `(...)`.
- La consulta que la contiene se denomina **consulta externa o principal**.
- Permiten dividir consultas complejas en partes manejables.

### Clasificación según qué devuelven y dónde se ubican

1. **Según el resultado que retornan:**
   - **Escalar (un único valor):** 1 fila y 1 columna.
   - **De lista de valores:** múltiples filas y 1 sola columna.
   - **De fila única:** 1 fila con múltiples columnas.
   - **De tabla:** múltiples filas y múltiples columnas.

2. **Según la cláusula donde aparecen:**
   - **`WHERE`**: Para filtrar registros individuales basándose en el resultado de la subconsulta.
   - **`HAVING`**: Para filtrar grupos de datos (`GROUP BY`) según un criterio calculado por una subconsulta.
   - **`FROM`**: Para utilizar el resultado de la subconsulta como una **tabla temporal o derivada** (requiere alias con `AS`).
   - **`SELECT`**: Para incluir un valor calculado como columna del resultado (debe ser una subconsulta **escalar**).

---

### 2.1. Subconsultas en `WHERE` y `HAVING`

#### A. Operadores Básicos de Comparación (`=`, `<>`, `>`, `<`, `>=`, `<=`)
- **Sintaxis:** `expresion operador ( subconsulta )`
- **Requisito:** La subconsulta debe devolver **un solo valor** (1 fila, 1 columna).

**Ejemplo del apunte (en `WHERE`):** Productos con precio mayor o igual al producto más caro de "Samsung":
```sql
SELECT *
FROM producto
WHERE precio >= (
    SELECT MAX(precio)
    FROM producto
    INNER JOIN fabricante ON producto.codigo_fabricante = fabricante.codigo
    WHERE fabricante.nombre = 'Samsung'
);
```

**Ejemplo del apunte (en `HAVING`):** Departamentos cuyo sueldo promedio supera el sueldo promedio general:
```sql
SELECT departamentoID, AVG(sueldo)
FROM empleados
GROUP BY departamentoID
HAVING AVG(sueldo) > (
    SELECT AVG(sueldo)
    FROM empleados
);
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Continentes cuyo gasto promedio en defensa supera el gasto promedio mundial:
```sql
SELECT continente, AVG(defensa_millones_usd) AS prom_defensa
FROM paises
GROUP BY continente
HAVING AVG(defensa_millones_usd) > (
    SELECT AVG(defensa_millones_usd)
    FROM paises
);
```

---

#### B. Operadores `IN` y `NOT IN`
- **Sintaxis:** `expresion [NOT] IN ( subconsulta )`
- **Requisito:** La subconsulta devuelve **una columna y cero, una o varias filas**.
- **`IN`**: Verdadero si la expresión es igual a **alguno** de los valores devueltos.
- **`NOT IN`**: Verdadero si la expresión es distinta a **todos** los valores devueltos (o si la subconsulta no devuelve filas).

**Ejemplos del apunte:**
```sql
-- Ciudades donde hay algún cliente
SELECT ciudadID, nombre
FROM ciudad
WHERE ciudadID IN (SELECT ciudadID FROM clientes);

-- Clientes que no han realizado ningún pedido
SELECT *
FROM cliente
WHERE clienteID NOT IN (SELECT clienteID FROM pedido);
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Países registrados en la tabla `paises` que nunca figuran como país operador en `incidentes`:
```sql
SELECT id_pais, nombre
FROM paises
WHERE id_pais NOT IN (
    SELECT DISTINCT id_pais_operador
    FROM incidentes
    WHERE id_pais_operador IS NOT NULL
);
```

---

#### C. Operadores `ANY` y `ALL` (y su equivalencia en SQLite)
Se combinan con un operador de comparación (`=`, `<>`, `>`, `<`, `>=`, `<=`) frente a una lista de valores:
- **`expresion operador ANY ( subconsulta )`**: Verdadero si la comparación se cumple para **al menos uno** de los valores de la subconsulta.
- **`expresion operador ALL ( subconsulta )`**: Verdadero si la comparación se cumple para **todos** los valores de la subconsulta (o si la subconsulta no devuelve filas).

**Ejemplos del apunte (SQL estándar):**
```sql
-- Operarios que ganan más que ALGÚN supervisor
SELECT empleadoID, nombre
FROM empleados
WHERE categoria = 'operario'
  AND sueldo > ANY (SELECT sueldo FROM empleados WHERE categoria = 'supervisor');

-- Operarios que ganan más que TODOS los supervisores
SELECT empleadoID, nombre
FROM empleados
WHERE categoria = 'operario'
  AND sueldo > ALL (SELECT sueldo FROM empleados WHERE categoria = 'supervisor');
```

> [!IMPORTANT]
> **`ANY` y `ALL` en SQLite:**  
> **SQLite NO implementa los operadores `ANY` ni `ALL`** (arroja error de sintaxis). Para obtener el mismo resultado en SQLite, debes reescribir la consulta usando `MIN()`, `MAX()`, `IN` o `NOT IN` según esta tabla de equivalencias:

| Expresión con `ANY` / `ALL` | Equivalencia obligatoria en SQLite |
| :--- | :--- |
| `expr = ANY (subconsulta)` | `expr IN (subconsulta)` |
| `expr <> ALL (subconsulta)` | `expr NOT IN (subconsulta)` |
| `expr > ANY (subconsulta)` | `expr > (SELECT MIN(...) FROM ...)` |
| `expr >= ANY (subconsulta)` | `expr >= (SELECT MIN(...) FROM ...)` |
| `expr > ALL (subconsulta)` | `expr > (SELECT MAX(...) FROM ...)` |
| `expr >= ALL (subconsulta)` | `expr >= (SELECT MAX(...) FROM ...)` |
| `expr < ANY (subconsulta)` | `expr < (SELECT MAX(...) FROM ...)` |
| `expr <= ANY (subconsulta)` | `expr <= (SELECT MAX(...) FROM ...)` |
| `expr < ALL (subconsulta)` | `expr < (SELECT MIN(...) FROM ...)` |
| `expr <= ALL (subconsulta)` | `expr <= (SELECT MIN(...) FROM ...)` |

**Ejemplo del apunte reescrito para SQLite:**
```sql
SELECT empleadoID, nombre
FROM empleados
WHERE categoria = 'operario'
  AND sueldo > (
      SELECT MAX(sueldo)
      FROM empleados
      WHERE categoria = 'supervisor'
  );
```

**Ejemplo aplicado a `defensa_fadena.sqlite` (en SQLite):** Países fuera de la OTAN cuyo presupuesto de defensa supera al de **todos** los países de la alianza `CSTO`:
```sql
SELECT nombre, bloque_alianza, defensa_millones_usd
FROM paises
WHERE bloque_alianza <> 'OTAN'
  AND defensa_millones_usd > (
      SELECT MAX(defensa_millones_usd)
      FROM paises
      WHERE bloque_alianza = 'CSTO'
  );
```

---

#### D. Comparación de múltiples columnas (Constructores de tuplas/filas)
Se pueden comparar varios campos simultáneamente:
- `(expr1, expr2, ...) operador ( subconsulta )`
- `(expr1, expr2, ...) [NOT] IN ( subconsulta )`

La subconsulta debe devolver **exactamente la misma cantidad de columnas** que la tupla de la izquierda.

**Ejemplos del apunte:**
```sql
-- Facturas con descuento y monto máximo simultáneamente
SELECT *
FROM facturas
WHERE (descuento, monto) = (
    SELECT MAX(descuento), MAX(monto)
    FROM facturas
);

-- Clientes que compraron en algún mismo mes y año que el cliente 282 (Estándar)
SELECT DISTINCT clienteID
FROM facturas
WHERE (MONTH(fecha), YEAR(fecha)) IN (
    SELECT MONTH(fecha), YEAR(fecha)
    FROM facturas
    WHERE clienteID = 282
);
```

> [!IMPORTANT]
> **Manejo de Fechas en SQLite (`strftime`):**  
> SQLite no posee las funciones `MONTH()` ni `YEAR()`. Las fechas se almacenan como texto (`YYYY-MM-DD`) y se extraen partes mediante `strftime()`:
> - **Año:** `strftime('%Y', fecha)`
> - **Mes:** `strftime('%m', fecha)`
> - **Día:** `strftime('%d', fecha)`

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Sistemas que tuvieron incidentes en el mismo año y mes que algún incidente reportado por `forbes.com`:
```sql
SELECT DISTINCT id_sistema
FROM incidentes
WHERE (strftime('%Y', fecha), strftime('%m', fecha)) IN (
    SELECT strftime('%Y', i.fecha), strftime('%m', i.fecha)
    FROM incidentes i
    INNER JOIN fuentes f ON i.id_fuente = f.id_fuente
    WHERE f.dominio = 'forbes.com'
);
```

---

#### E. Cuidado con los Valores Nulos (`NULL`) en `NOT IN` y `ALL`
Si la subconsulta usada en un `NOT IN` o `ALL` devuelve **aunque sea un único valor `NULL`**, toda la comparación se evalúa como `UNKNOWN/NULL` y **la consulta principal no devuelve ninguna fila**.

**Soluciones:**
1. Filtrar los nulos dentro de la subconsulta: `WHERE columna IS NOT NULL`.
2. Reemplazar nulos por un valor por defecto con `COALESCE(columna, valor_por_defecto)`.

**Ejemplo del apunte:**
```sql
SELECT *
FROM facturas
WHERE descuento >= ALL (
    SELECT COALESCE(descuento, 0)
    FROM facturas
);
-- En SQLite: WHERE descuento >= (SELECT MAX(COALESCE(descuento, 0)) FROM facturas);
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Países que **no** han sido origen de ningún equipamiento involucrado en incidentes (teniendo en cuenta que `incidentes.id_pais_origen` tiene valores `NULL`):
```sql
SELECT nombre
FROM paises
WHERE id_pais NOT IN (
    SELECT id_pais_origen
    FROM incidentes
    WHERE id_pais_origen IS NOT NULL -- Imprescindible para que NOT IN funcione
);
```

---

#### F. Operadores `EXISTS` y `NOT EXISTS`
- **Sintaxis:** `[NOT] EXISTS ( subconsulta )`
- Evalúan únicamente **si la subconsulta devuelve al menos una fila (`EXISTS`) o ninguna (`NOT EXISTS`)**, sin importar qué columnas se seleccionen (suele ponerse `SELECT 1` o `SELECT *`).
- Cortocircuito: En cuanto encuentra la primera fila que cumple la condición, detiene la búsqueda y devuelve verdadero (para `EXISTS`) o falso (para `NOT EXISTS`).
- No sufren el problema de los valores `NULL` que afecta a `NOT IN`.

**Ejemplos del apunte:**
```sql
-- Clientes con algún pedido posterior al 10/08/2025
SELECT clienteID, nombre
FROM cliente
WHERE EXISTS (
    SELECT pedidoID
    FROM pedido
    WHERE cliente.clienteID = pedido.clienteID
      AND pedido.fecha > '2025-08-10'
);

-- Clientes que no han realizado ningún pedido
SELECT *
FROM cliente
WHERE NOT EXISTS (
    SELECT pedidoID
    FROM pedido
    WHERE cliente.clienteID = pedido.clienteID
);
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Tipos de equipo que registran al menos un sistema capturado (`id_estado = 2`):
```sql
SELECT t.id_tipo, t.nombre
FROM tipos_equipo t
WHERE EXISTS (
    SELECT 1
    FROM sistemas s
    INNER JOIN incidentes i ON s.id_sistema = i.id_sistema
    WHERE s.id_tipo = t.id_tipo
      AND i.id_estado = 2
);
```

---

### 2.2. Subconsultas Correlacionadas vs. No Correlacionadas

| Característica | Subconsulta No Correlacionada | Subconsulta Correlacionada |
| :--- | :--- | :--- |
| **Dependencia** | Independiente de la consulta externa. | Hace referencia a columnas de la consulta externa. |
| **Ejecución** | Se ejecuta **una sola vez** al principio; su resultado sirve de entrada fija a la consulta externa. | Se ejecuta **una vez por cada fila** evaluada por la consulta externa. |
| **Orden** | 1º Subconsulta $\rightarrow$ 2º Consulta externa. | 1º Consulta externa (fila a fila) $\rightarrow$ 2º Subconsulta. |

**Ejemplo del apunte (Correlacionada con operador `>`):** Empleados cuyo sueldo supera el sueldo promedio de su propio departamento:
```sql
SELECT e1.nombre, e1.sueldo
FROM empleados e1
WHERE sueldo > (
    SELECT AVG(salario)
    FROM empleados e2
    WHERE e2.departamentoID = e1.departamentoID
);
```

**Ejemplo aplicado a `defensa_fadena.sqlite` (Correlacionada):** Países cuyo presupuesto en defensa supera el promedio de los países de su mismo continente:
```sql
SELECT p1.nombre, p1.continente, p1.defensa_millones_usd
FROM paises p1
WHERE p1.defensa_millones_usd > (
    SELECT AVG(p2.defensa_millones_usd)
    FROM paises p2
    WHERE p2.continente = p1.continente
);
```

---

### 2.3. Subconsultas en la Cláusula `FROM` (Tablas Derivadas)
Permiten tratar el resultado de un `SELECT` como una tabla temporal dentro del `FROM`.
- **Obligatorio:** Asignar un alias a la subconsulta (`AS nombre_alias`).
- Muy útil para hacer agregaciones en dos pasos (por ejemplo, el máximo de un promedio).

**Ejemplo del apunte:** Máximo precio promedio entre las categorías de productos:
```sql
SELECT MAX(promedio.precio_promedio)
FROM (
    SELECT categoria, AVG(precio) AS precio_promedio
    FROM producto
    GROUP BY categoria
) AS promedio;
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Obtener el promedio de incidentes por sistema dentro de cada tipo de equipo:
```sql
SELECT conteo.id_tipo, AVG(conteo.cant_incidentes) AS prom_incidentes_por_sistema
FROM (
    SELECT s.id_tipo, s.id_sistema, COUNT(i.id_incidente) AS cant_incidentes
    FROM sistemas s
    LEFT JOIN incidentes i ON s.id_sistema = i.id_sistema
    GROUP BY s.id_tipo, s.id_sistema
) AS conteo
GROUP BY conteo.id_tipo;
```

---

### 2.4. Subconsultas en la Cláusula `SELECT`
- Deben ser **obligatoriamente escalares** (retornar 1 fila y 1 columna).
- Suelen ser subconsultas correlacionadas para mostrar un valor agregado junto al detalle de cada fila.

**Ejemplo del apunte:** Nombre de cada empleado junto al salario promedio de su departamento:
```sql
SELECT nombre,
       (SELECT AVG(salario)
        FROM empleados AS e2
        WHERE e2.departamento = e1.departamento) AS salario_prom_dep
FROM empleados e1;
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Listar cada país junto con la cantidad total de incidentes en los que figura como operador:
```sql
SELECT p.nombre,
       (SELECT COUNT(*)
        FROM incidentes i
        WHERE i.id_pais_operador = p.id_pais) AS total_incidentes_operador
FROM paises p;
```

---

### 2.5. Subconsultas en `INSERT`, `UPDATE` y `DELETE`

1. **En `INSERT INTO`:** Rellena una tabla con el resultado de un `SELECT` (las columnas devueltas deben coincidir en cantidad y tipo; no requiere paréntesis alrededor del `SELECT`).
   ```sql
   INSERT INTO aprobadosMatem (alumnoID, nombre, nota)
   SELECT a.alumnoID, a.nombre, c.nota
   FROM alumnos a
   INNER JOIN calificaciones c ON (a.alumnoID = c.alumnoID)
   WHERE c.nota > 6 AND c.materia = 'Matem';
   ```
2. **En `UPDATE`:** Utiliza subconsultas en el `WHERE` (o en el `SET`) para decidir qué filas actualizar.
   ```sql
   UPDATE empleados
   SET sueldo = sueldo * 1.1
   WHERE departamentoID IN (
       SELECT departamentoID
       FROM departamentos
       WHERE presupuesto > 80000
   );
   ```
3. **En `DELETE`:** Utiliza subconsultas en el `WHERE` para filtrar qué filas eliminar.
   ```sql
   DELETE FROM empleados
   WHERE sueldo < (SELECT AVG(sueldo) FROM empleados);
   ```

---

## Parte 3: Operadores de Conjunto (DML)

Permiten combinar los resultados de dos o más consultas `SELECT`:

```sql
sentencia_SELECT_1
UNION | INTERSECT | EXCEPT [ ALL ]
sentencia_SELECT_2
[ ORDER BY columna [ ASC | DESC ] ];
```

### Reglas Generales
- Ambas sentencias `SELECT` deben retornar **la misma cantidad de columnas** y con **tipos de datos compatibles** posición a posición.
- Por defecto **eliminan duplicados** (Nota: en SQLite, `UNION ALL` mantiene duplicados; sin embargo, SQLite estándar sólo soporta `ALL` en `UNION ALL`, mientras que en el estándar SQL general se menciona `[ALL]` para los tres).
- La cláusula **`ORDER BY`** sólo puede escribirse **una única vez al final** de toda la operación de conjunto y ordena el resultado global.

| Operador | Descripción | ¿Conmutativo? |
| :--- | :--- | :---: |
| **`UNION [ALL]`** | Filas que están en el primer `SELECT`, en el segundo, o en ambos. | Sí |
| **`INTERSECT`** | Filas que aparecen **tanto en el primer `SELECT` como en el segundo** (intersección). | Sí |
| **`EXCEPT`** | Filas que están en el **primer `SELECT` pero NO están en el segundo** (diferencia / minus). | **No** |

**Ejemplos del apunte:**
```sql
-- Ciudades donde hay clientes o vendedores (con duplicados)
SELECT ciudad FROM clientes
UNION ALL
SELECT ciudad FROM vendedores;

-- Ciudades donde hay clientes Y vendedores
SELECT ciudad FROM clientes
INTERSECT
SELECT ciudad FROM vendedores;

-- Ciudades donde hay clientes PERO NO vendedores
SELECT ciudad FROM clientes
EXCEPT
SELECT ciudad FROM vendedores;
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:** Países que han sido **proveedores (origen)** de sistemas en incidentes, pero **nunca operadores**:
```sql
SELECT id_pais_origen AS id_pais FROM incidentes WHERE id_pais_origen IS NOT NULL
EXCEPT
SELECT id_pais_operador AS id_pais FROM incidentes;
```

---

## Parte 4: Sentencias de Definición de Datos (DDL)

### 4.1. Vistas (`VIEW`)
Una **vista** es una **tabla virtual** definida por una consulta `SELECT`.
- **No almacena datos físicamente**; al consultarla, ejecuta su `SELECT` subyacente y muestra siempre los datos actualizados dinámicamente.
- **Ventajas principales:**
  1. **Seguridad:** Oculta columnas o filas sensibles a determinados usuarios.
  2. **Reducción de complejidad y reutilización:** Encapsula `JOIN`s y cálculos complejos para usarlos como si fueran una tabla simple.
  3. **Independencia lógica de datos:** Si las tablas base se dividen o reestructuran, la vista puede mantener la misma interfaz para las aplicaciones.

**Sintaxis de Creación y Eliminación:**
```sql
CREATE VIEW nombre_vista [ (nombre_col1, nombre_col2, ...) ]
AS
sentencia_SELECT;

DROP VIEW nombre_vista;
```

**Ejemplo del apunte:**
```sql
CREATE VIEW v_emp_jef (empID, empNombre, jefID, jefNombre) AS
SELECT e.empID, e.nombre, j.empID, j.nombre
FROM empleados e
LEFT OUTER JOIN empleados j ON (e.jefeID = j.empID);

-- Uso posterior de la vista:
SELECT v.ventasID, v.fecha, vej.empNombre, vej.jefNombre
FROM ventas v
INNER JOIN v_emp_jef vej ON (v.empleadoID = vej.empID);
```

**Ejemplo aplicado a `defensa_fadena.sqlite`:**
```sql
CREATE VIEW v_incidentes_detalle AS
SELECT i.id_incidente,
       i.fecha,
       s.nombre AS sistema,
       t.nombre AS tipo_equipo,
       p_op.nombre AS pais_operador,
       p_or.nombre AS pais_origen,
       e.nombre AS estado
FROM incidentes i
INNER JOIN sistemas s ON i.id_sistema = s.id_sistema
INNER JOIN tipos_equipo t ON s.id_tipo = t.id_tipo
INNER JOIN paises p_op ON i.id_pais_operador = p_op.id_pais
LEFT JOIN paises p_or ON i.id_pais_origen = p_or.id_pais
INNER JOIN estados e ON i.id_estado = e.id_estado;
```

---

### 4.2. Índices (`INDEX`)
Estructuras adicionales que **aceleran la recuperación de datos** (`SELECT` con `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`) evitando escanear la tabla completa fila por fila (*full table scan*).

**Contrapartida (Costo):**
- Consumen **espacio de almacenamiento** adicional.
- **Ralentizan las operaciones de escritura** (`INSERT`, `UPDATE`, `DELETE`) porque el motor debe actualizar los índices ante cada cambio.

**Buenas prácticas de indexación:**
- Indexar columnas usadas frecuentemente en `WHERE`, condiciones de `JOIN` y `ORDER BY`.
- Priorizar columnas con **alta cardinalidad** (muchos valores distintos).
- **No** indexar tablas pequeñas.

**Clasificación:**
1. **Agrupados (*Clustered*):** Determinan el orden físico de almacenamiento de las filas en el disco. **Sólo puede existir 1 por tabla** (generalmente la `PRIMARY KEY`).
2. **No Agrupados (*Non-Clustered*):** Estructura separada con punteros a las filas físicas. Puede haber **varios por tabla** (es el tipo por defecto al hacer `CREATE INDEX`).
3. **Únicos (`UNIQUE`):** Garantizan que no se repitan valores en la columna o combinación de columnas.
4. **Compuestos:** Abarcan dos o más columnas simultáneamente.

**Sintaxis y Ejemplos del apunte:**
```sql
-- Índice simple no agrupado
CREATE INDEX idx_apellido ON empleados (apellido);

-- Índice único
CREATE UNIQUE INDEX idx_correo ON empleados (email);

-- Índice compuesto
CREATE INDEX idx_nombrecompleto ON empleados (apellido, nombre);

-- Eliminación de índices (según SGBD; en SQLite se usa: DROP INDEX nombre_indice;)
DROP INDEX nombre_indice ON tabla;
ALTER TABLE tabla DROP INDEX nombre_indice;
```

---

### 4.3. Otros Objetos: Procedimientos Almacenados y Funciones (UDF)

> [!NOTE]
> Se incluyen en la teoría para conocer las capacidades de los SGBD clientes/servidor (PostgreSQL, SQL Server, MySQL, Oracle). **SQLite no soporta `CREATE PROCEDURE` ni `CREATE FUNCTION` mediante SQL** (al ser embebido, las funciones personalizadas se registran desde el lenguaje anfitrión como Python o C).

| Característica | Procedimientos Almacenados (`PROCEDURE`) | Funciones Definidas por Usuario (`FUNCTION`) |
| :--- | :--- | :--- |
| **Modificación de datos** | Permite `INSERT`, `UPDATE`, `DELETE` y `SELECT`. | **No** permite modificar datos (sólo lectura/cálculo). |
| **Parámetros y retorno** | Parámetros de entrada y salida; puede devolver 0, 1 o varios valores. | Sólo parámetros de entrada; **siempre devuelve 1 valor**. |
| **Invocación** | Mediante `CALL` o `EXEC` (no dentro de un `SELECT`). | Se incrusta directamente en expresiones (`SELECT`, `WHERE`). |

```sql
-- Sintaxis general de Procedimiento
CREATE PROCEDURE nombre_procedimiento (param1 tipo, param2 tipo)
AS
BEGIN
    -- Sentencias SQL
END;

-- Sintaxis general de Función
CREATE FUNCTION nombre_funcion (param1 tipo, param2 tipo)
RETURNS tipo_dato
AS
    RETURN <expresion>
END;
```

---

## Parte 5: Sentencias de Control de Datos (DCL)

El **Lenguaje de Control de Datos (DCL)** administra la **seguridad, permisos y control de acceso** sobre los objetos de la base de datos (tablas, vistas, procedimientos). *(Nota: SQLite, al ser una base de datos en un único archivo local sin servidor de usuarios, no utiliza `GRANT` ni `REVOKE`).*

- **`GRANT`**: Otorga privilegios a usuarios o roles.
  ```sql
  GRANT <privilegios> ON <objeto> TO <usuario>;
  -- Ejemplo:
  GRANT SELECT, INSERT, DELETE ON empleados TO usuarioA;
  ```
- **`REVOKE`**: Quita privilegios previamente otorgados.
  ```sql
  REVOKE <privilegios> ON <objeto> FROM <usuario>;
  -- Ejemplo:
  REVOKE INSERT ON empleados FROM usuarioA;
  ```

---

## Parte 6: Sentencias de Control de Transacciones (TCL)

Una **transacción** es una secuencia de una o más operaciones (`INSERT`, `UPDATE`, `DELETE`) que se ejecutan como una **única unidad lógica e indivisible de trabajo**.

### Propiedades ACID
- **A — Atomicidad (*Atomicity*):** Todo o nada; o se completan todas las operaciones de la transacción, o no se aplica ninguna.
- **C — Consistencia (*Consistency*):** La transacción lleva la base de datos de un estado válido a otro estado válido respetando todas las reglas e integridades.
- **I — Aislamiento (*Isolation*):** Las transacciones concurrentes no interfieren entre sí.
- **D — Durabilidad (*Durability*):** Una vez confirmada (`COMMIT`), los cambios persisten de forma permanente incluso ante fallos del sistema.

### Comandos Principales de TCL
- **`BEGIN TRANSACTION` (o `BEGIN`)**: Inicia explícitamente una transacción.
- **`COMMIT`**: Confirma y guarda de forma permanente todos los cambios realizados en la transacción actual.
- **`ROLLBACK`**: Deshace todas las operaciones realizadas desde el `BEGIN TRANSACTION` (o hasta un `SAVEPOINT` indicado).
- **`SAVEPOINT nombre_punto`**: Crea un punto de control intermedio dentro de una transacción larga para permitir revertir parcialmente (`ROLLBACK TO SAVEPOINT nombre_punto`) sin cancelar toda la transacción.

**Ejemplos del apunte:**
```sql
-- Ejemplo 1: Uso de SAVEPOINT y ROLLBACK parcial
BEGIN TRANSACTION;

INSERT INTO pedidos (id, cliente_id, total) VALUES (1, 101, 50.00);

SAVEPOINT punto_verificacion_1;

UPDATE inventario SET stock = stock - 1 WHERE producto_id = 101;

-- Si falla la operación 2, revertimos solo hasta el SAVEPOINT (conservando el INSERT)
ROLLBACK TO SAVEPOINT punto_verificacion_1;

COMMIT;
```

```sql
-- Ejemplo 2: Transferencia entre cuentas (Atomicidad clásica)
BEGIN TRANSACTION;

UPDATE cuentas SET balance = balance - 100 WHERE cuentaID = 1;
UPDATE cuentas SET balance = balance + 100 WHERE cuentaID = 2;

COMMIT;
```
