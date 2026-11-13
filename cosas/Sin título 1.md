# UD4 – Realización de consultas

Módulo: Bases de datos · DAM · CEAC

---

## Índice

- Introducción
- Objetivos
- Esquema de referencia
- 1. Proyección, selección y ordenación de registros
- 2. Operadores. Operadores de comparación. Operadores lógicos
- 3. Consultas de resumen
- 4. Consolidación de registros
- 5. Composiciones internas
- 6. Composiciones externas
- 7. Subconsultas
- 8. Combinación de múltiples selecciones
- 9. Optimización de consultas
- Resumen
- Glosario

---

## Introducción

- Una base de datos almacena información estructurada que se consulta con instrucciones precisas.
- No basta con acceder a la tabla: hay que elegir columnas, filtrar filas, ordenar resultados y combinar información de varias tablas.
- Se estudian las consultas SQL más habituales: proyección, selección, ordenación, operadores de comparación y lógicos, consultas de resumen, agrupaciones, composiciones internas y externas, subconsultas, operadores de conjunto y funciones de ventana.
- También se introduce la optimización: plan de ejecución, índices, revisión de filtros y actualización de estadísticas.
- Permiten escribir consultas más claras, reducir lecturas innecesarias y obtener resultados ajustados a cada necesidad de la aplicación.

## Objetivos

- Identificar las herramientas y sentencias para realizar consultas en bases de datos.
- Realizar consultas simples sobre una tabla.
- Realizar consultas sobre varias tablas mediante composiciones internas y externas.
- Ejecutar consultas resumen y con subconsultas.
- Realizar consultas que impliquen múltiples selecciones.
- Aplicar criterios de optimización para reducir lecturas innecesarias y ajustar los tiempos de respuesta.
- Aplicar funciones de ventana para realizar análisis y clasificaciones sobre conjuntos de datos.

## Esquema de referencia

Las tablas `clientes` y `pedidos` se usan como esquema de referencia en varios ejemplos.

**clientes(id_cliente, nombre, ciudad)**

|id_cliente|nombre|ciudad|
|---|---|---|
|1|Ana|Madrid|
|2|Luis|Barcelona|
|3|Marta|Madrid|
|4|Carlos|Valencia|

**pedidos(id_pedido, id_cliente, fecha, importe)**

|id_pedido|id_cliente|fecha|importe|
|---|---|---|---|
|101|1|2026-01-15|120,50|
|102|2|2026-01-18|75,00|
|103|1|2026-02-02|45,30|
|104|3|2026-02-10|200,00|

---

## 1. Proyección, selección y ordenación de registros

- Dos acciones al buscar datos:
    - **Proyección:** elige las columnas que se desean mostrar.
    - **Selección:** filtra las filas que cumplen las condiciones indicadas.
- **Una consulta bien diseñada no debe pedir todos los datos de forma indiscriminada, sino solo las columnas y filas necesarias para resolver una necesidad concreta.**
- Proyección → `SELECT` (alias, cálculos, `DISTINCT`).
- Selección → `WHERE` (operadores lógicos y de comparación).
- Ordenación → `ORDER BY` (ascendente o descendente, varias columnas, posición de los `NULL`).

### 1.1. Cláusula SELECT y proyección de columnas: alias, expresiones calculadas y DISTINCT

- `SELECT` indica exactamente qué datos se muestran. Esa elección de columnas se llama **proyección**.

```sql
SELECT nombre, correo FROM clientes;
```

Muestra solo `nombre` y `correo`, aunque la tabla tenga más campos.

- **Alias de columna:** nombre temporal con un título más legible en el resultado. No cambia el nombre real de la columna en la base de datos.

```sql
SELECT nombre AS Nombre_Cliente FROM clientes;
```

- **Expresiones calculadas:** cálculos directos a partir de los valores de las columnas.

```sql
SELECT producto, precio * cantidad AS total FROM lineas_pedido;
```

- **DISTINCT:** se escribe justo después de `SELECT`. Elimina duplicados del resultado y muestra cada valor una sola vez.

```sql
SELECT DISTINCT ciudad FROM clientes;
```

**Resultado esperado:** 3 filas — Madrid, Barcelona, Valencia (una por cada ciudad distinta de `clientes`).

### 1.2. Cláusula WHERE y selección de filas: condiciones simples y compuestas

- `WHERE` funciona como un filtro. **`SELECT` decide qué columnas se muestran; `WHERE` decide qué filas aparecen.**
- Condiciones **simples:** operadores de comparación (igual, distinto, mayor que, menor que).

```sql
SELECT nombre, edad FROM clientes WHERE edad >= 18;
```

- Condiciones **compuestas:** operadores lógicos `AND` y `OR`.
    - `AND`: se cumplen todas las condiciones.
    - `OR`: se cumple al menos una.
    - Al mezclar operadores conviene usar **paréntesis** para evitar resultados inesperados.

|Tipo|Herramienta|Ejemplo|
|---|---|---|
|Simple|Comparación (`=`, `>`)|`edad = 18`|
|Compuesta|Lógicos (`AND`, `OR`)|`edad = 18 AND ciudad = 'Madrid'`|

```sql
SELECT nombre, ciudad, edad FROM clientes
WHERE ciudad = 'Madrid' AND (edad < 18 OR edad > 65);
```

Muestra clientes de Madrid que son menores de edad o mayores de 65 años.

> 💡 **Recuerda:** la proyección determina qué columnas se muestran; la selección filtra qué filas aparecen en el resultado final.

### 1.3. Cláusula ORDER BY: ordenación ascendente y descendente, múltiples columnas y valores NULL

- `ORDER BY` organiza los resultados. Funciona con números, textos y fechas.
- Por defecto el orden es **ascendente** (A→Z, menor→mayor). Para el inverso se usa **`DESC`**.
- Se puede ordenar por **varias columnas** a la vez (para resolver empates).

```sql
-- Ascendente
SELECT nombre, apellido FROM alumnos ORDER BY apellido ASC;

-- Descendente
SELECT nombre, salario FROM empleados ORDER BY salario DESC;

-- Varias columnas
SELECT nombre, provincia, apellido FROM alumnos
ORDER BY provincia ASC, apellido ASC;
```

