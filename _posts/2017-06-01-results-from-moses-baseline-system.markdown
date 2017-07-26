---
layout: post
title:  "MT output from the Moses baseline system"
date:   2017-06-01 18:21:00 +0300
categories: moses
---
I now have the [Moses baseline system][MBS] working on a small virtual machine (8 GB RAM and 2 cores). I was interested to see just how well an MT engine of this sort (trained with only about 130k sentence pairs) would work on real-world data. For my test, I randomly picked a selection of French sentences from online news and translated them with the baseline system. Here are the results (along with some corrections and comments).

Note that the baseline system requires an input sentence that is slightly edited from the normal human sentence. For example, punctuation is treated as separate words, so *l'article* (which I normally think of as two words) becomes three separate words: *l ' article*.

### Example 1

French: 
> faire revenir les militants sur le terrain et convaincre que le vote est utile .

English: 
> bring activists on the ground and convince that the vote is useful .

This is the example sentence from the [Moses baseline system][MBS]. Actually, it is a sentence fragment and not a proper sentence. I included it to make sure that my baseline system produces the same output as the system described online.

### Example 2

French: 
> Les États-Unis et la Chine préparent une nouvelle résolution pour d'éventuelles mesures punitives supplémentaires contre Pyongyang.

English: 
> The United States and China are preparing a new resolution to d'éventuelles additional punitive measures against Pyongyang.

Except for the untranslated *d'éventuelles*, this is a reasonable translation. I would write something like *The United States and China are preparing a new resolution on possible further punitive measures against Pyongyang*.

### Example 3

French: 
> François Bayrou , le nouveau ministre de la justice , avait pris le jour de sa nomination « l ’ engagement » que le projet de loi sur la moralisation de la vie publique serait sur la table du conseil des ministres avant les élections législatives .

English: 
> François Bayrou , the new Minister of justice , had taken the day his appointment “ engagement ” that the bill on the moralizing public life would be on the table of the Council of Ministers before the parliamentary elections .

Now that I actually understand this sentence (searching [Linguee] for *la moralisation de la vie publique* pulls up plenty of odd human translations), the machine translation seems almost comprehensible. I have to wonder why *l’engagement* is in quotes.

My translation: *On the day he was appointed, the new Justice Minister François Bayrou "committed" to ensuring that the law on accountability in public life is on the table of the Council of Ministers prior to the parliamentary elections.*

### Example 4

French: 
> Le périmètre de cette réforme fondatrice du quinquennat d ’ Emmanuel Macron est désormais fixé après une dernière réunion d ’ arbitrage entre le président de la République et son premier ministre, Edouard Philippe, dimanche 28 mai .

English: 
> The perimeter reform of the founding of quinquennat of Immanuel Macron is now set after a last meeting of trade-off between the president of the Republic and his first ministre, Edouard Philippe, Sunday 28 May .

Funny to see *Emmanuel* translated (*Immanuel*). Proper nouns cause many headaches for MT. *quinquennat* looks very out of place. I translated it as *5-year term*. I don't think this MT output is understandable without seeing the French.

My translation (maybe incorrect due to my ignorance of French political life): *The outline of this seminal reform for Emmanuel Macron's 5-year term is now set after a final arbitration meeting between the President of the Republic and and his Prime Minister, Edouard Philippe on Sunday 28 May.*

### Example 5

French: 
> Alors que l ’ affaire Ferrand vient brouiller le message gouvernemental, le ministre de la justice préfère garder le silence pour le moment .

English: 
> As the case Ferrand comes confuse the message gouvernemental, the Justice Minister prefers to keep silent for the moment .

The beginning is awkward but this is mostly understandable. 

My translation: *With the Ferrand affair blurring the government's message, the Justice Minister prefers to keep silent for the moment.*

### Example 6

French: 
> Quinze jours avant le bac, les candidats peuvent discuter en direct, lors de Facebook Live et de tchats, avec des professeurs de différentes matières pour se préparer au mieux .

English: 
> 15 days before the bac, candidates can discuss in direct, during Facebook live and tchats, with professors of different materials to prepare for the better .

This translation seems nearly comprehensible. One problem that stands out is *to prepare for the better*.

My translation: *15 days before their baccalauréat examination, candidates can talk to teachers of different subjects via Facebook Live and chat in order to better prepare for the exam.*

### Example 7

French: 
> A la veille de la présidentielle , des journalistes du « Monde » revisitent les lieux qui ont marqué leur jeunesse pour raconter , à la première personne, comment leur « petite France » s ’ est transformée .

English: 
> On the eve of the presidency , journalists , the “ World ” revisitent the places that have marked their youth to tell , to the first personne, how their “ small ” France became .

This is an ugly translation with mistakes and some common words left untranslated. I doubt whether the output is understandable without a knowledge of French. 

Just what *petite France* means here is hard to tell without more context, but I assume it is not a neighborhood in Strasbourg. Could a computer ever come up with a reasonable translation of such a complex sentence?

My translation: *On the eve of the presidential election, a group of journalists from Le Monde revisit places that shaped their youth to provide first-person accounts of how the France they knew when they were young has been transformed.* 

### Summary

All in all, I think it is not a bad result for a small MT engine. Now I am curious to see what I can do with a larger data set.

[MBS]: http://www.statmt.org/moses/?n=Moses.Baseline
[Linguee]: http://www.linguee.com/english-french/search?source=auto&query=moralisation+de+la+vie+publique
