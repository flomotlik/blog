---
layout: post
title: How we autoscale our EC2 Infrastructure
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: How we use the EC2 API to Autoscale our Infrastructure and keep track of all changes.
image: http://blog.railsonfire.com/images/amazon/aws.png
---

***This is part two of our Amazon blogposts which deals with how exactly
we autoscale our infrastructure and what our plans are for the future.
You should read [Part 1](/2012/10/04/Switching-to-Amazon-EC2.html) as
well as it lays out why we went with EC2 in more detail.***

***This took a little longer than anticipated, sorry for that, but we were
extremely busy over the last weeks to get our technology as well as our
business to the next level and have some great news to announce in
December.***

###Why Auto scale
Every company, and especially every startup, has to focus solely on
creating value for a customer. This is and always has been the guiding
factor for the success of a company. When you create value and satisfaction
for your customers, they pay you. As simple as this sounds this is what
business boils down to.

The limiting factor for all businesses in talking to their customers,
developing new features and giving great and fast support is ***time***. The
more you have of it, the more you can use it to create value. The goal
is to create as much ***value over the time*** your users engage with your
product.

![Value over Time](/images/amazon/valueovertime.png)

We started Railsonfire to build a product that helps people create more
value for their customers by focusing on what builds value and not on
keeping the infrastructure up.

We apply this thought process to all of our development and software as
well. We automate religiously, so as few tasks as absolutely necessary
have to be handled manually.

By managing all of our backend infrastructure automatically and at the
same time providing safeguards and checks to make sure our servers work
as expected we can focus on delivering value, not on managing
infrastructure.

And of course auto-scaling not only helps us to develop much faster,
but also to provide a better and at the same time cheaper service to our
customers as we really only use what we need.

As explained in the [last
post](/2012/10/04/Switching-to-Amazon-EC2.html) EC2 helps us a lot in
building and maintaining such an infrastructure. They provide a wide range of
technologies we can build upon and access through their api. Even though
cloud infrastructure seems to be more expensive in the short term the
added speed and agility easily compensate for the higher expenses.

Furthermore by auto-scaling and using Reserved Instances you can lower the
costs dramatically. Not to forget we simply do not have to hire any
server admin. We can focus all of our engineering power on building the
tools as there are no maintenance tasks to be performed.

###How do we auto-scale
At [Railsonfire](https://www.railsonfire.com) we have implemented a Scaling
class that has one scale method and
compares our queues with our currently running EC2 workers and then
decides to start or shut down our instances. By providing only one
externally available method that only takes where it was called from as
a parameter we make sure that the right decision is made
every time regardless of the caller.

For example there may be a situation where a new build is created, but
we have lots of spare resources, so the right decision is to shut some
of them down. By letting our scaling infrastructure scale down even when a
new build is created we always make sure that the appropriate number of
resources is available.

You can see a simplified version of our logic to scale up/down our
servers in the next gist.
<script src="https://gist.github.com/4079165.js?file=Scaler.rb"></script>

So when there are more idle workers than open jobs we scale down.

When all test servers are working, there are open jobs and
the number of instances that are currently starting (***instances_count
- workers.count*** gives the number of EC2 instances starting up, but
  not connected to the queue) is less than the number of open jobs we
start a new server.

There are 3 separate occasions where we start our Scaling infrastructure.

* When a new build is created
  we make sure that there are enough resources available for the build
to be performed
* When a build is finished
  so we can shut down any unused resources
* Every ten minutes
  to have an additional safeguard that makes sure that nothing got
stuck

We have experienced that it is important to have a regularly running
process that makes sure that the infrastructure is scaled to where it
needs to be. We solve that by having a Heroku worker started through
their scheduling addon every ten minutes.

####Log your scheduling
Auto-scaling without proper logging and record keeping is like driving
blindfolded. Especially if you detect problems and have no idea how to
debug them.

We heavily use Google Docs to keep track of metrics in our application
and our server infrastructure is no exception there. We have implemented
our own [GSMetrics](https://github.com/railsonfire/gsmetrics) gem to
help us with pushing data into Google Docs. Works really well and makes
collecting and analysing our data very easy.

![Auto-scal log](/images/amazon/auto-scale-log.png)

Whenever a server is started or stopped we write the according data to a
spreadsheet so we can then analyse if all servers are properly accounted
for, how long they were running and so on.

Additionally we have implemented several automated tasks that regularly
check and compare the number of workers registered with our queue as
well as the number of test servers to make sure all of them are running
fine.

This has given us great insight as well as accountability for our
infrastructure. We have automated every single step and additionally
have built safeguards along the way to automatically correct and then notify
our team.

###The future of our infrastructure
We are currently working on the next iteration of our service which will
see some changes to our auto-scaling infrastructure. We are moving to
stronger EC2 instances to give us better speeds. This will decrease the
number of workers running, as each worker can run several builds at the
same time.

The basics of our auto-scaling effort will still be in place though and
guarantee an infrastructure that scales to any size without us having to
worry about it getting more complex.
