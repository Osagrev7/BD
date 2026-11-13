En `WHERE` elegir filas 

```sql
UPDATE empleados
SET salario = salario * 1.05
WHERE id_departamento In ( 
	SELECT id_departamento
	FROM departametos
	WHERE nombre = 'ventas'
);
```

comprobación previa el mismo `WHERE ... IN (...)` pero con `SELECT * FROM empleados`.

En `SET` calcula el valor. 

```sql
UPDATE productos
SET precio = (
	SELECT precio_recomendado
	FROM catalogo_precios
	WHERE catalogo_precios.id_producto = producto.id_producto
)
WHERE id_producto IN (SELECT id_producto FROM catalogo_precios);
```

Sincroniza precios con otra tabla sin actualizar a mano.