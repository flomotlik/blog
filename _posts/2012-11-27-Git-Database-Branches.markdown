---
layout: post
title: Git database branches
author: Clemens Helm
twitter: clemenshelm
google: 116883220210675577340
description: How to keep a database synchronized with your branches.
image: http://blog.railsonfire.com/images/og_tag_gitbranches.jpg
---
![Branches](http://blog.railsonfire.com/images/branches.jpg)
We love Git. At [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) we are currently working on a new version of our platform with lots of new features. For every feature we create a separate Git branch and once it’s done we integrate the feature into the main application. This keeps us from having half-baked features spoiling our service.

There’s only one problem with this approach: While the code is perfectly separated in its own feature branch, the database will be the same for all branches. Unfortunately I tend to forget about this fact.

That’s why last week I spent quite some time fixing weird bugs. I modified the database working on a new feature, but eventually I had to fix something on another branch. These database changes broke a couple of tests on the other branch that assumed attributes existed in the database that didn’t anymore.

Right after I realized the mistake I thought about how to deal with this problem. I found two solutions:

### Solution #1: Regenerate the database on branch change

Git offers a nice feature named *hooks*. A hook gets called when certain events happen in the repository. For example, [Railsonfire](https://www.railsonfire.com/?utm_source=blog&utm_medium=link&utm_campaign=blog) gets informed by a *post-receive* hook whenever there’s a new version of your product in the repository so we can download and test it.

There are also client-side hooks which support the Git workflow on your computer. To fix my database problem I could write a *post-checkout* hook that always resets the database to the current branch’s version. Unfortunately this approach has two downsides:

1. All data gets wiped every time I change branches. I personally toggle branches quite frequently, sometimes just to have a quick look at what state another branch is in. So losing all data on every toggle is just annoying.
2. Wiping and recreating the database also takes a while. Not much, just a few seconds. But again, when you are used to toggling branches quickly, this is just annoying, too.

### Solution #2: A separate database for each branch

The solution I finally sticked with was creating a separate database for each branch. When I create a branch, I also create a database. Unfortunately there’s no Git hook to help me with this, but I create branches less often than I change branches. So not really annoying. This solution keeps all data for a branch in the database and always provides the correct schema, no matter what branch I am on.

#### How do I implement this?

This is an example for a Rails application, but I figure it will be easy enough to use it with any technology. For convenience I use the *git* gem for the test and development environment to. In production I won’t need this behavior, so my `Gemfile` looks like this:

<script src="https://gist.github.com/4156617.js?file=gistfile1.rb"></script>

Now I can use the git gem to retrieve the current branch and patch the `database.yml` file using ERB:

<script src="https://gist.github.com/4156638.js?file=gistfile1.yml"></script>

Voilà, this is all there is to do.

I like this solution for its simplicity and robustness. You could improve it by creating a *post-checkout* hook to create a database for a branch if it doesn’t exist yet, but this slows down toggling branches. I personally prefer to toggle fast and create the database myself – in Rails I only need one command to create it and set it up with test data.

Have you come across this problem as well? How did you solve it? I’m already curious to read your comments!