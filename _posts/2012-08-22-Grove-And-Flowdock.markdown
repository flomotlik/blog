---
layout: post
title: Grove.io and Flowdock Notifications
author: Flo
description: How to set up Grove and Flowdock Notifications
---

After releasing Hipchat Notifications and reimplementing our
Notifications backend we now proudly support Grove.io and Flowdock as
well.

You can read the following and the Hipchat Setup guide at our [Help Page
Article](http://help.railsonfire.com/setup/Notifications.html) as well.

##Flowdock setup
Go to your Flowdock [Account Token
page](https://www.flowdock.com/account/tokens) and copy the flow API
token for the specific Flow you want to get notifications sent to.

Then in your application folder run

    railsonfire config add

and enter as the key ***flowdock\_key*** and as value your API Key.

You will get a Message that from now on this Team Inbox is set up to
receive
notifications from Railsonfire.

##Grove.io setup
Go to the channel you want to get notifications to and click on the
settings link in the upper right corner. Then go to service integrations
and click on channel token. Copy your channel Token from there.

Then in your application folder run

    railsonfire config add

and enter as the key ***grove\_key*** and as the value your channel
token. You will get a Message that from now on this channel is set up to
receive
notifications from Railsonfire.

Setting up your notifications is a simple and effective way to stay on
top of your workflow and see what happens and changes in your system.
Use it.
