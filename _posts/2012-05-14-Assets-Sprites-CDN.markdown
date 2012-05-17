---
layout: post
title: How to make your website superfast with the Asset Pipeline, Sprites and Amazon Cloudfront
author: Flo
description: How to make your website superfast with the AssetPipeline, Sprites and Amazon Cloudfront
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

We set static_cache_control to ***"public, max-age=31536000"*** so the browser caches all static assets for up to a year.

In production every asset has a hash added to its name, so whenever the file changes the browser requests the latest version as the hash and therefore the whole filename changes.

Through this mechanism you do not have to invalidate any CDN cache at all, but you can by simply setting ***config.assets.version*** this will change the Hash as well and make the browser request every asset again.

<script src="https://gist.github.com/2694525.js?file=development.rb"></script>
<script src="https://gist.github.com/2694525.js?file=production.rb"></script>

All of your assets will be precompiled during deployment to Heroku.

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
It is now very easy to create sprites for your website. Create an SCSS File like the following, which expects your buttons to be in ***app/assetsimages/buttons***
<script src="https://gist.github.com/2694525.js?file=buttons.scss"></script>

This will create a css file for you which references the specific buttons.

For example consider you have ***btn\_signup.png*** and ***btn\_signup\_hover.png*** in your buttons folder. You will then get a ***buttons-btn_signup*** css class that you can put onto any element and which sets the background accordingly.

The following example creates a link that has ***btn-signup*** as its background and gets a hover state. The second css class ***button*** is there to set the height and width of the button.
<script src="https://gist.github.com/2694525.js?file=button_example.html.haml"></script>

You have to make sure that you set the height and width of every button correctly, so the background fills the element exactly. Setting height and width is best practice in general as the Browser doesn't have to redraw the page when new images are loaded.

You can read more about compass in their [Reference Documentation](http://compass-style.org/reference/compass/)

###Gzip

To make sure all of your assets and pages are compressed before they are sent to your users add Rack:Deflater to your config.ru file.
<script src="https://gist.github.com/2694525.js?file=config.ru"></script>

This will make sure that all your pages are compressed and all the assets that are stored in Cloudfront later on are stored there compressed as well.

##Amazon Cloudfront

Now that all assets are set up correctly we can go on to configure [Amazon Cloudfront](http://aws.amazon.com/cloudfront/) as our CDN. With Cloudfront you can set a Custom Origin pointing to your application. Thus upon the first time you hit a specific Cloudfront url it sends a request to your application and from then on caches the result.

To get started go to the cloudfront tab in your [aws console](https://console.aws.amazon.com/cloudfront/home) and click Create Distribution.

![Amazon AWS Console](/images/assets/aws-console.png)

In the first step choose Download as this distribution is used for caching static assets.
![Distribution Settings](/images/assets/distribution-download.png)

In the next step enter your Domain name (or Heroku App Name) as the ***Origin Domain Name***. Every requests that will go to your Cloudfront distribution will be made to this URL and the result will be cached. If you use https set the ***Origin Protocol Policy*** to Match Viewer, so we can later set it to https and all transfers between your application and Cloudfront are encrypted.
![Distribution Origin](/images/assets/distribution-origin.png)
The default values set for the caching behaviour are sufficient. Cloudfront will use the Cache headers we set earlier and store all assets for up to a year.
![Distribution Caching](/images/assets/distribution-caching.png)
In the next window make sure the Distribution state is set to enabled.
![Create Distribution](/images/assets/distribution-create.png)

You will be presented with a last overview of your new distribution and can create it then.

Now you can set the asset host for your application. In your production.rb set ***config.action_controller.asset_host***. We prefer to set it to **ENV['ASSET_HOST']** as we can easily switch to another distribution then, which is very handy when using a staging server. Simply create another distribution for your staging environment and point there in your staging config.

In either case you have to point the asset_host to the distributions ***Domain Name*** which should in the end look something like ***https://d2d3cu3tt4cei5.cloudfront.net***, though in the AWS console *https* is not shown.

<script src="https://gist.github.com/2694525.js?file=production.rb"></script>

Before deploying into production test this on your staging app. Make sure that the assets are loaded from cache after your first request. In Chrome open the [developer tools](http://www.chromium.org/devtools) and go to the network tab.

If you reload the page with *F5* chrome will not use your cache, so make sure you either click somewhere in the page or simply open the page again through the NavBar.

You should see lots of *(from cache)* for your requests. Click through your application and make sure that all of your assets are loaded from cache the second time you request them.

![Chrome Network Tab](/images/assets/chrome.png)

Check your Heroku logs as well to be sure only the bare minimum of reqeusts are sent to your application.

If all works fine Congratulations you have made your application much more responsive. Now go and build something awesome (and [tell me about it](mailto:flo@railsonfire.com)).

###Conclusion
Combining Unicorn on Heroku with the Asset Pipeline and Amazon Cloudfront gives you an incredible platform to scale from. Only the bare minimum of requests are sent to your application and caches are used all along the way to make your application fast, responsive and cheap to run.

If you have any questions regarding the setup or anything else you can send an email to [flo@railsonfire.com](mailto:flo@railsonfire.com), a Tweet to [@Railsonfire](https://twitter.com/#!/railsonfire) or use the Olark Chat Box in the right hand corner.

###Thanks
Thanks to [Arvid Andersson](http://blog.arvidandersson.se/2011/10/03/how-to-do-the-asset-serving-dance-on-heroku-cedar-with-rails-3-1) and [Tom Coleman](http://bindle.me/blog/index.php/395/caches-cdns-and-heroku-cedar) for their blogposts.