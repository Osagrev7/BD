`GRAND` entrega privilegios para leer o modificar información.

```sql
GRANT SELECT, INSERT ON cliente TO usuario_lectura;
```

`WITH GRANT OPTION` se añade al final para que el usuario pueda compartir sus permisos. Le da mucha responsabilidad, así que requiere control.

```sql 
GRANT SELECT ON cliente TO  usuaro_lectura WITH GRANT OPTION;
```

