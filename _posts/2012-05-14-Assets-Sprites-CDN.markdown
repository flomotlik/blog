---
layout: post
title: How to make your website even faster with the Asset Pipeline, Sprites and Amazon Cloudfront
author: Flo
description: How to make your website even faster with the AssetPipeline, Sprites and Amazon Cloudfront
---

***This is the second in a two part series on how we set up Railsonfire with Heroku. Read the [first part](/2012/05/06/Unicorn-on-Heroku.html) that deals with Heroku and how to use it more efficiently with Unicorn.***

In the first part we introduced our Unicorn setup that helps you manage several concurrent requests on one Heroku Dyno. The best optimization though is to remove all unnecessary requests completely.

As the Heroku Cedar stack has no Proxy in front of your Rails application any more static assets have to be delivered through your application. This is very inefficient and keeps your dynos or unicorn workers busy with serving your assets.

This blogpost shows how we use the Asset Pipeline, Compass and Amazon Cloudfront to serve all of our assets fast without sending any requests to our application directly.

##Asset Pipeline
***The [asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html) provides a framework to concatenate and minify or compress JavaScript and CSS assets. It also adds the ability to write these assets in other languages such as CoffeeScript, Sass and ERB.***

The [official Guide](http://guides.rubyonrails.org/asset_pipeline.html) has a brief an easy introduction on how to use the asset pipeline and [upgrade from earlier versions of Rails](http://guides.rubyonrails.org/asset_pipeline.html#upgrading-from-old-versions-of-rails). Another great resources is the [Railscast on the asset pipeline](http://railscasts.com/episodes/279-understanding-the-asset-pipeline).

We will briefly introduce our configuration and then go over our CSS, Javascript and Image setup.

###Configuration

At first add the necessary Gems to your Gemfile. It is assumed that you use the latest versions available of all gems.
<script src="https://gist.github.com/2694525.js?file=Gemfile"></script>

Then you need to configure the asset pipeline in ***application.rb, development.rb and production.rb***

<script src="https://gist.github.com/2694525.js?file=application.rb"></script>
<script src="https://gist.github.com/2694525.js?file=development.rb"></script>
<script src="https://gist.github.com/2694525.js?file=production.rb"></script>

###CSS

To minimize the requests needed for loading our site we create a single application.css through the asset pipeline. The inherent problem with this setup is that css definitions for different subpages can potentially clash. To make sure this doesn't happen we give the body tag a special id composed of the controller and action name of the current site.

<script src="https://gist.github.com/2694525.js?file=application.html.erb"></script>

Then we start every css file that only contains scss for a specific page with the id of that page and name the file **controllername**\_**actionname**.scss:

<script src="https://gist.github.com/2694525.js?file=home_index.scss"></script>

It's definitely not an ideal solution, but it works fine and we haven't figured out any better way to do this.

###Javascript

We compile all our Javascript into one application.js file, but as we are not very javascript heavy we didn't need to do any optimizations aside from compression here.

###Images

We use Spriting for all of the images in our application. Spriting is the process of combining several images into one bigger image to lower the number of requests to your website.

We use the [spriting feature of compass-rails](http://compass-style.org/help/tutorials/spriting/) heavily in our application. You should have already added compass-rails to your Gemfile before.

Configure compass by adding a compass.rb file in ***config/compass.rb***
<script src="https://gist.github.com/2694525.js?file=compass.rb"></script>

Now put your images into ***app/assets/images*** or one of its subfolders.
It is now very easy creating sprites for your website. Create an SCSS File like the following, which expects your buttons to be in ***app/assetsimages/buttons***
<script src="https://gist.github.com/2694525.js?file=buttons.scss"></script>

This will create a css file for you which references the specific buttons.

For example consider you have a ***btn_signup.png*** and ***btn_signup_hover.png*** in your buttons folder. You will then get a ***buttons-btn_signup*** css class that you can put onto any element and which sets the background accordingly.

The following example creates a link that has ***btn-signup*** as its background and gets a hover state. The second css class ***button*** is there to set the height and width of the button.
<script src="https://gist.github.com/2694525.js?file=button_example.html.haml"></script>

You have to make sure that you set the height and width of every button correctly, so the background fills the element exactly. Setting height and width is best practice in general as the Browser doesn't have to redraw the page when new images are loaded.

You can read more about compass in their [Reference Documentation](http://compass-style.org/reference/compass/)

###Gzip

To make sure all of your assets and pages are compressed before they are sent to your users add Rack:Deflater to your config.ru file.
<script src="https://gist.github.com/2694525.js?file=config.ru"></script>

This will make sure that all your pages are compressed and all the assets that are stored in Cloudfront later on are stored there compressed as well.

##Amazon Cloudfront



http://blog.arvidandersson.se/2011/10/03/how-to-do-the-asset-serving-dance-on-heroku-cedar-with-rails-3-1
http://bindle.me/blog/index.php/395/caches-cdns-and-heroku-cedar