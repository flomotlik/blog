---
layout: post
title: Get started with testing from top to bottom
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Getting started with testing is hard, so start from the top, not bottom
image: http://blog.codeship.io/images/200x200_casper_js.png
---
![Iceberg top to bottom](/images/top-to-bottom-testing/iceberg-top-down-testing.jpg)

*Image via foldedstory.com*

We are regularly talking to teams who are just starting out with testing
their application. Some of them have a few tests, but don't know where
to go next. Others don't test at all and need help to get started.

One team mentioned they felt overwhelmed as
there is so much they could test. Feeling overwhelmed by the blank page
we have in front of us is an experience everyone goes through at some
point in their life.

By starting to test your application from top to bottom you can get over
this easily.

## Start from the top

By testing your application you can always be sure the most important steps
users can take in your application work. So why not start with what is
most important to your product. Why not start with a few high level
scenarios and over time start testing the lower parts of your system.

As easy as this sounds we've seen teams struggle with coming to that
conclusion and then following through.

Start by defining what the most important steps in your product are. We've come
up with a little exercise to help with that.

Get everyone in your team to write down 8 scenarios they think
are the most important ones in your application. They should do it
separately and order them according to how important each step is to
them.

All of them should be
structured with ***Given***, ***When*** and ***Then*** steps.
The process of defining the scenarios is based on [Behavior driven
development](http://en.wikipedia.org/wiki/Behavior-driven_development)
which is supported by various testing tools.

For example

    Given: I am on the landing page
    When:  I click on Signup
    And:   I enter my Email Address and Password
    Then:  I should be logged in and see the welcome page
    And:   I should receive a welcome email

Another example would be

    Given: I am on the detail page of a product
    When:  I click buy
    And:   I log into my account
    And:   I enter my payment details
    Then:  I will be shown an overview of my purchase
    And:   I can finish the purchase

And one more

    Given: I am on my profile page
    When:  I am searching for a person
    And:   I send them a friendship request
    Then:  The person will receive a message

After everyone is finished discuss the scenarios everyone came up with and
rank them according to how important they were to all of you. Now write
tests from top to bottom for the scenarios on the list.

You now have tests written for the most important steps users take
through your application. Next time you implement something new you can
easily make sure you do not destroy the essential parts of your product.

The next time you build a new feature just add the tests immediately.

You can over time start going from [testing your high level scenarios](http://en.wikipedia.org/wiki/Behavior-driven_development) to
more [lower level testing](http://en.wikipedia.org/wiki/Unit_testing).

[Our blogpost about
Casperjs](http://blog.codeship.io/2013/03/07/Smoke-Testing-with-Casperjs.html) shows you
how to implement your first high level tests. Tools like [Cucumber for
ruby](http://cukes.info/), [Lettuce for python](https://github.com/gabrielfalcao/lettuce)
or [Selenium](http://docs.seleniumhq.org/) are great tools to get
started as well.

This should only be the beginning of testing your application. But
getting started is often the hardest part.

## Conclusions
Testing is not just about making sure your application works, but
giving you the confidence to change any part of your system. When you
can refactor your code or implement a new feature and still be sure that
everything works you can work much faster.

Having a strong security net in place lets you take more risks and
try more interesting paths. And those paths are often the ones that
make your product great.

We will expand this into a series of blogposts about how to get started
with testing. Please leave a comment on what you would like to have as
the next topic.
