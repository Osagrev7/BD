La gestión de usuarios en un SGBD concite en controlar quien puede acceder a la base de datos y que operaciones puede realizar. 
### Creación de usuarios 

Se crea una cuenta para que una persona o aplicación pueda conectarse al SGBD. 

```sql
-- MySQL
CREATE USER 'ana'@'localhost'
IDENTIFIED BY 'ClaveSegura123!';
```

```sql
-- PostgreSQL
CREATE USER ana
WITH PASSWORD 'ClaveSegura123!';
```

### Modificación de usuarios

Permite cambia características de una cuenta, como su contraseña, sus propiedades o sus restricciones de acceso. 

```sql
-- MySQL
ALTER USER 'ana'@'localhost'
IDENTIFIED BY 'NuevaClave456!';
```

```sql
-- PostgreSQL
ALTER USER ana
WITH PASSWORD 'NuevaClave456!';
```

### Eliminación de usuarios 

Las instrucción `DROP USER` elimina una cuenta del SGBD.

```sql
-- MySQL
DROP USER 'ana'@'localhost';
```

```sql
-- PostgreSQL
DROP USER ana
```

Desactivar una cuenta puede ser más seguro que eliminarla, si se necesita conservar el historial de auditoría.

### Autentificación de usuarios 

Es el proceso por el que el SGBD verifica la identidad de quien intenta conectarse. 

Los métodos mas habituales son: 

- políticas de contraseñas,
- autenticación externa,
- certificados,
- autenticación multifactorial.

```bash
mysql -u ana -p
```