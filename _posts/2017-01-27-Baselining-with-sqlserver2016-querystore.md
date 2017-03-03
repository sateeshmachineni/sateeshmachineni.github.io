---
layout: post
title: Baselining with SQLServer 2016 QueryStore
category: SQLServer
comments: true
tags:
    - sqlserver
---
 
 I am big fan of baselining SQLServer,it helps you  understand
 
1.)Why my server is slow all of a sudden  
2.)Maximum IO speed your  disk's can sustain with out any performance impact  
3.)Do you have CPU pressure  
4.)what are the queries that are using maximum IO,CPU,Memory over time   

Many more...and most importantly ,this helps you answer all why ****** ? questions..


With SQLServer 2016,Baselining became much more easier..with the introduction of QueryStore..

when you enable QueryStore..SQLServer captures plans of all queries that ran on the server.

