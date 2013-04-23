---
layout: post
title: "Testing Tuesday #2: From test-driven development to behavior-driven development"
author: Clemens Helm
twitter: clemenshelm
google: 116883220210675577340
description: Codeship Testing Tuesday #2 – TODO.
keywords: Codeship, TDD, test driven development, behavior driven development, behaviour driven development, testing tuesday, testing tools, testing methodologies, develop iteratively, hosted testing, testing in the cloud
image: TODO
---
This is the second blog post of our new *Testing Tuesday* series. Every Tuesday we
will share our insights and opinions on the software testing space. Drop by every
Tuesday to learn more!

<hr>

In [last weeks Testing Tuesday](/2013/04/16/tests-make-software.html) we talked about test-driven development. We considered it the basis for behavior-driven development. But what's the difference between them?

Test-driven development focuses on the *developer's* opinion on how parts of the software should *work*. Behavior-driven development focuses on the *users'* opinion on how they want to use your application.

## Behavior-driven development

The problem with test-driven development is that it's rather a paradigm than a process. Test-driven development describes the circle of writing tests first, and application code afterwards – followed by an optional refactoring. But it doesn't make any statements about

* Where do I begin to develop?
* How much should I test?
* How should tests be structured and named?

Also the name *test*-driven development caused confusion. How can you test something that's not there yet?

It was Dan North [1] who noticed all these unsolved questions and came up with a solution. He suggested that instead of *writing tests* you should think of *specifying behavior*. Behavior is how the *user* wants the application to behave.

When you develop behavior-driven, you always start with the piece of functionality that's most important to your user. I consider this phase as taking the developer hat off and putting the user hat on. In my experience, this is the hardest part of the process: Often you don't know what's the most important behavior for your user. However, over time and with a lot of feedback it becomes more apparent. But we'll get to that in a later blog post.

For now, let's assume you know what's most important to your users. So we know where to get started, but how do we specify behavior? In traditional unit testing frameworks like Test::Unit a test would look like this:

<script src="https://gist.github.com/clemenshelm/5443299.js"></script>

This certainly works, but there are some problems:

1. There's the word test in the class name and the method name.
2. The syntax is understandable but still appears artificial.
3. The test name doesn't state what it really tests.

In behavior-driven development you specify behavior in whole sentences. Not just the naming but also the code syntax should read naturally:

<script src="https://gist.github.com/clemenshelm/5395845.js"></script>

Now we know that a user lets you assign a name. But what's the real value of this syntax?

* *Focus:* You test exactly what the sentence says. Not more, not less. This will let you write fine-grained and expressive behaviors.
* *Documentation:* Just by reading the sentence, your team-mates will understand what this behavior is about. They don't need to read a single line of code.
* *Regression:* When you run all your behaviors later on, they become regression tests. When a regression test fails, you immediately see what behavior of your application is broken.

But while this syntax is useful for specifying fine-grained behavior of  your application's components, it still doesn't say anything about the intentions of your users.

Dan North suggested a template that describes your features in in natural language:

```
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
```
Source: [2]

Each story has a title and a short description what the story is about. The format of this description is always the same:

* *In order to* get some benefit
* *As* the user you are developing for
* *I want* what this feature does

This description is always followed by a list of scenarios containing `Given` steps (what has happened before), `When` steps (what actions the user performs) and `Then` steps (the desired outcome for the user).

Also here applies the principle: Start with the behavior that's most important to the user. Find out the *feature* that's most important and within the feature always select the *scenario* that's most important. This way you always stay aligned with your user's needs and focus on the stuff that matters.

There are tools like [Cucumber](http://cukes.info/) that enable you to test your natural language features automatically. I will introduce Cucumber on one of the next Testing Tuesdays.

## Conclusions

Many people refer to behavior-driven development as "test-driven development done right" [3]. In fact, behavior-driven development is a set of best practices that advice you how to develop software by centering your users.

I barely scratched the surface of behavior-driven development here, so I especially recommend to check out the references and further reading.

In the next few episodes of Testing Tuesday I will introduce a few tools made for Behavior-driven development. Stay tuned!

References:

* [[1] Dan North's original blog post](http://dannorth.net/introducing-bdd/)
* [[2] Wikipedia: Behavior-driven development](http://en.wikipedia.org/wiki/Behavior-driven_development)
* [[3] Get Your TDD Right with BDD](http://codingcraft.wordpress.com/2011/11/12/bdd-get-your-tdd-right/)

Further reading:
* [The RSpec Book: Behaviour-Driven Development with RSpec, Cucumber, and Friends](http://pragprog.com/book/achbd/the-rspec-book)