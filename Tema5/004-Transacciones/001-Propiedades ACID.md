
| Propiedad    | Signidicado                                                     | Ejemplo                                                 |
| ------------ | --------------------------------------------------------------- | ------------------------------------------------------- |
| Atomicidad   | Se ejecuta completa o no se ejecuta nada.                       | No restar dinero sin súmalo en otra cuenta              |
| Consistencia | Respeta las reglas de la BD (PK, FK, restricciones, tipos).     | No registrar pedido de cliente inexistente              |
| Aislamiento  | Las transacciones simultaneas no interfieren incorrectamente.   | Dos usuarios no sobreescriben el mismo dato sin control |
| Durabilidad  | Lo confitamado (COMMIT) se conserva auque haya un fallo despues | Una venta confirmada sigue tras reiniciar               |

Consistencia para de un estado valido a otro estado valido. 

Aislamiento según el nivel configurado, se ven o no cambios de otras transacciones sin confirmar.

```sql 
BEGIN;
UPDATE cuentas SET saldo = saldo - 100 WHERE id_cuenta = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id_cuenta = 2;
COMMIT;   -- si una falla → ROLLBACK
```

