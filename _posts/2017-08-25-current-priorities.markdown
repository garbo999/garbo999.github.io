---
layout: post
title:  "Current priorities"
date:   2017-08-25 12:23:00 +0300
categories: moses
---
## Current priorities

Now that I have got the [EMS running]({% post_url 2017-08-21-first-complete-run-moses-experiment-management-system %}), I am hoping to move forward with some more interesting experiments. Here are some of the things I want to get done this week:

* Run the EMS with a larger German-English data set

  I ran my first experiment with only about 178k sentence pairs. Now I want to try a substantially larger data set. The "news" data set for the [WMT 17 machine translation conference](http://www.statmt.org/wmt17/translation-task.html) seems like a good choice.

* Get the EMS graph generation working 

  The graphics are the last piece of the EMS that is not working as of today. The problem must be related to the fact that my test machine doesn't have a GUI (Ubuntu Server). ([DONE]({% post_url 2017-08-29-ems-graphics %}))

* Set up the EMS web interface

  I need to set up a LAMP server to see all the nice statistics that the EMS generates.

* Try out Moses server and see if it can interface with SDL Trados 2017

  I am going to a [post-editing class](http://aticom.de/aktuelle-termine/workshop-pe-mue-apps./) next week in Düsseldorf hosted by [ATICOM](http://aticom.de/) and I am hoping to get this done ~~beforehand~~ relatively soon. If no one has built a Moses/SDL interface yet, it could be interesting to give it a try. The SDL Translation Memory API would probably allow that.

