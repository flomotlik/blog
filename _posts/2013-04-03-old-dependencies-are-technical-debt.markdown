---
layout: post
title: Old dependencies is your second biggest technical debt
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Always stay up to date in dependencies, otherwise you will run into constant trouble
image: http://blog.codeship.io/images/200x200_casper_js.png
---

Let's get one thing out of the way immediately. Your biggest technical debt 
is and always will be not having tests.
Not being able to verify that the latest change didn't break some other
part of your application will cost you endlessly every single day.

Implement a new feature, better do a manual check of all the old features! No
time for good and exclusively manual QA (if that even exists), you just broke
your production server. Don't pass go, don't collect 200$.

Code smells or a bad infrastructure can be rewritten as long as you have a way to
assess if it still works. You won't be able to do that though without tests in place.

We have recently written about [how to get started with testing](http://blog.codeship.io/2013/03/15/Testing-top-to-bottom.html),
so take a look there to get started. It pays off immediately.

Another very real problem that hits development teams at the worst of times is having 
outdated dependencies in your application.

##Stay fast, stay up to date
Being able to introduce changes into your application, and especially production
system, is one of the most important metrics to watch in your development. Anything
that could potentially cause trouble is worth investing time to make sure
it will never impact your team.

Having outdated dependencies slows you down in different ways. 

First of all new versions often include new features that can help you in building a 
better and more efficient product. Staying up to date with the latest changes and features 
will reduce the number of changes and the complexity you have to deal with at one point.
It doesn't necessarily mean to always use the latest and greatest new improvement a dependency
provides, but at least being up to date and being able to deal with the upgrade is a big
improvement.

The second problem you will have shows itself as soon as you are hit with a bug that is caused by one
of the dependencies. If you stay up to date, an update is then probably only a minor effort to fix
the bug in your system. Due to the complexity of software a simple update of one dependency may
force you to update different parts as well. If that pile of outdated code becomes bigger the time
you need to fix it grows dramatically.

And we know that pain very well. Even though we update regularly only a slip of 3 months for some of our libraries
caused a big pain to bring everyting up to date. If we did it immediately it would have taken far less time
to get all the tests to pass again.

##Conclusions
In our team speed and being able to change our application frequently and quickly
is one of the biggest assets. By using tools like [Versioneye](http://www.versioneye.com/), 
[Gemnasium](https://gemnasium.com/) or simply ***bundle outdated*** we can make sure that we are
up to date and keep moving fast, so we build the best product we can. Having a process in place that
takes care of the small steps today can go a long way in preventing you from taking a huge hit
to your development tomorrow.