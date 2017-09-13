---
layout: post
title:  "EMS run with three corpora (part 2)"
date:   2017-09-13 11:07:00 +0300
categories: moses
---
I was working on running the [EMS with three corpora]({% post_url 2017-08-30-EMS-run-with-three-corpora %}) in late August before I went to the ATICOM post-editing course. The EMS crashed again and I wasn't able to finish. The crash occurred in the `TRAINING:build-ttable` step. The cause of the crash was not immediately clear from the error output (excerpt below):

{% highlight shell %}
...
Loading lexical translation table from /home/gary/workspace/experiment/model/lex.9.f2e.........................................................................................................................................................Killed
{% endhighlight %}

However, once again, it seems plausible that Moses used up the machine's RAM and the offending process was killed by the operating system. I was able to add another 8GB RAM to my virtual machine (total = 16GB) and reran the EMS. I also doubled the number of cores from 4 to 8 and it looks like the EMS at least partially took advantage of that. The tuning step took up 4 hours and it took 13 hours to finish up, but now I have a new engine to test.

First, the scores for the engine:

{% highlight shell %}
test: 18.20 (1.018) BLEU-c ; 19.25 (1.018) BLEU
{% endhighlight %}

These BLEU scores are almost 2 points higher than [my first EMS run](% post_url 2017-08-21-first-complete-run-moses-experiment-management-system %) with only one corpus. But still not near the results from the [baseline system](http://www.statmt.org/moses/?n=Moses.Baseline) (which might have been cherry-picked for the good scores!?).

I will post the final Moses graph here for the sake of curiosity:

![Moses graphical plan of action 'graph.4.png'](/assets/img/graph.11.png)


