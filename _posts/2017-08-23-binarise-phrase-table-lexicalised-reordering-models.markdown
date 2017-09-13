---
layout: post
title:  "Binarise phrase table and lexicalised reordering models"
date:   2017-08-23 15:11:01 +0300
categories: moses
---
After running the Experiment Management System for the 1st time, I wanted to translate ("decode") some German sentences to see with my own eyes how well my MT engine is working. Here I got an error when attempting to run the Moses decoder:

{% highlight shell %}
gary@moses:~/workspace/experiment$ ~/workspace/mosesdecoder/bin/moses -tt -f ~/workspace/experiment/tuning/moses.tuned.ini.3
Defined parameters (per moses.ini or switch):
  config: /home/gary/workspace/experiment/tuning/moses.tuned.ini.3 
  distortion-limit: 6 
  feature: UnknownWordPenalty WordPenalty PhrasePenalty PhraseDictionaryMemory name=TranslationModel0 num-features=4 path=/home/gary/workspace/experiment/model/phrase-table.1 input-factor=0 output-factor=0 LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/gary/workspace/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz Distortion KENLM name=LM0 factor=0 path=/home/gary/workspace/experiment/lm/toy.binlm.1 order=5 
  input-factors: 0 
  mapping: 0 T 0 
  report-segmentation-enriched: 
  weight: LexicalReordering0= 0.0130405 0.0270736 0.0841264 0.12192 0.0283947 0.126977 Distortion0= 0.0401765 LM0= 0.0873653 WordPenalty0= -0.261099 PhrasePenalty0= -0.00783908 TranslationModel0= 0.0497548 0.0444622 0.0783108 0.0294595 UnknownWordPenalty0= 1 
line=UnknownWordPenalty
FeatureFunction: UnknownWordPenalty0 start: 0 end: 0
line=WordPenalty
FeatureFunction: WordPenalty0 start: 1 end: 1
line=PhrasePenalty
FeatureFunction: PhrasePenalty0 start: 2 end: 2
line=PhraseDictionaryMemory name=TranslationModel0 num-features=4 path=/home/gary/workspace/experiment/model/phrase-table.1 input-factor=0 output-factor=0
FeatureFunction: TranslationModel0 start: 3 end: 6
line=LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/gary/workspace/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz
Initializing Lexical Reordering Feature..
FeatureFunction: LexicalReordering0 start: 7 end: 12
line=Distortion
FeatureFunction: Distortion0 start: 13 end: 13
line=KENLM name=LM0 factor=0 path=/home/gary/workspace/experiment/lm/toy.binlm.1 order=5
FeatureFunction: LM0 start: 14 end: 14
Loading UnknownWordPenalty0
Loading WordPenalty0
Loading PhrasePenalty0
Loading LexicalReordering0
Loading table into memory...done.
Loading Distortion0
Loading LM0
Loading TranslationModel0
Can't read /home/gary/workspace/experiment/model/phrase-table.1
{% endhighlight %}

The error message at the end is `Can't read /home/gary/workspace/experiment/model/phrase-table.1`.

Here is a look at the directory in question (`~/workspace/experiment/model/`).

{% highlight shell %}
gary@moses-working-6aug17-8gb-fra1-01:~/workspace/experiment$ ll model/
total 568492
drwxrwxr-x  2 gary gary      4096 Aug 21 13:37 ./
drwxrwxr-x 10 gary gary      4096 Aug 23 10:41 ../
-rw-rw-r--  1 gary gary  24393417 Aug 19 14:45 aligned.1.grow-diag-final-and
-rw-rw-r--  1 gary gary  82204905 Aug 19 14:46 extract.1.inv.sorted.gz
-rw-rw-r--  1 gary gary  76467926 Aug 19 14:46 extract.1.o.sorted.gz
-rw-rw-r--  1 gary gary  83095290 Aug 19 14:46 extract.1.sorted.gz
-rw-rw-r--  1 gary gary  22205033 Aug 19 14:45 lex.1.e2f
-rw-rw-r--  1 gary gary  22205033 Aug 19 14:45 lex.1.f2e
-rw-rw-r--  1 gary gary      1100 Aug 19 14:52 moses.ini.1
-rw-rw-r--  1 gary gary 191756138 Aug 19 14:51 phrase-table.1.gz
-rw-rw-r--  1 gary gary  79781219 Aug 19 14:47 reordering-table.1.wbe-msd-bidirectional-fe.gz
{% endhighlight %}

The only phrase table here is `phrase-table.1.gz` and not `phrase-table.1`. In fact, it is a monster file with about 192 MB! 

