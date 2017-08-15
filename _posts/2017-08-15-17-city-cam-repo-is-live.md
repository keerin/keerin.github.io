---
layout: post
title:  "City Cam Repo live"
date:   2017-08-15 2017 13:45:00 +0100
categories: "making-things"
author: "Kieran"
---
While I was searching for things to make I came across a tweet that said "I wish there was a website or an app where you could just watch building and traffic cameras in major cities. I just enjoy that stuff.".

So I decided to make it.

I followed the same basic layout for a Sinatra project as outlined in this [video](https://www.youtube.com/watch?v=Xso-YdGLwhl){:target="_blank"}. After some small changes for the project this means I start with the following files and directories:

- city-cam-repo
 - - config
 - - - database.yml
 - - db
 - - - seed.csv
 - - public
 - - - css
 - - - - normalize.css
 - - - - skeleton.css
 - - - images
 - - - - favicon.png
 - - views
 - - - about.erb
 - - - index.erb
 - - - layout.erb
 - - - submit.erb
 - - app.rb
 - - config.ru
 - - Gemfile
 - - Gemfile.lock
 - - model.rb
 - - Procfile

A bunch of these files are for Heroku or are Sinatra specific, such as the config directory, Procfile and config.ru. Sinatra expects some things to be in certain places, so it knows you will have your css files in ~/public/css, your view files in ~/views/ and so on. I've also called the model file model.rb for easier reference, and called the controller app.rb as is the convention for Sinatra files.

Adding in basic routing into app.rb and a basic html layout in layout.rb means you start with a really good framework to flesh things out. The basic routing in app.rb looks like this:

	get '/' do

	erb :index, :layout => :layout
	end

	get '/about' do
		erb :about, :layout => :layout
	end

	get '/submit' do
		erb :submit, :layout => :layout
	end

The basic html layout is from the Skeleton css boilerplate and looks like this in layout.erb:

	<!-- Starts layout -->

	<!DOCTYPE html>
	<html lang="en">
	<head>

		<meta charset="utf-8">
		<title>City Cam Repo</title>
		<meta name="description" content="">
		<meta name="author" content="">

		<meta name="viewport" content="width=device-width, initial-scale=1">

		<link href="//fonts.googleapis.com/css?family=Raleway:400,300,600" rel="stylesheet" type="text/css">


		<link rel="stylesheet" href="/css/normalize.css">
		<link rel="stylesheet" href="/css/skeleton.css">

		<link rel="icon" type="image/png" href="images/favicon.png">

	</head>

	<body>

		<div class="container">

			<!-- Logo -->
			<div class="row">
		 		<div class="offset-by-four centered logo">
		 			<h1><a href="/">City Cam Repo</a></h1>
		 		</div>
			</div>

			<!-- Navigation -->
			<div class="row">
				<nav class="offset-by-four centered">
					<ul>
						<li><a class="button button-primary" href="/about">About</a></li>
						<li><a class="button button-primary" href="/submit">Submit</a></li>
					</ul>
				</nav>
			</div>

	<!-- Ends layout include -->

		<%= yield %>
	
		<footer id="footer" class="centered">			
			<p class="copyright">Made by <a href="https://twitter.com/thekieran" target="_blank">@thekieran</a></p>				
		</footer> <!-- end footer -->
			</div> <!-- End container -->
		</body>
	</html>

And also looks like this in the index.erb (I did the same thing in the oher views):

	<!-- Starts index -->
		<div class="row">
			<div class="twelve columns">
				<p>Here be stuff</p>
			</div>
		</div> <!-- End row -->

	<!-- Ends index -->

I wrote up the model as well, and I use DataMapper as an ORM. Aside from the DataMapper config stuff, the model looks like this:

	class Link
		include DataMapper::Resource
	
		property :id,							Serial
		property :link_name,			String
		property :url, 						String,:length => 4..100
		property :submitted_by,		String
		property :date_submitted,	DateTime
	end

I then created some fake data in seed.csv. Running the usual commands to install all the commands and create the database and the table indicated in the model class "Link" created a local working version.

After this I changed the css to make it look a bit nicer. I then filled out the html in the other view files. This meant adding a static table to index.erb, a content block to about.erb, and a form for submissions to submit.erb so users can help add content.

I used [Formspree](https://www.formspree.io){:target="_blank"} to power the submission form so I cna start as quickly as possible. It's a waste of my time to make submission form for a V1 when something like Formspree works. I love Formspree.

That's the front-end done as far as I want it to be at this stage, and because of the basic code added to app.rb, it all works and can be deployed to Heroku which is what I did.

The next thing to do is to make sure the data in the database appears in the index view. This took me a little bit of time to work out.

Because you are using a Link class in the model and including model.rb in app.rb, you can use the Link class the same as any other class. This means I can write the below:

`@items = Link.all`

What caught me off guard was that I was expecting an array to come back, but I was given something called a DataMapper::Collection. It took me a while to read through the docs and work out that what I needed to write was:

	@items = Array.new << Link.all.each do |item|
		puts #{item.url}, #{item.link_name}.#{item.submitted_by}, #{item.date_submitted}"
	end

And then I can call this in index.erb with:

`@items`

And I'll get a string with the whole payload.

When I figured this out, all I had to do was split the code up so that I am setting up the items array in app.rb and setting up the iteration in the view. I think there is a better way to do this in strict MVC style, but it's done now. This means the only code left in app.rb is:

	get '/' do

	@items = Array.new

	erb :index, :layout => :layout
	end

	get '/about' do
		erb :about, :layout => :layout
	end

	get '/submit' do
		erb :submit, :layout => :layout
	end

And index.erb looks like this:

	<div class="row">
			<table class="twelve columns">
				<tr>
					<th>Link Name</th>
					<th>Submitted By</th>
					<th>Date</th>
						<% @items << Link.all.each do |item| %>
						<tr>
							<td><a href="<%= item.url %>" target="_blank" rel="nofollow"><%= item.link_name %></a></td> <td><%= item.submitted_by %></td> <td><%= item.date_submitted.strftime("%d/%m/%Y") %></td>
						<% end %>
						</tr>
			</table>
		</div>
  
Now I have the data being spit out from the database into a table. This will probably look very basic to a lot of people but I've never worked with data in this way before so I was pretty happy with this!

The final challenge I faced was getting this deployed to Heroku. For some reason, no database tables were being created when I pushed to live. If I had hair, I'd have been pulling it out here!

I looked at the commands being executed when I pushed to live to see if I could spot any errors. I did. I seen that bundler's config had been commited before I created a .gitignore file, so postgres related gems were not being installed on live. I used the "heroku run bash" command to spin up a bash shell so I could manually remove it.

Once I did this, everything worked! You won't see any trace of this in the commit history on Github because I wiped it completely and redid everything in a fresh directory and deployed everything from here.

I spent a few hours a day for three days on this, but [City Cam Repo](https://citycamrepo.herokuapp.com){:target="_blank"} is now live!

The to do list now looks like this:

* Add real data
* Add Google Analytics
* Add pagination
* Add a "Report dead link" feature
* Add a domain name
* Set up www, https and domain redirects

That's another session of around 2-3 hours which I'll do some other time. Pretty happy with my work at this point. I learned how to use Skeleton to wireframe out a basic layout very quickly, and learned a nice little technique to pull stuff from a database into a view. I did it a much more basic way previously.

In terms of what I do next, I want to do the following things this month:

* Complete the City Cam Repo to do list
* Complete the Havamal to do list
* Complete Chapter 11 and 12 of the Rails Tutorial
