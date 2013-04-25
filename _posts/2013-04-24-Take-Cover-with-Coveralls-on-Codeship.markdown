---
layout: post
title: Combine Coveralls and Codeship to track your code coverage
author: Florian Motlik
twitter: flomotlik
google: 115123333592137547204
description: Code coverage can be
image: http://blog.codeship.io/images/coveralls/200x200_codeship_coveralls_integration.png
---

![Coveralls UI](/images/coveralls/codeship_coveralls_integration.png)

When starting with continuous integration and deployment, code
coverage is one of the tools that improve your workflow significantly.

Being able to quickly grasp which parts of your application aren't well tested is incredibly important.
Especially tracking your code coverage over time is a powerful feature.

With this in mind we were always searching for some way to help our users get started
with integrating code coverage into their workflows. Today we are happy to announce that
we worked together with [Coveralls.io](https://coveralls.io) to integrate their code
coverage service.

##Coveralls.io

After a test run [Coveralls](https://coveralls.io) automatically collects your code coverage data, uploads it to their servers
and gives you a nice interface to dig into it. They even show you trends and changes on coverage
for all of your files. Really awesome.

![Coveralls UI](/images/coveralls/coveralls.png)

At the beginning [Coveralls](https://coveralls.io) supported only Ruby, but through user contribution they now support many different languages
including Python, PHP, NodeJS, C/C++ and Scala according to their [docs](https://coveralls.io/docs).

[Coveralls](https://coveralls.io) is a product of the great team at [Lemur Heavy](http://lemurheavy.com/)
whose portfolio is impressive (they've created the Heroku Postgres Interface for example).

##Getting Started

Starting with [Coveralls](https://coveralls.io) and [Codeship](https://www.codeship.io/) is easy. Their [docs](https://coveralls.io/docs/ruby) do a great job of guiding
you, but all there is to set up your Ruby app is add a .coveralls.yml file to your codebase that contains
your [Coveralls](https://coveralls.io) key:

<script src="https://gist.github.com/flomotlik/5459823.js"></script>

It is also possible to set this in the Environment setting of your [Codeship](https://www.codeship.io/) project

    COVERALLS_REPO_TOKEN=YOUR_COVERALLS_REPO_TOKEN

then simply require the Gem in your Gemfile

    gem 'coveralls', require: false

and put the initializers into your spec_helper.rb or env.rb depending on which framework you use

<script src="https://gist.github.com/flomotlik/5459802.js"></script>

If you want to combine the coverage data from different frameworks, add the following to your spec_helper.rb
and env.rb (also take a look at Coveralls [docs](https://coveralls.io/docs/ruby) on that topic).

<script src="https://gist.github.com/flomotlik/5459801.js"></script>

Then you need to add a rake task that pushes your coverage report as soon as your build is finished.

<script src="https://gist.github.com/flomotlik/5459883.js"></script>

![Coveralls UI](/images/coveralls/pushtask.png)

This will push to [Coveralls](https://coveralls.io) only if your tests are successful.

##Conclusions

Code coverage is just one more tool to use to build great software. By making this ridiculously easy
[Coveralls](https://coveralls.io) have taken away any excuse not to do it. Combined with continuous integration and
deployment on [Codeship](https://www.codeship.io/) you can build better software faster. Have fun coding.