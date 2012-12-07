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
you can't afford to refrain from it:

* making releases a breeze
* faster development
* immediate user feedback
* happy developers

[One of our last posts](/2012/11/19/Amazon-auto-scaling.html) covered why it is important to deliver value to your customers with continuous deployment.

That’s why we gathered 7 steps for you to get started with continuous deployment.
Leave a comment if you have any additional tips on how to speed up
development.

### Test/Test/Test, but be smart about it
If you do not test your software automatedly, you are doing it wrong. There is no way around automated testing.

You don't have to write a test for every small piece of code.
But if you can’t test your main features automatedly, you are already screwed.

Witout tests you release buggy software. You break features that worked in the past and you slow down your innovation. In a well tested application you can introduce changes everywhere without the fear of breaking anything. This gives you the power to change EVERYTHING at all times.

In our next post we will get you started with tools like [Selenium](http://seleniumhq.org/)
or [Phantomjs](http://phantomjs.org/). There are various integrations in every programming language, so stay tuned!

### Build small services
Split your system into small parts that solve one specific problem
and interact with other parts through clearly defined interfaces.

You will innovate faster, build more stable systems and improve your team workflow thanks to the clear boundaries between single parts of your application.

### Automate Deployment
If you run more than one command to deploy your application, you are doing it wrong. Have all of your deployment steps accessible through one command. The command shouldn't require additional configuration.

As soon as it is even a little more complicated you won't do it frequently and this slows down your development.

### Automate Rollback
Reverting your application to an old version should be as easy as releasing a new version. This is especially important for your database. You probably won’t risk a rollback if it is hard to do and involves several steps.

### Deploy to Staging
If you are not continuously deploying to your production system, at least do so for a staging environment, so you can evaluate it anytime.

The latest version of your application should always be running somewhere, so
your team can access and test it.

### Use your staging environment
If you build a product that you use yourself, always work on the staging environment. As soon as you break something you will be the first to notice and you will fix it quickly, because you rely on it.

### Automatically deploy to production
With all these steps done you are ready to deploy continuously to your production system.
You have built all necessary guards to ensure that only working code is released.
And if a bug slips through, you can go back to a stable version fast.

###Conclusion
By following these steps you will get your project on track for continuous deployment and boost your development speed. If you have any questions, just drop us a line here or [email](mailto:help@railsonfire.com) us.