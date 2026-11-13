Son reglas de la normalización que permiten organizar la información, reducir datos duplicados y evitar errores al insertar, modificar o eliminar registros. 

# Primera forma normal 1FN 

Una tabla esta en 1FN cuando cada celda contiene un único valor y ni existen grupos de valores repetidos en una misma columna. 

Ejemplo incorrecto: 

| id_alumno | nombre | telefonos                |
| --------- | ------ | ------------------------ |
| 1         | Ana    | 600-111-222, 600-333-444 |
| 2         | Luis   | 600-111-333              |

El problema es que Ana tiene dos teléfonos en la misma celda. 

Solución:  

| id_alumno | nombre | telefonos   |
| --------- | ------ | ----------- |
| 1         | Ana    | 600-111-222 |
| 1         | Ana    | 600-333-444 |
| 2         | Luis   | 600-111-333 |
Ahora cada celda contiene un único valor.

# Segunda forma normal 2FN 

Una tabla es 2FN cuando, cumple la 1FN, Todos los atributos que no forman parte de una clave candidata dependen de la clave completa, no solo de una parte de ella. 

Esto es especialmente importante cuando la PK esta formado por varias columnas. 

Ejemplo incorrecto:

Tabla `Matriculas`, cuya clave primaria es `(ID_Alumno, ID_Asignatura)`.

|ID_Alumno|ID_Asignatura|Nombre_Alumno|Nota|
|---|---|---|---|
|1|A01|Ana|8|
|1|A02|Ana|9|
|2|A01|Luis|7|

El problema es que `Nombre_Alumno` depende solo de `ID_Alumno`, no de la clave completa.

Solución: separar la información en dos tablas.

Tabla `Alumnos`:

|ID_Alumno (PK)|Nombre_Alumno|
|---|---|
|1|Ana|
|2|Luis|

Tabla `Matriculas`:

| ID_Alumno (FK) | ID_Asignatura (PK) | Nota |
| -------------- | ------------------ | ---- |
| 1              | A01                | 8    |
| 1              | A02                | 9    |
| 2              | A01                | 7    |

Así, el nombre del alumno se almacena una sola vez.

# Tercera Forma Normal (3FN)

Una tabla está en 3FN cuando, Cumple la 2FN, No existen dependencias transitivas de atributos no primos respecto de una clave candidata.

En términos sencillos, un atributo que no es clave no debe depender de otro atributo que tampoco sea clave.

Ejemplo incorrecto:

Tabla `Empleados`:

| ID_Empleado (PK) | Nombre | ID_Departamento | Nombre_Departamento |
| ---------------- | ------ | --------------- | ------------------- |
| 1                | Ana    | D01             | Ventas              |
| 2                | Luis   | D02             | Informática         |
| 3                | Marta  | D01             | Ventas              |

Aquí se producen estas dependencias funcionales:

- `ID_Empleado → ID_Departamento`

- `ID_Departamento → Nombre_Departamento`


Por tanto, el nombre del departamento depende indirectamente del identificador del empleado.

Solución: separar la información en dos tablas.

Tabla `Empleados`:

|ID_Empleado (PK)|Nombre|ID_Departamento (FK)|
|---|---|---|
|1|Ana|D01|
|2|Luis|D02|
|3|Marta|D01|

Tabla `Departamentos`:

|ID_Departamento (PK)|Nombre_Departamento|
|---|---|
|D01|Ventas|
|D02|Informática|

Así se evita repetir el nombre de cada departamento.



# 

|Forma normal|Regla principal|¿Qué evita?|
|---|---|---|
|1FN|Cada celda contiene un único valor.|Grupos de valores múltiples en una celda.|
|2FN|1FN y dependencia de toda la clave.|Dependencias parciales.|
|3FN|2FN y ausencia de dependencias transitivas de atributos no primos respecto de las claves candidatas.|Dependencias transitivas indebidas.|
