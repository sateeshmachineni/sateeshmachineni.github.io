---
layout: post
title: Don't trust missing index recommendations blindly
tags: [sqlserver]
---

Missing index recommendations should be not blindly trusted,even for a single query..Below is a small issue i faced today..You need to 
download [performancev3](http://tsql.solidq.com/resources/) sample database from Itzik Ben-Gan to follow  below example..

<b>Some Context</b>     
You have 2 million rows in an order table and you need to get first order for each customer  and you need to filter it by employee

so below is the query i have 

```sql
;with cte
as
(
select empid,custid,orderdate,row_number() over (partition by custid 
order by orderdate) as rownum
from 
orderstest where empid=486
)
select * from cte where rownum=1
```

For the above query to work effectively, you will need to have a POC index.so i created one like below

```sql
create index nci_order1 on orderstest(custid,empid,orderdate)
```

Imagine you have created a wrong index at first,like below..

```sql
create index nci_order1 on orderstest(custid,empid,orderdate)
```

You will get a missing index recommendation like this 

<img  src="/img/Missing.PNG"/>

and if you create index based on that recommendation..

```sql
create index nci_order1 on orderstest(empid)include(custid,orderdate)
```

you will get below sort..

<img  src="/img/sort.PNG"/>


Above sort can be completely eliminated, if you had created right index(first index in our case) and this also shows, you can't trust missing
index recommendations blindly.

To Learn more on more windows functions,i would refer this book from my favourite author Itzik Ben-Gan and this book :[http://tsql.solidq.com/books/windowfunctions2012/](http://tsql.solidq.com/books/windowfunctions2012/)

Thanks for reading!













