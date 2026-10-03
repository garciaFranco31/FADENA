# RESOLUCIÓN TP 1 - SISTEMAS DE TRATAMIENTO DE DATOS

1. 
```sql
    SELECT id_pais, nombre, bloque_alianza, poblacion
    FROM paises
    WHERE continente = "Europa"
    ORDER BY nombre ASC
```
La cantidad de paises que se muestran son 24.

2. 
```sql
    SELECT id_incidente, id_sistema, id_estado, fecha
    FROM incidentes
    WHERE fecha >= "2025-08-01" 
    ORDER BY fecha DESC
```
La cantidad de incidentes que se muestran luego de ejecutada la consulta son 341.

3.
```sql
    SELECT nombre, id_tipo
    FROM sistemas
    WHERE nombre IS NOT "Unknown" 
    ORDER BY nombre ASC
```
La consulta devuelve 1185 sistemas

4.
```sql
    SELECT COUNT(*) AS total_incidentes_capturados
    FROM incidentes
    WHERE id_estado = 2
```
Hay en total 5432 incidentes capturados

5. 
```sql
    SELECT continente
    FROM paises
    GROUP BY continente
    HAVING count(*) >= 3
```
El continente que mayor cantidad de países tiene es Europa.

6.
```sql
    SELECT id_pais, id_tipo, SUM(var_total) AS total_variaciones
    FROM resumen_diario
    GROUP BY id_pais, id_tipo
    ORDER BY total_variaciones DESC
    -- total de variaciones 1448

    SELECT id_pais, nombre
    FROM paises
    WHERE id_pais = 25
    -- el país es Russia

    SELECT id_tipo, nombre
    FROM tipos_equipo
    WHERE id_tipo = 9
    -- El tipo de equipo es Infantry Fighting Vehicles
```

7.
```sql
    SELECT nombre, continente, pib_millones_usd, defensa_millones_usd
    FROM paises
    ORDER BY defensa_millones_usd DESC
    LIMIT 1
```
El país con mayor gasto en defensa, es Estados Unidos.

8.
```sql
  SELECT i.id_incidente, i.fecha, i.id_sistema, i.id_estado, p.nombre 
    FROM incidentes i
    INNER JOIN paises p ON i.id_pais_operador = id_pais
    WHERE p.nombre = "Ukraine" and i.fecha > "2025-08-01"
    ORDER BY i.fecha DESC
```
El incidente más reciente es el de id 1962, que fue realizado el 21-08-2025


9.
```sql
    SELECT s.id_sistema, s.nombre, t.nombre AS tipo_equipo
    FROM sistemas s
    INNER JOIN tipos_equipo t ON s.id_tipo = t.id_tipo
    WHERE t.nombre = "Aircraft"
    ORDER BY s.nombre DESC
```
Son 46 sistemas cuyo tipo de equipo es Aircraft.

10.
```sql
    SELECT AVG(defensa_millones_usd)
    FROM paises
```
El promedio global de gasto en defensa es de 61.633,43 millones de USD

