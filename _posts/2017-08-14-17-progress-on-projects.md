---
layout: post
title:  "Progress on projects"
date:   2017-08-11 2017 18:42:00 +0100
categories: "learning-rails"
author: "Kieran"
---
I've been off work for the last few days so I've managed to fix both [Accountables](https://accountables.herokuapp.com){:target="_blank"} and [Havamal].(https://havamal.herokuapp.com/){:target="_blank"}. Now they both work and are live as long as the free Heroku dynos are spinning.

A few weeks ago, I decided to make a habit of finishing things. Any goals I have for accomplishing things will never be met if I don't finish anything.

Working out how to fix the stuff I already made was a start to this. I wanted ideas for smaller projects I could complete either with the stuff I already know, or that I was confident I could learn in a short time. To get these ideas, I searched Twitter for "I wish their was a website that". I scrolled through the results and pulled out the following:

1. A website where people type in symptoms and it always returns the result "you're probably just tired and overreacting"

2. A website that keeps track of the TV shows you watch.

3. A website where you can watch building and traffic cameras in major cities.

4. A website that spoils new movies in detail

5. A website that shows "on this day x years ago" but it compares what Trump is doing vs what Obama was doing that day

6. A bookmarklet button you can press that says "I hate ur sleazy website with its 15 pop-ups" that would report directly to Google

7. A website that mails you a different prayer card every month, and each prayer card comes with questions and reflections to meditate on for the whole month

8. A website that tells you how long you've been following someone

9. A website where people can leave honest review for blogging courses/products

10. A website that lists all your unplayed Steam games

From this list, I picked #1 to do first. Partially because it's the easiest but mostly because the idea of this existing really made me laugh.

I planned it out on paper and then recorded my screen as I created it live. You can see the [video here](youtu.be/Xso-YdGLwhl){:target="_blank"}, the [Github repo here](https://github.com/keerin/symptoms){:target="_blank"} and the live site [here](https://checkyoursymptoms.herokuapp.com/){:target="_blank"}.

I've also tweeted it out to the guy who tweeted the wish for this in the first place. Again, because the idea of someone tweeting something silly out in February and getting exactly what you wished for from a stranger in August just makes me laugh every time I think about it.

The next one I wanted to tackle was #3. I chose this as the next thing to work on because it sounds doable, as long as it was a website where links to public cctv camera feeds were submitted by users and the links are then displayed.

So far I have set up the front and back end skeleton so that requests are routed and views are rendered correctly, there is a passable front-end, and the submission form works. I've called it City Cam Repo and you can see it [here].(https://citycamrepo.herokuapp.com/){:target="_blank"}

Next, I need to work out how to get each record from the database into the table on the index page. This has proven more difficult that I expected, but I also think I'm overthinking it. I'm gonna leave this for a few days and come back to it at the weekend.

In the spirit of actually finishing things, I can always deploy as is and manually update the html table. This is not a great option but shipped is better than not shipped, so fuck it.

The list of things to do on the City Cam Repo project are:

1. Show atual data from the db in each table row
2. Pagination
3. Report dead link button

Then the basic stuff like force redirect to https and www, analytics and set up a domain.

I know it sounds a bit silly to start a habit of finishing things by starting a bunch of stuff while pausing other things! I do want to finish the RoR tutorial series because it is very good, and I want to be making more complex things in future. I'm still doing small chunks of the course weekly, so I've not actually stopped this and I am on track to complete in September.

Next month I'll also be doing #9 on the list - a website where you can leave honest reviews of blogging courses/products. This seems to be very similar to the City Cam Repo site, but with each link going to a review page. So, only a step more difficult but still something new for me.

I really like the reactions I'm getting from the people I got the ideas from when I tell them I'm working on their idea!

I was thinking about how you could monetize this. I don't think people will pay money to leave reviews, but I do think that the owners of these courses or products will pay for the ability to flag reviews as being for older versions of their course/product where they have corrected issues flagged in reviews, and also for the ability to reply to respond to reviews.

There is also the possibility of native ads. The target audience will be people who will spend money on blogging courses and products. It only makes sense to advertise related items.
