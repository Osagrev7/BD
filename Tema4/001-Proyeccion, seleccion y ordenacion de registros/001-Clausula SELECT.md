La clausula SELECT permite elegir que columnas queremos visualizar cuando realizamos una consulta a una base de datos. 

### Proyección

Esta elección de columnas recibe el nombre de **proyección**. 

por ejemplo imaginemos una tabla llamada `clientes`.

|id|nombre|correo|ciudad|
|---|---|---|---|
|1|Ana|ana@gmail.com|Madrid|
|2|Luis|luis@gmail.com|Barcelona|
|3|Marta|marta@gmail.com|Madrid|
```sql
--si queremos mostrar solamente el nombre y correo de los clientes
SELECT nombre, correo
FROM clientes;
```

> la proyección selecciona columnas, no filas 

### Alias
**Alias** es un nombre temporal que damos a una columna para que aparezca con un titulo mas claro en el resultado de la consulta. 

no modifica el nombre original de la columna en la base de datos. 

```sql
SELECT nombre AS Nombre_Cliente
FROM clientes;
```

### Expresiones calculadas

**Expresiones calculadas** Permiten realizar operaciones matemáticas directamente en una consulta. 

Esto significa que podemos calculas nuevos valores utilizando los datos almacenados en las columnas, sin necesidad de modificar la tabla original. 

Ejemplo práctico: Imaginemos una tabla `linea_pedidos`:

| producto | precio | cantidad |
| -------- | ------ | -------- |
| Teclado  | 20 €   | 2        |
| Ratón    | 10 €   | 3        |
| Monitor  | 150 €  | 1        |
```sql 
-- queremos calcular el importe total de cada producto 
SELECT producto,
       precio * cantidad AS total
FROM lineas_pedido;
```
### DISTINCT
**DISTINCT** sirve para eliminar los valores duplicados de los resultados de una consulta. 

Se escribe inmediatamente después de `SELECT`. 

```sql 
SELECT DISTINCT ciudad
FROM clientes;
```


>`DISTINCT` elimina filas duplicadas del resultado de la consulta, no borra datos de la tabla original 

