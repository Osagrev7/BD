
| Problema                 | Qué ocurre                                                                                                     |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| **Lectura sucia**        | Se lee un dato modificado por otra transacción **sin COMMIT**; si hace ROLLBACK, el dato nunca existió         |
| **Lectura no repetible** | Se lee el mismo registro 2 veces y da valores distintos (otra transacción lo modificó y confirmó entre medias) |
| **Lectura fantasma**     | La misma consulta (misma condición) devuelve filas distintas (otra transacción insertó/eliminó filas)          |
| **Escritura perdida**    | Dos transacciones modifican el mismo dato y una sobrescribe el cambio de la otra                               |

No siempre dan errores visibles, pero producen resultado incorrectos(ventas, reservas, informes, movimientos bancarios). 

Escritura perdida grave en inventario, saldo, reservas, edición compartida. 

```sql
-- Ejemplo de lectura sucia
-- Transacción A
BEGIN;
UPDATE productos SET stock = 0 WHERE id_producto = 10;   
-- sin COMMIT
-- Transacción B
SELECT stock FROM productos WHERE id_producto = 10;      
-- podría leer 0
-- Si A hace ROLLBACK, B leyó un valor que nunca fue definitivo
```

Se previenen con regla claras que organicen a los usuarios que acceden al mismo dato a la vez.