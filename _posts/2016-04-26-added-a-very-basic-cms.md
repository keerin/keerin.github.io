---
layout: post
title:  "Added A Very Basic CMS"
date:   2016-04-26 2016  19:52:00 +0100
categories: "learning-rails"
author: "Kieran"
---
This evening I added a very basic CMS to my very basic blog.

First, I installed the gem by adding the following into my gemfile:

          gem 'devise'
{: .language-ruby}

then running

          $ bundle install

I then ran the generator as follows:

          $ rails generate devise:install

Next, I ran the model generator and the views generator:

          $ rails generate devise admin
          $ rails generate devise:views admin

I want to grant logged in users full CRUD access, but everyone else should only have read access. To do this, I added the below to the posts controller:

          before_filter :authenticate_admin!, except: [ :index, :show ]
{: .language-ruby}

In order to only show the create, update and destroy links in the show template, I sinply had to add a little bit of logic as such:

          <% if admin_signed_in? %>
            <%= link_to "Edit post", edit_post_path(@post) %>
            <%= link_to "Delete", post_path(@post), method: :delete, data: {confirm: "Are you sure?"} %>
          <% end %>
{: .language-erb}

And finally, in order to allow the admin user to log out, I added the following to the template:

          <% if admin_signed_in? %>
            | <%= link_to "New Post", new_post_path %>
            | <%= link_to('Logout', destroy_admin_session_path, :method => :delete) %>
          <% end %>
{: .language-erb}

This turned out to be a lot easier than I expected. At least for my very simple use-case, this is super simple. The official [README](http://devise.plataformatec.com.br/){:target="_blank"} and [How To](https://github.com/plataformatec/devise/wiki/How-Tos){:target="_blank"} pages are also very easy to read and helpful.
