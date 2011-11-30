---
layout: post
title: Backend Metrics done right
author: Flo
description: How we use Google Docs the measure the shit out of our Backend Data
---
![Measure all the things](http://railsonfire.github.com/images/gsmetrics/allthethings.jpg)

I love Mixpanel, Optimizely and Woopra. Diving into Google Analytics or Kissmetrics gives me regular nerdboners. But as nice as those services are the most important source for your metrics is and always will be your database.

The problem is this is by definition always very specific to your system, thus you need to implement it yourself.

To make this easy for us I implemented a gem called [GSMetrics](https://github.com/railsonfire/gsmetrics) that lets us keep track of our data in Google Spreadsheets. We use it to run a daily job on Heroku to take all the data we are interrested in and write it into a Spreadsheet. We then analyse that data either directly on on Google Spreadsheets or via other means.

I will go through the initial setup you have to do to get it running as well as a little rundown of how we use it with the Heroku Scheduler Addon.


##Setup
To start using the gem you need to have a google Oauth2 token. You can simply run

    gsmetrics

which will guide you through creating a new Oauth API token and give you a code block in the end that you can simply copy to create a new GSMetrics Session. The client_id, client_secret and token used will be filled out in the code block already.

You then pass it the ID of the document you want to access (You can get that from the google docs url of that document) and it will show you all the different worksheets and give you the exact code to use for gsmetrics to access those worksheets.

You can read more about authorization with Google and OAuth2 in their [Documentation](http://code.google.com/apis/accounts/docs/OAuth2.html)

##How does it work
The following code block is an example of how to use GSMetrics:

    session = GSMetrics::Session.new(client_id,client_secret,refresh_token)
    worksheet = session.worksheet document_id, worksheet_id
    worksheet << 1
    worksheet << 2
    worksheet.append(5)
    worksheet.save_row

It simply creates a Session in which you can access different worksheets in different documents. You simply add a data value via either << or append. As soon as your row is finished you call save_row. This will store your data in the first empty line of the worksheet and remove the local data so you can start with your next row.

If you add several new rows in a short period of time Google times out your requests. To solve that simply add a short delay (for example with sleep) into your code and google doesn't mind then. We used a two second delay after every five rows for the initial import of our data.

## Heroku Scheduler FTW
We use the [Heroku Scheduler Addon](http://devcenter.heroku.com/articles/scheduler) to run a job every day that goes through our database, takes alle the relevant data, aggregates it and calls gsmetrics.

This makes it incredibly easy to gather backend data and get meaningfull value out of it. Whenever we need to change our metrics it is a matter of minutes to change them, push the new change to github and in a matter of minutes it is tested and deployed (by Railsonfire of course).