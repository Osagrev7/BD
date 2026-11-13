Una vista es tabla virtual creada a partir de una consulta SQL, normalmente, no almacena los datos directamente, sino que muestra los datos de una o varias tablas existentes. 

Sirve para simplificar consultas, controlar el acceso a los datos y mostrar información especifica. 

## 1. Ejemplo de una vista

Tenemos una tabla llamada `Empleados`:

| ID  | Nombre | Departamento | Salario |
| --- | ------ | ------------ | ------- |
| 1   | Ana    | Informática  | 2500    |
| 2   | Luis   | Ventas       | 1800    |
| 3   | Marta  | Informática  | 2700    |

Queremos ver únicamente los empleados de Informática. 

>Una vista nunca almacena datos por si misma: funciona como un filtro sobre las tablas base. 

