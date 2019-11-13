---
layout: post
title: "Introduction of Event Extraction"
date: 03/11/2019
description: "A detailed introductory article of event extraction"
tag: Natural Language Processing
---   

## <center>Introduction of Event Extraction</center>

### 1. Definition of Event Extraction
Event Extraction (EE) constitutes a challenging task with the purpose of quickly identifying events and their entities in a large number of documents. An event is described by a set of participants (i.e. attributes or roles) whose values are text excerpts.    
  
The EE implies identifying instances of specified types of events in text and the arguments associated with them. Each event is represented by a phrase, a sentence or a span of text, the ***event trigger***(most often single verbs or phrasal verbs, but also nouns, phrasal nouns, pronouns and adverbs), which evokes that event. After the detection and classification of the triggers, the arguments of the event must be found. Event arguments are entity mentions or temporal expressions that are involved in an event (as participants).  
  
These two main sub-tasks, trigger and argument role prediction are highly interdependent: trigger identification is usually treated as an independent task and argument finding and attribute assignment are each dependent on the results of trigger identification. Finally, the event extraction task can be treated as an n -ary relation extraction task, where the components of a relation can be the triggers that represent the central component and the arguments that are related to the trigger.   
  
For instance, this sentence:  
  
“There was the free press in Qatar, Al Jazeera, but ***its’ offices in Kabul and Baghdad*** were ***bombed*** by ***Americans***.”   
  
Typically, an event in a text is expressed by the following components:   
  
- **Event mention** : an occurrence of an event with a particular type. These are usually sentences or phrases that describe an event. The sentence above is an ***Attack*** event mention. 
- **Event trigger** : the word that most clearly expresses the event mention. The ***Attack*** from the sentence is revealed by the event trigger word ***bombed*** . 
- **Event argument** : an entity mention, temporal expression or value (e.g. Sentence , Crime , Job-Title ) that serves as a participant or attribute with a specific role in an event mention. 
- **Argument role** : the relationship between an argument and the event in which it participates. The argument roles that should be extracted in this case are: Americans that has the role of an ***Attacker*** , the ***Places*** where the event produced are Kabul and Baghdad and the ***Target*** of the bombing is comprised by the offices in Kabul and Baghdad. ***Attacker*** and ***Target*** are roles of arguments that are specific for ***Conflict.Attack*** event type.  
  

### 2. Event Extraction Evaluation
The event extraction task has been developed through several evaluations, mainly MUC, ACE, and TAC. More precisely, the evaluation in this context is based on event mentions and event arguments.The evaluation is based on the standard metrics: Precision (P), Recall (R), and F-measure (F1), defined by the following equations:  

![](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/images/posts/Introduction-of-event-extraction/1.png)    
  
True positives are the samples classified as belonging correctly to a class. False negatives are classified as not belonging to a class, incorrectly. False positives are the samples classified as belonging to a class, incorrectly. Precision is the fraction of relevant samples among the retrieved samples, while recall is the fraction of relevant samples that have been retrieved over the total amount of relevant samples. The F1 is the harmonic mean between these two. Because the data in EE tasks usually suffer from class imbalance, the author compute the micro-averages of these metrics for aggregating the contributions of all classes.  
the following criteria are used to determine the correctness of a predicted event mention:  
- A trigger is correct if its event subtype and offsets match those of a reference trigger.
- An argument is correctly identified if its event type and offsets match those of any of the reference argument mentions.

### 3. Event Detection
#### 3.1 Definition of Event Trigger
Event Detection (ED) is considered a crucial and quite challenging subtask of event extraction and involves identifying instances of specific types of events in text and classifying them into event types precisely.  Associated with each event mention is a phrase, the event trigger (most often a single verb, noun, phrasal verbs, andnouns, but also pronouns and adverbs), which evokes that event. More precisely, this subtask involves identifying event triggers and classifying them into a specific type.It can be challenging because the same event might appear in the form of multi-word expressions and these expressions might represent different events in different contexts.  

An event trigger is a word or multi-word that depicts the occurrence of an event in a text. The problem can be thought of in the following manner: given a trigger or word, there are two possibilities: the trigger can represent an event of interest, or it can represent another event or something else in which the author have no interest.  
  
