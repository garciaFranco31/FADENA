# Trabajo Práctico Nro 2: SQL Avanzado (2026)

**Materia:** Sistemas de Tratamiento de Datos — Licenciatura en Ciberdefensa (FADENA)  
**Base de datos:** `defensa_fadena.sqlite`  
**Convención temporal:** Para las comparaciones `2024 → 2025` utilizar el mismo período: del **1 de enero al 21 de agosto** de cada año (`'2024-01-01'` a `'2024-08-21'` y `'2025-01-01'` a `'2025-08-21'`), ya que la fecha máxima disponible en 2025 es `2025-08-21`.

---

## Ejercicios propuestos

### Ejercicio 1: Tipos de equipo con más incidentes (Top-5)
Relacionar `incidentes`, `sistemas` y `tipos_equipo`. Mostrar `tipo_equipo` y `cantidad_incidentes`, agrupar por tipo, ordenar de mayor a menor y limitar a cinco resultados.
- **Pregunta:** ¿Qué tipo de equipo lidera el ranking?
- **Pauta:** *(JOIN + COUNT + GROUP BY + ORDER BY + LIMIT)*

```sql
SELECT t.nombre AS tipo_equipo, COUNT(*) AS cantidad_incidentes	
FROM tipos_equipo AS t 
INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema) 
GROUP BY t.nombre
ORDER BY cantidad_incidentes DESC
LIMIT 5;
```

#### Resultado de la consulta
| tipo_equipo | cantidad_incidentes |
| :--- | ---: |
| Infantry Fighting Vehicles | 7445 |
| Tanks | 7261 |
| Trucks, Vehicles, and Jeeps | 6795 |
| Armoured Personnel Carriers | 5305 |
| Armoured Fighting Vehicles | 2052 |

- **Respuesta:** Lidera el ranking el tipo de equipo **`Infantry Fighting Vehicles`** con **7.445** incidentes.

---

### Ejercicio 2: Sistemas con más incidentes destruidos (Top-5)
Relacionar `incidentes`, `sistemas` y `estados`. Considerar solamente los registros cuyo estado sea `'Destruido'`. Mostrar `sistema` y `cantidad_destruidos`, ordenar de mayor a menor y limitar a cinco resultados.
- **Pregunta:** ¿Qué sistema lidera el ranking?
- **Pauta:** *(JOIN + WHERE + GROUP BY + ORDER BY + LIMIT)*

```sql
SELECT s.nombre AS sistema, COUNT(*) AS cantidad_destruidos
FROM sistemas AS s
INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
INNER JOIN estados AS e ON (i.id_estado = e.id_estado)
WHERE e.nombre = 'Destruido'
GROUP BY s.nombre
ORDER BY cantidad_destruidos DESC
LIMIT 5;
```

#### Resultado de la consulta
| sistema | cantidad_destruidos |
| :--- | ---: |
| BMP-2(K) | 1390 |
| MT-LB | 1358 |
| BMP-1(P) | 1262 |
| Ural-4320 | 1173 |
| KamAZ 6x6 | 1035 |

- **Respuesta:** Lidera el ranking el sistema **`BMP-2(K)`** con **1.390** unidades destruidas.

---

### Ejercicio 3: Porcentaje de incidentes destruidos por tipo de equipo
Calcular para cada tipo de equipo el porcentaje de incidentes destruidos respecto del total de incidentes de ese tipo. Mostrar `tipo_equipo`, `total_incidentes`, `destruidos` y `porcentaje_destruidos`. Considerar solamente tipos con 100 incidentes totales o más.
- **Pregunta:** ¿Qué tipo presenta el porcentaje más alto?
- **Tip:** Utilizar dos subconsultas en `FROM` como tablas derivadas, una para los totales y otra para los destruidos, y relacionarlas por `id_tipo` (o nombre de tipo de equipo). El porcentaje se puede calcular en la consulta principal como: `total_destruidos * 100.0 / total_incidentes` (a esto se le puede aplicar un `ROUND(cálculo, 2)` para limitar decimales).

