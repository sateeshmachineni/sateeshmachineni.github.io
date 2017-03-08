---
layout: post
title: Difference between TraceFlags 1204 and 1222
tags:   sqlserver
---
 
 I did a quick search to know what are the differences,but i am unable to find any...So thought of posting this 
 
 Traceflag `1204` was introduced in SQLServer 2000 and it outputs deadlock info to SQLServer error log in text format..
 
 Below is a sample of Traceflag `1204`from my instance..(stripped node 2 info)
 

 >Node:1
RID: 6:1:36667:0               CleanCnt:3 Mode:X Flags: 0x2
 Grant List 0:
   Owner:0x800E3640 Mode: X        Flg:0x0 Ref:0 Life:02000000 SPID:57 ECID:0 XactLockInfo: 0x815166F4
  SPID: 57 ECID: 0 Statement Type: SELECT Line #: 1
   Input Buf: Language Event: select * from t1
 Requested By: 
   ResType:LockOwner Stype:'OR'Xdes:0x81516C70 Mode: S SPID:56 BatchID:0 ECID:0 TaskProxy:(0x81678378) Value:0x800e3800 Cost:(0/216)


>Victim Resource Owner:
ResType:LockOwner Stype:'OR'Xdes:0x815166D0 Mode: S SPID:57 BatchID:0 ECID:0 TaskProxy:(0x81540378) Value:0x800e3480 Cost:(0/216)

 The above info is divided into nodes,with each participating query divided into one node and finally it shows Resource list as well
 
 Now Let's see,what trace flag `1222` outputs
 
 
 
 >deadlock-list
 deadlock victim=process66af28
  process-list
   process id=process66af28 taskpriority=0 logused=216 waitresource=RID: 6:1:36665:0 waittime=3369 ownerId=2548482 transactionname=user_transaction lasttranstarted=2017-01-13T15:58:13.267 XDES=0xffffffff815166d0 lockMode=S schedulerid=1 kpid=182300 status=suspended spid=57 sbid=0 ecid=0 priority=0 transcount=1 lastbatchstarted=2017-01-13T15:58:28.807 lastbatchcompleted=2017-01-13T15:58:17.073 clientapp=Microsoft SQL Server Management Studio - Query hostname=DEV-WKST-121 hostpid=169220 loginname=sample isolationlevel=read committed (2) xactid=2548482 currentdb=6 lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200
    executionStack
     frame procname=adhoc line=1 sqlhandle=0x02000000f299431ea3398702b2d18a7464d256a4d8ab2963
select * from t1     
    inputbuf
select * from t1
   
  
 ><b> resource-list</b>
  ridlock fileid=1 pageid=36667 dbid=6 objectname=PerformanceV3.dbo.t2 id=lockffffffff85b1e8c0 mode=X associatedObjectId=72057594049724416
   owner-list
     owner id=process66af28 mode=X
    waiter-list
     waiter id=process66b978 mode=S requestType=wait
   ridlock fileid=1 pageid=36665 dbid=6 objectname=PerformanceV3.dbo.t1 id=lockffffffff85b1eac0 mode=X associatedObjectId=72057594049658880
    owner-list
     owner id=process66b978 mode=X
    waiter-list
     waiter id=process66af28 mode=S requestType=wait


This output is much more readable and is divided into two sections `processlist` and `resourcelist`.Saving this as XML and opening this xml file ,makes this mcuh easier to read..

that's pretty much ,what i wanted to share..

Thanks for reading ..!
 
 
