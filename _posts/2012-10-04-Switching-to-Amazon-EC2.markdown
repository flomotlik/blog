---
layout: post
title: How and why we switched to Amazon EC2
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: A comparison about the advantages and disadvantages of various platform-as-a-service providers like Amazon EC2, Rackspace, Linode or Hetzner.
image: http://blog.codeship.io/images/amazon/aws.png
---

***This will be a two part series due to popular demand. This first part deals
with the decisions behind switching to Amazon, the second part will
deal with the nitty gritty implementation details we use to auto scale
our infrastructure.***

***UPDATE: [PART 2](http://blog.codeship.io/2012/11/19/Amazon-auto-scaling.html) is
now released, so go and read it as well***

For a good number of months we had our infrastructure based on
[Hetzner](http://www.hetzner.de/) but due to our service growing and a
need to scale dynamically we decided to research cloud platforms and
move to the cloud. Additionally we experienced severe networking
issues from time to time which forced us to switch.

###Requirements
We had several other requirements as well, but following are our core
needs a cloud service has to address.

####Scaling

We want to be able to scale to any size at any time without
limiting ourselves by having to set up or manage physical servers. A
cloud provider with good automation support was thus the only viable route to go for us.

####Flexibility

We want to stay flexible in what type of server infrastructure (number
of cores, RAM size, ...) we use so we can innovate continuously and also
provide different infrastructure for the differing needs of our
customers. This also has a major role in our costs as we can use the
exact server type we need for a specific task.

####Automation

As a hosted continuous deployment service releasing changes and scaling automatically is a necessity.
When automating the whole process of deployment and scaling
we can provide a much better service with fewer points of failure and
faster recovery in case of an error. Additionally it allows us to
decouple the scaling of our infrastructure with scaling our team. We
simply do not have to hire additional server admins because we run
a much larger infrastructure.

###Providers
<img src="http://blog.codeship.io/images/amazon/aws.png" title="amazon web services" style="width: 200px; float: left; margin-right: 5px;"/>We looked at various Providers including [Rackspace](http://www.rackspace.com/),
[Linode](http://www.linode.com/), [ElasticHosts](http://www.elastichosts.com/)
and [Amazon EC2](http://aws.amazon.com/). Especially Rackspace was great
and their support is just incredible, but in the end
we decided to go with AWS EC2 as they provide a lot of flexibility with their
various [Instance Types](http://aws.amazon.com/ec2/instance-types/). We looked into ElasticHosts only a little, but
they seem fine as well. Linode was out of the picture fast, as they do
not provide per hour pricing.

###Implementation

We will split this section into two parts, Automated Deployment and
Automated Scaling, which describe how we introduce changes to our backend
infrastructure and how we scale our backend respectively.

####Automated Deployment
One of our guiding principles is, not surprisingly, to automate
deployment as much as possible. Implementing continuous deployment for a
website is rather easy (especially when using Codeship)
compared to continuously deploying Amazon AMIs. We use the following workflow

  1. When pushing to our backend repository on GitHub a new Codeship
     build is triggered
  2. The build runs the backend unit tests to make sure our scripts work
     fine
  3. After all unit tests succeed a new Amazon Instance is started with a
     default Ubuntu 12.04 image. We then connect to that image via SSH,
     upload the necessary setup scripts and start a background setup
     process.
  4. The setup process includes installing various linux packages and
     setting up our LXC guest system which we use to virtualize the EC2
     servers. This process takes approximately 2 hours as we compile and install lots
     of software.
  5. After everything is set up a test build with our test
     repository is run to make sure everything is installed correctly
     and works as expected.
  6. If everything works fine we instruct Amazon to create a new AMI
     that we can use as a base image to start new servers.
  7. As soon as the AMI is ready it is automatically used by our
     scaling system.

We have used this process before we switched to Amazon as well with our
Hetzner Infrastructure, but improved it drastically when we switched to
Amazon. It makes changing our backend incredibly easy and gives us
extreme power and control in innovating our service. We simply love it
and couldn't imagine working without it anymore.

And not to forget having this automated system makes it much safer and
less error prone. It is really hard now to accidentally introduce errors
into the system as it is checked several times and improved continuously.

####Automated Scaling

We automatically scale our infastructure up and down depending on the
number of builds we currently need to run. Every time a new build is
started we make sure that enough resources are available for the build
to start immediately. If there aren't enough backend servers running we
start another EC2 instance. As EC2 instances start in a matter of seconds
the delay is only very minimal and not noticeable.

Upon completion of every build and every ten minutes through a cron job
we check if there are any EC2 resources currently not necessary and stop
the servers accordingly. We make sure that we use as much of every EC2
hour as possible by only stopping them shortly before they incur further
expenses.

###Conclusion

It took us quite some time to build the current infrastructure and the
automation to get the most out of it. But this is only the beginning. We
have major plans to innovate on this very stable platform to give you
unparalleled speed, flexibility and prize. For example we will shortly
give you the ability to run your tests on Instances with up to 8 cores
to parallelize your tests and reduce the time your tests take
dramatically.

We are really thrilled for the future of Codeship and can't wait to
tell you all the good news and updates we have in store over the next
weeks.
