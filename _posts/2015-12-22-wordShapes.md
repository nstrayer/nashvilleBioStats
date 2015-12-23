---
layout: post
author: "Nick"
title:  "Shape of Words"
categories: [data visualization, rstats]
tags: [data visualization, rstats, plots]
desc: A random walk style visualization of word and sentence shape. 
---



## Intro: 

After seeing [The Art of Pi](http://www.visualcinnamon.com/portfolio/the-art-in-pi) by Nadieh Bremer. I was inspired to use this random-walk style technique to interrogate the shape of other things. Pretty much every non-real number had already been taken so I figured why not move out of the math domain and into something more... personal. 

Many efforts have been made to look into the "shape" of stories (see the post above this one for my take on that,) but not much attention has been paid to a smaller scale. By converting a word or small sentence to a series of numbers and then generating a walk based upon those numbers we are able to construct a kind of fingerprint for any given set of characters. Keep in mind this fingerprint means absolutely nothing is arbitrary in the most pure sense of the word, but it is interesting none-the-less. 

The plotting and walk code is almost unabashedly wholesale taken from Nadieh Bremer's [github repo](https://github.com/nbremer/artinpi) for her project.

## Converting a string to numbers: 

We will take a string and split it on each letter. From here we will assign a number from 1-26 to each letter corresponding to their position in the alphabet. E.g. a = 1, b = 2, etc. We allow a space to equal 27. 



{% highlight r %}
stringToVec = function(string){
  string = tolower(string) #get all the string to lowercase
  dict = c(letters, c(" "))
  as.vector(sapply(strsplit(string, "")[[1]],  function(d){
    if(d %in% dict){
      return(which(dict == d) ) #ignore non-coded characters
    } else return(0)
    }))
}
# A simple test: 
(test = stringToVec("Hello, World"))
{% endhighlight %}



{% highlight text %}
##  [1]  8  5 12 12 15  0 27 23 15 18 12  4
{% endhighlight %}

Awesome, now that we have that we just need a way to translate these numbers into a random-walk style coordinate list. 


{% highlight r %}
toRW = function(vec){
  
  N <- length(vec)
  Ncols <- 500
  largest <- 27 
  
  x <- y <- rep(NULL, length(vec))
  x[1] <- 0
  y[1] <- 0
  
  #Calculate new position for each digit, based on the position of the old digit and the 
  #angle is determined by the digit itself
  for (i in 2:length(vec)){
      x[i] <- x[(i-1)] + sin((pi*2)*(vec[i]/largest)) #scale by largest observed val
      y[i] <- y[(i-1)] + cos((pi*2)*(vec[i]/largest)) 
  }#for i
  
  #Return the dataframe. 
  data.frame(val=vec, x=x, y=y, ID=1:(length(x)), 
             ColID = 1:(length(x))%%Ncols, stringsAsFactors=F)
}

#again test this to make sure it's working. 
kable(toRW(test))
{% endhighlight %}



| val|          x|          y| ID| ColID|
|---:|----------:|----------:|--:|-----:|
|   8|  0.0000000|  0.0000000|  1|     1|
|   5|  0.9182161|  0.3960798|  2|     2|
|  12|  1.2602363| -0.5436129|  3|     3|
|  12|  1.6022564| -1.4833055|  4|     4|
|  15|  1.2602363| -2.4229981|  5|     5|
|   0|  1.2602363| -1.4229981|  6|     6|
|  27|  1.2602363| -0.4229981|  7|     7|
|  23|  0.4581131|  0.1741605|  8|     8|
|  15|  0.1160929| -0.7655321|  9|     9|
|  18| -0.7499325| -1.2655321| 10|    10|
|  12| -0.4079123| -2.2052247| 11|    11|
|   4|  0.3942108| -1.6080662| 12|    12|




Now make a plot function to draw the "random" walk. 


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

## Now to test it out. 

<div style="text-align: center; font-weight: bold">"Nicholas Strayer"</div>

{% highlight r %}
plotRW(toRW(stringToVec("Nicholas Strayer")))
{% endhighlight %}

<img src="/nashvilleBioStats/figures/source/2015-12-22-wordShapes/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

---

<br>

<div style="text-align: center; font-weight: bold">"Vanderbilt University"</div>
<img src="/nashvilleBioStats/figures/source/2015-12-22-wordShapes/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

---

<br>

<div style="text-align: center; font-weight: bold">"Nashville Tennessee"</div>
<img src="/nashvilleBioStats/figures/source/2015-12-22-wordShapes/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

---

<br>

<div style="text-align: center; font-weight: bold">"Biostatistics are best statistics"  </div>
<img src="/nashvilleBioStats/figures/source/2015-12-22-wordShapes/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />
