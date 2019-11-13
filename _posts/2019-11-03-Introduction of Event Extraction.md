---
layout: post
title: "Introduction of Event Extraction"
date: 03/11/2019
description: "A detailed introductory article of event extraction"
tag: Natural Language Processing
---   

## Introduction of Event Extraction

### Definition of Event Extraction
Event Extraction (EE) constitutes a challenging task with the purpose of quickly identifying events and their entities in a large number of documents. An event is described by a set of participants (i.e. attributes or roles) whose values are text excerpts.    
  
The EE implies identifying instances of specified types of events in text and the arguments associated with them. Each event is represented by a phrase, a sentence or a span of text, the ***event trigger***(most often single verbs or phrasal verbs, but also nouns, phrasal nouns, pronouns and adverbs), which evokes that event. After the detection and classification of the triggers, the arguments of the event must be found. Event arguments are entity mentions or temporal expressions that are involved in an event (as participants).  
  
These two main sub-tasks, trigger and argument role prediction are highly interdependent: trigger identification is usually treated as an independent task and argument finding and attribute assignment are each dependent on the results of trigger identification. Finally, the event extraction task can be treated as an n -ary relation extraction task, where the components of a relation can be the triggers that represent the central component and the arguments that are related to the trigger.   
  
For instance, this sentence:  
  
“There was the free press in Qatar, Al Jazeera, but its’ offices in Kabul and Baghdad were bombed by Americans.”   
  
Typically, an event in a text is expressed by the following components:   
  
- Event mention : an occurrence of an event with a particular type. These are usually sentences or phrases that describe an event. The sentence above is an Attack event mention. 
- Event trigger : the word that most clearly expresses the event mention. The Attack from the sentence is revealed by the event trigger word bombed . 
- Event argument : an entity mention, temporal expression or value (e.g. Sentence , Crime , Job-Title ) that serves as a participant or attribute with a specific role in an event mention. 
- Argument role : the relationship between an argument and the event in which it participates. The argument roles that should be extracted in this case are: Americans that has the role of an Attacker , the Places where the event produced are Kabul and Baghdad and the Target of the bombing is comprised by the offices in Kabul and Baghdad. Attacker and Target are roles of arguments that are specific for Conflict.Attack event type.
