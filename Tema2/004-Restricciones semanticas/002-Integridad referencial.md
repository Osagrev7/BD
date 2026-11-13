Es una regla que garantiza que las relaciones entre tablas sean validas y coherentes. 

Se consigue mediante una FK, que hacen referencia a una clave primaria o a otra clave única de una tabla. 

Sirve: 
- Evita referencias a registro que no existen.
- Mantiene la coherencia entre las tablas relaciones.
- Controla que sucede al eliminar o modificar registros relacionados.

```sql
CREATE TABLE ventas (
  id_venta INT PRIMARY KEY,
  id_cliente INT NOT NULL,
  fecha DATE,
  importe DECIMAL(10,2),
  FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);
```