|Orden|Textos|Números|
|---|---|---|
|Ascendente|De la A a la Z|Del menor al mayor|
|Descendente|De la Z a la A|Del mayor al menor|

- **Valores `NULL`:** el sistema los coloca en una posición determinada (principio o final). **Esta posición puede variar según el SGBD**, aunque algunos permiten indicar expresamente si los `NULL` van al principio o al final.

> 💡 **Recuerda:** `ORDER BY` aplica orden ascendente por defecto, aunque puede indicarse orden descendente mediante `DESC`.

---

## 2. Operadores. Operadores de comparación. Operadores lógicos

- **Operadores de comparación:** símbolos matemáticos y comandos como `BETWEEN`, `IN` y `LIKE` (rangos numéricos, listas de valores o textos concretos).
- **Operadores lógicos** `AND`, `OR`, `NOT`: evalúan varias reglas a la vez. Conviene agrupar con paréntesis.
- **Operadores especiales:** `IS NULL`, `IS NOT NULL`, `EXISTS`, `ANY`, `ALL`.
    - `EXISTS`, `ANY` y `ALL` se usan para comprobar resultados de consultas secundarias.
    - **Subconsulta:** consulta incluida dentro de otra instrucción SQL.

### 2.1. Operadores de comparación: =, <>, <, >, <=, >=, BETWEEN, IN y LIKE

- Se escriben normalmente dentro de `WHERE`.
- `=` → coincidencias exactas.
- `>=` (y similares) → cantidades que superan un límite.
- `<>` → distinto; excluye los registros que no cumplen la condición.

```sql
SELECT nombre, edad FROM clientes WHERE edad >= 18;
SELECT producto, precio FROM productos WHERE precio <> 0;
```

- **`BETWEEN`:** intervalo específico; **incluye normalmente los dos extremos**.
- **`IN`:** comprueba si un dato está dentro de una lista exacta de valores.
- **`LIKE`:** busca partes o patrones de texto. Comodines: `%` = cualquier cadena de caracteres; `_` = un único carácter.

```sql
WHERE edad BETWEEN 18 AND 65
WHERE ciudad IN ('Madrid', 'Barcelona', 'Valencia')
WHERE nombre LIKE 'Mar%'
```

El último busca nombres que empiecen por "Mar": Marta, Marcos, Marina.

|Operador|Uso práctico|
|---|---|
|`=`|Busca datos exactamente iguales|
|`BETWEEN`|Encuentra valores dentro de un rango determinado|
|`LIKE`|Localiza un fragmento de texto específico|

### 2.2. Operadores lógicos: AND, OR y NOT; precedencia y uso de paréntesis

- Combinan condiciones dentro de `WHERE` para acotar la búsqueda.
- `AND`: exige que se cumplan todas las condiciones.
- `OR`: devuelve resultados cuando se cumple al menos una.
- `NOT`: invierte una regla y descarta información (excluye registros que cumplan una condición concreta).
- **Orden de evaluación:** lo que está entre paréntesis se resuelve antes. En SQL, `AND` suele tener prioridad sobre `OR`, por lo que los paréntesis ayudan a expresar exactamente la intención de la consulta.

|Operador|Significado|Ejemplo práctico|
|---|---|---|
|`AND`|Todas las reglas son ciertas|`edad >= 18 AND ciudad = 'Madrid'`|
|`OR`|Al menos una regla es cierta|`ciudad = 'Madrid' OR ciudad = 'Valencia'`|
|`NOT`|Descarta una regla|`NOT activo`|

> 💡 **Recuerda:** los paréntesis permiten controlar el orden de evaluación de las condiciones y evitar resultados inesperados.

### 2.3. Operadores especiales: IS NULL, IS NOT NULL, EXISTS y ANY/ALL

**IS NULL / IS NOT NULL**

- Localizan valores ausentes o presentes en una columna.
- **No se debe usar `= NULL`**, porque `NULL` representa ausencia de valor y se trata con una lógica especial.

```sql
SELECT nombre FROM clientes WHERE telefono IS NULL;
SELECT nombre FROM clientes WHERE telefono IS NOT NULL;
```

**EXISTS**

- Comprueba si hay información relacionada: revisa si una búsqueda interna (subconsulta) encuentra datos. Si obtiene resultados, la condición de la consulta externa se cumple.
- Ejemplo de uso: saber si un cliente tiene facturas antes de borrarlo.

```sql
SELECT nombre FROM clientes c
WHERE EXISTS (SELECT 1 FROM pedidos p WHERE p.id_cliente = c.id_cliente);
```

Devuelve los clientes con al menos un pedido. **Resultado esperado:** 3 filas — Ana, Luis, Marta (Carlos queda excluido porque no tiene ningún pedido).

**ANY / ALL**

- Comparan un valor con una lista. Se combinan con un operador de comparación (`>`, `<`, `>=`, `<=`).
- `ANY`: el dato solo necesita cumplir la regla con **un** elemento.
- `ALL`: el dato debe cumplir la regla con **todos** los elementos.

```sql
-- ANY: mayor que al menos uno de los precios de la subconsulta
WHERE precio > ANY (SELECT precio FROM productos WHERE categoria = 'Libros')

-- ALL: mayor que todos los precios de la subconsulta
WHERE precio > ALL (SELECT precio FROM productos WHERE categoria = 'Libros')
```

|Operador|Aplicación|Ejemplo práctico|
|---|---|---|
|`IS NULL`|Localizar valores ausentes|Clientes sin teléfono|
|`EXISTS`|Comprobar resultados|Filtrar clientes con compras|
|`ANY` / `ALL`|Comparar con listas|Productos más caros que otros|

---

## 3. Consultas de resumen

- Interesa obtener valores resumidos en lugar de revisar filas individuales: ventas totales, medias, recuentos (ej.: volumen total de ventas, salario medio de los empleados).
- **Funciones de agregación:** `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`. Calculan directamente sobre el conjunto de filas, sin procesarlas una a una.
- **Cuidado con los `NULL`:** muchas funciones de agregación los ignoran, mientras que `COUNT(*)` cuenta todas las filas.
- **`GROUP BY`:** agrupa las filas de una tabla; se usa junto con funciones de agregación (ej.: ventas agrupadas por ciudad).
- **`HAVING`:** filtra los grupos resultantes usando los cálculos ya realizados. Se diferencia de `WHERE`, que filtra solo registros individuales antes de agrupar.

