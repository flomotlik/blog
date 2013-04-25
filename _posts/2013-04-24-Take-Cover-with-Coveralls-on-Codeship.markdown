---
layout: post
title: Combine Coveralls and Codeship to see exactly how your code coverage is evolving
author: Florian Motlik
twitter: flomotlik
google: 115123333592137547204
description: Code coverage can be
image: http://blog.codeship.io/images/coveralls/200x200_codeship_coveralls_integration.png
---

![Coveralls UI](/images/coveralls/codeship_coveralls_integration.png)

When starting with continuous integration and and especially continuous deployment code
coverage is one of those tools that can help tremendously in your workflow.

Being able to quickly grasp which parts of your application aren't well tested and
deserve a little more time is incredibly important. Especially with historical data
it is a very powerful tool.

With that in mind we were always searching for some way to help our users get started
with integrating code coverage into their workflow. We are happy to announce today that
we worked together with [Coveralls.io](https://coveralls.io) to integrate their code
coverage service.

##Coveralls.io

After a test run Coveralls automatically collects your code coverage data, uploads it to their servers
and gives you a nice interface to dig into it. They even show you trends and changes on coverage
for all of your files. Really awesome.

![Coveralls UI](/images/coveralls/coveralls.png)

Coveralls started with support for Ruby, but through user contribution they have support for many different languages
including Python, PHP, NodeJS, C/C++ and Scala according to their [docs](https://coveralls.io/docs)

[Coveralls](https://coveralls.io) is a product of the great team at [Lemur Heavy](http://lemurheavy.com/)
whose portfolio is rather impressive (they've done the Heroku Postgres Interface for example).

##Getting Started

Starting with coveralls is incredibly easy. Their [Docs](https://coveralls.io/docs/ruby) do a great job of guiding
you, but all you have to do to integrate your Ruby app is first add a .coveralls.yml file to your codebase that contains
your coveralls key:

<script src="https://gist.github.com/flomotlik/5459823.js"></script>

It is also possible to set this in the Environment setting of your Codeship project

    COVERALLS_REPO_TOKEN=YOUR_COVERALLS_REPO_TOKEN

then simply require the gem in your Gemfile

    gem 'coveralls', require: false

and put the initializers into your spec_helper.rb or env.rb depending on which framework you use

<script src="https://gist.github.com/flomotlik/5459802.js"></script>

If you want to combine the coverage data from different frameworks add the following to your spec_helper.rb
and env.rb (also take a look at coveralls [docs](https://coveralls.io/docs/ruby) on that topic).

<script src="https://gist.github.com/flomotlik/5459801.js"></script>

You then need to add a rake task that pushes your coverage report as soon as your build is finished.

<script src="https://gist.github.com/flomotlik/5459883.js"></script>

![Coveralls UI](/images/coveralls/pushtask.png)

This will push to coveralls only if your tests are successful.

##Conclusions
Code coverage is just one more tool to use to build great software. By making this ridiculously easy
Coveralls have taken away any excuse not to do it. Combined with continuous integration and
deployment on Codeship you can build better software faster. Have fun coding.