The key is that the phrase table needs to be converted into another format so it can be partially read into memory. This is explained [here](http://www.statmt.org/moses/?n=Moses.Baseline) (under *Testing*) and [here](http://www.statmt.org/moses/?n=Advanced.RuleTables#ntoc3) (under *Compact Phrase Table*).

To make this step, I had to recompile Moses with the `--with-cmph` option since a quick check with `moses -h` shows that *PhraseDictionaryCompact* is missing from the available *Available feature functions*. 

{% highlight shell %}
gary@moses:~/workspace$ ./mosesdecoder/bin/moses -h
...
Available feature functions:
BleuScoreFeature ConstrainedDecoding ControlRecombination CorrectionPattern CountNonTerms CoveredReferenceFeature DeleteRules DesegModel Distortion DynamicCacheBasedLanguageModel EditOps ExampleLM ExamplePT ExampleStatefulFF ExampleStatelessFF ExampleTranslationOptionListFeature Generation GlobalLexicalModel HyperParameterAsWeight InMemoryPerSentenceOnDemandLM InputFeature KENLM LexicalReordering MaxSpanFreeNonTermSource Model1Feature NieceTerminal OpSequenceModel PhraseBoundaryFeature PhraseDictionaryALSuffixArray PhraseDictionaryBinary PhraseDictionaryDynamicCacheBased PhraseDictionaryFuzzyMatch PhraseDictionaryGroup PhraseDictionaryMemory PhraseDictionaryMemoryPerSentence PhraseDictionaryMemoryPerSentenceOnDemand PhraseDictionaryMultiModel PhraseDictionaryMultiModelCounts PhraseDictionaryOnDisk PhraseDictionaryScope3 PhraseDictionaryTransliteration PhraseDistanceFeature PhraseLengthFeature PhraseOrientationFeature PhrasePairFeature PhrasePenalty ProbingPT ReferenceComparison ReloadingLM RulePairUnlexicalizedSource RuleScope RuleTable SRILM SetSourcePhrase SoftMatchingFeature SoftSourceSyntacticConstraintsFeature SourceGHKMTreeInputMatchFeature SourceWordDeletionFeature SpanLength SparseHieroReorderingFeature SyntaxInputWeight SyntaxRHS TargetBigramFeature TargetConstituentAdjacencyFeature TargetNgramFeature TargetPreferencesFeature TargetWordInsertionFeature TreeStructureFeature UnalignedWordCountFeature UnknownWordPenalty WordPenalty WordTranslationFeature
{% endhighlight %}

Note: It is necessary to install the [cmph](http://cmph.sourceforge.net/) hashing library before compiling Moses.

Here is the command I used to compile Moses (I had to add the `-a` option because otherwise Moses was compiled without the cmph library; I found that suggestion [here](http://moses-support.mit.narkive.com/9UOIYHZZ/phrasedictionarycompact-problem), but I am not sure why it works):

{% highlight shell %}
./bjam -a --with-boost=/home/gary/workspace/boost_1_60_0  
  \ --with-srilm=/home/gary/workspace/srilm 
  \ --with-cmph=/home/gary/workspace/cmph/cmph-2.0 -j4
{% endhighlight %}

Now I need to convert my monster phrase table to the compact format. Here is the command I used:

{% highlight shell %}
nohup nice ~/workspace/mosesdecoder/bin/processPhraseTableMin \
   -in ~/workspace/experiment/model/phrase-table.1.gz -nscores 4 \
   -out ~/workspace/experiment/model/phrase-table
{% endhighlight %}

Note: A `-threads` parameter can also be added, e.g. `-threads 4`. The full list of options is [here](http://www.statmt.org/moses/?n=Advanced.RuleTables).

Note that `nohup nice` is good to use here in case the conversion takes a long time and the shell connecting to the remote server crashes. The conversion process continues in the background even if the shell is closed.

Next comes a similar conversion for the reordering table:

{% highlight shell %}
nohup nice ~/workspace/mosesdecoder/bin/processLexicalTableMin \
   -in ~/workspace/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz \
   -out ~/workspace/experiment/model/reordering-table
{% endhighlight %}

Note: Like with `processPhraseTableMin`, I also tried adding `-threads 8` for `processLexicalTableMin` and it worked.

The next question I had was which `moses.ini` file to use (since the EMS generated several plausible files). A quick [question on the mailing list](https://www.mail-archive.com/moses-support@mit.edu/msg15552.html) suggested that `./tuning/moses.tuned.ini.3` is correct (or `./tuning/moses.tuned.ini.1`, which must have come from an earlier tuning run).

So now I modify `./tuning/moses.tuned.ini.3` as described [here](http://www.statmt.org/moses/?n=Moses.Baseline) under *Testing*. The original values from `./tuning/moses.tuned.ini.3` are commented out:

{% highlight shell %}
...
# PhraseDictionaryMemory name=TranslationModel0 num-features=4 path=/home/gary/workspace/experiment/model/phrase-table.1 input-factor=0 output-factor=0
PhraseDictionaryCompact name=TranslationModel0 num-features=4 path=/home/gary/workspace/experiment/model/phrase-table.minphr input-factor=0 output-factor=0
...
#LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/gary/workspace/experiment/model/reordering-table.1.wbe-msd-bidirectional-fe.gz
LexicalReordering name=LexicalReordering0 num-features=6 type=wbe-msd-bidirectional-fe-allff input-factor=0 output-factor=0 path=/home/gary/workspace/experiment/model/reordering-table
{% endhighlight %}

Now I can try to translates some German sentences into English by running the decoder:

{% highlight shell %}
~/workspace/mosesdecoder/bin/moses -f ~/workspace/experiment/tuning/moses.tuned.ini.3
~/workspace/mosesdecoder/bin/moses -tt -f ~/workspace/experiment/tuning/moses.tuned.ini.3
{% endhighlight %}

Here are a few sample sentences from online news in German. Clearly I need to try with a larger parallel corpus as my next step.

### Example 1

German: 
> 96 Prozent der Muslime fühlen sich Deutschland verbunden

English: 
> 96 % of the Muslims feel connected Germany

### Example 2

German: 
> Wanderer nach gewaltigem Bergsturz in der Schweiz vermisst

English: 
> Wanderer after massive Bergsturz in Switzerland missing

### Example 3

German: 
> Ein gewaltiger Felsabbruch hat acht Deutsche, Österreicher und Schweizer beim Wandern in den Schweizer Alpen überrascht.

English: 
> One powerful Felsabbruch has eight Germans, Austrians, and Swiss in Wandern in the Swiss Alps by surprise.






