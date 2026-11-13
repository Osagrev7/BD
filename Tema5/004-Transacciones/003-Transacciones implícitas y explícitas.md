|Tipo|Quién controla|Detalle|
|---|---|---|
|**Implícita**|El SGBD|Modo **autocommit** (activado por defecto en muchos SGBD): cada INSERT/UPDATE/DELETE se confirma solo. Una vez confirmado, **no se puede deshacer** con ROLLBACK|
|**Explícita**|El programador|Define inicio (`BEGIN`) y final (`COMMIT`/`ROLLBACK`)|

Explicitas recomendadas cuando la operación tiene varios pasos (ventas + stock + factura + movimiento contable).

Algunas instrucciones DLL (`CREATE`, `ALTER` , `DROP`, `TRUNCATE`) pueden comportarse de forma especial dentro de una transacción según el SGBD, consulta la documentación. 