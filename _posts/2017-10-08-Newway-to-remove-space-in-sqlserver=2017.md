---
layout: post
title: TRIM function in SQLSERVER 2017
tags: [sqlserver]
---

SQLSERVER 2017 introduced a new function called `TRIM`.It's just a replacement of `LTRIM,RTRIM`..Let's see a quick demo to see how that works

Given below string

` '    abc    def ghi    '`

what if we need to eliminate trailing and ending spaces

we can use `LTRIM,RTRIM` like below

```sql

select    RTRIM(LTRIM('    abc    def ghi   '))
```

but with SQLSERVER2017, we can use `TRIM` like below

```sql
select    TRIM('    abc    def ghi   ')
```

Also keep in mind this function doesn't remove space in between characters

Thanks for reading
