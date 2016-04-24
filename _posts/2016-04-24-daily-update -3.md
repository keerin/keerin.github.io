---
layout: post
title:  "Daily Update #3"
date:   2016-04-24 2016  16:25:00 +0100
categories: "learning-rails"
author: "Kieran"
---
Today I added TinyMCE to my blog.

To do this, I basically just followed the [instructions here](https://richonrails.com/articles/adding-tinymce-to-your-rails-application){:target="_blank"}. I only included tinymce in the view page I wanted it to be used in, so I'm not loading more than I need to. I saved and tested, and [it looks](/learning-rails/2016/04/24/tinymce-test/) and works like it should. Success!

My next action was to look into ActiveAdmin and try installing and running it. I really hated it. Maybe too advanced for me?

I managed to get an admin panel up and running, and added a page to manage my posts. It does this by linking with, and reading from any model you specify. I even managed to install TinyMCE so that it worked in the admin panel! But I came up short when trying to then save these updates.

I tried for about half an hour but I didn't even understand the errors I was given, so I just gave up completely.

I decided to just roll my own administration page, and started setting up Devise but ran out of time. The idea is to create an admin user, then add authentication control logic to the 'show.html.erb' template so that you cannot see the edit or delete buttons unless you are logged in.

After this, I'll create a new 'admin.html.erb' template where I will list all posts, and allow the admin to edit or delete from there as well.

This can all be done next week however. it's not especially difficult, it will just be learning how to use Devise with Rails.
