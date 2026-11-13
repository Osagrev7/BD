Utilizamos para combinar varias condiciones y decidir que datos queremos que aparezcan en una búsqueda. Se utiliza dentro de `WHERE`. 

| Operador | Significado                 | Ejemplo                                    |
| -------- | --------------------------- | ------------------------------------------ |
| `AND`    | Todas las reglas ciertas    | `edad >= 18 AND ciudad = 'Madrid'`         |
| `OR`     | Al menos una cierta         | `ciudad = 'Madrid' OR ciudad = 'Valencia'` |
| `NOT`    | Invierte/descarta una regla | `NOT activo`                               |

### Precedencia

Es simplemente el orden en que que SQL lee y ejecuta los operadores. 

Orden habitual es: `NOT` `AND` Y `OR`. Es decir `AND` tiene prioridad sobre `OR`. 

```sql
SELECT *
FROM alumnos
WHERE ciudad = 'Madrid' OR ciudad = 'Valencia'
AND edad >= 18;
--primero se resuelve AND
```

### Paréntesis 

Sirven para decirle a SQL que condición queremos que se resuelvan primero.

```sql
--ejecuata priemero los dentro de (), despues otra condicion 
SELECT *
FROM alumnos
WHERE (ciudad = 'Madrid' OR ciudad = 'Valencia')
AND edad >= 18;
```
