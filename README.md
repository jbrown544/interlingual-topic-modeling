# Unsupervised Thematic Discovery in Non-English Natural Language Corpora

[![Built with spaCy](https://img.shields.io/badge/made%20with%20❤%20and-spaCy-09a3d5.svg)](https://spacy.io)

## Project Objective

In this exploration, the primary goal is to discover methods for applying modern NLP thematic modeling to generate intuition over non-English language corpora without requiring source document translation. Secondary goals are to discover techniques for hyperparameter optimization and to develop reusable interlingual tools portable across language sources.  Inputs into the proposed processes will be unstructured therefore the design will make few assumptions about inputs allowing for flexibility with varied sources and languages.

![pyLDAvis Visualization](./img/header.png)

## Method

The method applied to achieve the project objective was to stack a series of software tools and provide utility functions to accomplish a series of analysis steps. This toolkit allows flexibility while exploring the discovery of latent topics in many natural languages.

Once these steps and associated tools are defined below, a series of applications will be attempted to determine their efficacy for the project's objective. The intention of providing such capabilities is to enable further exploration and improvements. After an explanation of the tools and various demonstrations, ideas for further exploration will be offered based upon the experiences in this project. 

## Software Tools

* [spaCy 3.1.2 "Industrial-Strength Natural Language Processing"](https://spacy.io/)
* [Gensim 4.1.2 "Topic modelling for humans"](https://radimrehurek.com/gensim/index.html)
* [pyLDAvis 3.3.1 "Python library for interactive topic model visualization"](https://pyldavis.readthedocs.io/en/latest/readme.html)
* [Google Colab Notebooks](https://colab.research.google.com/)
* [Google Cloud Translation API](https://cloud.google.com/translate/)

## Analysis Steps

1. Language-Neutral Text Processing Pipeline
1. Coherence Evaluation
1. Translation Integration
1. Model Visualization

### Language-Neutral Text Processing Pipeline

The text processing pipeline used in this exploration is based around the language models available in spaCy. The processing pipeline is very generic taking advantage of capabilities in spaCy which can be varied by changing out the model in use.

After processing texts in a stream with the ```pipe()``` method on the given language model, lemmatized tokens are filtered by:

+ Parts-of-Speech
+ Language-Specific Stop words
+ Alphabetic-only Tokens
+ Lemma Within a Length Range

### Coherence Evaluation

For model evaluation, a configurable evaluation function is provided which builds models asynchronously to take advantage of available processor cores. Coherence measures are gathered and plotted for the requested series of test variable values. The optimal model is returned with the value of the tested variable.

### Translation Integration

#### Translation Caching

### Model Visualization

### Other Tools

## Demonstrations

### Data Samples

* ABU : la Bibliothèque Universelle 
	* [7 works in French by Jules Verne](http://abu.cnam.fr/BIB/) 
* Corpora Collection Leipzig University
	* [30K 2019 Spanish RSS News Samples](https://wortschatz.uni-leipzig.de/en/download/Spanish#spa-ar_web_2019)
* European Parliament Proceedings Parallel Corpus 1996-2011
	* [Parallel Corpus Polish-English 01/2007-11/2011](http://www.statmt.org/europarl/)

### French Sample Analysis

![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/%27From_the_Earth_to_the_Moon%27_by_Henri_de_Montaut_39.jpg/327px-%27From_the_Earth_to_the_Moon%27_by_Henri_de_Montaut_39.jpg)

*From the Earth to the Moon* [Public Domain]( https://commons.wikimedia.org/w/index.php?curid=11412182)

This sample consists of 7 works by the French author Jules Verne. These are the works listed in length order (longest to shortest) and linked to their Wikipedia plot summaries:
	
+ [Five Weeks in a Balloon](https://en.wikipedia.org/wiki/Five_Weeks_in_a_Balloon#Plot_summary)
+ [Around the World in Eighty Days](https://en.wikipedia.org/wiki/Around_the_World_in_Eighty_Days#Plot)
+ [Robur the Conqueror](https://en.wikipedia.org/wiki/Robur_the_Conqueror#Plot_summary)
+ [From the Earth to the Moon](https://en.wikipedia.org/wiki/From_the_Earth_to_the_Moon#Plot)
+ [The Begum's Fortune](https://en.wikipedia.org/wiki/The_Begum%27s_Fortune#Plot_summary)
+ [The Blockade Runners](https://en.wikipedia.org/wiki/The_Blockade_Runners#Plot_introduction)
+ [The Mutineers of the Bounty](https://en.wikipedia.org/wiki/The_Mutineers_of_the_Bounty)

### Spanish Sample Analysis

### English-Polish Parallel Sample Analysis

## Ideas for Further Exploration

[Project References](https://github.com/jbrown544/interlingual-topic-modeling/blob/main/REFERENCES.md)

Authored with [ghostwriter](https://wereturtle.github.io/ghostwriter/index.html)
