# SQL-Tips

## Error al comparar cadenas con diferente ordenación

> No se puede resolver el conflicto de intercalación entre "SQL_Latin1_General_CP1_CI_AS" y "Modern_Spanish_CI_AS" 

Añadimos la clausala COLLATE DATABASE_DEFAULT en los JOIN o los WHERE

``` [SQL]
 select
 (select top 1 cmc from tasacdi.[Datos].[MHAPmunicipi] as a
 where a.provinciid = municipi.provinciid COLLATE DATABASE_DEFAULT 
 and a.municipiid = municipi.municipiid COLLATE DATABASE_DEFAULT) ,*
 from municipi
 where paisid = '034' 
```

## Conectar a localdb

Servidor (localdb)\MSSQLLocalDB

## Ejemplo de trigger que simula un identity en un campo int o bigint que no lo es
create trigger tbl_i
on dbo.test
INSTEAD OF insert
as 
begin
declare @maxitem_id int
declare @codigo char(2)
declare @otromas char(10)

declare ins cursor LOCAL FAST_FORWARD for select codigo,otromas from inserted

-- Get the current maximum. Use 1 if no rows ...
set @maxitem_id = null
select @maxitem_id = max(id) + 1 from dbo.test
if @maxitem_id is null set @maxitem_id = 1

open ins
goto NEXTins

while @@fetch_status = 0
begin
insert dbo.test (id,codigo,otromas ) values (@maxitem_id,@codigo,@otromas)
set @maxitem_id = @maxitem_id + 1
NEXTins: fetch ins into @codigo,@otromas
end
close ins
deallocate ins

end
go
