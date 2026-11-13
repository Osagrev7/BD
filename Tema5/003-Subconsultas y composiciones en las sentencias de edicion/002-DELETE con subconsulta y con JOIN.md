```sql
--Subconsulata
DELETE FROM pedidos 
WHERE id_cliente IN (
	SELECT id_cliente FROM clientes WHERE activo = false
);
```

Comprobar antes con `SELECT * FROM pedidos WHERE id_cliente IN (...)`

```sql 
-- MySQL (JOIN)
DELETE pedidos
FROM pedidos
INNER JOIN clientes ON pedidos.id_clientes = clientes.id_cliente
WHERE cliente.activo = false;

-- PostgreSQL (USING)
DELETE FROM pedidos 
USING clientes 
WHERE pedidos.id_cliente = clientes.cliente
	AND clientes.activo = false;
```

Misma lógica, distinta sintaxis, revisar las particularidades del SGBD.

En DELETE incluir condición si no se quiere borrar toda la tabla, y comprobar antes con SELECT.

según SGBD y nivel de aislamiento, un borrado puede bloquear temporalmete las filas hasta que acabe la transaccion. 

