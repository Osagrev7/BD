
`INSET` que la FK exista en la tabla padre.

`DELETE` Si hay hijos se impide el borrado o se aplica una acciona referencial. 

Las acciones en cascada no son automáticas, solo si se definen (`ON DELETE CASCADE`, `ON UPDATE CASCADE`). 

|Acción referencial|Comportamiento|
|---|---|
|`RESTRICT`|Impide borrar/actualizar el padre si hay hijos|
|`NO ACTION`|Similar a RESTRICT; se comprueba al final de la operación o transacción (según SGBD)|
|`CASCADE`|Propaga el borrado o la actualización a los hijos|
|`SET NULL`|Pone NULL en la FK de los hijos (si la columna lo permite)|
|`SET DEFAULT`|Pone el valor por defecto en la FK (si está definido)|

```sql
-- Cascada
FOREING KEY (id_cliente) REFERENCES clientes(id_cliente) ON DELETE CASCADE
-- Restriccion
FOREING KEY (id_cliente) REFERECES clientes(id_cliente) ON DELETE RESTRICT
```

CASCADE al borrar un cliente se borran sus pedidos (con precaución)
RESTRICT no se puede borrar un cliente mientras tenga pedidos. 

