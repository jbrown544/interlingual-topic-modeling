# Unsupervised Thematic Discovery in Non-English Natural Language Corpora

[![Built with spaCy](https://img.shields.io/badge/made%20with%20❤%20and-spaCy-09a3d5.svg)](https://spacy.io)

## Objective

In this exploration, the primary goal is to discover methods for applying modern NLP thematic modeling to generate intuition over non-English language corpora without requiring source document translation. Secondary goals are to discover techniques for parameter optimization and to develop reusable interlingual tools portable across language sources.  Inputs into the processes will be unstructured therefore the design will make few assumptions allowing for flexibility with varied sources and languages.

![pyLDAvis Visualization](./img/header.png "Topic Model Visualization")

## Methods and Tooling

The method applied to explore the project objective is to stack a series of software tools embellished with custom utility functions to accomplish a series of preparation and analysis steps. This "toolkit" approach allows flexibility for exploring latent topics across many natural language sources.

After describing these steps and associated tools below, their application will be demonstrated to determine their efficacy for this project's objective. The intention of describing the tools and methods is to enable further exploration beyond the demonstrations given. After the explanation of these tools and their demonstration, ideas for further exploration will be offered based upon the experiences of this project. 

The Python notebook implementing these steps and tools is contained in this repository as [Interlingual\_Topic\_Modeling.ipynb](https://github.com/jbrown544/interlingual-topic-modeling/blob/main/Interlingual_Topic_Modeling.ipynb).

## Software Tools

* [spaCy 3.1.3 "Industrial-Strength Natural Language Processing"](https://spacy.io/)
* [Gensim 4.1.2 "Topic modelling for humans"](https://radimrehurek.com/gensim/index.html)
* [pyLDAvis 3.3.1 "Python library for interactive topic model visualization"](https://pyldavis.readthedocs.io/en/latest/readme.html)
* [Google Cloud Translation API](https://cloud.google.com/translate/)
* [Google Colab Notebooks](https://colab.research.google.com/)

**spaCy** is applied to both parse input text and identify useful features by analyzing the natural language structure within the text. Since this exploration covers multiple language sources, spaCy's many language models which can be interchanged behind its API will be used to allow creation of a single set of text processing functions.

**Gensim** provides topic modeling capabilities and coherence analysis. The outputs from spaCy's preprocessing will be translated into processable "bag-of-words" models then processed using topic modeling algorithms provide by Gensim. Translation integration explained below is worked into Gensim's ```Dictionary``` data structure which is referenced by pyLDAvis explained below.

**pyLDAvis** provides visualizations which easily integrate with Gensim models. These visualizations will be used to explore the created models. These visualizations will also be the view into language translations.

A **Google Colab** notebook is used to contain and execute the source code for this project. Given this processing load is excessive for the resources made available to Colab, it is advisable to connect Colab to a [local Jupyter instance](https://research.google.com/colaboratory/local-runtimes.html) or execute this notebook on a suitably powerful workstation in Jupyter.

**Google Cloud Translation** is used to augment the discovered features through term translation. To run the translations and generate a local repository of cached translation terms, **you will need to establish your own Google Cloud account and include your own authentication key in the ```/keys``` folder of the project**. There is cost associated with cloud translations which at the time of this writing allowed for 500K chars/month free then $20(USD) per 1M chars/month thereafter. A caching facility built into translations for this project helps manage expense if you retain copies of the cache file ```xlat.json```. For an idea of potential expense, this project was developed over two billing cycles (1M free chars) and overage charges were less than $10(USD)

## Steps to Analysis

Here are the overall steps applied in processing each language for high level context:

1. Language-Neutral Processing Pipeline and EDA
1. Modeling
1. Coherence Evaluation
1. Translation Integration
1. Visualization and Analysis

Each step will be elaborated below.

### 1. Language-Neutral Processing Pipeline and EDA

The text processing pipeline used in this exploration is based around the language models available in spaCy. The processing pipeline is very generic taking advantage of capabilities in spaCy which can be varied by changing out the language model in use.

After processing texts in a stream with the ```pipe()``` method on the given language model, lemmatized tokens are filtered by:

+ Parts-of-Speech
+ Language-Specific Stop words
+ Alphabetic-only Tokens
+ Lemma Within a Length Range

A general EDA function computes various useful statistics over the text input. The statistics provided were loosely based upon a demonstration of the general EDA capabilities of the tool Splunk. The EDA independently processes using its own language model pipeline as it takes measure of the entire corpora versus the subset used by processing.

See the Google Colab notebook subsection "Language-Neutral Processing Pipeline" for processing functions. A function is available using Gensim's simple parser for comparison purposes.

See the Google Colab notebook subsection "Text Exploratory Data Analysis (EDA)" for the EDA function.

### 2. Modeling

In this pipeline, modeling and coherence evaluation have been integrated. In the following coherence evaluation step, the model showing the most promise is produced for continued processing. If the evaluation step does not produce the desired model, an additional coherence evaluation step is sometimes introduce which only evaluates a single value of a test variable. This ensures the single evaluated model is guaranteed to be the one output for further processing.

The evaluations performed focus on the LDA and HDP models available in Gensim. Both models decompose documents into sets of topics which are themselves distributions over the vocabularies contained in the documents. LDA is a familiar modeling technique but requires the number of topics be specified and that the corpus be fully processed. HDP provides similar capabilities, but can infer the number of topics from the text. HDP may still be tuned within given parameters and will be compared to the more familiar LDA with the chosen data sets.

### 3. Coherence Evaluation

For model evaluation, a configurable evaluation function is provided which builds test models asynchronously to take advantage of available processor cores. Coherence measures are gathered and plotted for the requested series of test variable values. The optimal model is returned with the value of the tested variable.

This configurable function can be customized to either review a model directly or to review topics taken from the model for cases where the Gensim coherence model capabilities do not yet support a particular model class. In this demonstration, an HDP model is used which is not documented as yet supported by Gensim coherence modeling, but both approaches yield equivalent results in informal trials.

Additionally, a sorting key function can be configured to select the optimal model based upon criteria. Functions for finding lowest values and those nearest one are included for use with the demonstrated coherence metrics.

### 4. Translation Integration

Translations are integrated for visualization by creating an alternate Gensim dictionary provided to pyLDAvis. This translated dictionary is created by persisting the Gensim generated dictionary to a file, augmenting the terms in the file with the requested language translation, then reconstituting an instance of the augmented dictionary. This augmented dictionary may then be used in pyLDAvis to present both the target and source language terms.

#### Note: Caching

Because there are costs associated with the cloud translation API, as features are translated they are persisted in a JSON cache file. This file contains dictionaries for each language pair developed. Each language pair dictionary contains individual terms previously translated (source vs. target language). If a project notebook is being executed in an ephemeral environment (such as Google Colab), remember to download a permanent copy of the cache files in the ```\caches``` folder. Generating the same translations repeatedly using the cloud API can become unnecessarily expensive otherwise.

### 5. Visualization and Analysis

Models are depicted using pyLDAvis dimensional reductions. This allows models usually exist with dimensional impossible to conceptualize to be be viewed on screen. These may be viewed by opening a notebook in either Google Colab or the nbviewer link accessible from with GitHub. The GitHub code viewer does show cell outputs, but does not show these visualizations which will appear as a blank output cell. [Here](https://www.youtube.com/watch?v=IksL96ls4o0) is an excellent video which helps explain the operation of pyLDAvis. It demonstrates an older version of the tool, but provides insight into how latent topics may be analyzed in a model.

# Demonstrations

## Language Samples

1. **French:** ABU la Bibliothèque Universelle 
	* source: [7 works in French by Jules Verne](http://abu.cnam.fr/BIB/)
	* project copies: [/text/books/fr/](https://github.com/jbrown544/interlingual-topic-modeling/tree/main/text/books/fr)
1. **Spanish:** Corpora Collection Leipzig University
	* source: [30K 2019 Spanish RSS News Samples](https://wortschatz.uni-leipzig.de/en/download/Spanish#spa-ar_web_2019)
	* project copies: [/text/news/es/](https://github.com/jbrown544/interlingual-topic-modeling/tree/main/text/news/es)
1. **Polish/English Parallel:** European Parliament Proceedings Parallel Corpus 1996-2011
	* source: [Parallel Corpus Polish-English 01/2007-11/2011](http://www.statmt.org/europarl/)
	* project copies: [/text/gov/multi/](https://github.com/jbrown544/interlingual-topic-modeling/tree/main/text/gov/multi)

### French Language Sample Analysis

<figure>
<img src="https://raw.githubusercontent.com/jbrown544/interlingual-topic-modeling/main/img/327px-'From_the_Earth_to_the_Moon'_by_Henri_de_Montaut_39.jpg" height="400" width="300"/>
<figcaption>Henri de Montaut, <a href="https://commons.wikimedia.org/w/index.php?curid=11412182">Public Domain</a>, via Wikimedia Commons</figcaption>
</figure>

This sample consists of seven works by the French author Jules Verne. These are the works listed in length order (longest to shortest) and linked to their Wikipedia plot summaries:
	
+ [Five Weeks in a Balloon](https://en.wikipedia.org/wiki/Five_Weeks_in_a_Balloon#Plot_summary)
+ [Around the World in Eighty Days](https://en.wikipedia.org/wiki/Around_the_World_in_Eighty_Days#Plot)
+ [Robur the Conqueror](https://en.wikipedia.org/wiki/Robur_the_Conqueror#Plot_summary)
+ [From the Earth to the Moon](https://en.wikipedia.org/wiki/From_the_Earth_to_the_Moon#Plot)
+ [The Begum's Fortune](https://en.wikipedia.org/wiki/The_Begum%27s_Fortune#Plot_summary)
+ [The Blockade Runners](https://en.wikipedia.org/wiki/The_Blockade_Runners#Plot_introduction)
+ [The Mutineers of the Bounty](https://en.wikipedia.org/wiki/The_Mutineers_of_the_Bounty)

TODO

### Spanish Language Sample Analysis

<figure>
<img src="https://raw.githubusercontent.com/jbrown544/interlingual-topic-modeling/main/img/109899_newsstand_300.jpg" width="300"/>
<figcaption>Melbeans, <a href="https://commons.wikimedia.org/wiki/File:109899_newsstand_300.jpg">Public Domain</a>, via Wikimedia Commons</figcaptiion>
</figure>

TODO

### English-Polish Parallel Language Sample Analysis

<figure>
<img src="https://raw.githubusercontent.com/jbrown544/interlingual-topic-modeling/main/img/Leeuwarden%2C_de_vlaggen_van_de_Europese_Unie_op_de_Stationsweg_IMG_3726_2018-05-21_12.47.jpg" width="300"/>
<figcaption>Michielverbeek, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons</figcaption>
</figure>

TODO

## Ideas for Further Exploration

1. An interesting idea for additional exploration is to retrofit the processing pipeline introduced here with the library [tomotopy](https://bab2min.github.io/tomotopy/v0.12.2/en/). According to this [post by Eduardo Coronado Sroka](https://towardsdatascience.com/dont-be-afraid-of-nonparametric-topic-models-part-2-python-e5666db347a), Gensim can be difficult when using HDP and there may be advantages with tomotopy. Initial investigation indicated there [may be code](https://github.com/bab2min/tomotopy/blob/main/examples/lda_visualization.py) to help visualize tomotopy output using pyLDAvis.
2. The JSON cache file currently persisting term translations could be modified to use a shared repository of translations such as a database service. Maintaining the information in a file is not always convenient. Interrogating cloud APIs for the translation of individual terms can become expensive.
3. The parallel language sample used in this exploration did not generate ideal topic coherence. Investigating other translations such as public domain books may prove interesting for comparing topic models generated in works with equivalent meaning.
4. Performing machine translations on work then applying this process also offers interesting territory for exploration. This is especially the case if the machine translation product introduces noise into the information potentially reducing coherence.

## More Information

[Project References](https://github.com/jbrown544/interlingual-topic-modeling/blob/main/REFERENCES.md)

Authored with [ghostwriter](https://wereturtle.github.io/ghostwriter/index.html)
