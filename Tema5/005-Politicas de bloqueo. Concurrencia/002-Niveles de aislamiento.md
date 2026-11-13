| Nivel              | Qué hace                                                                                                           | Protección | Velocidad |
| ------------------ | ------------------------------------------------------------------------------------------------------------------ | ---------- | --------- |
| `READ UNCOMMITTED` | Más permisivo; permite lecturas sucias (no apto para operaciones críticas)                                         | Muy baja   | Muy alta  |
| `READ COMMITTED`   | Solo lee datos confirmados; evita lecturas sucias, pero la misma consulta puede dar resultados distintos           | Básica     | Alta      |
| `REPEATABLE READ`  | Un registro leído no cambia al releerlo; según SGBD protege de algunas lecturas fantasma                           | Alta       | Media     |
| `SERIALIZABLE`     | Más estricto; equivale a ejecutar las transacciones una tras otra; puede reducir rendimiento o aumentar conflictos | Muy alta   | Baja      |
Valores orientativos el rendimiento real depende del SGBD, las consultas, los índices y la carga. 

Se elige según equilibrio seguridad rendimiento (consulta estadística tolera variación operación bancaria o reserva exige mas control). 

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
SELECT * FROM reservas WHERE id_evento = 10;
COMMIT;
```

(Sintaxis MySQL/MariaDB; en PostgreSQL `SET TRANSACTION` va **después** de `BEGIN`.)

A mayor aislamiento mas anomalías evitadas, pero mas bloqueos y menos rendimientos. 