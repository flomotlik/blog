---
layout: post
title: Getting efficient when working with Heroku
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: Getting efficient when working with Heroku. Just create a ***h*** function that calls the heroku gem with the last parameter being the heroku app name.
keywords: Codeship, Heroku, efficient programming, top 10 tools for faster programming, tips, tricks, heroku tutorial
image: http://blog.codeship.io/images/unicorn/heroku.png
---
Heroku provides a great infrastructure, but using their Gem often feels
a little bit clunky. Especially when viewing the Heroku logs or getting
into the console typing out all of the commands is time consuming.

To make working working with Heroku a little easier we started to use
bash aliases and functions. Simply add them to your bash config.

<script src="https://gist.github.com/2769640.js?file=bashrc"></script>

***UPDATE:*** [@_tomekw](https://twitter.com/#!/_tomekw) ported the
functions to ZSH, which is awesome. Thx. 
<script src="https://gist.github.com/2834383.js?file=zshrc"></script>

This simply creates a ***h*** function that calls the heroku gem with
the last parameter being the heroku app name.

For example to show the config for our production app I simply type
***hp config***

I personally do not like to have our Heroku production application as a
git remote as it forces me to always go through the proper way of testing
and deployment with Codeship.

Thus I often need to provide the app name which is nicely solved by
these functions.

They provide a nice template which you can use to build more complex
functions or aliases for your daily development.

As you use the Heroku Gem often on a daily basis even the smallest
productivity gain has tremendous effects on your time. Give it a try.

[Heroku Plus](https://github.com/bkuhlmann/heroku_plus) seems to be
another gem worth looking at for more efficient Heroku Usage. They have
very nice support for multiple accounts.

So take a look at those and if you have any more efficiency tips add
them in the comments section. I will update this post accordingly.
