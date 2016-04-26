---
layout: post
title:  "Daily Update #5"
date:   2016-04-26 2016  17:49:00 +0100
categories: "learning-rails"
author: "Kieran"
---
Today I've read through [this link](www.link.com){:target="_blank"}, which shows how to use Devise. I noteiced that Devise uses a method called "is\_authorised?", which you can use to create login and access logic. Very similar to how I've used Warden in Sinatra, so should not be too bad at all.

I previously wrote that I wanted to sit and create the views needed for Devise, but I also seen that I can just run to following to get this done:

        rails generate devise:views

This makes things a lot easier, and I can just focus on hiding the create, update and destroy actions behind an admin user. This means that I should be finished this tonight.

A colleage from work noted that he would be interested in designing and building a nicer front-end for this project, which is cool. As a junior front-end dev, he is interested in getting as much front-end experience as possible. He will be able to take what I build and build around the .erb templating. Because we use PHP and Twig at work, I don't think he will have many problems bulding .erb template pages.

Pretty excited to have a basic cms and basic blog set up, so I can start to plan and build more advanced (from me) features.
