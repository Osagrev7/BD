
| Comando                       | Función                   | Efecto                             |
| ----------------------------- | ------------------------- | ---------------------------------- |
| `BEGIN` / `START TRANSACTION` | Iniciar la transacción    | Abre el bloque                     |
| `COMMIT`                      | Guardar los cambios       | Los hace persistentes              |
| `ROLLBACK`                    | Deshacer la transacción   | Revierte cambios no confirmados    |
| `SAVEPOINT`                   | Crear un punto de control | Permite volver a un punto concreto |

```sql 
BEGIN;
UPDATE productos SET stock = stock - 1 WHERE id_producto = 20;
INSERT INTO pedidos (id_cliente, fecha, total)
VALUES (999, CURRENT_DATE, 49.99);   -- cliente 999 no existe → falla
ROLLBACK;  -- deshace también el UPDATE del stock
```

SAVEPOINT (procesos largos): 

```sql 
SAVEPOINT punto_contol;
ROLLBACK TO SAVEPOINT punto_control;
```

Deshace solo lo posterior al punto, sin anular el resto. 

