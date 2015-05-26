---
layout: post
title: "Testing Tuesday #2: From test-driven development to Behavior-driven development"
author: Clemens Helm
twitter: clemenshelm
google: 116883220210675577340
description: How Behavior-driven development helps you focus on your users' needs instead of your tests.
keywords: Codeship, TDD, test driven development, Behavior driven development, behaviour driven development, testing tuesday, testing tools, testing methodologies, develop iteratively, hosted testing, testing in the cloud
image: http://blog.codeship.io/images/23-04-2013-TT-from-TDD-to-BDD/200x200_tt_from-tdd-to-bdd.jpg
---
![Illustration testing tuesday: Tests make software](/images/23-04-2013-TT-from-TDD-to-BDD/codeship_tt_from-tdd-to-bdd.jpg)
This is the second blog post of our new *Testing Tuesday* series. Every Tuesday we will share our insights and opinions on the software testing space. Drop by every Tuesday to learn more!

<hr>

[Last Testing Tuesday](/2013/04/16/tests-make-software.html) we talked about test-driven development. We considered it the basis for Behavior-driven development. But what's the difference between them?

Test-driven development focuses on the *developer's* opinion on how parts of the software should *work*. Behavior-driven development focuses on the *users'* opinion on how they want your application to *behave*.

## Behavior-driven development

Test-driven development is rather a paradigm than a process. It describes the cycle of writing a test first, and application code afterwards – followed by an optional refactoring. But it doesn't make any statements about

* Where do I begin to develop?
* What exactly should I test?
* How should tests be structured and named?

The name *test*-driven development also caused confusion. How can you test something that's not there yet?

It was Dan North [1] who noticed all these unsolved questions and came up with a solution: He suggested that instead of *writing tests* you should think of *specifying behavior*. Behavior is how the *user* wants the application to behave.

When your development is Behavior-driven, you always start with the piece of functionality that's most important to your user. I consider this phase as taking the developer hat off and putting the user hat on. Once you've specified the user needs, you put the developer hat back on and implement your specification.

In my experience, this is the hardest part of the process: Often you don't know what's the most important Behavior for your user. However, over time and with a lot of feedback it becomes more apparent. But we'll get into that in a later blog post.

For now, let's assume we know what's most important to our users. So we know where to get started, but how do we specify Behavior? In traditional unit testing frameworks like Test::Unit a test would look like this:

<script src="https://gist.github.com/clemenshelm/5443299.js"></script>

This certainly works, but there are some flaws:

1. There's the word test in the class name and the method name, but we'd like to specify requirements instead.
2. The syntax is understandable but still appears artificial.
3. And most importantly: The test name doesn't state what it really tests.

In Behavior-driven development you specify Behavior in whole sentences. Not just the naming but also the code syntax should read naturally:

<script src="https://gist.github.com/clemenshelm/5395845.js"></script>

Now we know that a user lets you assign a name. But what's the real value of this syntax?

* *Focus:* You test exactly what the sentence says. No more, no less. This will let you write fine-grained and expressive specifications.
* *Documentation:* Just by reading the sentence, your team-mates will understand what this Behavior is about. They don't need to read a single line of code.
* *Regression:* When you run all these specifications later on, they become regression tests. When a regression test fails, you immediately see what Behavior of your application is broken.

But while this syntax is useful for specifying fine-grained Behavior of your application's components, it still doesn't say anything about the intentions of your users.

Dan North suggested a template that lets you describe features in natural language:

    Story: Returns go to stock

    In order to keep track of stock
    As a store owner
    I want to add items back to stock when they're returned

    Scenario 1: Refunded items should be returned to stock
    Given a customer previously bought a black sweater from me
    And I currently have three black sweaters left in stock
    When he returns the sweater for a refund
    Then I should have four black sweaters in stock

    Scenario 2: Replaced items should be returned to stock
    Given that a customer buys a blue garment
    And I have two blue garments in stock
    And three black garments in stock.
    When he returns the garment for a replacement in black,
    Then I should have three blue garments in stock
    And two black garments in stock
Source: [2]

Each story has a title and a short description of what the story is about. The format of this description is always the same:

* *In order to* get some benefit
* *As* the user you are developing for
* *I want* what this feature does

This description is always followed by a list of scenarios containing `Given` steps (what has happened before), `When` steps (what actions the user performs), and `Then` steps (the desired outcome for the user).

Here, we also apply the principle: Start with the Behavior that's most important to the user.

1. Find out the *feature* that's most important.
2. Within the feature always select the most important *scenario*.

This way you always stay aligned with your user's needs and focus on the stuff that matters.

There are tools like [Cucumber](http://cukes.info/) that enable you to test your natural language features automatically. I will introduce Cucumber on one of the next Testing Tuesdays.

## Conclusions

Many people refer to Behavior-driven development as "test-driven development done right" [3]. In fact, Behavior-driven development is a set of best practices that advise you on how to develop software by centering your users.

In the past software development was mainly about technical solutions. This has changed! Behavior-driven development focuses on the *purpose* of your work to people who will use it. This way you will create better software and successfully address your customers' needs. The technical solution arises from this process almost by itself.

In the next few episodes of Testing Tuesday I will introduce a few tools for Behavior-driven development. Stay tuned!

I barely scratched the surface of Behavior-driven development here, so I especially recommend checking out the references and further info (see below).

References:

* [[1] Dan North's original blog post](http://dannorth.net/introducing-bdd/)
* [[2] Wikipedia: Behavior-driven development](http://en.wikipedia.org/wiki/Behavior-driven_development)
* [[3] Get Your TDD Right with BDD](http://codingcraft.wordpress.com/2011/11/12/bdd-get-your-tdd-right/)

Further info:

* [YouTube video: BDD vs TDD (explained)](https://www.youtube.com/watch?v=mT8QDNNhExg)
* [The RSpec Book: Behaviour-Driven Development with RSpec, Cucumber, and Friends](http://pragprog.com/book/achbd/the-rspec-book)