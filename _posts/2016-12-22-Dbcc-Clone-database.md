---
layout: post
title: DBCC Clone Database
categories: [sqlserver]
---

Imagine you have to troubleshoot slow running queries on a particular client database remotely.The data is sensitive and client can’t share his database..To help in this cases,

SQLServer 2016 SP1 introduced new feature called `DBCC CLONE`
.Below is the total syntax..

``` sql

DBCC clone('source db name','target db name') [WITH [NO_STATISTICS][,NO_QUERYSTORE]]

```

When the above command finishes,we will be having a READ ONLY clone database with the name tsqlclone ,The above command also writes to error log as well..

below is snip from my local instance..

<img  src="/img/dbcc-clone.png"/>




Please refer to below KB for indepth details on how this works..



[DBCC CLONEDATABASE to generate a schema and statistics only copy of a user database in SQL Server 2014 SP2 and SQL Server 2016 SP1](https://support.microsoft.com/en-in/kb/3177838)

For earlier versions,we can use generate scripts option to generate schema ,statistics and run them on empty database to get similar functionality      
