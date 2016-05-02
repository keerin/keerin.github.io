---
layout: post
title:  "Daily Update #6"
date:   2016-04-27 2016  22:13:00 +0100
categories: "learning-rails"
author: "Kieran"
---
This evening I've jumped in and created a new view and method in my existing posts controller so I can separate the management of posts from the "show" page.

The way I did this was by going to my posts_controller and adding the following code:

    def manage
      @posts = Post.all.order("created_at DESC")
    end
{: .language-ruby}

This allows me to view all the posts in descending order in a manage view. This was added to the /views directory with the below code:

    <h2>Manage Posts</h2>
      <ul>
        <% @posts.each do |post| %>
          <li>
            <%= link_to post.title, post_path(post)%> | 
            <%= link_to "Edit post", edit_post_path(post) %> | 
            <%= link_to "Delete", post_path(post), method: :delete, data: {confirm: "Are you sure?"} %>
          </li>
        <% end %>
      </ul>
{: .language-erb}

I finished off the amends to the views by adding management options to the layout file as such:

    <% if admin_signed_in? %>
      | <%= link_to "Manage", "/posts/manage" %>
      | <%= link_to "New Post", new_post_path %>
      | <%= link_to('Logout', destroy_admin_session_path, :method => :delete) %>
    <% end %>
{: .language-erb}

In order to render these in the browser, I needed to add a get route to the routes.rb file, which looks like this:

    resources :posts do
       collection do
         get 'manage'
       end
    end
{: .language-ruby}

This all works fine. It still looks ugly like a newborn baby, but that's fine, because it is.

The problem I found, as soon as I was finished, is that I've done all of this completely wrong. It's super, super dirty.

I shouldn't be plopping new methods inside the posts_controller at all. For what I am trying to do here, I need a management controller. Right now, I only have a CMS for the posts. If I create other pages which are not posts on the website, my CMS admin user will have no control over them, or even access to them.

I've found my work for tomorrow. I need to read about routing in Rails. Routing is the very heart of Sinatra, so I understood it perfectly there. In Rails, it's a little different. I'll be reading [this article on the Rails site](http://guides.rubyonrails.org/routing.html){:target="_blank"} tomorrow so I have a better idea of how all this works.

Finally, I realised I'll need to work out how to create pagination and archives for this Jekyll blog, as well as my Rails blog! My plan going forwards is to sort out a new manage controller and uderstand how all the magic works, then move away from features completely and learn about testing. Until then, no pagination.
