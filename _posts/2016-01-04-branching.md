---
layout: post
author: "Nick"
title:  "Git Conflict Management"
date: 2016-01-04 05:00:00
categories: [git, collaboration, commandline]
tags: [research, teams]
desc: A quick tutorial on how to deal with conflicts in pull requests. 
---



### Scenario: You have a merge conflict

Unfortunately github can't handle this in browser. 

It is up to the lord of the repository to handle this. 

Say you have a pull request from the branch `problemBranch` with a merge conflict to master: 

<div style="text-align: center;">
    <img src = "{{ site.baseurl }}/assets/mergeConflict.png" alt = "conflict message" width = "600">
    <br>
</div>

---

### Steps to handle: 

1) Go to the command line and enter your repo's directory: 
  
  {% highlight bash %}
  cd path/to/project/
  {% endhighlight %}
2) Make sure everything is up to date with the github hosted repo: 
  
  {% highlight bash %}
  git pull
  {% endhighlight %}
3) Make a branch to handle the merge conflict:  
  
  {% highlight bash %}
  git checkout -b HR_Department origin/problemBranch
  {% endhighlight %}
4) Merge with the master to see problems: 
  
  {% highlight bash %}
  git merge master
  {% endhighlight %}
  Git ever so kindly automatically modifies the files with conflicts in them to point out where they overlap. (If you want to see a list of the files that have this problem simply run the command `git diff --name-only --diff-filter=U`.)
  
  Now head to the file (or files) in question to see the conflict. 

<div style="text-align: center;">
    <img src = "{{ site.baseurl }}/assets/conflict.png" alt = "conflict" width = "600">
    <br>
</div>

Starting at the `<<<<<<<` we have the line as it is in the new branch and after the `=======` we have the line as it is in the branch we are attempting to merge into, the whole conflict is ended by `>>>>>>>`. 

At this point you must decide which line to keep (, or include both of them) and edit the text manually. 

<div style="text-align: center;">
    <img src = "{{ site.baseurl }}/assets/conflictFixed.png" alt = "conflict" width = "600">
    <br>
</div>

5) Now merge the conflict free branch to master. 
  You can do this in either the github interface (see previous) or the command line. 
  

{% highlight bash %}
git checkout master
git merge --no-ff HR_Department
git push origin master
{% endhighlight %}
  
6) Revel in the seamlessly modified code. 
