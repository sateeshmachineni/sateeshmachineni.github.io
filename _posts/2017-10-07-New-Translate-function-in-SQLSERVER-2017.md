---
layout: post
title: Translate function in SQLSERVER
tags:   sqlserver
---

SQLSERVER 2017 introduced a new feature called TRANSLATE which works similar to `REPLACE` function,but very flexible and powerfull.

Let's see a quick demo to understand how this works..

```sql
declare @a nvarchar(max)

set @a='a(b(c?,/'
```

If we want to get only alphabets from above string like 'abc'

we can use replace like below to achieve this

``` sql


select 
REPLACE(
REPLACE(
REPLACE(
REPLACE(@a,'(',''),
'?',''),
',',''
),
'/','')
```

But with SQLSERVER 2017/VNext.. you can use `TRANSLATE` like below

``` sql
select TRANSLATE(@a,'(?,/','    ')
```

below is the output..

>a b c   

few points to note..

- Translate preserves white space
- Number of characters in second expression should be equal to third expression.so below code won't work

```sql
declare @a nvarchar(max)

set @a='a(?'

select TRANSLATE(@a,'(?',']]]')
```

That's Pretty much it.Thanks for reading









