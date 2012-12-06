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

In one of our [last posts](/2012/11/19/Amazon-auto-scaling.html) we
talked about why it is important to deliver value to your customers and
how continuous deployment can help there.

Afterwards we got asked a lot 'So continuous deployment is great, but how
do I get started'. There are a couple of steps you can take to get started with it quickly.

We will go through them, but of course there are many different ways to
approach this. Leave a comment to tell us about what exactly you did
to get continuous deployment working for you.

### Test/Test/Test, but be smart about it
If you do not test your software in some automated fashion, you are
simply doing it wrong. There is just no way around that.

Of course you don't have to write a test for every small piece of code.
Not every simple helper method needs to be tested. But if you do not
have automated tests for your main business workflows you are already
screwed.

You are not just hit by releasing buggy software all the time and
breaking features that worked in the past, but you slow down your
innovation. If you have a well tested application you can introduce
changes everywhere without the fear to break important features. This
gives you the power to question and change EVERYTHING all the time.

Start at the top by using tools like [Selenium](http://seleniumhq.org/)
or [Phantomjs](http://phantomjs.org/) and the various integrations there
are in every programming language. You can even use
[SauceLabs](https://saucelabs.com/) who provide hosted Selenium tests to
get started. As soon as you have tests for your main business workflows
(signup, login, payment, ...) you can start going into more specific
parts of your application.

### Build small services
This is also kind of a no-brainer. The smaller and more isolated a
service is the easier it is to test and introduce change.

Additionally you can deploy parts of your whole stack without impacting
any other parts not changed by that deployment.

You can innovate faster, build much more stable systems and improve your
team workflow as you have clear boundaries between the different parts
of your application.

### Automate Deployment
If you have to run more than one command to deploy you are doing it
wrong. You need to have all of the steps accessible via one simple
command. The command shouldn't have any parameters and be very clear to
everyone.

As soon as it is even a little more complicated you just won't do it
that often and this hurts your development speed.

### Automate Rollback
Rolling back to an old version, especially the database, should be as
easily accessible as pushing a new release. When going back to a stable
version involves several steps and is hard to do you just wont take the
risk to do it a lot.
### Deploy to Staging
If you are not deploying to your production system continuously you
should at least push to a staging environment all continuously, so you
can evaluate it anytime.

All of your team should be able to see and test the latest version of your code before
pushing it to your production system.
### Use your staging environment
If you have a product that your team uses (which you should) always work
on a staging environment. As soon as you break anything you will notice
first and fix fast as you need it to be stable.
### Automatically deploy to production
When you have done all of the above now you are ready to deploy
continuously to your production system. You have built the necessary
guards to make sure only good code is released to your production system
and if a bug slips through you can be sure you can go back to a stable
version fast.

###Conclusion
By following the layed out steps you can start getting your project on
track for coontinuous deployment and really boost your development
speed. If you have any more questions you can always send us a message
or an email.
