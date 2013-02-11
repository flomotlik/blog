---
layout: post
title: How to make your testing awesome with PhantomJS
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: How to switch from Selenium as your Capybara driver to Phantomjs for better testing
image: http://phantomjs.org/images/phantomjs-logo.png
---

[Phantomjs](http://phantomjs.org) is a great tool for end to end tests of your application. It
 provides a webkit based headless browser, so you can click through your web application even
on a server. Our main use case is it's capybara integration, but Phantomjs can be used
for lots of different scenarios.

We have over the life of our codebase tried lots of [Capybara](https://github.com/jnicklas/capybara)
drivers to find the one that fits best into our test environment and the way we
work. We started at first with [akephalos](https://github.com/bernerdschaefer/akephalos),
moved on to Selenium and switched
that for [capybara-webkit](https://github.com/thoughtbot/capybara-webkit).

All of them have their benefits, but every one has it's special quirks
that get you in trouble regularly. Especially Selenium broke regularly
when new Firefox versions were released which were incompatible with
older versions of the selenium-webdriver gem. Another problem we found
with one of our customers was that Selenium as well as capybara-webkit
had problems with links that were covered by javascript overlays.
Phantomjs correctly failed and provided the right error messages.

Finally we started using Phantomjs with great success.

The main reason for us to switch was the ability
to fail tests when javascript fails on the page. Neither Selenium nor
capybara-webkit support this, but it is absolutely crucial. In our team
everyone has the ability to deploy new code to production so we really
need to make our tests hard and safe. Making sure there are no
javascript errors on your page is an absolute necessity. We had a
handful of instances where a small change in javascript broke all of
mixpanel and google analytics code, which is obviously bad.

###Getting started
Getting started with Phantomjs and Capybara is pretty easy and straight forward. Make
sure to download the latest version from the [Phantomjs
website](https://www.phantomjs.org) and follow the installation
instructions on their [install page](http://phantomjs.org/download.html)

Following is a minimal setup gist to get you started. The Poltergeist
gem integrates Phantomjs with Capybara and supports a lot of great extensions. Take a look at their
[customization options](https://github.com/jonleighton/poltergeist#customization) to
get a better feeling what you can do. Especially the debug is really
handy.
<script src="https://gist.github.com/flomotlik/4757094.js"></script>

If you want to use Capybara with the [parallel_tests
gem](https://github.com/grosser/parallel_tests) you can use the setup we
use for our application:
<script src="https://gist.github.com/flomotlik/4757186.js"></script>

Additionally this raises javascript errors when they occur and gives
Phantomjs a little higher timeout.

###Conclusion
You should really give Phantomjs a try for your test environment, as it
provides a stable, easy to use and really powerful tool.
