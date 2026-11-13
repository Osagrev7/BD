Son permisos que determina que acciones puede realizar un usuario dentro de BD. 

Existen dos grandes categorías, de sistema y de objeto.

### **Privilegio de sistema**

Permite realizar operaciones sobre el propio SGBD o sobre la BD en un ámbito amplio, no limitada a una tabla concreta. 

Como crear usuarios, tablas, vistas, esquemas  y administrar sesiones o conexiones. 

### **Privilegio de objetos**

Permite realizar operaciones sobre un objeto especifico de BD, como una tablas, vista o procedimientos. 

Como `SELECT`, `INSERT`, `UPDATE`, `DELETE` Y `REFERENCES`.

### Granularidad de los privilegios 

Indica el nivel detalle con el que se concede los permisos. 

Cuanto mas granular es un sistema de permisos, mas precisamente podemos decidir que puede hacer cada usuario. 

### `GRANT` conceder privilegios 

permite asignar privilegios a usuarios o roles. 

```sql
GRANT SELECT ON Empleados TO ana;
```

###  `REVOKE`: retirar privilegios

Permite retirar privilegios previamente.

```sql
REVOKE SELECT ON Empleados FROM ana;
```