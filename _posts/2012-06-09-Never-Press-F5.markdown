---
layout: post
title: Never press F5 when developing your Web app
author: Flo
description: Never press F5 when developing your Web app
---

***TL:DR: Use [Livereload](http://livereload.com/) to never press F5 in your browser and if you use Vim try [my new
Extension](https://github.com/flomotlik/vim-livereload)***

A source of constantly wasted time when developing for the web is reloading
your pages. Especially when editing CSS continuously reloading your page
takes lots of time and focus away from your editor.

Of course you can edit CSS inside your browser, but then you have to
merge those changes back into your CSS or HTML files again, which is
error prone and boring.

A much better way instead is using [Livereload](http://livereload.com/).
Livereload reloads your
page whenever you change any file. When it detects changes in a CSS or
Javascript file it merges them into the current page. This is especially
great when you work on your CSS as any change is immediately visible in
your browser.

There are several ways to get started with Livereload. If you use a
Mac simply buy their App from [the App
store](http://itunes.apple.com/us/app/livereload/id482898991?mt=12). It costs $9.99 and works
very fine. You earn the cost back in productivity on day one. Easily.

There is a [guard
integration](https://github.com/guard/guard-livereload) for Livereload as well.

If you prefer having Livereload included in your development tools there
are several options. Their [Help
Pages](http://help.livereload.com/kb/editor-support/using-custom-scripts-to-support-other-editors)
show support for various popular editors like TextMate and Sublime2.
Sadly no Vim support.

So here it is the all new [Vim-Livereload Extension](https://github.com/flomotlik/vim-livereload)

###Vim-Livereload
The Vim-Livereload plugin tells your browser to reload the page you
watch over with Livereload on every save. Simply click the Livereload
button in your browser to listen to events and the next time you save
your file it will be reloaded and CSS will be merged automatically.

Setup is really simple (if you use Janus, never tried it with anything
else)

* Clone into ~/.janus folder
* cd into vim-livereload and run ***rake***

Rake initialises the submodules and pulls the latest changes. The plugin
needs Vim with Ruby support. It uses eventmachine to open a Socket and
receive requests from the [Livereload Chrome Browser
plugin](https://chrome.google.com/webstore/detail/jnihajbhpnppcggbcgedagnkighmdlei).
I only tested it with Chrome and the latest version in the chrome store.

###Conclusion
Give Livereload and the Vim plugin a try. It is incredibly easy and
makes you much more productive immediately.

As developers we should always thrive to be more efficient in our day to day work.
Cutting those small little actions you do thousands of times per day makes your
development lean and productive. If you have any tips on improving your
workflow step by step simply add a comment down below.

I really want to know your little tricks to be a better and faster developer.