```sql
SELECT ciudad, COUNT(*) AS total_clientes FROM clientes GROUP BY ciudad;
```

Agrupa los clientes por ciudad y cuenta cuántos hay en cada grupo. **Resultado esperado:** 3 filas — Madrid (2), Barcelona (1), Valencia (1).

### 3.1. Funciones de agregación: COUNT, SUM, AVG, MAX y MIN; comportamiento con NULL

- Calculan totales y devuelven un único resultado. Se aplican a toda una tabla o a grupos definidos con `GROUP BY`.
- **`COUNT`:** cuenta filas.
    - `COUNT(*)`: cuenta todas las filas.
    - `COUNT(columna)`: cuenta solo las filas donde esa columna no es `NULL`.
- **`SUM`:** suma los valores numéricos de una columna (ej.: ingresos totales).
- **`AVG`:** calcula la media (ej.: gasto medio por comprador).
- **`MAX`:** devuelve el valor más alto (ej.: producto más caro).
- **`MIN`:** devuelve el valor más bajo (ej.: artículo más barato).

```sql
SELECT COUNT(*) AS total_clientes FROM clientes;
SELECT SUM(importe) AS ventas_totales FROM pedidos;
SELECT AVG(salario) AS salario_medio FROM empleados;
```
``
- **`NULL`:** en general `SUM`, `AVG`, `MAX`, `MIN` y `COUNT(columna)` ignoran los `NULL`; `COUNT(*)` cuenta todas las filas aunque tengan nulos en alguna columna.

|Función|Acción|Ejemplo práctico|
|---|---|---|
|`COUNT`|Cuenta filas|Contar clientes|
|`SUM`|Suma números|Calcular ingresos|
|`AVG`|Calcula la media|Edad media|
|`MAX`|Busca el mayor|Producto más caro|
|`MIN`|Busca el menor|Artículo más barato|

### 3.2. Cláusula GROUP BY: agrupación de filas y uso conjunto con funciones de agregación

- `GROUP BY` agrupa las filas que comparten el mismo valor en una o varias columnas.
- Crea consultas de resumen que ofrecen una visión general de los datos.
- Después se aplican funciones de agregación (totales, recuentos, promedios, máximos o mínimos) dentro de cada grupo.

```sql
SELECT departamento, COUNT(*) AS total_empleados, AVG(salario) AS salario_medio
FROM empleados
GROUP BY departamento;
```

Agrupa los empleados por departamento y calcula cuántos hay y su salario medio.

|Herramienta|Función|Ejemplo práctico|
|---|---|---|
|Selección y proyección|Elegir filas y columnas|Ver productos|
|`GROUP BY`|Agrupar en categorías|Ventas por mes|
|Funciones de agregación|Calcular valores de resumen|Promedio de precios|

> 💡 **Recuerda:** `GROUP BY` trabaja con las funciones de agregación para transformar registros sueltos en un resumen por grupos.

### 3.3. Cláusula HAVING: filtrado de grupos y diferencias respecto a WHERE

- `WHERE` elimina datos individuales **al principio** del proceso.
- `HAVING` filtra los **grupos completos** una vez formados.

|Condición|Momento de uso|Elemento evaluado|
|---|---|---|
|`WHERE`|Antes de agrupar|Filas individuales|
|`HAVING`|Después de agrupar|Grupos completos|

- Ejemplo de uso: mostrar solo los cursos con más de veinte estudiantes.

```sql
SELECT curso, COUNT(*) AS total_estudiantes
FROM matriculas
GROUP BY curso
HAVING COUNT(*) > 20;
```

Agrupa las matrículas por curso y muestra únicamente los cursos con más de veinte estudiantes.

> 💡 **Recuerda:** `WHERE` filtra filas antes de agrupar; `HAVING` filtra grupos después de aplicar funciones de agregación.

---

## 4. Consolidación de registros

- **Consolidar registros:** transformar datos individuales en un resultado resumido, agrupado o clasificado.
- Herramientas: `DISTINCT`, agrupaciones con `GROUP BY`, funciones de agregación y funciones de ventana.
- **Composiciones internas:** muestran únicamente las filas con coincidencia en ambas partes.
- **Composiciones externas:** mantienen los registros de una tabla principal aunque no tengan pareja en la otra; los datos faltantes se rellenan con `NULL`. (Se desarrollan en los apartados 5 y 6.)
- **Consultas de resumen:** agrupan filas con valores comunes y aplican cálculos (sumar totales, sacar promedios). También puede aplicarse `DISTINCT`.
- **Funciones de ventana (WINDOW functions):** cálculos avanzados sobre bloques de información con `ROW_NUMBER`, `RANK` y `PARTITION BY`, **manteniendo intacto el acceso a cada fila individual**.

### 4.1. Eliminación de duplicados con DISTINCT: funcionamiento y casos de uso

- Si se proyectan columnas que comparten valores, el resultado puede contener filas repetidas.
- `DISTINCT` elimina duplicados **del resultado de la consulta, no de la tabla original**.
- Ejemplo: saber qué clientes han comprado. Sin `DISTINCT`, el `id_cliente` se repite tantas veces como pedidos tenga; con `DISTINCT`, cada cliente aparece una sola vez.

```sql
SELECT DISTINCT id_cliente FROM pedidos;
```

Devuelve el identificador de cada cliente con al menos un pedido, sin repetirlo.

- No debe confundirse con otras acciones: las consultas de resumen calculan totales, las uniones combinan datos de varias tablas y las subconsultas son búsquedas secundarias. `DISTINCT` **solo elimina duplicados** en el resultado.

|Búsqueda|Resultado en pantalla|Ejemplo|
|---|---|---|
|Normal|Muestra datos repetidos|Madrid, Madrid|
|`DISTINCT`|Muestra valores únicos|Madrid|