```sql
SELECT 
    t_totales.tipo_equipo,
    t_totales.total_incidentes,
    t_destruidos.destruidos,
    ROUND(t_destruidos.destruidos * 100.0 / t_totales.total_incidentes, 2) AS porcentaje_destruidos
FROM (
    SELECT t.nombre AS tipo_equipo, COUNT(*) AS total_incidentes    
    FROM tipos_equipo AS t 
    INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
    INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema) 
    GROUP BY t.nombre
) AS t_totales
INNER JOIN (
    SELECT t.nombre AS tipo_equipo, COUNT(*) AS destruidos    
    FROM tipos_equipo AS t 
    INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
    INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
    INNER JOIN estados AS e ON (i.id_estado = e.id_estado)
    WHERE e.nombre = 'Destruido'
    GROUP BY t.nombre
) AS t_destruidos ON (t_totales.tipo_equipo = t_destruidos.tipo_equipo)
WHERE t_totales.total_incidentes >= 100
ORDER BY porcentaje_destruidos DESC;
```

#### Resultado de la consulta
| tipo_equipo | total_incidentes | destruidos | porcentaje_destruidos |
| :--- | ---: | ---: | ---: |
| Helicopters | 243 | 199 | 81.89 |
| Self-Propelled Artillery | 1575 | 1269 | 80.57 |
| Aircraft | 569 | 458 | 80.49 |
| Trucks, Vehicles, and Jeeps | 6795 | 5181 | 76.25 |
| Multiple Rocket Launchers | 591 | 446 | 75.47 |
| Surface-To-Air Missile Systems | 709 | 522 | 73.62 |
| Infantry Mobility Vehicles | 1055 | 761 | 72.13 |
| Armoured Personnel Carriers | 5305 | 3786 | 71.37 |
| Infantry Fighting Vehicles | 7445 | 5298 | 71.16 |
| Unmanned Combat Aerial Vehicles | 506 | 353 | 69.76 |
| Armoured Fighting Vehicles | 2052 | 1378 | 67.15 |
| Radars | 137 | 92 | 67.15 |
| Mine-Resistant Ambush Protected | 727 | 485 | 66.71 |
| Tanks | 7261 | 4702 | 64.76 |
| Self-Propelled Anti-Aircraft Guns | 156 | 100 | 64.10 |
| Towed Artillery | 908 | 544 | 59.91 |
| Reconnaissance Unmanned Aerial Vehicles | 228 | 126 | 55.26 |
| Engineering Vehicles And Equipment | 827 | 452 | 54.66 |
| Command Posts And Communications Stations | 363 | 192 | 52.89 |
| Naval Ships | 137 | 66 | 48.18 |
| Self-Propelled Anti-Tank Missile Systems | 117 | 33 | 28.21 |

- **Respuesta:** El tipo de equipo con el porcentaje más alto de destruidos es **`Helicopters`** con un **81.89%** (199 destruidos sobre 243 incidentes).

---

### Ejercicio 4: Variación de incidentes 2024 → 2025 por tipo de equipo
Comparar la cantidad de incidentes de cada tipo entre `'2024-01-01'` y `'2024-08-21'` contra el mismo período de 2025. Mostrar `tipo_equipo`, `incidentes_2024`, `incidentes_2025` y `variacion = incidentes_2025 - incidentes_2024`.
- **Pregunta:** ¿Qué tipo tuvo el mayor crecimiento absoluto?
- **Tip:** Utilizar dos subconsultas en `FROM` y relacionarlas por `id_tipo` (o nombre de tipo de equipo).

```sql
SELECT 
    incidentes_2024.tipo_equipo,
    incidentes_2024.t_incidentes_2024 AS incidentes_2024,
    incidentes_2025.t_incidentes_2025 AS incidentes_2025,
    (incidentes_2025.t_incidentes_2025 - incidentes_2024.t_incidentes_2024) AS variacion
FROM (
    SELECT t.nombre AS tipo_equipo, COUNT(*) AS t_incidentes_2024
    FROM tipos_equipo AS t
    INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
    INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
    WHERE i.fecha BETWEEN '2024-01-01' AND '2024-08-21'
    GROUP BY t.nombre
) AS incidentes_2024 
INNER JOIN (
    SELECT t.nombre AS tipo_equipo, COUNT(*) AS t_incidentes_2025
    FROM tipos_equipo AS t
    INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
    INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
    WHERE i.fecha BETWEEN '2025-01-01' AND '2025-08-21'
    GROUP BY t.nombre
) AS incidentes_2025 ON (incidentes_2024.tipo_equipo = incidentes_2025.tipo_equipo)
ORDER BY variacion DESC;
```

