---
layout: post
title: Calculate moving averages
tags: [sqlserver]
---

Today i learnt a new way to calculate Moving averages and it is quite simple with Window Functions Frame clauses.

Thanks to [Itzik Ben-Gan](http://tsql.solidq.com/about/).

Below is sample data which is just a numbers table.If you are not aware of what is numbers table..Check this post 
[Why are numbers tables “invaluable”?](https://dba.stackexchange.com/questions/11506/why-are-numbers-tables-invaluable)

data looks like this

<img  src="/img/numbers.png"/>

Say we want to calculate average for this data .Expected output should be something like below


|  number       |  average      |
| ------------- | ------------- |
| 1             | (1+1)/1       |
| 2             | (1+2)/2       |
| 3             | (1+2+3)/3     |

This is simple..All we have to do it is use `avg` function

``` sql
select   
number,  
avg(number) over (order by id) as avg  
from  
number  

``` 

Now what if we want to calculate moving averages for only three rows..say some thing like this

|  number       |  average      |
| ------------- | ------------- |
| 1             | (1+1)/1       |  
| 2             | (1+2)/2       |
| 3             | (1+2+3)/3     |
| 4             | (4+2+3)/3     |
| 5             | (5+4+3)/3     |
| 6             | (6+5+4)/3     |

This is very easy with SQLSERVER 2012 `Frames` Clause..

below is the query

``` sql
select number, 
avg(number) over (order by number rows between 2 preceding and current row ) as avgg
from numbers
``` 

Windows functions are very powerfull and if you want to learn more..I suggest you explore this book
[Microsoft SQL Server 2012 High-Performance T-SQL Using Window Functions (Developer Reference)](https://www.amazon.com/gp/product/0735658366/ref=s9_simh_gw_p14_d2_g14_i1?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=center-2&pf_rd_r=0MJ2QGKQZXR9922BQYMH&pf_rd_t=101&pf_rd_p=470938631&pf_rd_i=507846)

That's pretty much it .Thanks for Reading.
