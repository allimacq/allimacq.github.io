---
layout: post
title:      "Sinatra Portfolio"
date:       2021-04-02 19:57:43 +0000
permalink:  sinatra_portfolio
---


For my Sinatra portfolio, I continued with the same idea for my previous project: Accessible Trails. This is something that once I finish Flatiron, I would actually like to build and launch.

For this project, I have three models: a user, trail, and state. The user has a username, password, and many trails. A state has many trails, and a name. A trail belongs to a user, a state, and has descriptive information (float and strings) like distance, name, surface type, and trail information.

Since I continued with the same idea, I modified the scraper class from my previous project to generate seed data for testing. This proved to be quite useful for many reasons, but particulary for testing if users can delete other users' data.

There are three controllers: the application, user, and trail. The user controller has routes for logging in, signing up, logging out, and viewing the user's homepage. The trail controller is the one that uses all of the basic CRUD functions. Users can create a trail, anyone can read a trail or trails in a state, and only users can update and delete trails they've created. Note that updating and deleting are made possible through Rack::MethodOverride.

If a user is logged in and tries to access the login or signup page, they are redirected. This is made possible by enabling sessions. I also used sessions in conjunction with rack-flash. Depending on what a user or visitor has tried to access, when they are redirected, a corresponding message appears at the top of the page.

I think the most challenging part of this project for me was the setup, particularly the config.ru file and environment.rb file. It took a bit of trial and error and a lot of googling, but I'm sure the more that I make projects, the more familiar it will become. All I can say is thank goodness for error messages and stackoverflow!

