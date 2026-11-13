Un rol es un conjunto de privilegios que se puede asigna r a uno o varios usuarios de BD. 

En lugar de dar permios uno por uno, Se crea un rol, que agrupa varios privilegios en mismo perfil

### Creación de roles 

La instrucción habitual es `CREATE ROLE`.

```sql
CREATE ROLE lectores;
```

Esto crea un rol llamado `lectores`, inicialmente sin privilegios que vamos a asígnale.

### Asignación de privilegios a un rol

Una vez creado el rol, podemos concederle privilegios mediante `GRANT`.

Por ejemplo, queremos que el rol lectores pueda consultar la tablas empleados. 

```sql
GRANT SELECT ON empleados TO lectores;
```

Ahora, cualquier usuario que tenga asignado ese rol podrá utilizar el privilegio de consulta. 

### Ventajas 

- [ ] menos tareas repetitivas 
- [ ] cambios mas sencillos 
- [ ] menos errores humanos 
- [ ] acceso limitado a lo que necesita cada puesto 

>Agrupar privilegios en roles evita fallos de seguridad y simplifica el mantenimiento. 
