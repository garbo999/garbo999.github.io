---
layout: post
title:  "Expectation maximization algorithm for IBM model 1"
date:   2017-06-05 14:01:00 +0300
categories: moses
---
The book [Statistical Machine Translation](http://www.statmt.org/book/) by Philipp Koehn starts with a discussion of the IBM models for machine translation.

I have been studying the IBM models and started working on the programming exercises in the Koehn text. My goal is to explore machine translation from the top down (working with Moses open-source software) while also trying to build a statistical machine translation engine from the ground up.

[Kaggle](https://www.kaggle.com/) allows users to create and upload programs which can then be executed in an online environment. Using Python, I wrote an implementation of the *expectation maximization algorithm for IBM model 1* which I [uploaded to Kaggle](https://www.kaggle.com/garbo999/machine-translation-ibm-model-1-em-algorithm/).

The data set I use to initially test the algorithm in this example is very small. In fact, it is the toy data set from Chapter 4 of the Koehn text. There are just 3 "sentence pairs":

* 'das Haus' --> 'the house' 
* 'das Buch' --> 'the book' 
* 'ein Buch' --> 'a book' 

The output from this algorithm is a set of probabilities for all the word pairs in the vocabulary. For example, we obatin the probability that 'Buch' translates as 'book' \(t\('book'\|'Buch'\)\). This should be HIGH. 

On the other hand, the algorithm also calculates the probability that 'Buch' translates as 'house' \(t\('house'\|'Buch'\)\). This should be LOW. 

It all sounds terribly obvious from a human translator perspective, but this how SMT works at the lowest level.