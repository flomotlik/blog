---
layout: post
title: Easily sync production and staging databases on Heroku
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Sync your production data into staging daily to have a better testing environment
image: http://blog.codeship.io/images/
---

Testing your new release or database migrations should always be done on a system that
is as close to production as it can be. Keeping the codebase and configuration synced
is straightforward on most systems, but keeping the data up to date can be a hassle.

Heroku provides some great tools to mirror your production data in your staging database.

We will give you a quick guide on how to set this up with heroku and their scheduler addon.

This guide is currently targeted at Rails apps on Heroku, as ruby and the Heroku gem is necessary to
perform the backup actions.

As long as your database is small enough you can do this during your deployment with Codeship. We will in a
first step show you how to set this up. As your database becomes bigger this step may take to long for every
deploy and you want to do this on a regular schedule. The second part of the blogpost will show how
you can set this up as well.

##Heroku Databases

For smaller production databases you might do well with a dev database for your staging environment. As it has
a limit of 10.000 lines though you might have to upgrade to the basic plan at 9$ per month as soon as your
database grows.

Being sure your data migrations work well on every deployment is definitely worth the 9$!

You can read more about the heroku databases and how to get started at their 
[Dev Center Article](https://devcenter.heroku.com/articles/heroku-postgresql)

##Sync Database on every deployment to staging

With codeship it is really easy to copy your production database into staging before every deploy. In your heroku
staging deployment just add the name of your production app into the restore_from field.

It will then do a backup of the production db and restore it into the staging application. This can take a while
for larger deployments though.

##Sync in regular intervals for bigger databases
You have to start by adding the Heroku gem to your Gemfile. This is necessary as Heroku currently doesn't provide
their heroku command on their virtual machines.

<script src="https://gist.github.com/flomotlik/5412759.js"></script>

Next you will need to add your Heroku API Key to the apps config so the gem can authenticate. You can find
your api key on [your account page](https://dashboard.heroku.com/account).

    heroku config:add HEROKU_API_KEY=YOU_API_KEY --app YOUR_STAGING_APP

Next add the following script to your codebase. Please change the placeholders for your production and staging
apps to the actual names of your app. You can put the script in script/database_copy.sh for example

<script src="https://gist.github.com/flomotlik/5412867.js"></script>

Now you need to add the Heroku Scheduler addon and go to the webpage to to run cron jobs on Heroku.

    heroku addons:add scheduler
    heroku addons:open scheduler

In your scheduler just add 'bash scripts/database_copy.sh' as a task and set the time it should run at.

Whenever you want to reset copy your production to your staging app you can also just run 

    heroku run bash scripts/database_copy.sh

##Conclusions

It is vitally important for your business to be sure that any change to your application or data model
doesn't impact your customers and business. Being able to test on staging and having an easy way to reset 
staging at all times gives you the additional security when you push new code daily.