> 💡 **Recuerda:** `DISTINCT` no modifica los datos almacenados; solo elimina duplicados en el resultado de una consulta concreta.

### 4.2. Agrupación y consolidación de datos: técnicas para obtener resúmenes representativos

- Se calculan resultados globales (sumas totales, promedios). Filtrar separa casos específicos; seleccionar columnas muestra solo los datos necesarios. Así los registros individuales se convierten en datos agregados que resumen el conjunto.
- Primer paso: juntar la información con elementos en común (ej.: ventas por mes), con condiciones si hace falta (ej.: cantidades mayores a un límite).

```sql
SELECT mes, SUM(importe) AS ventas_totales FROM ventas GROUP BY mes;
```

Agrupa las ventas por mes y calcula el importe total de cada grupo.

- Las composiciones internas y externas cruzan datos de tablas diferentes (ej.: clientes con sus compras). Para búsquedas avanzadas se puede emplear una **subconsulta** (una consulta dentro de otra instrucción SQL).

```sql
SELECT ciudad, COUNT(*) AS total_clientes FROM clientes
GROUP BY ciudad HAVING COUNT(*) > 10;
```

|Acción|Propósito|Ejemplo práctico|
|---|---|---|
|Selección|Filtrar filas|Ventas de hoy|
|Proyección|Elegir columnas|Mostrar nombres|
|Agrupación|Reunir filas por valores comunes|Sumar por mes|

### 4.3. Funciones de ventana (WINDOW functions): ROW_NUMBER, RANK y PARTITION BY

- Sirven, por ejemplo, para ordenar alumnos por nota o asignar posiciones individuales.
- **A diferencia de las agrupaciones normales, mantienen todas las filas visibles.**
- **`PARTITION BY`:** divide la tabla en grupos más pequeños (ej.: aulas). Se usa dentro de la cláusula **`OVER`**, que define sobre qué conjunto de filas trabaja la función de ventana. Añade una columna calculada sin ocultar las filas originales.
- **`ROW_NUMBER`:** asigna un número consecutivo a cada fila dentro de los grupos.
- **`RANK`:** otorga la misma posición a valores idénticos; **deja saltos en la numeración cuando hay empates**.

```sql
SELECT alumno, aula, nota,
       RANK() OVER (PARTITION BY aula ORDER BY nota DESC) AS posicion
FROM calificaciones;
```

Clasifica a los alumnos dentro de cada aula según su nota, sin ocultar ninguna fila.

|Valor de la nota|`ROW_NUMBER`|`RANK`|
|---|---|---|
|10|1|1|
|10|2|1|
|8|3|3|

---

## 5. Composiciones internas

- Relacionar `clientes` con `pedidos` permite obtener información combinada.
- **Composición interna (`INNER JOIN`):** solo muestra los resultados que coinciden en ambas partes, a diferencia de las externas.
- Se pueden **encadenar varios `JOIN`** para extraer datos de tres o más tablas, siempre que el código se mantenga claro y ordenado.
- **Self join:** unión de la tabla consigo misma. Requiere alias distintos para diferenciar cada copia (ej.: empleados con sus jefes directos en la misma tabla).
- Esta lógica también puede usar subconsultas: una subconsulta es una petición independiente dentro de la consulta principal, y su resultado puede usarse como criterio de filtrado en la consulta externa.

### 5.1. INNER JOIN: sintaxis, funcionamiento y casos en los que se usa

- Relaciona filas de dos tablas cuando existe coincidencia entre los campos indicados en la condición `ON`.
- Une exclusivamente los datos con relación directa en ambas tablas.
- Uso: emparejar cada cliente con su pedido. Quien no compró nada **no aparece** en el resultado (diferencia con las externas, que sí muestran los elementos sin coincidencias).
- **Estructura:** tabla inicial → `INNER JOIN` + segunda tabla → condición en `ON`.

```sql
SELECT clientes.nombre, pedidos.fecha_pedido, pedidos.importe
FROM clientes
INNER JOIN pedidos ON clientes.id_cliente = pedidos.id_cliente;
```

Muestra solo los clientes que tienen pedidos asociados. **Resultado esperado:** 4 filas — Ana (2026-01-15, 120,50), Luis (2026-01-18, 75,00), Ana (2026-02-02, 45,30) y Marta (2026-02-10, 200,00); Carlos no aparece al no tener pedidos.

> 💡 **Recuerda:** esta orden solo muestra datos con pareja exacta, descartando las filas que no tienen coincidencia en ambas tablas.

### 5.2. Composiciones con múltiples tablas: encadenamiento de JOINs y legibilidad de la consulta

- Se encadena `JOIN` para unir varias tablas; hay que indicar una **condición de relación entre cada par de tablas**.
- Ejemplo: tienda online que une clientes, pedidos y líneas de pedido para saber qué productos ha comprado cada cliente.

```sql
SELECT c.nombre, p.fecha_pedido, lp.producto, lp.cantidad
FROM clientes c
INNER JOIN pedidos p
ON c.id_cliente = p.id_cliente
INNER JOIN lineas_pedido lp
ON p.id_pedido = lp.id_pedido;
```

- **Legibilidad:** una consulta legible se revisa y modifica con menos riesgo.
    - Usar siempre **sangrías y saltos de línea** que separen cada paso.
    - Usar **alias claros** (`c` para clientes, `p` para pedidos) siempre que no dificulten la lectura.
- Revisar con cuidado: una condición mal escrita puede **multiplicar filas** o devolver resultados incorrectos.

### 5.3. Composición de una tabla consigo misma (self join): concepto, sintaxis y ejemplos prácticos

- **Self join (unión propia):** una misma tabla se referencia dos veces mediante alias distintos.
- Ejemplo: empleados donde algunos son jefes de otros. Un alias representa a los empleados y otro a los jefes. Se empareja el identificador del jefe con el empleado correspondiente (con `=`). Se pueden sumar condiciones con `AND`.

```sql
SELECT e.nombre AS empleado, j.nombre AS jefe
FROM empleados e
INNER JOIN empleados j ON e.id_jefe = j.id_empleado;
```

La misma tabla `empleados` se usa dos veces: una para leer los empleados y otra para leer sus jefes.

