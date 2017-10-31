---
layout: post
title: Query Tuning approach for SQLAZURE database
tags: [sqlserver]
---

Tuning SQLAZURE database is simple,once you understood the concept of DTU.Based on the tier we  procure our database is assigned some DTU units.
I have procured a  database in Standard S0 tier which has 10 DTU units..I can't really understand how this translates to amount of CPU,IO,Memory i will get..but few key take aways 

- Increasing DTU units increases performance or provides more power to database  
- If your workload is hitting the limits in one of CPU/Data IO/Log IO limits, you continue to receive the resources at the maximum allowed level, but you are likely to see increased latencies for your queries


With this points in mind,let's run some resource intensive queries  and I will show you my approach in tuning those..

Below is a query which asks for top3 order ids for each customer..There are around 100000 customers in the table and 2 million orders.

```sql
select 
* from Customers c
cross apply
(
select top 3 orderid from  Orders O
where O.custid=c.custid) b
```

This takes around 70 seconds to complete  on my S0 database..

now let's run the same in multiple sessions and see which database metric is overloaded.While those queries are running in background,lets run below DMV's
to understand more on resource utilization..

``` sql

select * from
sys.dm_exec_requests

select *
from sys.dm_db_resource_stats
order by end_time desc

```

Below are the screenshots of both the queries in their respective order



Few things obvious from screenshot..

1.Most of the queries are CPU bound 
2.There is also Memory and IO contention






