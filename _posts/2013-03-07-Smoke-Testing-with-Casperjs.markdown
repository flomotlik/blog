---
layout: post
title: Start testing your app with the awesome Casperjs
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Casperjs is the perfect tool to get started with Testing your deployed apps
image: http://phantomjs.org/images/phantomjs-logo.png
---
![Phantom](/images/phantomjs/phantom.jpg)
*Image by [petcoffr](http://www.flickr.com/photos/petcoffr/6050263571/sizes/l/in/photostream/).*

Here at Codeship we always strive to find the best ways to test our
application before we go live. We need to have all the necessary tests
and tools in place to make sure our application works.

We have invested a lot of time in our unit and acceptance testing, but
there is still the need to test your application after deployment or on
a staging infrastructure.

We have looked into several tools, including Selenium and Testacular but
found that Casperjs seems to provide the perfect mix of ease of use, while still
being powerful enough for our purposes.

While we are still starting to integrate it into our workflow we want to
show you the power it provides and give you an introduction to
get started.

###Why Casperjs
We wanted a tool that doesn't have any dependency on a specific language
or framework a team may use. While tools like Capybara are great and neatly
integrated into Rails they still need some effort to set up. And if you
don't use Rails you are out of luck.

The second requirement was that everyone on our team should be able to
write tests with it. The language to write the tests in therefore needs
to be easy and comfortable for all of us. Casperjs is based on
Javascript but has Coffeescript support built in. WIN!

Casperjs is developed by [Nicolas Perriault](https://github.com/n1k0) and based
on the wonderful Phantomjs developed by [Ariya
Hidayat](https://github.com/ariya)

##Getting Started
To begin you need phantomjs and casperjs installed on your local
machine. Should be simple on any OS to install.

Now that you got everything installed let's look at three quick examples.
The first one simply opens codeship.io, clicks the Jobs link and makes
sure we are at the jobs page.

The second one sets a specific user agent and the viewport. By default
Phantomjs sets the viewport to 400x300 which is rather small for a
webpage. Then it just clicks a menu item at casperjs.org and makes sure
the right pages are loaded. In the end it captures the current page as a
png image.

The third example uses translate.google.com to type in ***Guten Tag***
and translate it to ***Good day***. All with corresponding asserts in
place.

<script src="https://gist.github.com/flomotlik/5103432.js"></script>

You can run the file with ***casperjs test codeshiptest.coffee*** for
example. You can also test all of the files by putting them together in
a directory and giving the directory to casperjs. For example all of the
files are in ***caspertests*** you just run ***casperjs test
caspertests*** and it will run all testfiles in caspertests and its
subdirectories.

Casperjs supports many more assert commands that you can all see at
their [documentation page](http://casperjs.org/api.html#tester).

The test subcommand makes sure the casperjs var is set. You can read more
about it at the [casperjs test documentation
page](http://casperjs.org/testing.html).

##Conclusion
Casperjs provides a great tool to get started with testing your
application, be it in your local environment, against a staging or a
production server. Setup is very easy and fast and as it doesn't need to
be integrated into any other framework.

Give it a try and tell us what you are testing with Casperjs. Of course
Phantomjs as well as Casperjs are well supported on Codeship.
