---
layout: post
title: The all new Railsonfire Gem
author: Florian Motlik
twitter: leanvienna
google: 115123333592137547204
description: How to use the Railsonfire Gem to start testing your code!
---
To make the setup and interaction with Railsonfire as easy as possible we implemented a Ruby Gem. Right now you can set up Continuous Integration and Continuous Deployment for your project with the gem in less than a minute.

In the future we will develop the gem and our service to support developers from starting a project through development, testing and deployment. Our goal is to make these services a no-brainer to use so you can concentrate on developing your apps.

To use it to interact with Railsonfire a few simple steps are necessary.

### What to do

1. gem install railsonfire
2. railsonfire new
3. git commit -a
4. git push origin master

### Are there prerequisites?

* Logged in to Railsonfire and allowed access to GitHub
* Local git version controlled ruby project
* GitHub remotes added to local git repository

#### for Heroku?

* Authenticated with the Heroku gem
* Heroku remotes added to local git repository

### What is exactly happening?

A more in depth description on what is exactly happening when you call the railsonfire gem.

#### gem install railsonfire
to install the latest version. Ruby 1.9.2 and 1.8.7 are supported

#### railsonfire new
You need to call this command in the folder of your ruby project. The gem will load all GitHub remotes and if you have several let you decide on which one to use. Then an ssh key is added to your GitHub Project so we can access it from our servers.

#### Authenticate
When adding a new project to railsonfire for the first time you will be asked for your Railsonfire token. The gem will try to open your Railsonfire [Account Page](http://railsonfire.com/users), where you can find your Railsonfire token. Simply paste it into the command line.

#### Decide if you want heroku support
The gem will detect any heroku remotes and give you the option to deploy to them. If you decide so the gem will upload an ssh key to Heroku. You need to have authenticated with the Heroku gem before.

#### git commit -am "Adding railsonfire.yml file"
You have to add the railsonfire.yml that is created by the gem to your codebase. You can edit the file to set your test commands or the ruby versions you want to test with.

#### git push origin master
Simply push your changes to GitHub. From then on we will test and deploy all of your changes.