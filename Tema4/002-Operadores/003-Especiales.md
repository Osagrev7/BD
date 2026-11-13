Sirven para hacer búsquedas mas especificas en una base de datos, cada uno tiene una función diferente.

IS NULL  / IS NOT NULL 

Localizan valores ausentes o presentes en una columna. 

No se debe usar `= NULL`, porque representa ausencia de valor y se trata con un lógica especial. 

```sql
--IS NULL
SELECT nombre FROM clientes 
WHERE telefono IS NULL;
--IS NOT NULL
SELECT nombre FROM clientes 
WHERE telefono IS NOT NULL;
```

EXISTS 

Sirve para comprobar si existe al menos un registro que cumpla una condición. Comprueba si hay información relacionada.

```sql 
--para saber si un cliente tiene pedidos
SELECT nombre FROM clientes c 
WHERE EXISTS (
	SELECT 1 FROM pedidos p 
	WHERE p.id_cliente = c.id_cliente
);
--devueleve clientes con al menos 1 pedido
```

ANY/ALL

`ANY` sirve para comparar un valor con varios valores. 

La regla es muy sencilla solo tiene que cumplirse la condición con Uno de los valores como mínimo. 

```sql 
WHERE precio > ANY (
	SELECT precio FROM productos 
	WHERE categoria = 'libros'
);
```

Queremos productos cuyo precio sea mayor al menos uno de estos precios 10, 20 o 30.

`ALL` es parecido a `ANY`, pero aquí la condición es mas exigente. 

El valor tiene que cumplir la condición con TODOS los valores de la lista

```sql 
WHERE precio > ALL (
    SELECT precio FROM productos
    WHERE categoria = 'Libros'
);
```

Queremos productos cuyo precio sea mayor que 10 €, 20 € y 30 € al mismo tiempo.