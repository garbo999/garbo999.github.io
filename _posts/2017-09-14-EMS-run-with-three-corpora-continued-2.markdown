---
layout: post
title:  "EMS run with three corpora (part 3)"
date:   2017-09-14 15:28:00 +0300
categories: moses
---
I finally finished my EMS run with three corpora (see [part 1]({% post_url 2017-08-30-EMS-run-with-three-corpora %}) and [part 2]({% post_url 2017-09-13-EMS-run-with-three-corpora-continued %})). I want briefly compare this run with the [previous run]({% post_url 2017-08-21-first-complete-run-moses-experiment-management-system %}) that used less data and see if there is a visible improvement in translation quality. 

### Comparison of EMS runs

First, the BLEU scores:

* Previous run: test: 16.66 (1.002) BLEU-c ; 17.69 (1.002) BLEU
* Current run: test: 18.20 (1.018) BLEU-c ; 19.25 (1.018) BLEU

Next, the source corpora sizes:

* Previous run: 178221 sentence pairs (source: news-commentary-v8)
* Current run: 2848113 sentence pairs (= 178221 + 2399123 + 270769) (sources: news-commentary-v8 + commoncrawl + news-commentary-v12)

So the current run represents an increase in sentence pairs by a factor of about 16. However, the quality of the sources might not be so good. I will look at some actual sentences in the corpora a bit later.

### Translation output from the new engine

#### Example 1

German: 
> 96 Prozent der Muslime fühlen sich Deutschland verbunden

English: 
> 96 % of the Muslims feel connected to Germany (previously: 96 % of the Muslims feel connected Germany)

Only a small improvement, but now the translation is almost perfect. I would delete *the* from *the Muslims*.

#### Example 2

German: 
> Wanderer nach gewaltigem Bergsturz in der Schweiz vermisst

English: 
> Hikers after massive landslide in Switzerland missing (previously: Wanderer after massive Bergsturz in Switzerland missing)

A major improvement with two previously unknown words now translated. The only problem is that *missing* is in the wrong place.

#### Example 3

German: 
> Ein gewaltiger Felsabbruch hat acht Deutsche, Österreicher und Schweizer beim Wandern in den Schweizer Alpen überrascht.

English: 
> A huge Felsabbruch has eight Deutsche, Austrian and Swiss hiking in the Swiss Alps by surprise (previously: One powerful Felsabbruch has eight Germans, Austrians, and Swiss in Wandern in the Swiss Alps by surprise.)

*Felsabbruch* and *Deutsche* are still unknown words here but the translated did improve somewhat.

#### Example 4 (new from [www.tagesschau.de](http://www.tagesschau.de))

German: 
> Die Liste der Bieter für die insolvente Air Berlin hat sich stetig erweitert.

English: 
> The list of the bidders for the insolvent Air Berlin has always expanding.

(adequacy = 5/5, fluency = 2/5)

#### Example 5 (new)

German: 
> Bei vielen dürfte jedoch die finanzielle Voraussetzung fehlen.

English: 
> For many, however, is likely to the financial condition are missing. (my translation: "Many, however, do not satisfy the financial conditions.")

(adequacy = 4/5, fluency = 2/5)

#### Example 6 (new)

German: 
> Im Erfolgsfall hätte sie jedoch wohl mit kartellrechtlichen Auflagen zu rechnen und müsste einige Slots abgeben.

English: 
> In the case of , however , it would have to reckon with anti-trust requirements and would have to make some slots 

(My translation: "If it succeeded, however, it would have to reckon with anti-trust requirements and give up some slots.")

(adequacy = 3/5, fluency = 2/5)

#### Example 7 (new)

German: 
> Nordkorea hat südkoreanischen Angaben zufolge ein zunächst nicht weiter zu identifizierendes Geschoss aus seiner Hauptstadt Pjöngjang über Japan hinweg abgefeuert.

English: 
> North Korea has South Korean information, not a first to further identifizierendes floor from its capital Pyongyang about Japan fired across. 

(My translation: "According to South Korean sources, North Korea has fired a missle that has not yet been further identified from its capital Pyongyang over Japan.")

(adequacy = 3/5, fluency = 1/5)

### EMS run with three corpora
- [Part 1]({% post_url 2017-08-30-EMS-run-with-three-corpora %})
- [Part 2]({% post_url 2017-09-13-EMS-run-with-three-corpora-continued %})
- Part 3 (this post)
