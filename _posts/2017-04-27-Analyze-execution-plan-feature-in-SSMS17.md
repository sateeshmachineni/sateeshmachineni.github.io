---
layout: post
title: Analyze execution plan option in SSMS 17
tags: [sqlserver]
---

SSMS 17 was released a few days back,there were lot of bug fixes and some new features are introduced as well.One of the new feature,
i was excited about was, Analyze execution plan..

Lets see how this works..

This option is not present in SSMS, to see this option,you have to right click a saved plan as seen below..


<img  src="/img/analyzeplan.PNG"/>

then SQL tries to analyze the query and presents a list of issues,that can be improved in the plan.

For my plan,SQL thought cardinality estimation is incorrect,you can see the same from below screenshot


<img  src="/img/analyzeplan1.PNG"/>

This feature seems to be half baked for this version of SSMS release,since cardinality estimates were correct for my query.Also ,other
than checking for  cardinality estimates,this seems to do nothing...  

I don't think this is the way to release new features :(..



