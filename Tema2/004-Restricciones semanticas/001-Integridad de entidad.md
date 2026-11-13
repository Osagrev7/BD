Es una regla que garantiza que cada registro de una tabla puede identificarse de forma única y que su clave primaria no tenga valores nulos. 

Se consigue mediante la PK, que cumple dos condiciones: 

- No puede haber dos registros con la misma clave primaria. 
- La PK no puede estar vacía(`NULL`).

Si se repite, no se distinguen dos registro si esta vacía no hay identificación valida. 
en muchas BD se prefiere un identificador interno generado por el sistema 

| Cliente | Clave | Estado       |
| ------- | ----- | ------------ |
| Ana     | 123   | Correcta     |
| Luis    | vacio | Nula o vacia |
| Pedro   | 123   | Repetida     |
