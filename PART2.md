# Part 2: Unsupervised Thematic Discovery in Non-English Natural Language Corpora

[![Built with spaCy](https://img.shields.io/badge/made%20with%20❤%20and-spaCy-09a3d5.svg)](https://spacy.io)

Interactive Google Colab [project notebook.](https://colab.research.google.com/github/jbrown544/interlingual-topic-modeling/blob/main/Interlingual_Topic_Modeling_\(Part_2\).ipynb)

This project expands an earlier exploration which investigated the question of whether text analytics could produce an interlingual method for discovering latent meaning in a corpus of uninterpreted documents. In the earlier work, a combination of tools and processes revealed promising results and suggested improvements for further refining the evolving technique.
	
## Problem
	
As rationale for this work, human translations are labor intensive and require experienced specialists in specific language pairings (Spanish / English, for example). Machine translations are increasingly effective but can be costly to produce for a corpus of substantial size. The ability to discern themes quickly and inexpensively in text may benefit various endeavors including summarizing untranslated governmental documents, literature, intellectual property claims, social media dialog, and discovery in legal proceedings.

## Question
	
Before making costly translation investments, is it possible to efficiently discern the thematic content of a collection of untranslated documents by applying statistical text processing methods? Such methods can potentially reduce the dimensionality of a corpus focusing scarce translation resources on targeted phrases and terms which may summarize the latent meaning contained. This work continues an exploration of how to effectively deploy such statistical methods for interlingual dimensionality reduction.	
	
## Data
	
During the first iteration of this work, a parallel corpus was examined from Koehn (2005) consisting of quality translations of European Parliamentary proceedings into many of the E.U.'s official languages. A Polish / English sample was processed with earlier methods yielding interesting results. To pursue the evidence observable from side-by-side translation comparisons, the remaining translations in this corpus were reviewed. These subdivided into two language families: Uralic and Indo-European. Of these, statistical processing models were published only for some of the Indo-European samples. Among this family, subdivision was possible into subfamilies including Baltic, Germanic, Greek, Romance, and Slavic. One language was selected from each subfamily by the availability of statistical models for processing (respectively): Lithuanian, Danish, Greek, Italian, and Polish. For each of these samples, equivalent processing was conducted on both the non-English version and the English equivalent. All samples but one (see results) were composed of 150,000 lines treated as documents for processing.
	
## Methodology
	
The approach to this project's exploratory question has been evolving over two iterations. The common elements of the method have been as follows. First, minimal efforts are employed to prepare input text. These include downsampling for available processing resources and adjusting for text encoding. After parsing and tagging a corpus with the appropriate statistical language model, a self-tuning topic modeling algorithm is applied to reduce corpus dimensionality from terms into topics. Then, by applying target-language term translations provided by cloud services, the dimensionally reduced topic set is augmented. Finally, interactive visualizations present the discovered topics, their most frequent or relevant terms, and associated translations.
	
### Iteration 1
	
As background, earlier work began with the goal of exploring topic modeling on text in arbitrary natural languages. Generally, topic modeling begins with the process of discovering relative frequencies of terms which most represent latent topics. A topic, in this usage, is a particular distribution over the terms in a set of documents (a corpus). Documents may be decomposed into distributions over these topics which are themselves term distributions. This distribution of distributions method is used to explain the occurrences of individual terms contained in a document.
	
In the original iteration of the project, a topic modeling algorithm was discovered known as Hierarchical Dirichlet Process (HDP). This algorithm has the benefits of requiring little configuration or manual tuning and the ability to make incremental progress in discovering topic distributions. This was of interest given other common topic modeling techniques such as Latent Dirichlet Allocation (LDA) require model parameters to be known prior to model construction. LDA also must completely analyze any training data prior to producing a model. Topic modeling is an unsupervised technique therefore model parameters are not usually initially known requiring repeated (and in LDA's case, exhaustive) analysis to discover potential values. HDP makes incremental model improvements while automatically tuning parameters including the topic count. Due to this iterative ability, an HDP model is continually available in the most recently evolved state as processing time and / or data are increased.
	
### Iteration 2
	
During the original iteration of this work, a new library known as tomotopy (**to**pic **mo**deling **to**ol) was discovered which, according to a review by Sroka (2020b), yields better human-interpretable results than the Gensim HDP implementation employed originally. This feat is accomplished by limiting scalability. Gensim should be able to process larger corpora but potentially requires extensive hyperparameter tuning to do so. Simply put, tomotopy does less guesswork increasing accuracy but limiting scalability.
	
Other improvements incorporated included augmenting documents with extracted named entities and collocations (n-grams). Named entities include complete phrases such as organization and location names especially useful in comprehending topic meaning. Collocations are phrases whose terms' affinity suggests cohesive meaning. Collocations and named entities may overlap so identical occurrences were only counted once per document.
	
Technical challenges in this iteration included retrofitting several features lost during migration to tomotopy including direct integration with visualization libraries and Gensim's built-in term extremity reduction. A technical improvement migrated collected term translations from a local monolithic file into language-specific cloud storage objects for central management and versioning.
	
## Results
	
Improvements in the current iteration produced significant advancement toward this project's exploratory goal. The tomotopy HDP implementation efficiently drilled into each language source yielding similar topic counts across the translation pairings in some cases. The coherence scores measured yielded similar values across every parallel language pairing. Scores averaged approximately 0.85 on a scale of 1.0 with limited variation. Between translation pairs, variation was more limited with a maximum score difference of 0.0061. Topic counts yielded by tomotopy's HDP implementation converged over 1,000 arbitrary training iterations to levels manageable for visualization. Topic terms augmented with named entities and collocations yielded recognizable topics even with no a priori knowledge incorporated in modeling
	
Given topic interpretation is subjective, various impressions are mentioned here. In many cases, the top topics observed in the translation pairings appeared to be easily comparable while adjusting the relevance metric - the interactive visualization's slide control. While exact topic positions per paired model varied, it was not difficult to discern parallels. Consistent themes appeared between language pairings and even among languages given the sample texts may contain overlap in the E.U.'s parliamentary proceedings. 
	
For example, in Lithuanian a topic in the top 10 covering vocational training, education, and lifelong learning was also apparent in the paired English sample.
	
In the Danish sample, which has the possible advantage of being in the same Germanic language subfamily as English, topic counts were notably similar. There were parallel topics discussing both livestock and waste / renewables. A topic in the top few which stood out was interpreted as discussing democratic strife.
	
In the Greek sample the file size was half of the others. Every language file was downsampled to 150,000 lines to fit under GitHub's 25 MB manual upload limit. The Greek sample required reduction again by half to 75,000 lines because Greek characters appear to use wider character encodings. In the top 10 topics, there were notable parallels which appeared to discuss the environmental effects of emissions and separately transportation. Greek is also notable for being the subfamily containing a single relatively old language.
	
In the Italian sample there are parallel themes related to food production and livestock. The top topic in the parallel pair appears to discuss industrial competitiveness. There is another similarity discussing organized and exploitative crimes in conjunction with immigration.
	
In the Polish sample, the vocational training, education, and lifelong learning topic also appeared and was easily cross referenced in the English counterpart. Other similar topics immediately apparent discussed agricultural production, greenhouse emissions / renewables, and transportation.
	
## Conclusions
	
The efforts of this exploratory iteration have significantly refined the interlingual capabilities under development. Statistical language models have proven their ability to neutralize some of the differences among examined language samples allowing self-tuning topic modeling algorithms such as HDP to reduce the dimensionality in examined corpora. Model results yielded comprehensible topics providing insight into latent meanings in uninterpreted documents. 
	
Examining several languages derived from the common Europarl source provided comparative insight into language differences this experimental method aspires to mitigate. A more quantitative metric for comparing model output between parallel samples may be useful to measure semantic similarities discovered and is suggested as future work.
	
Among possible factors explaining topical differences, the process of translation itself may contribute to informational entropy between translation pairs. Intrinsic language conventions  may also possibly contribute to variational details between statistical language models. Statistical processing of source language texts for latent themes may help preserve communication patterns otherwise eroded during human or machine translations. The methods being evolved in this exploration have been incrementally demonstrating capability to transport meaning across language barriers using targeted topical translation. Further exploration into interlingual methods for discovering latent themes may help identify common thoughts and concepts encoded in humanity's many evolved natural languages.

## More Information

[Project References](./REFERENCES.md)

Authored with [ghostwriter](https://wereturtle.github.io/ghostwriter/index.html)
