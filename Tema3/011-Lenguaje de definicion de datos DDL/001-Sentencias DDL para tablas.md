`CREATE TABLE` crea una tabla nueva. en ella se define la clave primaria y se puede crear claves foráneas.  

```sql
CREATE TABLE cliente (
	id_cliente INT PRIMARY KEY,
	nombre VARCHAR(100) NOT NULL,
	email VARCHAR(150) UNIQUE
);
```

`ALTER TABLE` agrega o quita columnas sin perder los datos guardados. también sirve para anadir restricciones, cambiar tipo de datos y crear relaciones. 

```sql
ALTER TABLE cliente ADD COLUMN telefono VARCHAR(20);
```

`DROP TABLE` elimina la estructura y los datos. 

```sql
DROP TABLE cliente;
```

