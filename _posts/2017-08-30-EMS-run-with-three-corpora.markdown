---
layout: post
title:  "EMS run with three corpora"
date:   2017-08-30 10:52:00 +0300
categories: moses
---
I went to bed and the Moses EMS was running on my server but it crashed in the night. Here is the end of the last error file `~/workspace/experiment/steps/4/TRAINING_run-giza.4.STDERR` that was generated:

{% highlight shell %}
...
line 723000
ERROR: Execution of: /home/gary/workspace/bin/training-tools/mgizapp/snt2cooc /home/gary/workspace/experiment/training/giza.4/en-de.cooc /home/gary/workspace/experiment/training/prepared.4/de.vcb /home/gary/workspace/experiment/training/prepared.4/en.vcb /home/gary/workspace/experiment/training/prepared.4/en-de-int-train.snt
  died with signal 9, without coredump
{% endhighlight %}

Although there is no explicit error message that says that Moses ran out of memory, a quick search of the [Moses mailing list](https://www.mail-archive.com/moses-support@mit.edu/msg13307.html) for `died with signal 9, without coredump` suggests that is what happened. I am going to modify my EMS config file as described there. An important note in a [follow-up message](https://www.mail-archive.com/moses-support@mit.edu/msg13308.html) is that the EMS needs to be run this time with `-continue=1` (and NOT simply restarted). Let's see if this works. Here is the modified line in the config file:

{% highlight shell %}
training-options = "-mgiza -mgiza-cpus 4 -snt2cooc snt2cooc.pl"
{% endhighlight %}

By the way, Moses also updated the graphical plan of action before shutting down. Here is the latest file with the crashed steps in red (and the remaining steps in green):

![Moses graphical plan of action 'graph.4.png'](/assets/img/graph.4.png)


