---
layout: post
title: Ilasm and ILdasm to the rescue
tags: [sqlserver,c#]
---


Today, i was trying to learn Query store,so i took virtual lab course,after completing the  course i  downloaded  lab files to   practice on my local machine..

Those lab files contains some `.sql` files and an executable..The idea is that ,if you run the executable,executable will run those `.sql` files .

But when i ran the exe on my local machine,i got below error..

>Unhandled Exception: System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while 
establishing a connection to SQL Server. The server was not found or was not accessible. 
Verify that the instance name is correct and that SQL Server is configured to allow remote connections. 
(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) ---> 
System.ComponentModel.Win32Exception: The network path was not found
   --- End of inner exception stack trace -

 connection string is hardcoded inside EXE to 

>Data Source=SQL2016SP1HOl;Initial Catalog=QueryStoreDemo;Integrated Security=True

Above server name is virtual labs server.so the lab files can't be of any use to me ..

Luckily,that was not the end of the story.You can modify .NET DLL's or EXE's..below are the steps i followed in sequence...

use  `ildasm` to dump the [Common intermediate Language](https://en.wikipedia.org/wiki/Common_Intermediate_Language) code from EXE

``` sql

ildasm.exe c:\pathtoexe /out:qs.il

```

now that i  have IL code,i opened it using text editor and modified connection strings in sqlconnection class and saved the IL file


Finally , i used `ilasm` to convert the il code to exe

``` sql
ilasm c:\path to il  qs.exe
```

Voila,i now have an EXE,which i could use on my local machine.They could have wrote an EXE which could have accepted connections strings as arguments 




