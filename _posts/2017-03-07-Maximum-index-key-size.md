---
layout: post
title: Maximum size of index key
tags: [sqlserver]
---

we all knew Maximum size of an index key is limited to 900 bytes..But with SQlServer 2016,this limit has been removed..

Below is a sample demo of the same

```sql
---Run below query on SQLServer 2012

create table dbo.test(
id int,
col1 varchar(1700)
)

create index nci_test on dbo.test
(col1)
include(id)
```

now try to run below queries and you will be greeted with error on second query

```

insert into dbo.test
values (1,'a')

insert into dbo.test
values (1,replicate('a',1000))

```

below is the error message

>Msg 1946, Level 16, State 3, Line 1
Operation failed. The index entry of length 1000 bytes for the index 'nci_test' exceeds the maximum length of 900 bytes

Now try modfying above table DDL little bit like below

```sql

---run the query on SQL2016

create table dbo.test(
id int,
col1 varchar(1705)
)

create index nci_test on dbo.test
(col1)
include(id)

```

Below is the warning ,you will get

>Warning! The maximum key length for a nonclustered index is 1700 bytes. 
The index 'nci_test' has maximum length of 1705 bytes. 
For some combination of large values, the insert/update operation will fail.


You can download the demo scripts used [here](https://github.com/sateeshmachineni/sateeshmachineni.github.io/blob/master/Scripts/indexkeysizedemo.txt)



