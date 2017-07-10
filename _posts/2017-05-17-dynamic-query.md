---
layout: post
title:Interview Q2
tags: [sqlserver]
---

This question was part of an interview i attended,their site,didn't allowed special characters :(  ,so posted here with a link.
I feel good about this,as i wrote  this query in less than 30 minutes.so i kept it like this


This should be run with results to text ,we also should change the maximum characters limitation

```
if object_id('tempdb..#temp') is not null
drop table #temp

;with cte
as
(select
sch.name as 'Schemaname',
obj.NAME AS 'tableName',
col.name as 'ColName',
obj.create_Date,
obj.modify_date ,
id.is_identity,
row_number() over (order by obj.create_Date) as rownum
from
sys.objects  obj
join
sys.columns col
on obj.object_id=col.object_id
join
sys.schemas sch
on sch.schema_id=obj.schema_id
left join
sys.identity_columns id
on id.object_id=obj.object_id
 where is_ms_shipped<>1 and type='u'
)

select * into #temp from cte
 

--disable constraints
declare @disable nvarchar(max)
set  @disable=
(select distinct   
 stuff(( select  distinct ';'+' ALTER TABLE '+quotename(schemaname)+'.'+quotename(tablename) +'  NOCHECK CONSTRAINT ALL'
from #temp  c2 
for xml path('')),1,1,''))


--Enable constraints
declare @Enable nvarchar(max)
set  @Enable=
(select distinct   
 stuff(( select  distinct ';'+' ALTER TABLE '+quotename(schemaname)+'.'+quotename(tablename) +'   CHECK CHECK CONSTRAINT ALL'
from #temp  c2 
for xml path('')),1,1,''))
print @Enable


Declare @query nvarchar(max)
;with cte(query)
as
(
select  distinct 
  case when is_identity=1
 then 'SET IDENTITY_INSERT '+quotename(schemaname)+'.'+quotename(tablename) +' ON; '
  ELSE '' END 
  +
'Insert into '+'['+'destination'+']'+'.'+quotename(schemaname)+'.'+quotename(tablename)+char(10)
+'('+ stuff(( select ','+ colname  from #temp  c2 where
c2.tablename=c1.tablename  for xml path('')),1,1,'')+' ) '
 +char(10)+
 'select  '+ stuff(( select ','+ colname  from #temp  c2 where
c2.tablename=c1.tablename  for xml path('')),1,1,'')+
 ' from ' +quotename(db_name())+'.'+quotename(schemaname)+'.'+quotename(tablename)+char(10)
 +
   case when is_identity=1
 then 'SET IDENTITY_INSERT '+quotename(schemaname)+'.'+quotename(tablename) +' OFF; '
  ELSE '' END 
 from #temp c1
 )
 select @query =stuff((select ';' + query  from cte for xml path('')),1,1,'')

 select @disable +char(10)+char(10)+
        @query+char(10)+char(10)+
		@enable
		



```
