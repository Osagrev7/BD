**Dependencia funcional:** un dato permite identificar o conocer otros de forma directa (DNI → nombre, apellidos, fecha de nacimiento).

| Término | Significado | Ejemplo |
|---|---|---|
| **Determinante** | Atributo (o conjunto) que determina otros | DNI |
| **Atributo dependiente** | Lo que se descubre gracias al determinante | Apellidos, fecha de nacimiento |
| **Cierre de atributos** | Conjunto de todos los atributos que se pueden deducir a partir de uno o varios iniciales | Cierre de DNI = {Nombre, Apellidos, Fecha_Nacimiento} |

Comprender las dependencias permite **identificar claves candidatas** y estructurar mejor las tablas.

Se suele preferir un **identificador interno estable** que localice ca