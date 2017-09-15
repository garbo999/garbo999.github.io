---
layout: post
title:  "First complete run of the Moses Experiment Management System (EMS)"
date:   2017-08-21 20:46:00 +0300
categories: moses
---
I just finished my first complete run of the [Moses Experiment Management System (EMS)](http://www.statmt.org/moses/?n=FactoredTraining.EMS). Once this system is properly configured and the necessary tools are installed, it only takes a single command to build an MT engine based on a corpus of human translations (source/target files) along with a separate corpus of target language sentences. The overall processing time for my first run was about 4 hours. 

One huge advantage of the EMS is that if a single step fails, the results from the previous steps are automatically reused in subsequent attempts. This saves a lot of time since some of the steps can take hours to run, e.g. TRAINING:run-giza and TRAINING:run-giza-inverse (which I think are used to gather the IBM model data).

Of course, there are a number of problems to overcome to get the EMS up and running the first time. All in all, it took me a couple of weeks of part-time effort to solve the initial problems, most of which came down to figuring out how to install the tools and correctly point to them (as well as correctly formatted versions of the corpora) in the configuration file ([config.toy]((/assets/misc/config.toy))).

Once all of the steps have run (4:30 h in this case with 4 processors and 8 GB RAM), the final EMS step assesses how well the engine is working based on the BLEU score. Here is the initial result:

{% highlight shell %}
gary@moses:~/workspace/experiment$ cat evaluation/report.3
test: 16.66 (1.002) BLEU-c ; 17.69 (1.002) BLEU
{% endhighlight %}

At first glance, this initial score (17.69) does not seem very impressive since the score for the baseline system is 23.5. So now I will analyze why my system is not working as well as the baseline.

## The parallel corpus

The first step is to understand the input data for my experiment. The most critical is the "parallel corpus", i.e. human-translated sentences in the desired language pair (here, German-English). This is defined in the `[CORPUS]` section of the EMS configuration file.

{% highlight conf %}
[CORPUS]
max-sentence-length = 80
[CORPUS:toy]
# toy-data = /home/gary/workspace/corpus/training
raw-stem = $toy-data/news-commentary-v8.de-en 
{% endhighlight %}

Looking in the `/home/gary/workspace/corpus/training` directory, I find these files:

{% highlight shell %}
-rw-r--r-- 1 gary gary 21263333 Dec 30  2012 news-commentary-v8.cs-en.cs
-rw-r--r-- 1 gary gary 19250382 Dec 30  2012 news-commentary-v8.cs-en.en
-rw-r--r-- 1 gary gary 30468440 Dec 30  2012 news-commentary-v8.de-en.de
-rw-r--r-- 1 gary gary 24994800 Dec 30  2012 news-commentary-v8.de-en.en
-rw-r--r-- 1 gary gary 24887237 Dec 30  2012 news-commentary-v8.es-en.en
-rw-r--r-- 1 gary gary 29229103 Dec 30  2012 news-commentary-v8.es-en.es
-rw-r--r-- 1 gary gary 22393874 Dec 30  2012 news-commentary-v8.fr-en.en
-rw-r--r-- 1 gary gary 27496030 Dec 30  2012 news-commentary-v8.fr-en.fr
-rw-r--r-- 1 gary gary 22042729 Dec 30  2012 news-commentary-v8.ru-en.en
-rw-r--r-- 1 gary gary 45130169 Dec 30  2012 news-commentary-v8.ru-en.ru
{% endhighlight %}

Now I can begin to inspect the two files of interest (.de and .en):

{% highlight shell %}
gary@moses:~/workspace/corpus/training$ head -n 10 news-commentary-v8.de-en.*
==> news-commentary-v8.de-en.de <==
SAN FRANCISCO – Es war noch nie leicht, ein rationales Gespräch über den Wert von Gold zu führen.
In letzter Zeit allerdings ist dies schwieriger denn je, ist doch der Goldpreis im letzten Jahrzehnt um über 300 Prozent angestiegen.
Erst letzten Dezember verfassten meine Kollegen Martin Feldstein und Nouriel Roubini Kommentare, in denen sie mutig die vorherrschende optimistische Marktstimmung hinterfragten und sehr überlegt auf die Risiken des Goldes  hinwiesen.
Und es kam, wie es kommen musste.
Seit der Veröffentlichung ihrer Artikel ist der Goldpreis noch weiter gestiegen.
Jüngst erreichte er sogar ein Rekordhoch von 1.300 Dollar.
Im letzten Dezember argumentierten die Goldbugs, dass der Preis zweifellos in Richtung 2.000 Dollar gehen würde.
Beflügelt aufgrund des anhaltenden Aufwärtstrends, meint man nun mancherorts, dass Gold sogar noch höher steigen könnte.
Ein erfolgreicher Gold-Investor erklärte mir vor kurzem, dass die Aktienkurse über ein Jahrzehnt dahingedümpelt waren, bevor der Dow Jones-Index in den frühen 1980er Jahren die Marke von 1.000 Punkten überschritt.
Seit damals ist er auf über 10.000 Punkte gestiegen.

==> news-commentary-v8.de-en.en <==
SAN FRANCISCO – It has never been easy to have a rational conversation about the value of gold.
Lately, with gold prices up more than 300% over the last decade, it is harder than ever.
Just last December, fellow economists Martin Feldstein and Nouriel Roubini each penned op-eds bravely questioning bullish market sentiment, sensibly pointing out gold’s risks.
Wouldn’t you know it?
Since their articles appeared, the price of gold has moved up still further.
Gold prices even hit a record-high $1,300 recently.
Last December, many gold bugs were arguing that the price was inevitably headed for $2,000.
Now, emboldened by continuing appreciation, some are suggesting that gold could be headed even higher than that.
One successful gold investor recently explained to me that stock prices languished for a more than a decade before the Dow Jones index crossed the 1,000 mark in the early 1980’s.
Since then, the index has climbed above 10,000.
{% endhighlight %}

Clearly, someone is interested in gold (ah, 2008...)! This is a news database, obviously, but hopefully with more than just gold discussion. To be sure, I will check the end of these two files:

{% highlight shell %}
gary@moses:~/workspace/corpus/training$ tail -n 10 news-commentary-v8.de-en.*
==> news-commentary-v8.de-en.de <==
In Zeiten des Abschwungs kann sich das immense Kriminalitätsproblem des Landes nur verschärfen. Das gilt auch für die Arbeitslosigkeit, die im Bereich der offiziellen Wirtschaft bereits über 20 Prozent liegt.
Zuma weiß um die Dringlichkeit der Situation.
Immerhin ist er 67 Jahre alt und wird wahrscheinlich nur eine Amtszeit dienen. „Wir können uns keine Zeitverschwendung leisten“, sagt er.
Dem politischen Ökonomen Moeletsi Mbeki zufolge, ist Zuma im Grunde seines Herzens „ein Konservativer“.  In diesem Sinne vertritt Zuma das Südafrika von gestern.
Er ist Mitglied einer stolzen Generation, die die Apartheid bezwang – und der anschließend ein friedlicher Übergang zu einer schwarzen Mehrheitsregierung gelang.
Das bleibt eine der größten Errungenschaften in der jüngeren Geschichte.
Gleichzeitig scheint sich Zumas revolutionäre Generation mit der Führung Südafrikas in der nun seit 15 Jahren dauernden Ära nach der Apartheid noch immer unwohl zu fühlen.
In einer Region, wo die älteren Menschen sehr verehrt werden, muss Zumas Bindung an landestypische Traditionen eine gleichwertige Offenheit gegenüber den Bedürfnissen der Jugend des Landes gegenüberstehen.
Drei von zehn Südafrikanern sind jünger als 15 und das bedeutet, dass sie nicht einen Tag unter der Apartheid gelebt haben.
Irgendwie muss Zuma einen Weg finden, einerseits das Engagement seiner Generation hinsichtlich ethnischer Gerechtigkeit und nationaler Befreiung zu würdigen und andererseits den Massen, die täglich unter Klassenunterschieden leiden und sich nach materiellen Verbesserungen sehnen, mehr Mitwirkungsmöglichkeiten einzuräumen.

==> news-commentary-v8.de-en.en <==
In an economic slowdown, the country’s severe crime problem might only worsen; so might unemployment, which already tops 20% in the formal economy.
Zuma senses the urgency of the situation.
He is, after all, 67 years old and likely to serve only a single term in office. “We can’t waste time,” he says.
Yet, according to the political economist Moeletsi Mbeki, at his core, “Zuma is a conservative.” In this sense, Zuma represents yesterday’s South Africa.
He is part of the proud generation that defeated apartheid – and then peacefully engineered a transition to durable black-majority rule.
Their achievement remains one of the greatest in recent history.
At the same time, Zuma’s revolutionary generation still seems uneasy leading South Africa in a post-apartheid era that is now 15 years old.
In a region that reveres the elderly, Zuma’s attachment to his rural traditions must be matched by an equal openness to the appetites of the country’s youth.
Three in ten South Africans are younger than 15, meaning that they did not live a day under apartheid.
Somehow Zuma must find a way to honor his own generation’s commitment to racial justice and national liberation, while empowering the masses who daily suffer the sting of class differences and yearn for material gain.
{% endhighlight %}

OK, we start with gold and we end in South Africa... 

Now another obvious question is how many sentence pairs we have:

{% highlight shell %}
gary@moses:~/workspace/corpus/training$ wc -l news-commentary-v8.de-en.*
  178221 news-commentary-v8.de-en.de
  178221 news-commentary-v8.de-en.en
{% endhighlight %}

So it looks like we have 178221 German sentences and 178221 English sentences (1 sentence per line and each file has 178221 lines), and in an ideal world they are perfect translations of one another. Let's look at one sentence pair to see what we find:

German: 
> Erst letzten Dezember verfassten meine Kollegen Martin Feldstein und Nouriel Roubini Kommentare, in denen sie mutig die vorherrschende optimistische Marktstimmung hinterfragten und sehr überlegt auf die Risiken des Goldes  hinwiesen.

English: 
> Just last December, fellow economists Martin Feldstein and Nouriel Roubini each penned op-eds bravely questioning bullish market sentiment, sensibly pointing out gold’s risks.

This is just one random sentence out of 178221, but I can't complain about the quality. I can't even say for sure which language is the source language, although `fellow economists ... each penned op-eds` sounds like such idiomatic (American?) English that I would guess that English is the source language.

## The language model

Now I will move on to the next section: the language model. This is defined in the `[LANGUAGE MODEL]` of the configuration. The language model should assign a higher probability to idiomatic sentences in the target language (e.g. `This is a plausible sentence` should be more probable than `Plausible is a this sentence`). Therefore, the language model only uses a target language corpus (in this case, English).

Here is the main content of the `[LM]` section of my configuration file:

{% highlight conf %}
[LM]
# order of the language model
order = 5

[LM:toy]
### raw corpus (untokenized)
raw-corpus = $toy-data/news-commentary-v8.de-en.$output-extension
{% endhighlight %}

~~One important detail is that the language model is normally based on a DIFFERENT set of sentences than the parallel corpus. Here, I have defined the language model based on the SAME file as the parallel corpus (`news-commentary-v8.de-en.en`)! Since this clearly looks like a mistake, I am going to rerun my experiment and compare the results.~~ 

Hmmmmmm, closer examination of the sample EMS configuration files provided in `mosesdecoder/scripts/ems/example/` suggests this last statement is incorrect. I am going to move forward now and look for other problems (of which there appear to be plenty).

Here is the [follow-up to this post]({% post_url 2017-08-23-binarise-phrase-table-lexicalised-reordering-models %}).