#### Resultado de la consulta
| tipo_equipo | incidentes_2024 | incidentes_2025 | variacion |
| :--- | ---: | ---: | ---: |
| Mine-Resistant Ambush Protected | 78 | 284 | 206 |
| Infantry Mobility Vehicles | 110 | 297 | 187 |
| Armoured Fighting Vehicles | 311 | 402 | 91 |
| Unmanned Ground Vehicles | 3 | 50 | 47 |
| Command Posts And Communications Stations | 20 | 50 | 30 |
| Reconnaissance Unmanned Aerial Vehicles | 2 | 22 | 20 |
| Unmanned Combat Aerial Vehicles | 88 | 104 | 16 |
| Anti-Aircraft Guns | 2 | 4 | 2 |
| Artillery Support Vehicles And Equipment | 13 | 10 | -3 |
| Naval Ships | 27 | 24 | -3 |
| Jammers And Deception Systems | 7 | 3 | -4 |
| Self-Propelled Anti-Tank Missile Systems | 5 | 1 | -4 |
| Helicopters | 22 | 16 | -6 |
| Radars | 32 | 18 | -14 |
| Self-Propelled Anti-Aircraft Guns | 32 | 14 | -18 |
| Multiple Rocket Launchers | 101 | 71 | -30 |
| Self-Propelled Artillery | 292 | 262 | -30 |
| Towed Artillery | 153 | 116 | -37 |
| Armoured Personnel Carriers | 1041 | 996 | -45 |
| Surface-To-Air Missile Systems | 139 | 85 | -54 |
| Aircraft | 136 | 80 | -56 |
| Engineering Vehicles And Equipment | 269 | 123 | -146 |
| Trucks, Vehicles, and Jeeps | 965 | 744 | -221 |
| Infantry Fighting Vehicles | 1671 | 1105 | -566 |
| Tanks | 1553 | 861 | -692 |

- **Respuesta:** El tipo de equipo que tuvo el mayor crecimiento absoluto fue **`Mine-Resistant Ambush Protected`** con una variación de **`+206`** incidentes (de 78 a 284).

---

### Ejercicio 5: Fuentes con mayor variedad de sistemas (Top-5)
Relacionar `incidentes` con `fuentes`. Para cada dominio contar cuántos sistemas diferentes aparecen asociados a esa fuente mediante `COUNT(DISTINCT ...)`. Mostrar `dominio` y `sistemas_distintos`, ordenar de mayor a menor y limitar a cinco resultados.
- **Pregunta:** ¿Qué fuente lidera el ranking?

```sql
SELECT f.dominio, COUNT(DISTINCT i.id_sistema) AS sistemas_distintos
FROM fuentes AS f
INNER JOIN incidentes AS i ON (f.id_fuente = i.id_fuente)
GROUP BY f.dominio
ORDER BY sistemas_distintos DESC
LIMIT 5;
```

#### Resultado de la consulta
| dominio | sistemas_distintos |
| :--- | ---: |
| i.postimg.cc | 1108 |
| twitter.com | 416 |
| postimg.cc | 136 |
| postlmg.cc | 56 |
| x.com | 12 |

- **Respuesta:** Lidera el ranking la fuente **`i.postimg.cc`** con **1.108** sistemas distintos registrados.

---

### Ejercicio 6: Incidentes por millón de habitantes
Calcular para cada país operador la cantidad de incidentes por millón de habitantes: `cantidad_incidentes * 1000000 / poblacion`. Mostrar `pais`, `cantidad_incidentes`, `poblacion` e `incidentes_por_millon`.
- **Pregunta:** ¿Qué país operador presenta el valor más alto?
- **Tip:** Obtener primero los incidentes por operador mediante una subconsulta en `FROM` y luego relacionar ese resultado con `paises`.

