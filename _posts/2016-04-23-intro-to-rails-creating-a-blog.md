---
layout: post
title:  "Intro to Rails - Creating a very basic blog"
date:   2016-04-23 2016  18:43:00 +0100
categories: "learning-rails"
author: "Kieran"
---
[Mackenzie Child](https://mackenziechild.me/){:target="_blank"}
 made a 6-part video series called [Intro to Rails](https://www.youtube.com/watch?v=2gUbteh54RM&list=PL23ZvcdS3XPKnwg3lMv-JGNCn08kB0wsA&index=1){:target="_blank"}
 , in which you get a 10,000 foot overview of Rails, and then walk through building a basic CRUD application.

These are the notes I took after I had followed all six parts of the series. The first four videos were introductions to Rails and Ruby, and installing each. I watched all of the videos, but my notes skip ahead to the new stuff.

## Creating a new project

To start with, we run

        $ rails new blog

This creates a new directory called 'blog', generating files and folders inside it for us to edit and play with.

Rails strictly uses an MVC architecture. This means that an application is split into Models, Views and Controllers. Models are your databases, views are your template files etc, and controllers are the logic of the application.

## Generating the models and controllers required

In order to start writing the blog, we need to generate a model for posts, a controller file for posts and the views we need.

        $ rails generate model Post title:string body:text
 
The above command generates a file in the 'app/models' directory called post.rb, which contains a title column set to contain a string, and a body column, set to contain a block of text. We need to run '$ rake db:migrate' to create schema.rb in 'app/db' which shows the current state of the db.

        $ rails generate controller Posts index

This will generate a file called posts\_controller.rb in the '/app/controllers' directory, and give us an index method in the posts\_controller.rb file, adn an index view file in 'app.views'. Not only that, but in 'app/config' it has also created a 'get' entry in routes.rb.

This is familiar to me because I've done routing with Sinatra, and I can see this is adding everything I would do in Sinatra so far, but in a far more modular fashion.

## Controllers and routes

Running the following command:

    $ rake routes

in the command line is going to show us all the routes we have currently.

Changing the routes.rb file to read

    resources :posts

and running the '$ rake routes' command again will now instead show us the CRUD routes we need to create in order to make our post\_controller.rb REStful. For example, we might write a link\_to in a view template. To get the correct route, we just write 'post_path'.

## Creating RESTful actions

All RESTful actions need to be created in the appropriate controller file. In this case, we are just using post\_controller.rb.

We created:

        def index
        end

        def new
        end
    
        def create
        end
    
        def show
        end
    
        def update
        end
    
        def destroy
        end

This covers all of the CRUD actions we need, as well as allowing us to show all posts. We also create a private method to help us find the appropriate post which looks like this:

        def find_post
          @post = Post.find(params[:id])
        end

This is very familiar from Sinatra for me. We also created a before action so that the find\_post method was applied to the show, edit, update and destroy methods:

        before_action :find_post, only: [:show, :edit, :update, :destroy]

## Adding posts

Once we had all of the CRUD methods set up in the controller, we need to create a form that will allow users to add and edit posts from.

In order to stop us from repeatedly writing this form, we created the form in a partial. These are just template snippets saved in the '/views/posts' directory alongside the template files, but they are saved with an underscore at the start of the filename. We can use this in the appropriate templates by just adding the following to each:

        <%= render 'form' %>

I'm not going to write much about the form itself. It's a form written in ERB, with a do/end loop so we can add the appropriate attributes to the database.

We also changed the 'new' method to look like this:

        def new
          @post = Post.new
        end

This is straightforward. It sets a new object in the db equal to @post instance variable.

Next, we wrote the logic for the 'create' method so that when we hit Submit, something actually happens. This part was quite interesting actually. First we changed the code to look like this:

        def create
          @post = Post.new(post_params)
          if @post.save
            redirect_to @post
          else
            render 'new'
        end

The 'post\_params' bit is new to me. The way I explained it to myself was that we need to let Rails know what parameters we should allow our app post to the database in the same way 'attr_accessor' is used to show which variables are available to the user in a class.

We define 'post\_params' privately at the bottom of the file like so:

        private
        
        def post_params
          params.require(:post).permit(:title, :body)
        end

This is known as 'Rails strong parameters'. The rest of the 'create' method is understanble. If the post is saved, redirect the user to the newly posted article, otherwise render the new post page.

## Showing posts

We next have to create the 'show' actions so the user can actually see each blog post. First we create a template in '/views/posts' with the title of the post (@post.title) as the title of the page, and then contents of the post in a separate div (@posts.body).

We can test this by going to 'localhost:3000/posts/new' and creating a post. Because we have allowed find\_posts to apply to the 'show' method, it curently sits empty as there is no other logic needed.

In order to show the user all posts that exist, we added the following code to the 'index' method:

        def index
          @posts = Post.all.order("created_at DESC")
        end

In the 'index' template, we write code to loop through all posts and post out the title and a truncated snipped of the body:

        <% @posts.each do |post| %>
          <h3><%= post.title %></h3>
          <div><%= truncate(post.body, :length => 35) %></div>
          <br>
          <%= link_to "Read post", post %>
        <% end %>

This reads as such: for each object in @posts, print out the title in an H3 tag, and the body, truncated to 35 characters. We then also link to the actual post using the Rails 'link\_to' helper method and the post prefix we seen in the rake-generated CRUD reference earlier.

Next, we have update and delete actions to complete our CRUD app.

## Updating and deleting to round out the CRUD actions

Updating uses the 'edit' and 'update' methods. The 'edit' method only needs to find the correct post to edit, which we've done by creating our 'find\_post' helper method. The 'update' method is changed to contain a little bit of logic like so:

        if @post.update(post_params)
          redirect_to @post
        else
          render 'edit'
        end

We then created the 'destroy' logic:

          def destroy
            @post.destroy
            redirect_to root_path
          end

The above finds the post in the db and sends the destory command, then redirects the user back to whatever the root path is.

## Finishing touches

Finally, in the 'show' template, we want users to be able to edit posts, or delete them. We do this by writing two link\_to methods, and taking advatage of the prefixes rake showed us again:

        <%= link_to "Edit post", edit_post_path(@post) %>

        <%= link_to "Delete", post_path(@post), method: :delete, data: {confirm: "Are you sure?"} %>

In order to have the '/posts' directory set as the default index, we need to browse to the routes.rb file in '/config' and add the following:

        root 'posts#index'
        
This takes the posts controller action and assigns it to the index, so they are one and the same.

The last thing we did in this application was to mess about in the '/views/layouts' directory in order to add some navigation by adding the following code to the body of the layout template:

        <h1>Intro to Rails - Mackenzie Child</h1>
        <%= link_to "Home", root_path %> | 
        <%= link_to "New Post", new_post_path %>

I added an extra step here not in the video. I've initialised a git repo locally, created a GitHub repo for this project online and pushed the code over there.

            $ git init
            $ git add .
            $ git commit -m "First commit"
            $ git remote add origin {my_github_repo.git}
            $ git push origin master`

## Next steps

The next step for me is to create a very basic CMS for this very basic blog. Once I know the backend code is solid, I'll make it look a little nicer.

All I'll need to do here is provide extra options for an administrator when they view the index page, and the add user authentication, as well as an administrator account.

I've done this with Warden in Sinatra, but Devise seems to be the preferred option in Rails, so I'll watch some videos and learn how to use that.

I think I remember that Devise is built on top of Warden. If so, it shouldn't be any harder than Warden, and it may even be easier!

I've found two tutorials to take me through Devise which I'll watch this evening:

 * [Mackenzie Child -  Setup User Authentication with Devise](https://www.youtube.com/watch?v=zJYuLebl-Js){:target="_blank"}

 * [Stuk.io Codecast - User Authentication in Ruby on Rails using Devise](https://www.youtube.com/watch?v=ZEk0Jp2dThc){:target="_blank"}
 
[This post](https://www.youtube.com/watch?v=agzm_O-pZFE){:target="_blank"} by [codeplace.com](https://www.codeplace.com){:target="_blank"} Shows how to use a gem called [rails\_admin](https://github.com/sferik/rails_admin){:target="_blank"}
 which I'll also watch because it sounds too good to be true. On a quick Google search of this gem, I've also found another -  Active Admin, and a [nice little write up](http://batsov.com/articles/2011/11/20/admin-interfaces-for-rails-apps-railsadmin-vs-activeadmin/){:target="_blank"}. explaining the difference between the two.
 
Guess I've got a little bit of homework to do before I start this next project!
