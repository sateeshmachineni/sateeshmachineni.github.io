---
layout: post
title: Hash_bytes input limit in SQLserver 2016
tags: [sqlserver]
---

[Hash_Bytes](https://msdn.microsoft.com/en-us/library/ms174415.aspx) is a usefull function to calculate hashes for various inputs.Versions
earlier than SQL2014 ,limited input size to 8000 bytes..

With SQLServer 2016 RTM,the input limit is removed..

Below is a sample test that demonstrates this

```
select hashbytes('SHA1',replicate('a',8010)) as 8010length,hashbytes('SHA1',replicate('a',8010)) as 800length

```

Above code produces below output
```
0x937E57D0082C4382720716CA878F12C325F7BDE7,0x937E57D0082C4382720716CA878F12C325F7BDE7
```

As you can see ,output is same,this is due to the fact that,SQLServer considered only first 8000 characters.

Now let's see what happens ,when we run the same query in SQLServer 2016 or SQLAzure(Azure is always upgraded to latest verison,regardless of edition you choose)


