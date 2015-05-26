---
layout: post
title: "Testing Tuesday #1: Tests make software"
author: Clemens Helm
twitter: clemenshelm
google: 116883220210675577340
description: Codeship Testing Tuesday #1 – Introduction to test-driven development.
keywords: Codeship, TDD, test driven development, behavior driven development, behaviour driven development, testing tuesday, testing tools, testing methodologies, develop iteratively, hosted testing, testing in the cloud
image: http://blog.codeship.io/images/16-04-2013-TT-tests-make-software/200x200_tt_tests_make_software.jpg
---
![Illustration testing tuesday: Tests make software](/images/16-04-2013-TT-tests-make-software/codeship_tt-tests-make-software.jpg)
This is the first blog post of our new *Testing Tuesday* series. Every Tuesday we
will share our insights and opinions on the software testing space. Drop by every
Tuesday to learn more!

<hr>

At [Codeship](https://www.codeship.io/) we are huge fans of behavior-driven development
(BDD). In the next few blog posts we will show you how our development process with
BDD works and give you examples on how to get started.

Behavior-driven development is a collection of tools and methodologies based on test-driven
development. We will therefore cover the basics of test-driven development in this
post.

## Test-driven development

In test-driven development, software is developed iteratively. For each feature 
you begin with a test that represents the most important functionality of this feature.
This test should fail. For each test you perform the following steps:

1. Write the test and watch it fail.
2. Implement the *easiest solution* to make the test pass.
3. Refactor your code if necessary.

Once you are done, write another test until the feature is complete.

### Hands-on

I've written the following example in Ruby using the testing framework Rspec. I
will keep it simple, hoping that it won't be too hard for non-Rubyists to follow
along.

Let's say our example app needs users. We want to initialize a user with a name,
and make sure that we can retrieve the name afterwards.

Our first test would look something like this

<script src="https://gist.github.com/clemenshelm/5395845.js"></script>

Running this test yields the error that our app doesn't know what a User is (`uninitialized
constant User`). Our test fails, so we're on the right track! When you write tests,
always watch them fail first. This guarantees that you don't test something that
already works and you'll notice when there's something wrong with your test. You
write tests for code that doesn't exist yet, so they always need to fail first.

Let's create our user:

<script src="https://gist.github.com/clemenshelm/5391689.js"></script>

The test still misses something (`ArgumentError: wrong number of arguments(1 for 0)`).
We need to implement a constructor that accepts the user name as argument.

<script src="https://gist.github.com/clemenshelm/5395816.js"></script>

Great, the test tells us what to do next: `NoMethodError: undefined method 'name'`.
Alright, let's obey and make a method `name`:

<script src="https://gist.github.com/clemenshelm/5395823.js"></script>

Now the test tells us that it's still missing the user's name:
`user.name.should == "Paul" expected: "Paul" got: nil` But there's an easy fix to
this:

<script src="https://gist.github.com/clemenshelm/5395827.js"></script>

Hooray, it works! But hold on, now the name will be "Paul" no matter what name we
give to the user. Well, that's all we specified so far. By sticking to the easiest
possible solution you make sure that you don't implement anything that hasn't been
tested beforehand.

According to the test-driven development steps we could refactor the code now. Let's
leave it for now as I can't think of any improvements.

But obviously we need one more test here:

<script src="https://gist.github.com/clemenshelm/5395846.js"></script>

This test fails (`"user.name.should == "Sarah" expected: "Sarah" got: "Paul""`) 
which shouldn't suprise us too much. Let's fix this the easiest way possible:

<script src="https://gist.github.com/clemenshelm/5395831.js"></script>

Now it works! Time for refactoring again. Ruby gives us an easy way of reading attributes
with `attr_reader`. Let's use it to clean up the code:

<script src="https://gist.github.com/clemenshelm/5395838.js"></script>

### Why this effort?

Beautiful! We're done! But wouldn't it have been much faster if we just implemented
the user class without tests? It's trivial anyway!

Most apps start out as trivial and clean but end up as a dung pile only a few weeks
later. Once you have reached this point you'll realize that you are trapped. You
can hardly clean up the code because there's no way of making sure that everything
still works like before. The only possibility is to test everything manually after
each little refactoring. *Do you really think that's faster?*  You'll probably spend
days and weeks on it.

Test-driven development keeps you agile. You will be slower at first but you avoid
being stuck in the most critical situation: Imagine you've built this awesome new
app in record time and you are getting your first customers. Congratulations! You
start implementing this new feature your customers have asked for but suddenly your
customers report that they can't make payments anymore. As you try to fix this bug,
you receive messages from angry customers telling you that the login is broken. 
You're stuck. Instead of moving forward you're working 24/7 trying to reconstruct
the basic functionalities of your app.

Test-driven development makes you sleep safe. You can be sure that everything you
implemented still works. You can develop at a constant speed even years later when
your app has grown massively and you have forgotten all these little implementation
details. Your tests remember them and make sure they still work. Think of your tests
as your little friends that watch over your app so you don't have to.

### Test-driven development and continuous integration

Over time you will gather an impressive test suite that covers all features of your
application. The only problem is: The more tests you write, the longer they take
to run. If you run all the tests several times a day this will slow you down more
and more.

A good solution is to run only the tests that correspond to your current feature
and let the whole test suite run on a  [Continuous Integration](http://blog.codeship.io/2013/04/11/a-business-case-for-continuous-integration.html)
system like [Codeship](https://www.codeship.io/). This way you stay fast while still
ensuring that your entire app keeps working.

Codeship also offers [Continuous Deployment](http://blog.codeship.io/2012/12/05/Seven-steps-to-continuous-deployment.html).
This way every new feature is deployed automatically if all your tests pass. You
just need to take care of developing software anymore.

### Conclusions

Even though test-driven development seems to be additional effort at first, it yields
many benefits in the long term. Among them are agility, speed, and safety. The combination
with Continuous Integration and Deployment results in a solid software development
process proven by many modern software projects. At [Codeship](https://www.codship.io/)
we live by this process and we do our best to inspire others.

References:

* [Wikipedia: Test-driven develoment](http://en.wikipedia.org/wiki/Test_Driven_Development)
* [The RSpec Book: Behaviour-Driven Development with RSpec, Cucumber, and Friends](http://pragprog.com/book/achbd/the-rspec-book)