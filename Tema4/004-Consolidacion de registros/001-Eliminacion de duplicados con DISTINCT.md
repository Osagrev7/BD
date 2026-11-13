Si se proyectan columnas que comparten valores, el resultado puede contener filas repetidas.

`DISTINCT` elimina duplicador del resultado de la consulta, no de la tabla original. 

Ejemplo saber que clientes han comprado. Sin `DISTINCT`, el `id_cliente` se repite tantas vences como pedidos tenga, con `DISTINCT` cada cliente aparece una sola vez.

```sql 
SELECT DISTINCT id_cliente FROM pedidos;
```

Devuelve el identificador de cada cliente con al menos un pedido, sin repetirlo.  

No debe confundirse  con otras acciones las consultas de resumen calculan totales, las uniones combinan datos de varias tablas y las subconsultas son búsquedas secundarias. 
`DISTINCT` solo elimina duplicados en el resultado. 



