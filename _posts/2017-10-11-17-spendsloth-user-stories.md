---
layout: post
title:  "Spendsloth = User Stories"
date:   2017-10-11 2017 19:15:00 +0100
categories: "making-things"
author: "Kieran"
---
I've always been terrible at keeping track of my money. For too many months I get to the end only to have literally not even a penny. All of my bills get paid of course, but maybe not on time...

I decided to build a tool that would let me add all my outgoings, then deduct these from my incomings and not only show me what I have left which was safe to spend, but also email me this so I had a reminder on the first of every month.

This post will detail the user stories I've written for what I have called Spendsloth. The user stories details the minimum fuctionality I believe will be required for any kind of release - an "MVP" so to speak.

Below are all use cases I can imagine at this point. Each will be represented by a test. 

Below each is my estimated development time. I will time my work in Toggl and track progress in Trello.

This looks like it will take two weeks to go from git init to MVP (core functionality, bare bones front-end), with an average of 2.5hrs a day spent on the project.

## Sign up and login

### MM001

* As a visitor
* I can sign up using my email address and a password
* and receive an activation email

(Estimate: 4hrs)

### MM002

* As a pre-activated user
* I can click on a link in an activation email
* this will redirect me back to the website as a logged in user

(Estimate: 3hrs)

### MM003

* As a logged out user
* I can log in
* and choose "remember me" which sets a persistent cookie

(Estimate: 2hrs)

### MM004

* As a logged in user
* I can log out
* and I will be redirected back to the login page

(Estimate: 1hr)

### MM005

* As a logged out user
* I can use a forgotten password facility
* which emails me a link to allow me to reset my password

(Estimate: 4hrs)

### MM006

* As a user
* I can visit an account settings page
* and set my name
* and/or change my email
* and/or change my password
* and/or set my monthly income
* and/or delete my account

(Estimate: 1hr)

## Basic functionality

### MM007

* As a logged in user
* I can see the money left when all bills are subtracted from my monthly income

(Estimate: 2hrs)

### MM008

* As a logged in user
* I can view all bills added
* and see the value
* and see the description on click

(Estimate: 2hrs)

### MM009

* As a logged in user
* Hovering over each bill will present an option to edit
* the name
* and value
* and description
* and delete the bill

(Estimate: 2hrs)

### MM010

* As a logged in user
* I can add a new bill name
* and value
* and description
* and save it

(Estimate: 1hr)

## Advanced functionality

### MM011

* As a logged in user
* When adding or editing bills
* I can add one tag from a list of provided tags
* So that I can categorise my spending

(Estimate: 3hrs)

### MM012

* As a logged in user
* I can see all tags
* and a count of bills currently associated with each

(Estimate: 2hrs)

### MM013

* As a logged in user
* I can see a bar chart of my spending
* with £ on the y axis and the tags on the y axis
* so that I can visualise my spending

(Estimate: 5hrs)

## Total estimates

* Setup and configuration: 3hrs
* Sign up and login: 15hrs
* Basic functionality: 7hrs
* Advanced functionality: 10hrs

Total: 35hrs

