Permite ordenar los resultados de una consulta SQL según una o varias columnas. 

A diferencia de `WHERE` que filtra dilas, `ORDER BY` organiza las filas que aparecen en el resultado. 

```sql 
--De menor a mayor (De A a Z)
SELECT nombre, apellido 
FROM alumnos ORDER BY apellido ASC;
--De  mayor a menor (De Z a A)
SELECT nombre, salario 
FROM empleados ORDER BY salario DESC;
--Combinada
SELECT nombre, provincia, apellido 
FROM alumnos ORDER BY provincia ASC, apellido ASC;  -- varias columnas
```

| Tipo        | Instrucción SQL | Textos   | Números            |
| ----------- | --------------- | -------- | ------------------ |
| Ascendente  | `ASC`           | De A a Z | Del menor al mayor |
| Descendente | `DESC`          | De Z a A | Del mayor al menor |
El valor `NULL` representa un dato desconocido, inexistente o no disponible. Cunado lo ordenamos los SGBD pueden colocarlo al inicio o final, Algunos permiten indicarlo expresamente. 