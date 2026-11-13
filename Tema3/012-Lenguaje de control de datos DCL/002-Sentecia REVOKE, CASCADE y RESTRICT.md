`REMOKE` retira los permisos de un usuario sobre una tabla. 

```sql 
REVOKE INSERT ON cliente FROM usuario_lectura;
```

`CASCADE`quita el permiso al usuario y a todos los que lo recibieron de él (según el SGBD).

```sql 
REVOKE INSERT ON cliente FROM usuario_lectura CASCADE;
```

**RESTRICT**: bloquea la retirada si el usuario ya compartió el permiso con otros.

```sql 
REVOKE INSERT ON cliente FROM usuario_lectura RESTRICT;
```