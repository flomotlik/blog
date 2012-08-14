---
layout: post
title: Notifications and Selecting Branches
author: Flo
description: Use Hipchat Notifications and Select the Branches you want to build
---

Today we announce two new features to help you implement Railsonfire
in your workflow. We will show how to use Hipchat for notifications and
select/skip the branches you want to have built.

###Hipchat Notifications

You can set up Hipchat Notifications by simply calling

    railsonfire notifications hipchat

in your appliction folder. The command simply sets the following config
parameters (which you can also set with railsonfire config add)

    hipchat_key=xb9d35fhcle53n7586c09e9c1pedes
    hipchat_room=CI

When setting these config parameters a Test message will be posted to
Hipchat to make sure everything worked.
![Hipchat Notifications](/images/notifications/setup.png)

From now on every time a build is started and finished on Railsonfire a message will
be posted to Hipchat.
![Hipchat Notifications](/images/notifications/notifications.png)

We will expand from Hipchat to other Chat providers like Flowdock or
Campfire.

###Select and Skip Builds

After several feature requests (e.g. [@ayb](https://twitter.com/ayb) and
[@JohnMetta] (https://twitter.com/@JohnMetta) sent us nice and long
emails and helped us discuss the topic) to select which branches should be built
and how to skip specific branches or builds we have now implemented a
way to accomplish this.

We have implemented a way to opt-in (select branches) and opt-out (skip
branches/commits) of specifc builds.

We will briefly describe both featuers, but you can always read about
them on our [Help
Pages](http://help.railsonfire.com/setup/skip-and-select.html).

####Select branches to be built
To limit the branches you want to build you can set the ci_branches
config parameter with a comma separated list of branch names.

    railsonfire config add
    KEY:ci_branches
    VALUE: master,production

This will only build the ***master*** and ***production*** branch from
now on.

####Skip CI

To Skip a branch or push simply add ***--skip-ci*** either to the
branch name or the commit message of the last commit before you push.

So for example a branch that is named ***featurex--skip-ci*** will be
skipped.

###Conclusion
With Hipchat Notifications and select/skip you can now use Railsonfire
more than ever before in your day to day workflow and configure it to
fit the way you and your team works and deploys.

If there are any other features you would like to see in Railsonfire
tell us via [Twitter](https://twitter.com/railsonfire), our Olark Chat
box in the right lower corner or by [email](mailto:flo@railsonfire.com).
