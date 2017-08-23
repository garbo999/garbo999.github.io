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

The key here is that the phrase table needs to be converted into another format so it can be partially read into memory ([as explained here under *Testing*](http://www.statmt.org/moses/?n=Moses.Baseline) as well as [here under *Compact Phrase Table*](http://www.statmt.org/moses/?n=Advanced.RuleTables#ntoc3)).

To make this step, I need to compile Moses with the `--with-cmph` option since a quick check with `moses -h` shows that *PhraseDictionaryCompact* is missing from the available *Available feature functions*. 

{% highlight shell %}
gary@moses:~/workspace$ ./mosesdecoder/bin/moses -h
...
Available feature functions:
BleuScoreFeature ConstrainedDecoding ControlRecombination CorrectionPattern CountNonTerms CoveredReferenceFeature DeleteRules DesegModel Distortion DynamicCacheBasedLanguageModel EditOps ExampleLM ExamplePT ExampleStatefulFF ExampleStatelessFF ExampleTranslationOptionListFeature Generation GlobalLexicalModel HyperParameterAsWeight InMemoryPerSentenceOnDemandLM InputFeature KENLM LexicalReordering MaxSpanFreeNonTermSource Model1Feature NieceTerminal OpSequenceModel PhraseBoundaryFeature PhraseDictionaryALSuffixArray PhraseDictionaryBinary PhraseDictionaryDynamicCacheBased PhraseDictionaryFuzzyMatch PhraseDictionaryGroup PhraseDictionaryMemory PhraseDictionaryMemoryPerSentence PhraseDictionaryMemoryPerSentenceOnDemand PhraseDictionaryMultiModel PhraseDictionaryMultiModelCounts PhraseDictionaryOnDisk PhraseDictionaryScope3 PhraseDictionaryTransliteration PhraseDistanceFeature PhraseLengthFeature PhraseOrientationFeature PhrasePairFeature PhrasePenalty ProbingPT ReferenceComparison ReloadingLM RulePairUnlexicalizedSource RuleScope RuleTable SRILM SetSourcePhrase SoftMatchingFeature SoftSourceSyntacticConstraintsFeature SourceGHKMTreeInputMatchFeature SourceWordDeletionFeature SpanLength SparseHieroReorderingFeature SyntaxInputWeight SyntaxRHS TargetBigramFeature TargetConstituentAdjacencyFeature TargetNgramFeature TargetPreferencesFeature TargetWordInsertionFeature TreeStructureFeature UnalignedWordCountFeature UnknownWordPenalty WordPenalty WordTranslationFeature
{% endhighlight %}

Note: It is necessary to install the [cmph](http://cmph.sourceforge.net/) hashing library before compiling Moses.

Here is the command I used to compile Moses (I had to add the `-a` because otherwise Moses was compiled without the cmph library; I found that suggestion [here](http://thread.gmane.org/gmane.comp.nlp.moses.user/13854), but I am not sure why it works):

{% highlight shell %}
./bjam -a --with-boost=/home/gary/workspace/boost_1_60_0 --with-srilm=/home/gary/workspace/srilm --with-cmph=/home/gary/workspace/cmph/cmph-2.0 -j4
{% endhighlight %}

Now I need to convert my monster phrase table to the compact format.