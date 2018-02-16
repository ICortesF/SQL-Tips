# SQL-Tips

## Error al comparar cadenas con diferente ordenación

> No se puede resolver el conflicto de intercalación entre "SQL_Latin1_General_CP1_CI_AS" y "Modern_Spanish_CI_AS" 

Añadimos la clausala COLLATE DATABASE_DEFAULT en los JOIN o los WHERE

``` [SQL]
``` select
``` (select top 1 cmc from tasacdi.[Datos].[MHAPmunicipi] as a
``` where a.provinciid = municipi.provinciid COLLATE DATABASE_DEFAULT and a.municipiid = municipi.municipiid COLLATE DATABASE_DEFAULT) ,*
``` from municipi
```  where paisid = '034'  ```

## Conectar a localdb

Servidor (localdb)\MSSQLLocalDB