- Alternativa: usar una subconsulta. La mejor opción depende del caso, del SGBD y del plan de ejecución.
- Una vez unidos los datos, se pueden usar consultas de resumen para contar los empleados a cargo de cada coordinador.

> 💡 **Recuerda:** para cruzar una tabla consigo misma, siempre es necesario asignarle alias diferentes para que el sistema distinga ambas referencias.

---

## 6. Composiciones externas

- Combinan información de varias tablas **sin exigir coincidencia exacta** en todas las filas.
- Conservan los registros de una tabla aunque no haya coincidencias en la otra; los espacios sin datos se rellenan con `NULL`.
- **`LEFT JOIN`:** mantiene todos los registros de la primera tabla (izquierda).
- **`RIGHT JOIN`:** conserva todos los datos de la segunda tabla (derecha).
    - En la práctica `LEFT JOIN` es más habitual porque permite leer la consulta de izquierda a derecha. Muchos `RIGHT JOIN` pueden reescribirse cambiando el orden de las tablas.
- **`FULL OUTER JOIN`:** conserva todos los datos de ambas tablas. No todos los SGBD lo aceptan directamente; en esos casos se combinan otras sentencias para lograr el mismo resultado.
- **`CROSS JOIN`:** producto cartesiano; combina cada elemento de una tabla con los de la otra, sin condición de filtrado. Genera todas las combinaciones posibles y puede aumentar mucho el coste de ejecución.

### 6.1. LEFT JOIN y RIGHT JOIN: funcionamiento, diferencias y casos de uso

- A diferencia de `INNER JOIN` (solo coincidencias), `LEFT JOIN` y `RIGHT JOIN` conservan registros aunque no tengan pareja en la otra tabla.
- **`LEFT JOIN`:** conserva todos los registros de la tabla izquierda. Si una persona no tiene compras, las columnas de la tabla de compras aparecen como `NULL`.

```sql
SELECT c.nombre, p.importe
FROM clientes c
LEFT JOIN pedidos p ON c.id_cliente = p.id_cliente;
```

Muestra todos los clientes, incluso los que todavía no tienen pedidos.

- **`RIGHT JOIN`:** hace el trabajo contrario (mantiene la tabla derecha). Es válido, pero muchas veces se prefiere `LEFT JOIN` invirtiendo el orden de las tablas, por ser más claro de leer y mantener.

|Tipo|Conserva datos|Sin coincidencia|
|---|---|---|
|`LEFT JOIN`|Tabla izquierda|`NULL` en columnas sin coincidencia|
|`RIGHT JOIN`|Tabla derecha|`NULL` en columnas sin coincidencia|

> 💡 **Recuerda:** las composiciones internas exigen coincidencias en ambas listas, pero las externas mantienen los datos solitarios.

### 6.2. FULL OUTER JOIN: concepto, sintaxis y emulación en SGBD que no lo soportan nativamente

- `FULL OUTER JOIN` conserva los registros de ambas tablas, tengan o no coincidencia.

```sql
SELECT c.nombre, p.importe
FROM clientes c
FULL OUTER JOIN pedidos p ON c.id_cliente = p.id_cliente;
```

Mostraría clientes con pedidos, clientes sin pedidos y pedidos sin cliente asociado, si existieran.

> 💡 **Recuerda:** `FULL OUTER JOIN` rellena con valores nulos aquellos datos sin pareja en la tabla opuesta.

|Tipo|Parejas|Datos sueltos|
|---|---|---|
|Interna|Sí|No|
|Externa|Sí|Sí|

- Sintaxis: escribir `FULL OUTER JOIN` entre las dos tablas.
- Algunos SGBD no lo reconocen → se hace una **emulación manual** (imitar su funcionamiento paso a paso).

> 🧠 **Saber más:** en MySQL se simula usando `UNION` para juntar dos consultas distintas sin perder registros.

```sql
SELECT c.nombre, p.importe
FROM clientes c
LEFT JOIN pedidos p ON c.id_cliente = p.id_cliente
UNION
SELECT c.nombre, p.importe
FROM clientes c
RIGHT JOIN pedidos p ON c.id_cliente = p.id_cliente;
```

`UNION` ayuda a evitar duplicados entre ambas partes. Para emular `FULL OUTER JOIN` se combina un `LEFT JOIN` y un `RIGHT JOIN` mediante `UNION`, de modo que se conserven los registros de ambas tablas.

### 6.3. CROSS JOIN: producto cartesiano, casos de uso y precauciones de rendimiento

- `CROSS JOIN` genera el producto cartesiano: combina cada fila de la primera tabla con cada fila de la segunda.
- Mezcla los datos **sin condición `ON`** que relacione las tablas.

```sql
SELECT c.color, t.talla
FROM colores c
CROSS JOIN tallas t;
```

Con 3 colores y 4 tallas → 12 combinaciones.

> 💡 **Recuerda:** el cruce total multiplica datos: una tabla de 100 filas unida a otra de 100 genera 10.000 resultados.

- El volumen crece de forma multiplicativa; al mezclar catálogos enteros puede aumentar mucho el coste de ejecución.
- Solo se usa para evaluar todas las opciones posibles de un conjunto.
- Para evitar resultados excesivamente grandes, conviene aplicar filtros o limitar los conjuntos de entrada.

---

## 7. Subconsultas

- Una subconsulta es una consulta incluida dentro de otra instrucción SQL. Puede usarse para calcular un valor, filtrar registros o construir una tabla derivada.
- Consultas anidadas: filtran usando como criterio el resultado de otra consulta; suelen ir en `WHERE` con operadores de comparación.
- **Subconsulta correlacionada:** la instrucción interna usa valores de la consulta externa; se evalúa fila por fila y depende de esa información exterior.
- También pueden ir en `FROM` y `SELECT`: se crean **tablas derivadas**, a las que siempre se debe asignar un **alias**.
- **CTE (expresiones de tabla comunes):** organizan el código paso a paso. Se escriben con `WITH` y dan nombre a una consulta auxiliar para reutilizarla en la consulta principal.

### 7.1. Subconsultas en la cláusula WHERE: escalares, de fila y de tabla; operadores IN, ANY y ALL

