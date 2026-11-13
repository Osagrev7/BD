| Operador             | Uso                                                               |
| -------------------- | ----------------------------------------------------------------- |
| `=`                  | Coincidencia exacta                                               |
| `<>`                 | Distinto                                                          |
| `<`, `>`, `<=`, `>=` | Menor / mayor (o igual)                                           |
| `BETWEEN`            | Dentro de un **intervalo** (normalmente incluye los dos extremos) |
| `IN`                 | Dentro de una **lista** de valores                                |
| `LIKE`               | Patrón de texto                                                   |
```sql
WHERE edad BETWEEN 18 AND 65
WHERE ciudad IN ('Madrid', 'Barcelona', 'Valencia')
WHERE nombre LIKE 'Mar%'      -- Marta, Marcos, Marina
```

Comodines LIKE:
`%` = cualquier cadena.
`_` = un único carácter. 