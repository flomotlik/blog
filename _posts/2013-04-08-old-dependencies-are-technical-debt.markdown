---
layout: post
title: Old dependencies is your second biggest technical debt
author: Florian Motlik
twitter: flomotlik 
google: 115123333592137547204
description: Always stay up to date in dependencies, otherwise you will run into constant trouble
image: http://blog.codeship.io/images/200x200_casper_js.png
---

Being able to introduce changes into your application is incredibly important. Having outdated 
dependencies slows you down in various ways.

##Stay fast, stay up to date

First of all new versions often include new features that help you build a 
better product. Staying up to date with the latest version and features 
will reduce the effort when you eventually have to update all your dependencies.

The second problem will appear when you have to fix a bug caused by one dependency. 
If you stay up to date, it is easier to fix the bug by updating the one dependency. 
Due to the complexity of software updating one dependency may require you to update others as well. 
If that pile of outdated code becomes grows, the time you need to update it grows exponentially.

We know that pain very well. Even though we update regularly only a slip of 3 months for some of our libraries
caused a big pain to bring everyting up to date. If we did it immediately it would have taken far less time
to get all the tests to pass again.

##No tests is your biggest technical debt

Your biggest technical debt is and always will be not having tests.
Not being able to verify that the latest change didn't break some other
part of your application will cost you endlessly every single day.

You can rewrite any part of your application to make it better, as long as you have a way to
prove that your application still works. But you can't prove it without tests.

We have recently written about [how to get started with testing](http://blog.codeship.io/2013/03/15/Testing-top-to-bottom.html). 
It pays off immediately.

##Conclusions
In our team speed and being able to change our application frequently and quickly
is one of the biggest assets. By using tools like [Versioneye](http://www.versioneye.com/), 
[Gemnasium](https://gemnasium.com/) or simply ***bundle outdated*** we can make sure that we are
up to date and keep moving fast, so we build the best product we can. 

We've seen many teams struggle with taking the time to update regularly and write tests.
Having a process in place that takes care of this helped a lot of teams, e.g. updating all dependencies every two weeks.
Taking care of the small steps today can go a long way in preventing you from taking a huge hit
to your development tomorrow.