- Una subconsulta puede filtrar datos obtenidos previamente. En `FROM` crea una tabla derivada. Las CTE definen una consulta auxiliar al inicio.
- **Escalar:** devuelve un único valor.

```sql
SELECT nombre, precio
FROM productos
WHERE precio > (SELECT AVG(precio) FROM productos);
```

- **De tabla:** puede devolver varios valores → se usa `IN`.

```sql
SELECT nombre
FROM clientes
WHERE id_cliente IN (SELECT id_cliente FROM pedidos);
```

Muestra clientes que aparecen en la tabla de pedidos.

- Con `ANY` y `ALL` se compara un valor con varios resultados:
    - `ANY`: la condición se cumple con al menos un valor.
    - `ALL`: se cumple con todos los valores devueltos.

### 7.2. Subconsultas correlacionadas: concepto, funcionamiento y diferencias respecto a las no correlacionadas

- **No correlacionadas:** totalmente independientes; resuelven su tarea **una sola vez** y entregan el resultado a la consulta principal. Ej.: buscar el precio más alto de una tienda para compararlo con otros productos.
- **Correlacionada:** no puede funcionar sola; lee continuamente los datos de la consulta exterior y avanza fila por fila. Para cada fila externa, el motor recalcula la subconsulta con el valor de esa fila.

```sql
SELECT e.nombre, e.salario, e.departamento
FROM empleados e
WHERE e.salario > (
  SELECT AVG(e2.salario)
  FROM empleados e2
  WHERE e2.departamento = e.departamento
);
```

Muestra empleados cuyo salario supera la media de su propio departamento. La subconsulta depende del departamento de cada empleado de la consulta externa.

> 💡 **Recuerda:** la subconsulta normal se ejecuta una sola vez, mientras que la correlacionada se repite por cada fila procesada.

|Característica|No correlacionada|Correlacionada|
|---|---|---|
|Independencia|Totalmente autónoma|Depende de la exterior|
|Operación|Se ejecuta una vez|Se ejecuta fila por fila|
|Rendimiento|Suele ser más eficiente|Puede tener mayor coste si se evalúa por muchas filas|

### 7.3. Subconsultas en FROM y SELECT: tablas derivadas, alias obligatorios y expresiones de tabla comunes (CTE)

- Las subconsultas pueden construir una **tabla derivada** dentro de la consulta principal.
- Al colocarlas como origen de datos, el sistema **siempre pide un alias obligatorio** (nombre temporal para referirse a esos datos después).

**Subconsulta en `FROM`:**

```sql
SELECT resumen.ciudad, resumen.total_clientes
FROM (
  SELECT ciudad, COUNT(*) AS total_clientes
  FROM clientes
  GROUP BY ciudad
) AS resumen
WHERE resumen.total_clientes > 10;
```

Crea una tabla derivada llamada `resumen`, que después se filtra en la consulta principal.

**CTE** (opción más limpia): se define la consulta auxiliar al inicio y luego se usa como una tabla normal.

```sql
WITH resumen AS (
  SELECT ciudad, COUNT(*) AS total_clientes
  FROM clientes
  GROUP BY ciudad
)
SELECT ciudad, total_clientes
FROM resumen
WHERE total_clientes > 10;
```

Las CTE hacen más legibles consultas largas o con varios pasos intermedios.

**Subconsulta en `SELECT`:** calcula un valor asociado a cada registro (ej.: gasto total de un cliente).

```sql
SELECT c.nombre,
  (SELECT SUM(p.importe)
   FROM pedidos p
   WHERE p.id_cliente = c.id_cliente) AS gasto_total
FROM clientes c;
```

Muestra cada cliente junto con el importe total de sus pedidos.

---

## 8. Combinación de múltiples selecciones

- Combinar resultados de varias consultas en una única lista permite mostrarlos juntos sin lanzar varias peticiones por separado.
- **Operadores de conjunto:** trabajan **apilando filas**, no cruzando columnas como los `JOIN`.
- `UNION`: junta datos de distintas consultas en una lista y elimina repetidos.
- `UNION ALL`: conserva los duplicados.
- Requisito: mismo número de columnas y tipos de datos compatibles en la misma posición.
- `ORDER BY` se aplica **una sola vez, al final** de la combinación.
- `INTERSECT`: solo los elementos que coinciden en las dos listas.
- `EXCEPT`: datos de la primera lista que no existen en la segunda.
    - Equivalen a otras estructuras tradicionales (`JOIN`, `NOT IN`), pero más directas.
    - Disponibilidad y sintaxis varían según el SGBD: conviene comprobar si lo soporta de forma nativa.
- `UNION ALL` suele tener menor coste que `UNION` porque no revisa duplicados, pero solo debe usarse cuando se quieren conservar las repeticiones o se sabe que no existen.

### 8.1. Operador UNION y UNION ALL: diferencias, compatibilidad de columnas y ordenación del resultado

- `UNION` apila filas **verticalmente** (los `JOIN` unen tablas lateralmente). Ej.: listas de clientes y proveedores → directorio general en una sola tabla.
- `UNION` elimina automáticamente los duplicados para dar una lista limpia.
- `UNION ALL` conserva las repeticiones; consume menos recursos porque no revisa duplicados.

```sql
-- UNION: lista única de contactos, sin duplicados
SELECT nombre, email FROM clientes
UNION
SELECT nombre, email FROM proveedores;

-- UNION ALL: conserva todas las filas, incluso repetidas
SELECT nombre, email FROM clientes
UNION ALL
SELECT nombre, email FROM proveedores;
```

- Las consultas deben tener la misma cantidad de columnas y tipos compatibles.
- La ordenación se añade **una única vez al final** del conjunto. No colocar un `ORDER BY` independiente en cada `SELECT`, salvo que el SGBD lo permita dentro de subconsultas específicas.

```sql
SELECT nombre, email FROM clientes
UNION
SELECT nombre, email FROM proveedores
ORDER BY nombre ASC;
```

### 8.2. Operadores INTERSECT y EXCEPT: concepto, sintaxis y equivalencias con JOIN y NOT IN

