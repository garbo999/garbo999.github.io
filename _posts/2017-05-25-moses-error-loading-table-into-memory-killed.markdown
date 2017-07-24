---
layout: post
title:  "Loading table into memory...Killed"
date:   2017-05-25 12:31:44 +0300
categories: moses
---
I initially tested Moses on a small virtual machine with only 2 GB RAM and 2 cores. I was able to train the system in less than 2 hours, but when I tried to translate a sentence, the decoder would crash.

{% highlight shell %}
...Loading table into memory...Killed
{% endhighlight %}


I eventually discovered (by running `top` simultaneously in another shell window) that the error was just a lack of memory. Once I switched to a larger machine (8 GB RAM), the error went away. 8 GB seems like a minimum to translate with Moses.