`INSERT ... VALUES` valores escritos a mano.
`INSERT ... SELECT` datos obtenidos de una consulta.

UTIL para copiar entre tablas, crear históricos, preparar informes, migraciones (cambio de programa y traslado del historial).

En traslados masivos usar transacciones (Se completa todo o se deshace). 

ETL: 

|Fase|Qué hace|
|---|---|
|Extracción|Obtener datos de las tablas origen|
|Transformación|Adaptar, filtrar o calcular los datos|
|Carga|Insertar en la tabla destino|

```sql
-- Con transformación (unir nombre y apellidos)
INSERT INTO clientes_historico (id_cliente, nombre_completo, email)
SELECT id_cliente, CONCAT(nombre, ' ', apellidos), email
FROM clientes
WHERE activo = false;

-- Copia de respaldo antes de limpiar
INSERT INTO pedidos_historico (id_pedido, id_cliente, fecha, total)
SELECT id_pedido, id_cliente, fecha, total
FROM pedidos
WHERE fecha < '2024-01-01';

-- Tras comprobar la copia, se borra el original
DELETE FROM pedidos WHERE fecha < '2024-01-01';
```

`INSERT ... SELECT` copia en una sola instrucción, pero en migración grande hay que combinarlo con comprobaciones y transacciones. 

