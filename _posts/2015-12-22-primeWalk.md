---
layout: post
author: "Nick"
title:  "Shape of Primes"
date: 2015-12-22 05:00:00
categories: [data visualization, rstats]
tags: [data visualization, rstats, plots, random-walk]
desc: A random walk style visualization of the first 100,000 prime numbers. 
---



## Intro: 

See [this]({{site.baseurl}}/2015/12/storyShapes.html) and [this]({{site.baseurl}}/2015/12/wordShapes.html) post for my intro on random-walk style visualizations. 

## Prime Numbers: 

Let's try a random-walk based upon the first 100,000 prime numbers. 

First we load in the data obtained from [here](https://www.mathsisfun.com/numbers/prime-number-lists.html).


{% highlight r %}
primes = read.table("/Users/Nick/Dropbox/vandy/musicCityStats/nashvilleBioStats/assets/primes-to-100k.txt")$V1
{% endhighlight %}

Now, as before, we translate these values into random-walk coordinates. 


{% highlight r %}
toRW = function(vec){
  
  N <- length(vec)
  Ncols <- 500
  
  x <- y <- rep(NULL, length(vec))
  x[1] <- 0
  y[1] <- 0
  
  #Calculate new position for each digit, based on the position of the old digit and the 
  #angle is determined by the digit itself
  for (i in 2:length(vec)){
      x[i] <- x[(i-1)] + sin( vec[i] ) #scale by largest observed val
      y[i] <- y[(i-1)] + cos( vec[i] ) 
  }#for i
  
  #Return the dataframe. 
  data.frame(val=vec, x=x, y=y, ID=1:(length(x)), 
             ColID = 1:(length(x))%%Ncols, stringsAsFactors=F)
}

#again test this to make sure it's working. 
kable(head(toRW(primes)))
{% endhighlight %}



| val|          x|          y| ID| ColID|
|---:|----------:|----------:|--:|-----:|
|   2|  0.0000000|  0.0000000|  1|     1|
|   3|  0.1411200| -0.9899925|  2|     2|
|   5| -0.8178043| -0.7063303|  3|     3|
|   7| -0.1608177|  0.0475719|  4|     4|
|  11| -1.1608079|  0.0519976|  5|     5|
|  13| -0.7406408|  0.9594444|  6|     6|




Make a plot function to draw the walk. 


{% highlight r %}
plotRW <- function(rwData){
  rwPlot <- ggplot(rwData, aes(x=x, y=y, group="1")) +
    geom_path(aes(color = factor(val)), size=0.5) + 
    coord_fixed(ratio = 1) + 
    theme_bw() +
    theme(line = element_blank(),
          text = element_blank(),
          line = element_blank(),
          title = element_blank(),
          legend.position="none",
          panel.border = element_blank(),
          panel.background = element_blank())
  return(rwPlot)
}
{% endhighlight %}

## Plot it! 

<div style="text-align: center; font-weight: bold">The First 100,000 Primes</div>

{% highlight r %}
plotRW(toRW(primes))
{% endhighlight %}

<img src="/nashvilleBioStats/figures/source/2015-12-22-primeWalk/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />
