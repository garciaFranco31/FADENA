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

```SQL
    SELECT t.nombre as tipo_equipo, count(*) as cantidad_incidentes	
    FROM tipos_equipo as t 
    INNER JOIN sistemas as s on (t.id_tipo = s.id_tipo)
    INNER JOIN incidentes as i on (s.id_sistema = i.id_sistema) 
    GROUP BY t.nombre
    ORDER BY cantidad_incidentes DESC
    LIMIT 5
```

---

### Ejercicio 2: Sistemas con más incidentes destruidos (Top-5)
Relacionar `incidentes`, `sistemas` y `estados`. Considerar solamente los registros cuyo estado sea `'Destruido'`. Mostrar `sistema` y `cantidad_destruidos`, ordenar de mayor a menor y limitar a cinco resultados.
- **Pregunta:** ¿Qué sistema lidera el ranking?
- **Pauta:** *(JOIN + WHERE + GROUP BY + ORDER BY + LIMIT)*

```SQL
    SELECT s.nombre AS sistema, count(*) AS cantidad_destruidos
    FROM sistemas as s
    INNER JOIN incidentes as i ON (s.id_sistema = i.id_sistema)
    INNER JOIN estados as e ON (i.id_estado = e.id_estado)
    WHERE e.nombre IS "Destruido"
    GROUP BY s.nombre
    ORDER BY cantidad_destruidos DESC
    LIMIT 5
```
#### Resultado de la consulta
| Sistema | cantidad_destruidos |
| --- | --- |
|BMP-2K	|1390 |
|MT-LB	|1358 |
|BMP-1(P) | 1262 |
|Ural-4320 | 1173 |
|KamAZ 6x6 | 1035 |

---

### Ejercicio 3: Porcentaje de incidentes destruidos por tipo de equipo
Calcular para cada tipo de equipo el porcentaje de incidentes destruidos respecto del total de incidentes de ese tipo. Mostrar `tipo_equipo`, `total_incidentes`, `destruidos` y `porcentaje_destruidos`. Considerar solamente tipos con 100 incidentes totales o más.
- **Pregunta:** ¿Qué tipo presenta el porcentaje más alto?
- **Tip:** Utilizar dos subconsultas en `FROM` como tablas derivadas, una para los totales y otra para los destruidos, y relacionarlas por `id_tipo` (o nombre de tipo de equipo). El porcentaje se puede calcular en la consulta principal como: `total_destruidos * 100.0 / total_incidentes` (a esto se le puede aplicar un `ROUND(cálculo, 2)` para limitar decimales).

```SQL

```

---

### Ejercicio 4: Variación de incidentes 2024 → 2025 por tipo de equipo
Comparar la cantidad de incidentes de cada tipo entre `'2024-01-01'` y `'2024-08-21'` contra el mismo período de 2025. Mostrar `tipo_equipo`, `incidentes_2024`, `incidentes_2025` y `variacion = incidentes_2025 - incidentes_2024`.
- **Pregunta:** ¿Qué tipo tuvo el mayor crecimiento absoluto?
- **Tip:** Utilizar dos subconsultas en `FROM` y relacionarlas por `id_tipo` (o nombre de tipo de equipo).

```SQL

```

---

### Ejercicio 5: Fuentes con mayor variedad de sistemas (Top-5)
Relacionar `incidentes` con `fuentes`. Para cada dominio contar cuántos sistemas diferentes aparecen asociados a esa fuente mediante `COUNT(DISTINCT ...)`. Mostrar `dominio` y `sistemas_distintos`, ordenar de mayor a menor y limitar a cinco resultados.
- **Pregunta:** ¿Qué fuente lidera el ranking?

```SQL

```

---

### Ejercicio 6: Incidentes por millón de habitantes
Calcular para cada país operador la cantidad de incidentes por millón de habitantes: `cantidad_incidentes * 1000000 / poblacion`. Mostrar `pais`, `cantidad_incidentes`, `poblacion` e `incidentes_por_millon`.
- **Pregunta:** ¿Qué país operador presenta el valor más alto?
- **Tip:** Obtener primero los incidentes por operador mediante una subconsulta en `FROM` y luego relacionar ese resultado con `paises`.

```SQL

```

---

### Ejercicio 7: Tipos de equipo por encima del promedio de incidentes
Calcular la cantidad de incidentes de cada tipo de equipo y mostrar solamente aquellos cuya cantidad sea superior al promedio de incidentes entre todos los tipos. Mostrar `tipo_equipo` y `cantidad_incidentes`.
- **Pregunta:** ¿Cuántos tipos superan el promedio?
- **Tip:** Usar una subconsulta en `HAVING` con operador `>`. Esta subconsulta hará el `AVG` sobre otra subconsulta (anidada) que calcule el total por tipo de equipo — similar a la principal—.

```SQL

```

---

### Ejercicio 8: Diferencia entre Tanks y el promedio de incidentes
Calcular la cantidad de incidentes correspondientes al tipo `'Tanks'` y compararla con el promedio de incidentes de todos los tipos de equipo. Mostrar `tipo_equipo`, `cantidad_incidentes`, `promedio_general` y `diferencia_promedio`.
- **Pregunta:** ¿Cuánto se encuentra Tanks por encima o por debajo del promedio?
- **Tip:** Utilizar una subconsulta escalar en `SELECT`. En este caso, la subconsulta NO es correlacionada, usa `AVG` contra otra subconsulta (anidada) de manera similar al ejercicio 7. Recuerde aplicar la diferencia entre el total de incidentes de `'Tanks'` y esta subconsulta.

```SQL

```

---

### Ejercicio 9: Países cuyo gasto en defensa supera el promedio global
Calcular el promedio global de `defensa_millones_usd` y mostrar solamente los países cuyo gasto sea superior a ese promedio. Mostrar `nombre` y `defensa_millones_usd`, ordenados de mayor a menor.
- **Pregunta:** ¿Cuántos países superan el promedio?
- **Pauta:** *(Subconsulta en WHERE)*

```SQL

```

---

### Ejercicio 10: Países con gasto superior al promedio que no aparecen como operadores
Partiendo del promedio global de `defensa_millones_usd`, mostrar los países cuyo gasto sea superior al promedio y que, además, no aparezcan como país operador en ningún incidente. Mostrar `nombre` y `defensa_millones_usd`.
- **Pregunta:** ¿Cuántos países cumplen ambas condiciones y cuáles son?
- **Pauta:** *(Subconsulta escalar + NOT EXISTS)*

```SQL

```