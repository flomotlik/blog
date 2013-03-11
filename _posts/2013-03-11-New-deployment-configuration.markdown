---
layout: post
title: Configure deployments in a minute
author: Clemenshelm
twitter: clemenshelm
google: 116883220210675577340
description: How to setup your deployments in a minute with Codeship.
image: http://blog.codeship.io/images/deploymentconfiguration/engineyard_capistrano.png
---
At Codeship we are always working on improving our service. We get great feedback
from our users telling us which parts of our web application are tedious to handle.
One of these tedious parts was the deployments configuration page.

By redesigning the deployment configuration page we wanted to ease configuring
even complex deployment setups. At Codeship our deployment configuration
has 2 steps:

1. Deploy to the staging server and check if everything went well.
2. Deploy to the production server.

If the deployment to staging fails, the application is not deployed to production.

With the new deployment configuration page we could insert a new deployment
within seconds. But what exactly has changed on the deployment configuration page?

## 1. Overview

The deployment configuration page gives a brief overview of all your deployments
now. Previously it listed all form fields of all your deployments.

![Screenshot deployment page overview](/images/deploymentconfiguration/overview.png)

## 2. Hiding optional configuration

You can selectively choose to edit one of your deployments. In edit mode we show
you only the fields required to make the deployment work.

![Screenshot edit mode collapsed](/images/deploymentconfiguration/edit-mode-collapsed.png)

If you want to benefit from the additional configuration options Codeship offers,
you can display the optional configuration as well.

![Screenshot edit mode expanded](/images/deploymentconfiguration/edit-mode-expanded.png)

Our intention here was to keep the interface as clear as possible while still
offering you a powerful configuration tool.

## 3. Moving deployment steps

Now it's possible to move deployments around. This is especially useful when you
want to insert commands between existing deployments. You can just create a
script containing the commands and move the script to the desired position.

Just grab the deployment step at its logo and drag it to the desired position.

![Screenshot dragging deployment steps](/images/deploymentconfiguration/move.png)

## 4. New deployment options

Something many of you wished for were deployment configurations for capistrano
and engineyard. Voilà, here they are!

![EngineYard and Capistrano deployment](/images/deploymentconfiguration/engineyard_capistrano.png)

## Conclusions

We hope that these additions will ease deploying your application. Maybe they
also encourage you to add additional tools to your deployment process, like
smoke tests or clean-up scripts.

Please let us know if there's something we can still improve. Are there any
hosting providers or deployment tools that you are missing? We're looking
forward to your comments!
