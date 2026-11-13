Son reglas que establecen condiciones especificas sobre los datos que los usuarios pueden introducir, modificar o almacenar para garantizar que sean validos y coherentes. 

Se suelen implementar mediante restricciones como `CHECK`,`NOT NULL`,`UNIQUE` y otras definidas según la necedades de la aplicación.

| Herramienta              | Qué hace                                                                                                       | Ejemplo                                 |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| **Constraint / `CHECK`** | Valida que el valor cumpla una condición                                                                       | Precio > 0                              |
| **Trigger (disparador)** | Rutina automática ante INSERT/UPDATE/DELETE en una tabla; para reglas más complejas, auditorías o validaciones | Registrar quién cambió un dato y cuándo |

```sql
CREATE TABLE producto (
  id_producto INT PRIMARY KEY,
  nombre VARCHAR(100),
  precio DECIMAL(10,2) CHECK (precio > 0)
);

-- Trigger simplificado de auditoría
CREATE TRIGGER auditoria_precio
AFTER UPDATE ON producto
FOR EACH ROW
INSERT INTO auditoria (tabla, id_registro, fecha)
VALUES ('producto', OLD.id_producto, NOW());
```

***comprobar***  hola 
