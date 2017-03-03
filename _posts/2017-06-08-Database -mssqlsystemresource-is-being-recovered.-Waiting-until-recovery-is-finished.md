---
layout: post
title: Database mssqlsystemresource is being recovered. Waiting until recovery is finished
---


This error occured for me when trying to establish a connection to SQLServer immediately, after SQL restart

>Database 'mssqlsystemresource' is being recovered. Waiting until recovery is finished. 
(Microsoft SQL Server, Error: 922)

As you must be aware `mssqlsystemresource` is called as resource database,which contains all system objects..below is what MSDN has to say about that..

>The Resource database is a read-only database that contains all the system objects that are included with SQL Server. SQL Server system objects, such as sys.objects, are physically persisted in the Resource database, but they logically appear in the sys schema of every database. <br/>
The Resource database does not contain user data or user metadata
