---
layout: post
title: Getting efficient when working with Heroku
author: Flo
description: Getting efficient when working with Heroku
---
Heroku provides a great infrastructure, but using their Gem often feels
a little bit clunky. Especially when viewing the Heroku logs or getting
into the console typing out all of the commands is time consuming.

To make working working with Heroku a little easier we started to use
bash aliases and functions.

<script src="https://gist.github.com/2769640.js?file=bashrc"></script>

This simply creates a ***h*** function that calls the heroku gem with
the last parameter being the heroku app name. I personally do not like to have our
Heroku production application as a git remote as it forces me to always
go through the proper way of testing and deployment with Railsonfire.

Thus I often need to provide the app name which is nicely solved by
these functions.

These functions provide a nice template which you can use to build more complex
functions or aliases for your daily development.

[Heroku Plus](https://github.com/bkuhlmann/heroku_plus) seems to be
another gem worth looking at for more efficient Heroku Usage. They have
very nice support for multiple accounts.

So take a look at those and if you have any more efficiency tips add
them in the comments section. I will update this post accordingly.
