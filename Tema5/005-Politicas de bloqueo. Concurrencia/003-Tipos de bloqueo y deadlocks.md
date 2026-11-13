Por modo:

|Tipo|Uso|Permiso para otros|
|---|---|---|
|Compartido|Lectura|Leer de forma simultánea|
|Exclusivo|Escritura (UPDATE, DELETE, algunos INSERT)|Impide escrituras incompatibles; puede limitar ciertas lecturas (según SGBD y aislamiento)|
Por alcance:

|Bloqueo|Efecto|
|---|---|
|De **fila**|Afecta solo a los registros implicados → **más concurrencia**|
|De **tabla**|Afecta a toda la tabla → puede **reducir el rendimiento**|
Bloqueo explicito de filas: 

```sql
BEGIN;
SELECT * FROM productos
WHERE id_producto = 10
FOR UPDATE;                      -- bloquea las filas seleccionadas
UPDATE productos SET stock = stock - 1 WHERE id_producto = 10;
COMMIT;
```

(Sintaxis y comportamiento pueden variar según el SGBD.)

**Deadlock (bloqueo mutuo):** dos transacciones esperan indefinidamente porque cada una tiene un recurso que la otra necesita.

- A bloquea la fila 1 y necesita la 2.
- B bloquea la fila 2 y necesita la 1.
- El SGBD detecta el deadlock y **cancela una** transacción; la cancelada normalmente **debe repetirse**.

**Cómo reducir el riesgo:**

- Transacciones lo más cortas posible.
- Acceder siempre a tablas y filas en el **mismo orden**.
- Crear **índices** adecuados (evitan bloqueos más amplios de lo necesario).
- No dejar una transacción abierta mientras el usuario decide.
- Controlar errores y **reintentar** si el SGBD cancela por deadlock.
- Usar el nivel de aislamiento adecuado, sin aplicar siempre el más restrictivo.