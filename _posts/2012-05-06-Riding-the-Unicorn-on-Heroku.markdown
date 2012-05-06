---
layout: post
title: Riding the Unicorn on Heroku
author: Flo
description: How to use all Heroku gives you, and more
---

***This is the first of a two part series on how we set up Railsonfire with Heroku. The second part will deal with Assets, Sprites and Amazon Cloudfront.***

***This guide is only relevant and tested with the Heroku Cedar stack.***

I have had several debates over the last couple of months whether Heroku is the way to go and especially if it is expensive or not. They provide a great service, but their documentation makes them look pretty bad when it comes to price. 35$ for basically one single concurrent request seems very expensive, especially when starting out with a new project (although of course the first dyno is free).

Heroku provides plenty of resources, but as it only allows to listen on one port you can run only one thin instance (as recommended by their [documentation](https://devcenter.heroku.com/articles/rails3)).

What we need here is a webserver that listens on one port, but can work through several concurrent requests. In comes Unicorn.

###What is Unicorn

[Unicorn](http://unicorn.bogomips.org/) is a ruby http server that starts one master process listening on one port and forks several worker processes. Every incoming client request is handed to a worker by the master and when finished the master returns the result to the client. Thus it only needs to listen on one port, but can work on several concurrent requests.

[Defunkt](https://github.com/defunkt) wrote a nice [blogpost](https://github.com/blog/517-unicorn) about unicorn some time ago that goes into more detail how GitHub uses it.

###Setup
To start using Unicorn all you have to do is:

1. Add the Gem to your *Gemfile*

    gem 'unicorn'

2. Add the *[Procfile](https://devcenter.heroku.com/articles/procfile)* and unicorn config
   <script src="https://gist.github.com/2621308.js"> </script>

3. Set the default Logger in **production.rb** to STDOUT, otherwise logging doesn't work.
   <script src="https://gist.github.com/2621482.js"> </script>

###Unicorn config

You can find all config parameters in the [unicorn documentation](http://unicorn.bogomips.org/Unicorn/Configurator.html).

Let's go quickly through the configuration we use

***worker_processes***: Setting the number of worker processes

***timeout***: Time after which a worker is restarted if unresponsive

***preload_app***: Load the application before forking workers. Set to true if you use NewRelic (which you should) or you [won't see any data](https://newrelic.com/docs/troubleshooting/im-using-unicorn-and-i-dont-see-any-data)

***before\_fork/after\_fork*** Disconnect in before\_fork and reconnect in after\_fork for your Database, Resque or other services. Without those handlers there will be regular database errors.

###NewRelic

Go to the NewRelic Page of your Heroku application and check your dynos and memory consumption.

####Average Memory Consumption

Check your Memory Consumption in NewRelic and set the ***worker_processes*** accordingly. The average consumption is shown on the Dyno tab of your heroku dashboard.

One Heroku Dyno has 512Mb of Memory, so make sure your combined workers do not exceed that maximum amount, or your dyno will be shut down.

![New Relic](/images/unicorn/new_relic.png)

On the right hand side of that same tab you can see the number of dynos. Make sure it is the same as you set in ***worker_processes***.

![New Relic](/images/unicorn/dynos.png)

###Benchmarks

I ran several tests with [blitz.io](http://blitz.io) and ApacheBench to determine how much the performance improved.

####Blitz.io

I used their free tier testing to run several dozen concurrent requests against our staging application. In a 60 second timeframe it went
from 1 request to the max number of concurrent users in the table.

I tried to maximize the number of hits, while keeping the number of timeouts low. A timeout is any request that takes longer than a second to process. When going over the maximum number of concurrent users for a certain worker the timeouts would grow drastically.

####Apache Bench

With Apache Bench I ran 1000 Requests with 100 concurrent connections against our staging application. Keep in mind that I ran those from my local machine over Wifi and while having the packages sent halfway round the globe to the Heroku Servers. That's why it is slow in absolute terms, but I ran them several times to make sure the relative values are correct.

####Results

<table>
  <tr>
    <th>Workers</th>
    <th>Hits</th>
    <th>Timeouts</th>
    <th>Concurrent Users</th>
    <th>AB in seconds
  </tr>
  <tr>
    <td>1</td>
    <td>1153</td>
    <td>26</td>
    <td>55</td>
    <td>40</td>
  </tr>
  <tr>
    <td>2</td>
    <td>2065</td>
    <td>105</td>
    <td>95</td>
    <td>36</td>
  </tr>
  <tr>
    <td>3</td>
    <td>2710</td>
    <td>21</td>
    <td>120</td>
    <td>30</td>
  </tr>
  <tr>
    <td>4</td>
    <td>3464</td>
    <td>0</td>
    <td>135</td>
    <td>25</td>
  </tr>
</table>

While it doesn't grow linear with the number of workers, 4 workers can nonetheless process many many more requests than a single worker. This is really worth investing your time into, as you already pay for those resources anyway.

###Conclusion
So in closing using Unicorn as your Heroku Webserver not only really pays off, but should be put into the Heroku documentation as default. By doing this they would hit their bottom line slightly, as users would need less dynos for now, but in the long term many more people would consider running their applications with Heroku. And it also makes for much happier users, as the current documentation doesn't explain why they do not mention Unicorn at all.

I actually talked to people and showed them our Unicorn setup, which convinced them that Heroku is not as expensive as it seems and especially when starting your project is a very viable alternative to having your own Server.

If you have any questions regarding the setup or anything else you can send an email to [flo@railsonfire.com](mailto:flo@railsonfire.com), a Tweet to [@Railsonfire](https://twitter.com/#!/railsonfire) or use the Olark Chat Box in the right hand corner.