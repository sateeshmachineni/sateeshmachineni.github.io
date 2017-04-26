---
layout: post
title:Computer internals
subtitle: Each post also has a subtitle
tags: [Computers]
---

Did you ever wondered  how  below peice of code in C++    works

``` cpp
#include<stdio.h>

int add(a,b);

void main()
{

int a,b;
a=10;
b=20;
int c=add(a,b);
printf("%d\n", c);
}

add(int a,int b)
{
return a+b;
}
```
I always wondered ,how people built computers and made them work with only 1 and 0's...I did a lot of research and  below are the books/videos that 
helped in my journey

1.StackOverflow   
2.Charles Petzold Code book
3.[Ben Eater Tutorials](https://www.youtube.com/user/eaterbc)  ..he has a knack for explaining things clearly  
5.This [tutorial by Peter Jay Salzman](http://www.dirac.org/linux/gdb/02a-Memory_Layout_And_The_Stack.php),helped me understand memory layout of a program and Stack Frames   

Now i am onto studying Pointers and then raid K&R C Programming book  



