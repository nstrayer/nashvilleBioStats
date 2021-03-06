---
layout: post
author: "Nick"
title:  "Branch Branch Revolution"
date: 2016-01-07 05:00:00
categories: [git, collaboration, commandline]
tags: [research, teams, introduction]
desc: An introduction to branching. What, when, where and why. 
---




## The Problem:

So here's the deal. You have a bunch of people (or just 2) working on a project involving code or really anything file based. What if you want to divy up jobs to those people and handle all of the code responsibly. What you might do is have someone take all the code. Make their changes, then email it to you, then you open up the files, grab the code you want, copy and paste it into your working script, test. Cry. It works, but can be super messy and often ends up with you having a file structure something like this...


{% highlight bash %}
-version1.R
-version2.R
-collaboratorVersion1.R
-bugFix.R
-myFigures/
  -fig1.png
  -fig1_fixed.png
-theirFigures/
  -fig304.jpg
...
{% endhighlight %}

There must be a better way. There is. Enter __branching.__

## Branching:

With git, a project can have multiple collaborators, each with their own copies of the code that is centrally synced by a service such as github. When you make a new  branch it is the equivalent to you taking the whole repository and copying it to a new folder. In fact you already have a branch out, it's called the `master`  branch and it is the default and typically most stable version of the code. 

Now returning to the example from before. Say you wanted collaborator number 1 to add a plot of covariates. Collaborator number 1 would then "checkout" a branch from the central git repo. Let's call it `addPlot`. They would then go into the code and add the snippit to generate the plot. Once they are satisfied with their work they would initiate something called a __pull request.__ 

## Pull Requests (or "I would like to merge."): 

This is essentially saying. _"I have made some change to the code that I think is good enough to be incorperated into the working version."_ The debate over how big of a change this needs to be is much bigger than this post so I will leave that up to you.  


You can do this entirely in the command line by simply merging; but I personally have found that this just ends up being confusing and leads to lots of googling of topics like "how to fix bad merge". A pull request checks to see if the merge works well and gives the collaborators a chance to give comments on the changes before attempting to mash the two code version together. 

In a service like github you have the great convenience of a nice user interface to do all this. I now do almost all of my merging this way. (Althugh sometimes you can't. See my [merge conflicts post.](http://nickstrayer.me/nashvilleBioStats//2016/01/branching.html))

(Tutorial on flow)[https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow ]

