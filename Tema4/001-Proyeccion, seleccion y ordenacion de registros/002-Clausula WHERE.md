La clausula `WHERE` permite filtrar los registros de una tabla. Es decir, muestra únicamente las filas que cumplen una determina condición. 

```sql 
-- Simple 
SELECT nombre, edad 
FROM clientes 
WHERE edad >= 18;
-- Compuesto
SELECT nombre, ciudad, edad 
FROM clientes
WHERE ciudad = 'Madrid' AND (edad < 18 OR edad > 65);
```

### Condiciones simples

|Operador|Nombre|Función|Ejemplo|
|---|---|---|---|
|`=`|Igual|Busca valores iguales.|`edad = 18`|
|`<>`|Distinto|Busca valores diferentes.|`edad <> 18`|
|`!=`|Distinto|Busca valores diferentes.|`edad != 18`|
|`>`|Mayor que|Busca valores superiores.|`edad > 18`|
|`<`|Menor que|Busca valores inferiores.|`edad < 18`|
|`>=`|Mayor o igual|Busca valores iguales o superiores.|`edad >= 18`|
|`<=`|Menor o igual|Busca valores iguales o inferiores.|`edad <= 18`|
### Condiciones Compuestas 

|Operador|Nombre|Función|Ejemplo|
|---|---|---|---|
|`AND`|Y|Deben cumplirse todas las condiciones.|`edad > 18 AND ciudad = 'Madrid'`|
|`OR`|O|Debe cumplirse al menos una condición.|`ciudad = 'Madrid' OR ciudad = 'Sevilla'`|
|`NOT`|NO|Niega una condición.|`NOT ciudad = 'Madrid'`|