The same event might appear in the form of various trigger expressions and an expression might represent different event types in different contexts.A trigger can also appear in the form of a multi-word expression. Therefore, there are two major challenges:
- One of the main challenges is that some trigger words are ambiguous indicators of particular types of events.
- Another challenge for this dataset is the number of triggers per sentence.
#### 3.2 Baseline CNN model for Event Detection
Firstly, compared with the feature-based methods that benefit from manual engineered feature sets, all neural-based methods perform better by avoiding error propagation from different NLP tools (parsers etc.) and by better representing the semantics of the words.  

CNNs are a good choice for event detection since they capture global representations of text and extract the most informative parts for the sequence of words and only considers their resulting activations.  

Experiments show also that the choice of pre-trained embeddings has an important impact on the performance of the ED task.The data used for training the embeddings, their size, and the training algorithm influence the performance.
![Baseline CNN model for ED, where Baghdad is the current trigger candidate in a context window of 2 × 4 + 1 words
](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/images/posts/Introduction-of-event-extraction/2.png)  
#### 3.3 Model Modification and Improvement Based on Baseline Model
So, our hypothesis is that representing the whole sentence in a way that it can predict the existence of a trigger will further help the model distinguish between event types. The author exploit different methods to add more semantic and syntax features.  

The second contribution aims at exploiting two techniques for incorporating the inner information of words as new features. Firstly, the author consider creating character-level features that can capture morphological and shape information of words, whereas word embeddings are effective to capture word-level syntactic and semantic information. For example, if the author take a new word not present in the training data,torturing,but present in the validation data, given its root and suffix (i.e.torturing), it is natural to guess that it is a variant of torture and the latter probably represents the same type of event, that being **Life.Injure**. At the same time, the author target one of the main concerns when working with word embeddings, that is to say, the treatment of the words that occur infrequently since word embedding models suffer from lack of enough training opportunity for infrequent words. Another advantage of adding character-level representations is the possibility of treatment of misspelled or custom words.  The second technique is a type of data augmentation that relies on morphological derivation and inflection generation, which practically adds variants of known triggers in train, targeting the same impact as the character-level features.The author introduce a neural network architecture where the author enrich every trigger candidate context with character-level features and the author perform experiments to observe the effect of augmenting the data with new variants of triggers.
##### 3.3.1 Add Sentence-level information for every trigger candidate
A common approach in creating a sentence representation is by using the final hidden state of an RNN or the max (or average) pooling from either RNNs hidden states or the last convolutional layers.  

Another approach is the encoder-decoder architecture, producing models also known as sequence-to-sequence models.  

For some tasks, people propose to use attention mechanism on top of the CNN or LSTM model to introduce an extra source of information to guide the extraction of sentence embeddings.  
![ED with Sentence Embedding](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/images/posts/Introduction-of-event-extraction/3.png)  
##### 3.3.2 Exploiting the internal structure of words for  Event Detection
While word-level embeddings are meant to capture syntactic and semantic information, character-level embeddings capture morphological and shape information.  

Comparing with other neural-based methods, the architecture that makes use of character-level features has the advantage of representing misspelled, custom or morphological variants of words from the dataset. 
![ED with Character-level Embedding](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/images/posts/Introduction-of-event-extraction/4.png)  

##### 3.3.3 Data Augmentation
The author propose adopting a type of data augmentation that relies on the generation of morphological derivations and inflections of triggers.By morphological derivation, the author mean the generation of other words from given words, often by adding a prefix or a suffix and by inflection generation, the author mean the modification of verbs, nouns and adjectives to account for different grammatical features such as tense or gender. The inflections are basically a bundle of morpho-syntactic features.  

The author decided to augment the true triggers from the training set of ACE 2015 Dataset,The author motivate our choice by the fact that, in this way, the author add morphological information to the models and this can increase the ability to distinguish different trigger forms.  

The proposed data augmentation technique that increases the training dataset with variants of words (inflections) creates a balance between precision and recall.  
### 4. Argument Role Prediction
This chapter focuses on the problem of argument role prediction, i.e., identifying the corresponding arguments to a trigger with a specific event type. This task comes as the second building block in the sequential pipeline that has as the first step the trigger detection and classification. In theory, the two tasks are highly interdependent but, in practice, they are applied as two separate steps: trigger identification is first treated as an independent task and argument finding and attribute assignment are each dependent on the results of trigger identification.  

