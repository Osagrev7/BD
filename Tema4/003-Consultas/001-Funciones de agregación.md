Sirven para hacer cálculos automáticamente con los datos de una tabla o a grupos  definidos con `GROUP BY`, sin tener que sumar o contar nosotros manualmente. 

`COUNT` sirve para contar cuantos registro o filas tenemos. 

```sql 
--cuenta todas las filas
SELECT COUNT (*) FROM clientes;
--cuenta solamente las filas que tinen un valor
SELECT COUNT (precio) FROM clientes;
```

`SUM` sirve para sumar todos los números de una columna. 

```sql 
SELECT SUM(importe) FROM pedidos;
```

`AVG` Sirve para calcular la media de los valores de una columna.

```sql 
SELECT AVG(salario) AS salario_medio FROM empleados;
```

`MAX` sirve para encontrar el valor mas alto de una columna.

```sql
SELECT MAX(importe) FROM pedidos;
```

`MIN` sirve para encontrar el valor mas bajo de una columna.

```sql
SELECT MIN(importe) FROM pedidos;
```

Cuando el hay valores `NULL` en general las funciones de agregación ignoran los valores `NULL`, Es decir no los tienes en cuenta.