- Permiten comparar listas (ej.: clientes de una tienda online con los de una tienda física) y saber quién compra en ambos sitios o solo en uno.
- **`INTERSECT`:** clientes que están en las dos listas (coincidencias exactas).
    - Mismo resultado con `INNER JOIN`. Diferencia: `INTERSECT` compara resultados completos de consultas; `INNER JOIN` combina tablas mediante una condición de relación.

```sql
SELECT id_cliente FROM compras_online
INTERSECT
SELECT id_cliente FROM compras_tienda;
```

Clientes que han comprado online y en tienda física.

- **`EXCEPT`:** toma la primera lista y quita los datos presentes en la segunda (ej.: clientes exclusivamente online).
    - Alternativa: subconsulta con **`NOT IN`**.
    - También `NOT EXISTS`, que suele ser **más seguro cuando pueden aparecer `NULL` en la subconsulta**.

```sql
SELECT id_cliente FROM compras_online
EXCEPT
SELECT id_cliente FROM compras_tienda;
```

Clientes que aparecen en compras online pero no en compras de tienda física.

> 💡 **Recuerda:** `INTERSECT` devuelve los resultados comunes entre consultas; `EXCEPT` devuelve los resultados de la primera consulta que no aparecen en la segunda.

|Operador|Función|Alternativa|
|---|---|---|
|`INTERSECT`|Muestra elementos comunes|`INNER JOIN`|
|`EXCEPT`|Muestra elementos exclusivos|`NOT IN`|

### 8.3. Casos de uso de los operadores de conjunto y comparativa de rendimiento

- Se emplean cuando ya existen varias consultas con estructuras compatibles y se quiere unir o comparar sus resultados.
- **No sustituyen a los `JOIN`:** no añaden columnas de otra tabla, combinan filas de distintas selecciones.
- Composiciones internas: solo datos coincidentes. Externas: conservan los registros de una tabla principal.
- `UNION ALL` puede tener menor coste que `UNION` porque no elimina duplicados.

**Casos habituales:**

- `UNION`: lista única de contactos procedentes de clientes y proveedores.
- `UNION ALL`: consolidar registros históricos cuando sabemos que no hay duplicados o se desea conservarlos.
- `INTERSECT`: localizar usuarios que aparecen en dos campañas distintas.
- `EXCEPT`: detectar clientes que compraron el año pasado pero no este año.

|Tipo de unión|¿Qué hace con los datos?|
|---|---|
|Composición interna|Solo guarda las coincidencias exactas|
|Composición externa|Mantiene completa la lista de referencia|

> 💡 **Recuerda:** la técnica elegida condiciona el coste de ejecución y la claridad del resultado.

---

## 9. Optimización de consultas

- Las aplicaciones necesitan extraer y combinar información con **tiempos de respuesta razonables**.
- Con millones de registros hay que evitar recorridos innecesarios. El motor crea un **plan de ejecución** (ruta interna de ejecución).
- Con **`EXPLAIN`** antes de la petición se consulta esa ruta estimada. Ayuda a descubrir problemas o pasos lentos. Puede mostrar si usará un índice, leerá una tabla completa, unirá tablas con un método concreto o revisará más filas de las necesarias.
- Mejoras: aplicar **índices** adecuados, escribir consultas más sencillas o reformular filtros para que el optimizador aproveche las estructuras disponibles.
- **Optimizador:** componente interno que evalúa el coste de cada posible ruta y elige la de **menor coste estimado**.
    - Para que sea fiable hay que **actualizar las estadísticas** periódicamente (así conoce la cantidad real de información).
- **Hint:** sugerencia directa en la consulta que orienta al sistema a usar un método concreto. Es una herramienta avanzada: usar con prudencia, porque puede empeorar el rendimiento si cambian los datos o la estructura de la base de datos.

### 9.1. El plan de ejecución: concepto, lectura con EXPLAIN y operaciones de acceso a datos

- **Plan de ejecución:** ruta que el optimizador considera más adecuada para encontrar la información. No siempre es la de menor coste real, pero es la estrategia elegida según estadísticas, índices y reglas internas.
- Se lee con **`EXPLAIN`**: al anteponerlo a la consulta, el sistema explica cómo buscará los datos según una estimación. Revela si hará una selección, un **escaneo completo**, una **búsqueda mediante índice**, una unión entre tablas o una agrupación.
- Algunos SGBD ofrecen variantes como **`EXPLAIN ANALYZE`**: ejecutan la consulta y muestran datos reales de tiempo y filas procesadas.
- Detalla cómo filtra la información, une tablas y agrupa los resultados.

|Elemento|Significado|
|---|---|
|Tipo de acceso|Indica cómo lee la tabla el sistema|
|Filas estimadas|Cantidad de datos que se van a revisar|

### 9.2. Estrategias de optimización: uso de índices, reescritura de consultas y evitar funciones en WHERE

- **Índices:** permiten localizar registros sin recorrer toda la tabla en determinados casos.
    - No conviene crearlos en todas las columnas: cada índice **ocupa espacio** y **debe actualizarse** al insertar, modificar o eliminar datos.
- **Reescribir consultas:** pedir los datos de forma más directa.
- **Evitar funciones en `WHERE`:** si se aplican funciones o transformaciones sobre la columna filtrada, el SGBD puede no aprovechar un índice normal y revisar más filas de las necesarias.
- Las comparaciones deben ser lo más limpias posible (ej.: buscar fechas entre enero y diciembre en lugar de extraer el año con una fórmula).

```sql
-- Dificulta el uso de índice
WHERE YEAR(fecha_compra) = 2023

-- Permite aprovechar el índice
WHERE fecha_compra >= '2023-01-01'
  AND fecha_compra < '2024-01-01'
```

La segunda forma aprovecha mejor un índice sobre `fecha_compra` y evita problemas si la columna contiene fecha y hora.

> 💡 **Recuerda:** si se modifican los datos dentro de la condición de búsqueda mediante fórmulas, el SGBD puede no aprovechar un índice normal de forma eficiente y verse obligado a revisar más filas, lo que puede empeorar el rendimiento.

