## 2. Integridad referencial

Los datos de un sistema se conectan entre si para tener sentido, Si se elimina o modifica, pueden quedar registros sin refencia lo que incoherecias.

**Qué es:** regla de las BD relacionales que mantiene la coherencia entre **claves primarias** y **claves foráneas**. Evita **registros huérfanos**.

- **Clave primaria (PK):** identifica de forma única cada registro.
- **Clave foránea (FK):** campo de otra tabla que apunta a esa PK.

```sql
--Ejmeplo de relaciones entre tablas
CREATE TABLE clientes (
  id_cliente INT PRIMARY KEY,
  nombre VARCHAR(100)
);

CREATE TABLE pedidos (
  id_pedido INT PRIMARY KEY,
  fecha DATE,
  id_cliente INT,
  FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);
```

La tabla pedidos solo puede registrar pedidos asociados a `clientes` existentes. Si se inserta un `pedidos` con un `id_cliente` que no existe en la tabla clientes, la base de datos rechaza la operación.

Añadir, modificar o eliminar datos afecta directamente a estas relaciones, hay que comprobar si otros datos dependen de ella.

Si una operación incumple estas reglas, el SGBD detiene la operación para proteger la coherencia de la información. 

# Vocabulario 

|Término|Significado|Ejemplo|
|---|---|---|
|Tabla padre / principal|Contiene el registro de referencia|clientes|
|Tabla hija / dependiente|Registros que dependen del padre|pedidos|
|Clave primaria|Identifica el registro principal|id_cliente|
|Clave foránea|Enlaza hijo con padre|pedidos.id_cliente|

>La integridad referencial evita modificar o borrar datos si eso deja
>información incompleta.
