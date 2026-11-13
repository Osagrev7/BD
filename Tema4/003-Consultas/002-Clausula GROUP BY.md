Sirve para juntar registros que tienen algo en común en una o varias columnas.

Crea una consulta de resumen que ofrecen una visión general de los datos. 

Después se aplican funciones de agregación dentro de cada grupo.

```sql 
--Agrupa los empledos por departamento y calcula la media
SELECT departamento, 
COUNT(*) AS total_empleados, AVG(salario) AS salario_medio
FROM empleados
GROUP BY departamento;
```

