---
layout: post
title: Followup to last weeks outage
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Why the system failed last week and what we did to not ever have that happen again
image: http://blog.codeship.io/images/avatar.png
---

Last week our service had a major outage for a day. We solved the
problem and implemented safeguards so this problem can't arise anymore.
After verifying that the fix worked several days we want to walk you through
the problem and how we solved it.

##We are sorry and thankful
We want to start by saying that we are really sorry that this happened and we
are working hard on preventing such failures in the future through
better monitoring and a stronger system.

The team that hit this bug got one month free service, as
we are very thankful that they helped us improve our system. Please
don't take that as a "Challenge accepted", but we are
grateful for any hints about problems with our system.

We rely on your feedback to make Codeship ***the most kickass product*** out there.

##Problem
The problem we faced last week was the vast
log output of one project stored in our database. Up to
150.000.000 characters (over 100 MB) for one test command.

This in turn slowed down our workers: Every time they had to update the log in the database
for this project, the SQL query would take more than 3 seconds.
As we update the log more frequently than that, soon we were faced
with an enormous stack of log updates that couldn't be processed in time.

Consequently our queue filled up to the brink and at some point our Redis service hit
its maximum storage capacity and denied any further connections.

This was when the whole system went belly up and couldn't process any more builds
(although all of them were stored in the database and rerun later)

##Resolution
It took us a while to spot the problem. At
first we thought that we hit some database level problems with Heroku and
upgraded our Postgres database there.

After discovering the real problem we limited the maximum log size for each command
to 200.000 characters. This is more
than enough to output any information necessary, while still keeping our
database manageable. From tens of thousands of logs stored in our database
only 410 exceeded this limit, so this change will affect hardly any projects.

When your log hits the limit we append a message that
informs you about it. The commands keep running without any interruption.

##Improvements
We didn't only fix this bug and thus made our infrastructure more
resilient but additionally removed several possible
bottlenecks we suspected to be responsible for the outage.

We upgraded to larger postgres databases on Heroku as well as
larger redis instances.

While reviewing code on our test servers we fixed a couple of potential problems
that could have hit us in the future.

Sorry again for the outage, but thanks for your patience.

##Hiring

By the way, if you are interrested in making our system even stronger: We
are currently hiring! We are looking for a web developer with lots of
experience in Ruby on Rails and another developer helping with our test server
infrastructure running on Amazon EC2 and implemented in Ruby. Take a look at our [Jobs
Page](https://www.codeship.io/jobs) to find out more.