One of the main challenges in evaluating such a system is that there are types of arguments that are common to many types of events. Place and Entity are common to most of the event types.  
#### 4.1 Definition of Event argument
In the ACE 2005 dataset, an event argument is defined as an entity mention, atemporal expression or a value (e.g.Crime,Sentence,Job-Title) that is involved in an event (as participants or attributes with a specific role in an event mention). An event argument has a type and a role. For example, in a ***Conflict.Attack*** event type,one event argument can be an ***Attacker*** with three possible types: PER, ORG, GPE(Person, Organization, Geo-political Entity). The entity types detection has close ties to the named entity recognition (NER), work that is beyond the purposes of this thesis.
#### 4.2 Detailed Procedure
Since the author formulated the event extraction as a two-stage, multi-class classification via two neural network models with automatically learned features, the author present our choices for these steps. In the first stage, event detection, the author uses the CNN with character-level features, to classify each word of a sentence to identify trigger words. If one sentence has triggers, the second stage is conducted, which applies the CNN presented in the previous section, to assign arguments to triggers and assign the roles to the arguments. The author formalize the task as follows. Let W=[w1,w2,... ,wn]be a sentence where n is the sentence length and wi is the i th token.  

For every sentenceW, the author check every w i to find the trigger words. If wi is a trigger word for some event of interest, the author then need to predict the roles of every argument candidate.  For every trigger detected and classified, the author use the predicted event subtype and classify every argument candidate from the sentence (filtered by Part- of-Speech (POS): the heads of all the noun or proper noun chunks). Thus, for every pair (detected trigger, candidate argument), the argument role is predicted. The first N possible roles are kept with their probability. From these roles, only those that can be part of the subset roles possible for the detected event subtype and the one who has a maximum probability is considered as a result. If there are no possible roles, then the candidate argument is categorized as an Other
#### 4.3 CNN-based Baseline Model for Argument Role Prediction 
The author base our model on a max-pooled CNN with word and positional embeddings as main features, which is the main architecture of the [3]. The input of the model for argument role prediction consists of sentences marked with a trigger word, its type, and one argument of interest. The author computes the maximal distance, in the training set, between the trigger and an argument linked by a relation (labeled with the event subtype) and choose an input width greater than this distance. The author ensure that every input has this length by trimming longer sentences and padding shorter sentences with a special token.
![CNN model for argument role prediction](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/images/posts/Introduction-of-event-extraction/5.png)  

### Appendix
**fastText**[2] is another popular word embedding model, which can be seen as a variant of Word2vec including character sequences in the learning of embeddings. **fastText** takes into consideration the internal structure of words, which can be of a great impact when working with morphologically rich languages (as in Finnish, Turkish or French).  

In the case of **fastText**, which incorporated character n-grams into the Skip-gram model.The author concluded that morphological information significantly improves the syntax and in contrast, it does not help for semantic tasks and even degrades the performance.  

Code and pre-trained embeddings available at https://github.com/facebookresearch/fastText  
### Bibliography
1. Emanuela Boroş. Neural Methods for Event Extraction. Artificial Intelligence [cs.AI]. Université Paris-Saclay, 2018. English. ffNNT : 2018SACLS302ff. fftel-01943841f
2. Bojanowski, P., Grave, E., Joulin, A., and Mikolov, T.(2017). Enriching word vectors with subword information.Transactions of the Association for Computational Linguistics, 5:135–146.
3. Chen, Y., Xu, L., Liu, K., Zeng, D., and Zhao, J. (2015b).Event extraction via dynamic multi-pooling convolutional neural networks.  In Proceedings of the 53rd Annual Meeting of the Association for Computational Linguistics and the 7th International Joint Conference on Natural Language Processing, volume 1, pages 167–176.
4. Sabour, S., Frosst, N., and Hinton, G. E. (2017).  Dynamic routing between capsules. In Advances in Neural Information Processing Systems,pages 3857–3867.
5. Hong, Y., Zhou, W., jingli, Zhou, G., and Zhu, Q. (2018). Self-regulation: Employing a generative adversarial network to improve event detection. In56th Annual Meeting of the Association for Computational Linguistics(ACL 2018), pages 515–526. Association for Computational Linguistics.
  
  
  
  
  
  **Please note that this is ariticle is a summary of the PhD thesis by Dr.Emanuela Boroş[1]**