```sql
SELECT 
    p.nombre AS pais,
    incidentes_operador.cantidad_incidentes,
    p.poblacion,
    ROUND(incidentes_operador.cantidad_incidentes * 1000000.0 / p.poblacion, 2) AS incidentes_por_millon
FROM (
    SELECT id_pais_operador, COUNT(*) AS cantidad_incidentes
    FROM incidentes
    GROUP BY id_pais_operador
) AS incidentes_operador
INNER JOIN paises AS p ON (incidentes_operador.id_pais_operador = p.id_pais)
ORDER BY incidentes_por_millon DESC;
```

#### Resultado de la consulta
| pais | cantidad_incidentes | poblacion | incidentes_por_millon |
| :--- | ---: | ---: | ---: |
| Ukraine | 11111 | 37800000 | 293.94 |
| Russia | 26851 | 143500000 | 187.11 |

- **Respuesta:** El país operador que presenta el valor más alto de incidentes por millón de habitantes es **`Ukraine`** con **293.94** incidentes por millón de habitantes (frente a 187.11 de `Russia`).

---

### Ejercicio 7: Tipos de equipo por encima del promedio de incidentes
Calcular la cantidad de incidentes de cada tipo de equipo y mostrar solamente aquellos cuya cantidad sea superior al promedio de incidentes entre todos los tipos. Mostrar `tipo_equipo` y `cantidad_incidentes`.
- **Pregunta:** ¿Cuántos tipos superan el promedio?
- **Tip:** Usar una subconsulta en `HAVING` con operador `>`. Esta subconsulta hará el `AVG` sobre otra subconsulta (anidada) que calcule el total por tipo de equipo — similar a la principal—.

```sql
SELECT t.nombre AS tipo_equipo, COUNT(*) AS cantidad_incidentes
FROM tipos_equipo AS t
INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
GROUP BY t.nombre
HAVING COUNT(*) > (
    SELECT AVG(totales.cantidad_incidentes)
    FROM (
        SELECT COUNT(*) AS cantidad_incidentes
        FROM tipos_equipo AS t2
        INNER JOIN sistemas AS s2 ON (t2.id_tipo = s2.id_tipo)
        INNER JOIN incidentes AS i2 ON (s2.id_sistema = i2.id_sistema)
        GROUP BY t2.nombre
    ) AS totales
)
ORDER BY cantidad_incidentes DESC;
```

#### Resultado de la consulta
| tipo_equipo | cantidad_incidentes |
| :--- | ---: |
| Infantry Fighting Vehicles | 7445 |
| Tanks | 7261 |
| Trucks, Vehicles, and Jeeps | 6795 |
| Armoured Personnel Carriers | 5305 |
| Armoured Fighting Vehicles | 2052 |
| Self-Propelled Artillery | 1575 |

- **Respuesta:** **6 tipos de equipo** superan el promedio general de incidentes (que es de `1309.03` incidentes por tipo).

---

### Ejercicio 8: Diferencia entre Tanks y el promedio de incidentes
Calcular la cantidad de incidentes correspondientes al tipo `'Tanks'` y compararla con el promedio de incidentes de todos los tipos de equipo. Mostrar `tipo_equipo`, `cantidad_incidentes`, `promedio_general` y `diferencia_promedio`.
- **Pregunta:** ¿Cuánto se encuentra Tanks por encima o por debajo del promedio?
- **Tip:** Utilizar una subconsulta escalar en `SELECT`. En este caso, la subconsulta NO es correlacionada, usa `AVG` contra otra subconsulta (anidada) de manera similar al ejercicio 7. Recuerde aplicar la diferencia entre el total de incidentes de `'Tanks'` y esta subconsulta.

