Ocurre cuando una operación intenta romper la relación entre dos tablas. El sistema SBDB la bloquea. 

Casos habituales 

- INSERT en la hija con FK inexiste (ej. cliente 50)
- DELETE del padre que aun tiene hijos (ej. cliente10)

Estrategias para resolverlo 

- Comprobar con `SELECT * FROM pedidos WHERE id_cliente = 10;`
- Borrar primeros los hijos y luego el padre, preferiblemente en transacción. 
- Definir acciones referenciales adecuadas (`CASCADE`, `RESTRICT`, `SET NULL`).
- Evitar modificar PK que ya se usan en otras tablas. salvo regla referencial clara.

|Operación|Causa del error|Solución|
|---|---|---|
|INSERT|FK que no existe en la tabla principal|Insertar antes el principal o corregir la FK|
|UPDATE|Se cambia PK/FK y se rompe la relación|Revisar la relación; `ON UPDATE CASCADE` si procede|
|DELETE|Borrar padre con hijos|Borrar hijos primero, usar transacción o `ON DELETE CASCADE`|
|DELETE|Conservar hijos sin borrar todo|`ON DELETE SET NULL` (si la FK admite nulos)|

>Agrupar operaciones relacionadas en transacciones para que un fallo no deje la información a medias. 

