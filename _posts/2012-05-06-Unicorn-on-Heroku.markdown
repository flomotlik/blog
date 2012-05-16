---
layout: post
title: How to get 4x the performance out of Heroku with Unicorn
author: Flo
description: How to get 4x the performance out of Heroku with Unicorn
---

***Update***: [Part Two](/2012/05/14/Assets-Sprites-CDN.html) was released.

***This is the first of a two part series on how we set up Railsonfire with Heroku. The second part will deal with Assets, Sprites and Amazon Cloudfront.***

***This guide is only relevant and tested with the Heroku Cedar stack.***

I have had several debates over the last couple of months whether Heroku is the way to go and especially if it is expensive or not. They provide a great service, but their documentation makes them look pretty bad when it comes to price.

$35 for basically one single concurrent request seems very expensive, especially when starting out with a new project (although of course the first dyno is free).

Heroku provides plenty of resources, but as it only allows to listen on one port you can run only one thin instance (as recommended by their [documentation](https://devcenter.heroku.com/articles/rails3)).

What we need here is a webserver that listens on one port, but can work through several concurrent requests. Sounds like a job for Unicorn.

###What is Unicorn

[Unicorn](http://unicorn.bogomips.org/) is a ruby http server that starts one master process listening on one port and forks several worker processes. Every incoming client request is handed to a worker by the master and when finished the master returns the result to the client. Thus it only needs to listen on one port, but can work on several concurrent requests.

[Defunkt](https://github.com/defunkt) wrote a nice [blogpost](https://github.com/blog/517-unicorn) about unicorn some time ago that goes into more detail how GitHub uses it.

###Setup
To start using Unicorn all you have to do is:

1. Add the Gem to your Gemfile
  <script src="https://gist.github.com/2621308.js?file=Gemfile"></script>

2. Create a *[Procfile](https://devcenter.heroku.com/articles/procfile)*
  <script src="https://gist.github.com/2621308.js?file=Procfile"></script>

3. Add unicorn config in config/unicorn.rb
   <script src="https://gist.github.com/2621308.js?file=unicorn.rb"></script>

4. Set the default Logger in **production.rb** to STDOUT, otherwise logging doesn't work.
   <script src="https://gist.github.com/2621482.js"> </script>

###Unicorn config

You can find all config parameters in the [unicorn documentation](http://unicorn.bogomips.org/Unicorn/Configurator.html).

Let's go quickly through the configuration we use

***worker_processes***: Setting the number of worker processes

***timeout***: Time after which a worker is restarted if unresponsive

***preload_app***: Load the application before forking workers. Set to true if you use NewRelic (which you should) or you [won't see any data](https://newrelic.com/docs/troubleshooting/im-using-unicorn-and-i-dont-see-any-data)

***before\_fork/after\_fork:*** Disconnect in before\_fork and reconnect in after\_fork for your Database, Resque or other services. Without those handlers there will be regular database errors.

###NewRelic

Go to the NewRelic Addon of your Heroku application and check your dynos and memory consumption in the dyno tab.

####Average Memory Consumption

Check your Memory Consumption in NewRelic and set the ***worker_processes*** accordingly. The average consumption is shown on the Dyno tab of your NewRelic dashboard.

![New Relic Menu](/images/unicorn/menu.png)

One Heroku Dyno has 512Mb of Memory, so make sure your combined workers do not exceed that maximum amount, or your dyno will be shut down.

![Memory](/images/unicorn/memory.png)

On the right hand side of that same tab you can see the number of dynos. Make sure it is the same as you set in ***worker_processes***.

![New Relic](/images/unicorn/dynos.png)

###Benchmarks

I ran several tests with ApacheBench to determine how much the performance improved.

I ran 1000 Requests with 100 concurrent connections against the landing page of our staging application. The following graph shows the time the requests took combined with 1-4 workers.

<script type="text/javascript">
google.load("visualization", "1", {packages:["corechart"]});
google.setOnLoadCallback(drawChart);
function drawChart() {
  var data = google.visualization.arrayToDataTable([
    ['Workers', 'Seconds'],
    ['1',  45],
    ['2',  20],
    ['3',  17],
    ['4',  11]
  ]);

  var options = {
    title: 'Apache Bench',
    vAxis: {title: 'Workers',  titleTextStyle: {color: 'red'}},
    hAxis: {minValue: 0, maxValue:50}
  };

  var chart = new google.visualization.BarChart(document.getElementById('ab_chart'));
  chart.draw(data, options);
}
</script>

<div id="ab_chart" style="width: 560px; height: 300px;"></div>

Going from one process to several increases performance drastically, from then on it is still a boost to your application, but not as drastically. However you have to find the right spot on how many workers you want unicorn to fork depending on your application. Having too many may shut down your dyno due to memory constraints.

###Conclusion
So in closing using Unicorn as your Heroku Webserver not only pays off, but should be put into the Heroku documentation at least as advanced information.

I actually talked to people and showed them our Unicorn setup, which convinced them that Heroku is not as expensive as it seems and especially when starting your project is a very viable alternative to having your own Server. With every new dyno you get several more concurrent requests, which is pretty neat.

If you have any questions regarding the setup or anything else you can send an email to [flo@railsonfire.com](mailto:flo@railsonfire.com), a Tweet to [@Railsonfire](https://twitter.com/#!/railsonfire) or use the Olark Chat Box in the right hand corner.

###Thanks
This post is very much built on Michael van Rooijen's [Blogpost](http://michaelvanrooijen.com/articles/2011/06/01-more-concurrency-on-a-single-heroku-dyno-with-the-new-celadon-cedar-stack/). Gists that helped with the setup were by [leshill](https://gist.github.com/1401792) and [jamiew](https://gist.github.com/2227268)