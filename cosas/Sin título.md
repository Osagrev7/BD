cosa que no veo 

crear un base de datos 

```
CREATE DATABASE tienda;
```

seleccionar la base de datos 

```
USE tienda;
```

Crear una tabla 

```
CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    ciudad VARCHAR(150)
);
```


insertar datos 

```
INSERT INTO clientes
(nombre, email, ciudad)
VALUES
('Juan', 'juan@gmail.com', 'madrid'),
('María', 'maria@gmail.com', 'barcelona'),
('marta', 'marta@gmail.com', 'madrid');
```

consultar los datos 

```
SELECT * FROM clientes;
```

![[Pasted image 20261111163229.png]]