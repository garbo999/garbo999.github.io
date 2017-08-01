---
layout: post
title:  "Trouble installing SRILM on Ubuntu 16.04.2 LTS"
date:   2017-07-17 09:11:00 +0300
categories: moses, srilm
---
I had trouble installing [SRILM](http://www.speech.sri.com/projects/srilm/), which is needed for Experiment.perl (the automated way to run experiments in Moses). Thankfully I got a quick response to my query on the [SRILM mailing list](http://mailman.speech.sri.com/pipermail/srilm-user/2017q3/001763.html).

Here is the error I was getting:

{% highlight shell %}
unknown:~/workspace/srilm> make World
make: execvp: /home/gary/workspace/srilm: Permission denied
make: *** /home/gary/workspace/srilm: Is a directory.  Stop.
{% endhighlight %}

Essentially a permissions problem!? I got an email response from Andreas Stolcke at UC Berkeley:

> What does `make -n World` say?

> Also, try setting SRILM on the command line:

{% highlight shell %}
make SRILM=$cwd World 
{% endhighlight %}

The latter suggestion worked and SRILM is now compiled on my machine. Not sure why it didn't work before because I was setting $SRILM in the Makefile.