```sql
SELECT 
    t.nombre AS tipo_equipo,
    COUNT(*) AS cantidad_incidentes,
    ROUND((
        SELECT AVG(totales.cantidad_incidentes)
        FROM (
            SELECT COUNT(*) AS cantidad_incidentes
            FROM tipos_equipo AS t2
            INNER JOIN sistemas AS s2 ON (t2.id_tipo = s2.id_tipo)
            INNER JOIN incidentes AS i2 ON (s2.id_sistema = i2.id_sistema)
            GROUP BY t2.nombre
        ) AS totales
    ), 2) AS promedio_general,
    ROUND(COUNT(*) - (
        SELECT AVG(totales.cantidad_incidentes)
        FROM (
            SELECT COUNT(*) AS cantidad_incidentes
            FROM tipos_equipo AS t2
            INNER JOIN sistemas AS s2 ON (t2.id_tipo = s2.id_tipo)
            INNER JOIN incidentes AS i2 ON (s2.id_sistema = i2.id_sistema)
            GROUP BY t2.nombre
        ) AS totales
    ), 2) AS diferencia_promedio
FROM tipos_equipo AS t
INNER JOIN sistemas AS s ON (t.id_tipo = s.id_tipo)
INNER JOIN incidentes AS i ON (s.id_sistema = i.id_sistema)
WHERE t.nombre = 'Tanks'
GROUP BY t.nombre;
```

#### Resultado de la consulta
| tipo_equipo | cantidad_incidentes | promedio_general | diferencia_promedio |
| :--- | ---: | ---: | ---: |
| Tanks | 7261 | 1309.03 | 5951.97 |

- **Respuesta:** El tipo **`Tanks`** se encuentra **5.951,97 incidentes por encima** del promedio general (`7261` incidentes frente a un promedio de `1309.03`).

---

### Ejercicio 9: Países cuyo gasto en defensa supera el promedio global
Calcular el promedio global de `defensa_millones_usd` y mostrar solamente los países cuyo gasto sea superior a ese promedio. Mostrar `nombre` y `defensa_millones_usd`, ordenados de mayor a menor.
- **Pregunta:** ¿Cuántos países superan el promedio?
- **Pauta:** *(Subconsulta en WHERE)*

```sql
SELECT nombre, defensa_millones_usd
FROM paises
WHERE defensa_millones_usd > (
    SELECT AVG(defensa_millones_usd)
    FROM paises
)
ORDER BY defensa_millones_usd DESC;
```

#### Resultado de la consulta
| nombre | defensa_millones_usd |
| :--- | ---: |
| United States | 997000 |
| China | 314000 |
| Russia | 149000 |
| Germany | 88500 |
| United Kingdom | 81800 |
| France | 64700 |

- **Respuesta:** **6 países** superan el gasto promedio global en defensa (`United States`, `China`, `Russia`, `Germany`, `United Kingdom` y `France`).

---

### Ejercicio 10: Países con gasto superior al promedio que no aparecen como operadores
Partiendo del promedio global de `defensa_millones_usd`, mostrar los países cuyo gasto sea superior al promedio y que, además, no aparezcan como país operador en ningún incidente. Mostrar `nombre` y `defensa_millones_usd`.
- **Pregunta:** ¿Cuántos países cumplen ambas condiciones y cuáles son?
- **Pauta:** *(Subconsulta escalar + NOT EXISTS)*

```sql
SELECT p.nombre, p.defensa_millones_usd
FROM paises AS p
WHERE p.defensa_millones_usd > (
    SELECT AVG(defensa_millones_usd)
    FROM paises
)
AND NOT EXISTS (
    SELECT 1
    FROM incidentes AS i
    WHERE i.id_pais_operador = p.id_pais
)
ORDER BY p.defensa_millones_usd DESC;
```

#### Resultado de la consulta
| nombre | defensa_millones_usd |
| :--- | ---: |
| United States | 997000 |
| China | 314000 |
| Germany | 88500 |
| United Kingdom | 81800 |
| France | 64700 |

- **Respuesta:** Cumplen ambas condiciones **5 países**: **`United States`**, **`China`**, **`Germany`**, **`United Kingdom`** y **`France`** (todos los del ejercicio anterior excepto `Russia`, que sí registra incidentes como operador).
