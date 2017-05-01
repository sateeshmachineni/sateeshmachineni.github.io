---
layout: post
title: SQLAzure DMV'S and DTU's explained
category: SQLAzure
comments: true
tag:  sqlazure
---

This post is  part of Troubleshooting SQLAzure series.You can view all the posts by clicking [here](https://sateeshmachineni.github.io/tags/sqlazure/)

SQLAzure is  database as service.It also  differs from our normal onpremises server in a number of ways,
most important part being DTU's.so for the rest of this post,we will focus on DTU's.


When  we procure our onpremises server,we will  purchase hardware(RAM,CPU,HardDisk(Raid 1 or 5..))  based on many factors. When we 
procure  SQLAzure database ,our database will be  assigned DTU's based on our service tier..


DTU simply put is the  quantity of `CPU,IO,Memory` assigned to our database tier based on our subscription.It is difficult to get
actual assigned values like (1 GB ram,1 core ,raid 1 disk) but few people were able to get some rough estimates on how this DTU's map to actual Hardware


so when  procuring  a database ,you will need to look at number of DTU's you have ,as this directly affects the performance.

Below is a quote from Microsoft on the same  (Emphasis in bold, mine)

>When your workload exceeds the amount of <b>any</b> of these resources, 
your throughput is throttled - resulting in slower performance and timeouts. 

 DTU's are directly tied to Performance,Less DTU's equates to less performance.tFurther Microsoft says

>Doubling the DTUs by increasing the performance level of a database equates to doubling the set of resource available to that database.


If you need to understand more on DTU,i recommend reading [What the heck is a DTU?](https://sqlperformance.com/2017/03/azure/what-the-heck-is-a-dtu)

Below screenshot shows   rough estimates of DTU to service tier  taken from [Andy Mallon](https://twitter.com/AMtwo) [post](https://sqlperformance.com/2017/03/azure/what-the-heck-is-a-dtu)

<img src="/img/DTU.PNG" />

I am not sure why Microsoft reinvented the wheel and introduced a new concept called DTU,But i strongly
wish Microsoft changes this  and provide a chart like the one we saw above

In the next post ,we will look at tools we have to troubleshoot SQLAzure  







