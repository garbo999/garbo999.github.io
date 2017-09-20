---
layout: post
title:  "EMS run with three corpora (part 2)"
date:   2017-09-13 11:07:00 +0300
categories: moses
---
I was trying to run the [EMS with three corpora]({% post_url 2017-08-30-EMS-run-with-three-corpora %}) in late August before I went to the ATICOM post-editing course. The EMS crashed again and I wasn't able to finish. The crash occurred in the `TRAINING:build-ttable` step. The cause of the crash was not immediately clear from the error output (excerpt below):

{% highlight shell %}
...
Loading lexical translation table from /home/gary/workspace/experiment/model/lex.9.f2e.........................................................................................................................................................Killed
{% endhighlight %}

Once again, it seemed likely that Moses used up the machine's RAM and the offending processes were killed by the operating system. 

Thankfully, I was able to add another 8GB RAM to my virtual machine (total = 16GB) and reran the EMS. The number of cores also doubled from 4 to 8 and it looks like the EMS at least partially took advantage of that. Just the tuning step took 4 hours, and overall it took 13 hours to finish up this EMS run (after spending half a day just running the GIZA and reverse GIZA steps), but now I have a new engine to test.

First, the scores for the engine:

{% highlight shell %}
test: 18.20 (1.018) BLEU-c ; 19.25 (1.018) BLEU
{% endhighlight %}

These BLEU scores are almost 2 points higher than [my first EMS run]({% post_url 2017-08-21-first-complete-run-moses-experiment-management-system %}) with only one corpus. But still nowhere near the results from the [baseline system](http://www.statmt.org/moses/?n=Moses.Baseline). Perhaps the baseline system (French-English) was cherry-picked for the good scores!?

Here is the final Moses graph from this EMS run:

![Moses graphical plan of action 'graph.4.png'](/assets/img/graph.11.png)


