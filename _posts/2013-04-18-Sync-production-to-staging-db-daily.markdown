---
layout: post
title: Easily sync production and staging databases on Heroku
author: Florian Motlik
twitter: flomotlik
google: 115123333592137547204
description: Sync your production data into staging daily to have a better testing environment
image: http://blog.codeship.io/images/database-copy/200x200_sync-prod-and-staging.png
---


![Sync image](/images/database-copy/codeship_sync-prod-and-staging.png)

*Image via [Arnaud LaPierre](http://arnaud-lapierre.com/)*

Testing your new release or database migrations should always be done on a system that is as similar 
to your production environment as possible. Keeping the codebase and configuration synced between production
and staging is straightforward on most systems, but keeping the data up to date can be a hassle.

Heroku provides some great tools to mirror your production data to your staging database.

We will give you a quick guide on how to set this up with Heroku and their scheduler add-on.

This guide is currently targeted at Rails apps on Heroku, as Ruby and the Heroku Gem are necessary to perform the backup actions.

As long as your database is small enough you can do this during your deployment with Codeship. We will 
show you in the first step how to set this up. As your database becomes larger this step may take too 
long for every deploy and you will want to do this on a regular schedule. The second part of the blog 
post will show how you can set this up as well.

##Heroku Databases

For smaller production databases you might do well with a dev database for your staging environment. 
But as it has a limit of 10.000 lines you might have to upgrade to the basic plan at 9$ per month as 
soon as your database grows.

Being sure your data migrations work well on every deployment is definitely worth the 9$!

You can read more about the Heroku databases and how to get started in their
[Dev Center Article](https://devcenter.heroku.com/articles/heroku-postgresql)

##Sync Database on every deployment to staging

With the Codeship it is really easy to copy your production database into staging before every deployment. 
In your heroku staging deployment just add the name of your production app into the restore_from field.

![Restore from other database on Codeship](/images/database-copy/restore_from.png)

It will then do a backup of the production db and restore it into the staging application. This can take a 
while for larger deployments though.

##Sync in regular intervals for bigger databases
You have to start by adding the Heroku Gem to your Gemfile. This is necessary as Heroku currently doesn't provide 
their heroku command on their virtual machines.

<script src="https://gist.github.com/flomotlik/5412759.js"></script>

Next you will need to add your Heroku API Key to the heroku config of your staging application so the Gem can authenticate. 
You can find your API key on [your account page](https://dashboard.heroku.com/account).

    heroku config:add HEROKU_API_KEY=YOU_API_KEY

Next add the following script to your codebase. Please change the placeholders for your production and staging 
apps to the actual names of your app. You can put the script in script/database_copy.sh for example

<script src="https://gist.github.com/flomotlik/5412867.js"></script>

Now you need to add the Heroku Scheduler add-on and go to the web page to run cron jobs on Heroku.

    heroku addons:add scheduler
    heroku addons:open scheduler

In your scheduler just add ***bash scripts/database_copy.sh*** as a task and set the time it should run at.

![Heroku Scheduler setup](/images/database-copy/scheduler.png)

Whenever you want to copy your production to your staging app you can also just run 

    heroku run bash scripts/database_copy.sh

##Conclusions

It is vitally important for your business to be sure that any change to your application or data model doesn't 
impact your customers and business. Being able to test on staging and having an easy way to reset staging at all 
times gives you the additional security when you push new code daily.
