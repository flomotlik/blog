---
layout: post
title: 7 Steps to Continuous Deployment
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: 7 steps to take to get to continuously deploying your application
image: http://blog.railsonfire.com/images/cd/adamgod.png
---
![Creation of Adam](/images/cd/adamgod.png)
*Image obtained from [Wikipedia](http://en.wikipedia.org/wiki/File:Creaci%C3%B3n_de_Ad%C3%A1n.jpg)*

Releasing new parts of your software daily has so many advantages, that
you can't afford not to do it.

* making releases a non-issue
* increased development speed
* faster user feedback
* developer happiness

In one of our [last posts](/2012/11/19/Amazon-auto-scaling.html) we
talked about why it is important to deliver value to your customers and
how continuous deployment can help there.

We have gathered 7 steps for you to get started with continuous deployment.
Leave a comment if you have any additional tips or steps you took to speed up your
development.

### Test/Test/Test, but be smart about it
If you do not test your software in some automated fashion, you are
simply doing it wrong. There is just no way around that.

You don't have to write a test for every small piece of code.
But if you do not have automated tests for your main features
you are already screwed.

You are not just hit by releasing buggy software all the time and
breaking features that worked in the past, but you slow down your
development. If you have a well tested application you can introduce
changes everywhere without the fear to break something. This
gives you the power to question and change EVERYTHING all the time.

Our next post will focus on how to start doing this with tools like [Selenium](http://seleniumhq.org/)
or [Phantomjs](http://phantomjs.org/) and the various integrations there
are in every programming language.

### Build small services
Split your system up into smaller parts that solve one specific problem
and integrate with other parts of your infrastructure through clearly
defined interfaces.

You can innovate faster, build much more stable systems and improve your
team workflow as you have clear boundaries between the different parts
of your application.

### Automate Deployment
If you have to run more than one command to deploy you are doing it
wrong. You need to have all of the steps accessible via one simple
command. The command shouldn't have any parameters or need any
additional configuration you have to remember.

As soon as it is even a little more complicated you just won't do it
that often and this hurts your development speed.

### Automate Rollback
Rolling back to an old version, especially for the database, should be as
easy as pushing a new release. When going back to a stable
version involves several steps complicated steps you just wont risk
releasing new software all the time.

### Deploy to Staging
If you are not deploying to your production system continuously you
should at least push to a staging environment all the time, so you
can evaluate it there.

The latest version of your code should always be running somewhere, so
your team can access and test it.

### Use your staging environment
If you have a product that you use yourself (e.g. project management tool) always use
your staging environment for your day to day work. As soon as you break anything you
will notice first and fix fast as you need it to accomplish your daily work.

### Automatically deploy to production
When you have done all of the above now you are ready to deploy
continuously to your production system. You have built the necessary
guards to make sure only good code is released
and if a bug slips through you can be sure you can go back to a stable
version fast.

###Conclusion
By following the layed out steps you can start getting your project on
track for coontinuous deployment and really boost your development
speed. If you have any more questions you can always send us a message
or an [email](mailto:help@railsonfire.com).
