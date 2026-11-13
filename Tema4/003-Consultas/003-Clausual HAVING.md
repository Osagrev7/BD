Sirve para filtrar grupos de datos después de haberlos agrupado con `GROUP BY`. 

Es decir, permite seleccionar únicamente los grupos que cumplen una condición 

```SQL
--Muestra solo los cursos con mas de 20 estudiantes
SELECT curso, COUNT(*) AS total_estudiantes
FROM matriculas
GROUP BY curso
HAVING COUNT(*) > 20;
```

Agrupa las matriculas por curso  y muestra únicamente los cursos con mas de 20 estudiantes. 

