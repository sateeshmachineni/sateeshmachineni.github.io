---
layout: post
title: Replace null values with values from prior  not column
tags: [tsql]
---

Given below data

```plsql
1	A
2	NULL
3	NULL
4	B
5	NULL
6	NULL
7	C
8	NULL
9	NULL
```

I would like to see below output..

```plsql
1	A    
2	A  
3	A  
4	B  
5	B  
6	B   
7	C   
8	C   
9	C   

```

One way to solve this would be to use subquery like below

```plsql
select id,isnull(c1.val,b.val) as val
from 
#temp c1
outer apply
(select top 1 val from #temp c2 where c2.id<c1.id and c2.val is not null order by c2.id desc) b
```

This will be horribly inefficient,as this will access table 9 times...you can infer the same from below execution plan


As you can see,there are 9 rebinds,since the outer value changed everytime,SQL has to go main table to get the  value

This may be one of the cases ,where cursor will be effective,since it will store the entire result set once which avoids table access

