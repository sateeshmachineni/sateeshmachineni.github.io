---
layout: post
title: How to view SQLAzure deadlocks
tags:   sqlazure
---

Deadlocks are captured automatically  in Azure Database.Below is how you can view  them

Using <kbd>SYS.EVENT_LOG</kbd> DMV  

```sql

SELECT * FROM sys.event_log   
WHERE event_type = 'deadlock'   
AND database_name = 'Database1';
    
```
 
This DMV used to take long time for us.so we contaced MSsupport,they gave me one more query just to read deadlocks,though you can modify the XMl

```sql

SELECT *, CAST(event_data as XML).value('(/event/@timestamp)[1]', 'datetime2') AS timestamp
                     ,CAST(event_data as XML).value('(/event/data[@name=&amp;amp;amp;quot;error&amp;amp;amp;quot;]/value)[1]', 'INT') AS error
                     ,CAST(event_data as XML).value('(/event/data[@name=&amp;amp;amp;quot;state&amp;amp;amp;quot;]/value)[1]', 'INT') AS state
                     ,CAST(event_data as XML).value('(/event/data[@name=&amp;amp;amp;quot;is_success&amp;amp;amp;quot;]/value)[1]', 'bit') AS is_success
                     ,CAST(event_data as XML).value('(/event/data[@name=&amp;amp;amp;quot;database_name&amp;amp;amp;quot;]/value)[1]', 'sysname') AS database_name
              FROM sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null)
              where object_name like '%deadlock%'
```



