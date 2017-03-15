---
layout: post
title: Reset SQLAzure Password
tags:   sqlazure
---

Changing SQLdatabase Password is very easy.you have to just log into the portal and change new one.see below screenshot for more details

<img  src="/img/changepassword.PNG"/>

All we have to do is click reset password,it won't ask for old password.Now that we know,how to change password.Let's see a few best practices with Logins in SQLAzure database.

If you can observe the screenshot,i am in the server tab while changing the password.This is because,Azure groups our databases under one logical server.

SQL Azure is database as a service,there is no concept of server,the logical server we just used was provided for ease of use..

But Microsoft has few things to say about this

>As Microsoft evolves the SQL Database service and moves towards higher guaranteed SLAs you may be required to switch to the contained database user model and database-scoped firewall rules to attain the higher availability SLA
and *higher max login rates for a given database*. Microsoft encourage you to consider such changes today.

This is not a hard thing to do as well

In earlier model,you will be creating a user like below

``` sql
create user marydb1 from login mary;
```

in new model,as per microsoft recommendations,you have to write some thing like below

``` sql
CREATE USER mary WITH PASSWORD = 'strong_password';
```

See how simple it is..

That's it .Thanks for reading,please leave a comment for any queries
