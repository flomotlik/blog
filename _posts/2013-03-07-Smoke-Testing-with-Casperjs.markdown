---
layout: post
title: Start testing your app with Casperjs
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Casperjs is the perfect tool to get started with testing your deployed applications
image: http://blog.codeship.io/200x200_casper_js.png
---
![Casperjs](/images/casperjs/Casper_JS_ghost.gif)

Here at Codeship we always strive to find the best ways to test our
application before we go live. We need to have all the necessary tests
and tools in place to make sure our application works.

We have invested a lot of time in our unit and acceptance testing, but
we also need to test our application after deployment or on
a staging infrastructure.

We have looked into several tools, including Selenium and Testacular, but
found that Casperjs provides the perfect mix of being easy to use and
being powerful enough for our purposes.

While we are still starting to integrate it into our workflow we want to
show you the power it provides and introduce you to its power already.

###Why Casperjs
We wanted a tool that doesn't have any dependencies on languages
or frameworks a team may use. While tools like Capybara are great and neatly
integrated into Rails they still need some effort to be set up. And if you
don't use Rails you are out of luck.

We also wanted everyone on our team to be able to write tests with it.
The syntax therefore needed to be easy and comfortable for all of us.
Casperjs is based on Javascript but has Coffeescript support built in. WIN!

Casperjs is developed by [Nicolas Perriault](https://github.com/n1k0) and based
on the wonderful Phantomjs developed by [Ariya
Hidayat](https://github.com/ariya)

##Getting Started
To get started with Casperjs you need Phantomjs and Casperjs installed
on your computer. Should be simple to install on every OS.

Now let's look at three quick examples.
The first one simply opens codeship.io, clicks the "Jobs" link and makes
sure we are at the jobs page.

<script src="https://gist.github.com/flomotlik/5103432.js"></script>

The second one sets the user agent header and the viewport size. By default
Phantomjs sets the viewport size to 400x300 which is rather small for a
web page. Then it clicks a menu item at casperjs.org and makes sure
the right pages are loaded. In the end it captures the current page as a
png image.

<script src="https://gist.github.com/flomotlik/5113089.js"></script>

The third example translates ***Guten Tag*** to ***Good day*** using
translate.google.com and asserts the correct translation.

<script src="https://gist.github.com/flomotlik/5113098.js"></script>

You can run the file with ***casperjs test codeshiptest.coffee*** for
example. Put all the files in a directory and pass the directory path to
casperjs, like ***casperjs test caspertests***. This will run all test files
in caspertest and its subdirectories.

You can find even more assert commands at the [Casperjs documentation page](http://casperjs.org/api.html#tester).

Please take a look at the [documentation for the ***then***
method](http://casperjs.org/faq.html#faq-step-stack) as it waits for
earlier steps to be finished before running the next step. This is
important to make sure your asserts work fine.

The test subcommand makes sure the casperjs var is set. You can read more
about it at the [casperjs test documentation
page](http://casperjs.org/testing.html).

##Conclusions
Casperjs is a great starting point for testing your application, on your
local machine as well as against a your hosted application.
Setup is very easy and fast as it doesn't need to
be integrated into any other framework.

Give it a try and tell us what you are testing with Casperjs. Of course
[Codeship](https://www.codeship.io)  supports Phantomjs as well as Casperjs.