|Estrategia|Propósito|Ejemplo práctico|
|---|---|---|
|Crear índices|Reducir lecturas en búsquedas frecuentes|Índice en la columna del documento de identidad|
|Reescribir consultas|Pedir los datos de forma directa|Buscar entre dos fechas en lugar de modificar el texto original|
|Evitar funciones|Permitir que el sistema use el índice|No usar fórmulas en la columna que se quiere filtrar|

### 9.3. Estadísticas del optimizador: concepto de coste, actualización de estadísticas y hint de consulta

- **Coste:** recursos estimados necesarios para devolver los resultados correctos. El optimizador busca un plan de bajo coste estimado.
- **Estadísticas:** resúmenes internos sobre distribución y volumen de datos. Se usan sobre todo en tareas complejas (unir datos coincidentes, búsquedas dentro de otra).
- Los datos cambian a diario → **conviene actualizar las estadísticas periódicamente**; si no, el sistema puede elegir planes de ejecución poco ajustados.
- **Hint de consulta:** instrucción manual escrita en el código para orientar y, en algunos SGBD, **forzar** al optimizador a usar una estrategia concreta. Sustituye la decisión automática por la estrategia indicada explícitamente.
    - Se usan en casos concretos, por ejemplo en consultas de resumen que agrupan miles de registros para calcular totales o promedios.

---

## Resumen

- Se ha estudiado cómo extraer información de una base de datos mediante consultas SQL.
- **Primero:** proyección de columnas, selección de filas y ordenación con `SELECT`, `WHERE` y `ORDER BY`; operadores de comparación, lógicos y especiales (`IS NULL`, `EXISTS`, `ANY`, `ALL`).
- **Después:** consultas de resumen, funciones de agregación, `GROUP BY`, `HAVING`, `DISTINCT` y funciones de ventana → transforman conjuntos de registros en resultados resumidos, clasificados o numerados sin perder el control sobre las filas originales.
- **Por último:** composiciones internas y externas, subconsultas, operadores de conjunto y optimización. El análisis del plan con `EXPLAIN`, el uso de índices y la revisión de condiciones permiten reducir recorridos innecesarios y ajustar el coste de ejecución.

---

## Glosario

- **Alias:** nombre corto y temporal asignado a una tabla para escribir referencias más breves. También puede aplicarse a una columna, con `AS`, para mostrar un título distinto sin modificar el nombre real del campo (`SELECT nombre AS Nombre_Cliente`).
- **ANY/ALL:** comparan un valor con los resultados de una subconsulta que devuelve varios valores. `ANY` se cumple si la condición es cierta para al menos uno; `ALL` exige que se cumpla para todos. Se combinan con un operador de comparación (`>`, `<`, `=`).
- **Cláusula DISTINCT:** elimina duplicados del resultado de una consulta, sin modificar los datos almacenados.
- **Cláusula ORDER BY:** ordena los resultados según el contenido de columnas específicas.
- **Composición externa:** combina listas conservando los registros de una de las tablas, o de ambas, aunque no existan coincidencias, rellenando con nulos los datos ausentes.
- **Composición interna:** combina listas mostrando solo las filas con valores coincidentes.
- **Consulta de resumen:** petición que calcula resultados de un conjunto global, sin detallar filas individuales.
- **CROSS JOIN:** combina cada fila de una tabla con todas las filas de otra (producto cartesiano), multiplicando el número de filas del resultado.
- **CTE:** expresión de tabla común, definida con `WITH` al inicio de la consulta; permite nombrar una consulta auxiliar y reutilizarla en la consulta principal.
- **EXCEPT:** operador de conjunto que devuelve las filas de la primera consulta que no aparecen en la segunda; ambas deben tener el mismo número de columnas y tipos compatibles.
- **EXPLAIN:** muestra el plan de ejecución previsto por el optimizador, incluyendo las operaciones de acceso a los datos y el orden en que se ejecutan.
- **FULL OUTER JOIN:** combina dos tablas conservando todos los registros de ambas, coincidan o no, y rellena con nulos los datos sin pareja.
- **Función de ventana:** cálculo sobre un conjunto de filas relacionadas con la fila actual sin colapsar el resultado en una sola línea. `ROW_NUMBER` numera correlativamente las filas; `RANK` asigna una posición según un criterio de orden dejando huecos cuando hay empates.
- **GROUP BY:** agrupa las filas que comparten un mismo valor en una o varias columnas, permitiendo calcular resultados por grupo con funciones de agregación.
- **HAVING:** filtra los grupos ya formados por `GROUP BY`, a diferencia de `WHERE`, que filtra las filas individuales antes de agrupar.
- **Índice:** estructura auxiliar asociada a una o varias columnas que acelera la localización de filas y evita recorrer la tabla completa.
- **INNER JOIN:** combina filas de dos tablas basándose en un dato común.
- **INTERSECT:** operador de conjunto que devuelve únicamente las filas que aparecen en los resultados de dos consultas; ambas con la misma cantidad de columnas y tipos compatibles.
- **LEFT JOIN:** une listas manteniendo toda la información de la tabla izquierda, aunque no existan coincidencias.
- **Operador lógico:** palabra clave que permite combinar o negar condiciones.
- **Operador UNION:** junta los resultados de varias consultas apilándolos en una lista vertical.
- **Optimizador de consultas:** componente que elige una ruta de ejecución eficiente para una consulta.
- **PARTITION BY:** se emplea con las funciones de ventana para dividir el resultado en particiones independientes; el cálculo se reinicia en cada partición sin reducir el número de filas devueltas.
- **Proyección:** acción de elegir únicamente columnas específicas de una tabla, omitiendo las demás para simplificar el resultado.
- **RIGHT JOIN:** une listas manteniendo toda la información de la tabla derecha, aunque no existan coincidencias en la izquierda.
- **Self join:** composición de una tabla consigo misma mediante dos alias distintos, para relacionar filas de la misma tabla (ej.: empleados con sus jefes directos).
- **Subconsulta:** consulta incluida dentro de otra instrucción SQL para obtener, filtrar o comparar datos.
- **Subconsulta correlacionada:** utiliza valores de la consulta externa y se evalúa una vez por cada fila procesada, a diferencia de la no correlacionada, que se ejecuta una única vez.