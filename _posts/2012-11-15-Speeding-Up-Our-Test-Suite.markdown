---
layout: post
title: Speeding up our test suite in development
author: Clemens Helm
twitter: clemenshelm
google: 116883220210675577340
description: How to speed up your test suite to keep developing productively.
image: http://farm1.staticflickr.com/9/13882389_d93fb7d418.jpg
---
![Speed gauges](http://farm1.staticflickr.com/9/13882389_d93fb7d418.jpg)  
*Image by [Juan-Calderon](http://www.flickr.com/photos/iguana_azul/).*

***When you practice test-driven development, most of the time you need to run only a small number of tests to validate your recent code changes. Unfortunately things change once you start refactoring. Refactoring models implicates your entire application. So in order to keep things from going down the drain, you’ll need to run all (or most of) your tests constantly. And that’s where things get tedious.***

At [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) we develop our Ruby on Rails web app test-driven with [Rspec](https://www.relishapp.com/rspec/rspec-rails/v/2-12/docs). Most of our specs are high level request specs (in a way integration tests) which are slow compared to controller or model specs. One of the reasons for this slowness is that request specs test the whole application stack. Another reason is that we changed the default capybara driver from [Rack::Test](https://github.com/brynary/rack-test) to [Poltergeist](https://github.com/jonleighton/poltergeist).

*“Poltergeist? But that’s so much slower!”*

Right! But we chose Poltergeist because it tests a web application more like a user experiences it. While Rack::Test only considers the HTML (or whatsoever) response from your application, Poltergeist launches a headless [WebKit](http://www.webkit.org/) browser, provided by [PhantomJS](http://phantomjs.org/).

This way we could not only test our backend application by navigating through the HTML code, but also test our awesome Javascript sugar.

*“Ok, so use Poltergeist only when your specs require Javascript!”*

That would be enough to test all of the Javascript functionality. Unfortunately Javascript can be a villain, too. If there’s a Javascript error, chances are that even things stop working that normally wouldn’t rely on Javascript. If Poltergeist comes across a Javascript error it will throw an exception immediately and thus fail the spec.

Because of this additional safety we decided on making Poltergeist the default capybara driver.

## Development vs. [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) CI

Using test-driven development will eventually take you to the point of misery, when running all your specs slows down your productivity unbearably. Poltergeist lets us reach this point at record speed. So what to do now? On the one hand we wanted to retain the quality of our web app, on the other hand we wanted to continue developing without feeling the need of poking our eyes out while waiting for the specs to succeed.

Running our spec suite on my computer with this setup took **8:45 minutes**.

The solution was [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) itself: We would speed up the specs in development as much as possible and let the Continous Integration server do all the cumbersome work.

### Step one: Use Poltergeist only for Javascript

I know, that’s what you always said. We decided on using Poltergeist for all request specs that required Javascript, and Rack::Test for the rest in development. On the continous integration server we’d still run all specs with Poltergeist.

This little change reduced the execution time to **5:00 minutes**.

### Step two: Skip slowest specs

There were some specs that took up to 40 seconds to run. These were used to check some quite complicated procedures that rarely changed. We decided to skip all specs for development that took longer than 10 seconds.

Rspec provides [tagging](https://www.relishapp.com/rspec/rspec-core/v/2-4/docs/command-line/tag-option) to achieve this: We tagged our slow tags with `speed: "slow"`. Running our specs with `rspec --tag ~speed:slow` reduced the execution time by half: **2:29 minutes**. Yei!

### Step three: Skip remote specs

In [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) we integrate a couple of external services like [Github](https://github.com/). Of course we also needed to verify that the communication with these services works. But a developer’s life is hard: Sometimes you are on a train, on a plane or there’s simply no network reception. And all of a sudden many of your specs fail.

Therefore we tried to remove the dependencies on external services for as many specs as possible. We tagged all specs that still required internet access with `remote: true` and skipped them in development.

Executing `rspec --tag ~@remote --tag ~speed:slow` now finished in: **1:28 minutes**.

This stunning result was good enough for now, so we stopped optimizing here. But for sure we well cover further optimizations in future blog posts.

## Results

By changing our test setup and skipping time-consuming tests in development we were able to cut down test execution time by more than **83%**. This way we could stay productive during development and still perform extensive checks on our web application using the [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) continous integration server.

Here’s a final overview of the results after each optimization step:

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>number of specs</th>
      <th>execution time (minutes)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>All tests with Poltergeist:</th>
      <td>128</td>
      <td>8:45</td>
    </tr>
    <tr>
      <th>All tests with Rack::Test/Poltergeist:</th>
      <td>128</td>
      <td>5:00</td>
    </tr>
    <tr>
      <th>Without slow specs (> 10s):</th>
      <td>107</td>
      <td>2:29</td>
    </tr>
    <tr>
      <th>Without slow and remote specs:</th>
      <td>99</td>
      <td>1:27</td>
    </tr>
  </tbody>
</table>