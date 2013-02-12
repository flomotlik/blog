---
layout: post
title: How to make your testing awesome with PhantomJS
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Why Phantomjs will make you forget Selenium as Capybara driver
image: http://phantomjs.org/images/phantomjs-logo.png
---

[Phantomjs](http://phantomjs.org) is a great tool for end to end tests of your application. It
 provides a headless browser based on webkit, that allows your tests to
navigate through the web application. At Codeship we use it's capybara integration, but Phantomjs can be used
for lots of purposes.

Throughout the lifetime of our codebase we have tried lots of [Capybara](https://github.com/jnicklas/capybara)
drivers to find the one that suits our test environment and workflow
We started of with [akephalos](https://github.com/bernerdschaefer/akephalos),
moved on to Selenium and switched
that for [capybara-webkit](https://github.com/thoughtbot/capybara-webkit).

All of them have their benefits, but also their quirks. Especially Selenium broke frequently
when new Firefox versions were released which were incompatible with
older versions of the selenium-webdriver gem. Another problem was that
Selenium had problems with links covered by modal panels. One of our
customers experienced the same behavior in capybara-webkit
Phantomjs correctly failed and provided the right error messages.

Finally we started using Phantomjs with great success.

Phantomjs’ main advantage for us was that it fails tests on javascript
errors. Neither Selenium nor
capybara-webkit support this, but it is absolutely crucial. In our team
everyone has the ability to deploy new code to the live system so we really
need to make our tests hard and safe. Making sure there are no
javascript errors on your page is mandatory. We had a
handful of instances where a small change in javascript broke all of
our javascript code.

###Getting started
Getting started with Phantomjs and Capybara is easy. Make
sure to download the latest version from the [Phantomjs
website](https://www.phantomjs.org) and follow the installation
instructions on their [install page](http://phantomjs.org/download.html)

Following is a minimal setup gist to get you started. The Poltergeist
gem integrates Phantomjs with Capybara and provides a lot of great extensions. Take a look at their
[customization options](https://github.com/jonleighton/poltergeist#customization) to
get a better understanding of what you can do.
<script src="https://gist.github.com/flomotlik/4757094.js"></script>

If you want to use Capybara with the [parallel_tests
gem](https://github.com/grosser/parallel_tests) you can use the setup we
use for Codeship:
<script src="https://gist.github.com/flomotlik/4757186.js"></script>

Additionally this raises javascript errors and increases the
Phantomjs timeout.

###Conclusions
Give Phantomjs a try for your test environment! It is a stable, easy to
use and a powerful friend.
