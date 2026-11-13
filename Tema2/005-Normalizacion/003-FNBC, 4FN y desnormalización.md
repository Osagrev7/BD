La FNBC y la 4FN son formas de normalización, mientras la desnormalización consiste en simplificar la estructura para mejorar el rendimiento en determinados casos. 

FNBC - forma normal de Boyce-Codd

La FNBC es una forma normal mas estricta que la tercera forma normal (3FN). 

Una tabla esta en FNBC cuando para toda dependencia funcional no trivial X→Y, X es una superclave. 

En otras palabras si un atributo determina a otro, el atributo o conjunto de atributos que determina debe identificar una fila de forma única. 

### Ejemplo

Tenemos esta tabla:

|Alumno|Asignatura|Profesor|
|---|---|---|
|Ana|Bases de datos|López|
|Luis|Bases de datos|López|
|Ana|Programación|García|

Sin embargo, `Asignatura` no es una superclave de la tabla, porque varios alumnos pueden cursarla. Por tanto, la tabla no cumple la FNBC.

Podemos descomponerla en dos tablas:

Tabla Alumno

|Alumno|Asignatura|
|---|---|
|Ana|Bases de datos|
|Luis|Bases de datos|
|Ana|Programación|

Tabla Asignatura

|Asignatura|Profesor|
|---|---|
|Bases de datos|López|
|Programación|García|

Así evitamos repetir el profesor para cada alumno.

# 4FN — Cuarta Forma Normal

La 4FN elimina las dependencias multivaluadas independientes que provocan combinaciones y duplicaciones innecesarias.

Una tabla está en 4FN si, para toda dependencia multivaluada no trivial X-> Y ,X es una superclave.

### Ejemplo

Un estudiante puede tener varios idiomas y varias aficiones, y ambas cosas son independientes.

|Estudiante|Idioma|Afición|
|---|---|---|
|Ana|Inglés|Música|
|Ana|Inglés|Fútbol|
|Ana|Francés|Música|
|Ana|Francés|Fútbol|

Ana habla dos idiomas y tiene dos aficiones. La tabla genera cuatro combinaciones, aunque los idiomas no dependan de las aficiones.

Podemos separarla en dos tablas:

EstudianteIdioma

|Estudiante|Idioma|
|---|---|
|Ana|Inglés|
|Ana|Francés|

EstudianteAfición

|Estudiante|Afición|
|---|---|
|Ana|Música|
|Ana|Fútbol|

De este modo, evitamos combinaciones innecesarias.

## 3. Desnormalización

La desnormalización consiste en introducir redundancia de forma deliberada para mejorar el rendimiento de las consultas.

Es lo contrario de normalizar: se combinan tablas o se almacenan datos calculados para reducir operaciones de unión (`JOIN`) y acelerar determinadas consultas.

### Ejemplo

En una base de datos normalizada tenemos:

Clientes

|ID|Nombre|
|---|---|
|1|Ana|
|2|Luis|

Pedidos

|ID pedido|ID cliente|Total|
|---|---|---|
|101|1|50 €|
|102|1|30 €|
|103|2|20 €|

Para consultar los pedidos con el nombre del cliente, necesitamos unir ambas tablas.

Con la desnormalización, podemos almacenar directamente el nombre del cliente en la tabla de pedidos:

|ID pedido|ID cliente|Nombre cliente|Total|
|---|---|---|---|
|101|1|Ana|50 €|
|102|1|Ana|30 €|
|103|2|Luis|20 €|

Así, algunas consultas son más sencillas y rápidas, pero si Ana cambia de nombre, hay que actualizar todas sus filas para evitar inconsistencias.

