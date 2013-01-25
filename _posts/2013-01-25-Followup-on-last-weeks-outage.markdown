---
layout: post
title: Followup to last weeks outage
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Why the system failed last week and what we did to not ever have that happen again
image: http://blog.codeship.io/images/avatar.png
---

Last week we had a major outage of our service for a day. We solved that
problem and implemented safeguards so this problem can't arise anymore.
After making sure for several days that it works we want to go through
the problem in depth and how we handled.

##Problem
The basic problem we faced last week was that for one project the
log output that was stored into our database was immense. Up to
150.000.000 characters for one command. This caused every SQL request
against this specific Step to become ***really slow*** (>3 seconds)

This in turn slowed down our workers as every time they had to write a
log update, into our database for the previously mentioned project, which was often, the request would be very slow.

The fallout of this was that since our queue filled up to the brink our Redis service hit it's maximum storage capacity and closed down for any further connection.

And this is when the whole system went belly up and no builds went
through any more (although all of them were stored in the database and
rerun at a later time)

##Resolution
It took us quite some time to figure out the exact problem as we at
first thought that we hit some database level problems with Heroku and
upgraded our Postgres database there.

After recognizing the specific problem we have now implemented a limit
of 200.000 characters for the log of every command you run. This is more
than enough to output any information necessary, while still keeping our
database manageable. From tens of thousands of logs stored in our database
only 410 are over this limit of 200.000 so this is only a problem for
very very few projects.

When a log hits the limit we append a message to the end of the command that
informs of the limit. The commands are run without any interruption.

##Improvements
We not only fixed this bug and thus made our infrastructure much more
resilient we additionally upgraded and fixed several possible
bottlenecks we suspected to be the problem.

We have now upgraded to bigger postgres databases on Heroku as well as
bigger redis instances.

While doing a code review on our test servers we fixed a couple of bugs
that could potentially hit us in the future.

##We are sorry and thankful
We want to end by saying we are really sorry that this happened and we
are working hard on preventing any such failures in the future through
better monitoring and a much stronger system.

We have given the team that hit this bug one month service for free, as
we are very thankful they helped us in making our system better. Please
don't take that as a "Challenge accepted" and shoot us down, but we are
very thankful for any hint we get about bugs or problems in our system.

We rely on your feedback to make this ***the most kickass product***.
