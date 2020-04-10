---
title: 'A survey on Multi-Domain Dialogue State Tracking'
date: 2020-01-23
permalink: /posts/2020/01/dst/
tags:
---

Welcome to my first research post! I will make a brief introduction to Dialogue State Tracking (DST) and then talk about recently raised concerns in DST. Some existing works will be introduced and I will make a few comments. An unreleased part is about my recent work on DST, which has been submitted to *IJCAI-2020* (due to the anonymity policy of program committees).

<!-- more --> 

## A Quick Look at Dialogue State Tracker

### What does an annotated dialogue look like in DST tasks? 

![An example of multi-domain dialogue state tracking" "An example of multi-domain dialogue state tracking](https://i.imgur.com/AUzi2u6.png)

> As shown in this figure, a user can first ask the system about booking a train and then transfer to asking about hotel rooms, which requires flexibility in the ability of domain-transition. Moreover, slots like train-departure and train-destination are quite confusable since they share a lot of similarities. In total, there are 1879 possible values in MultiWoz 2.0 and 1934 in MultiWoz 2.1, which creates a large number of combinations. In this case, our model has to first determine the correct domain-slot pairs to track, then accurately point out the right values. Another challenge is that complex reasoning is required in multi-turn dialogues.


### Why should we need DST?
Dialogue state tracking (DST) is a core component in task-oriented dialogue systems, such as restaurant reservation or ticket booking. The goal of DST is to extract user goals/intentions expressed during conversation and to encode them as a compact set of dialogue states, i.e., a set of slots and their corresponding values.

### Some Existing DST models

+ SpanPtr: It applies a pointer-network based model to find text spans with start and end pointers for each domain-slot pair.

+ FJST: It contains a bidirectional LSTM network to encode the dialog context and a separate feedforward network to predict each dialog state slot.

+ HyST: It is a hybrid approach based on hierarchical RNNs and an open-vocabulary generation.

+ DSTreader: It models the DST from the perspective of text reading comprehensions and applies a pre-trained BERT to set word embeddings.

+ TRADE: It contains a slot gate module for slots classification and a pointer generator for states generation.

+ DST-Span: Similar to TRADE, it treats all domain-slot pairs as span-based slots, where corresponding values for each slot are extracted through text spans (string matching) with start and end positions in the dialog context;

+ DST-Picklist: It treats all domain-slot pairs as picklist-based slots, where corresponding values for each slot are found in the candidate-value list;

+ DS-DST: It includes both span-based slots and picklist-based slots. In the default setting, as some slots related to time and number could have lots of possible values, e.g., the slot of arrive by of taxi domain could fall into {00 : 00 − 23 : 59} depends on the users. We treat these kinds of slots as span-based slots, resulting in five slot types across four domains (nine domain-slot pairs in total). For the other slots which have limited set of values, e.g., the slot of type of hotel domain has two candidate values {hotel, guest house}. We treat these kinds of slots as picklist-based slots, and there are in total 21 such domain-slot pairs.

## Recent Challenges: Open-Vocabulary and Open-Domain

### Why is the open\-character so important?

Over-dependence on domain ontology and lack of knowledge sharing across domains are two practical and yet less studied problems of dialogue state tracking. Existing approaches generally fall short in tracking unknown slot values during inference and often have difficulties in adapting to new domains.

### How is COPY Mechanism, or Pointer Network, so powerful?

Problems such as sorting variable sized sequences, and various combinatorial optimization problems belong to this class. Pointer-Network solves the problem of variable size output dictionaries using a recently proposed mechanism of neural attention. It differs from the previous attention attempts in that, instead of using attention to blend hidden units of an encoder to a context vector at each decoder step, it uses attention as a pointer to select a member of the input sequence as the output. We call this architecture a Pointer Net (Ptr-Net). Ptr-Nets can be used to learn approximate solutions to three challenging geometric problems – finding planar convex hulls, computing Delaunay triangulations, and the planar Travelling Salesman Problem – using training examples alone. Ptr-Nets not only improve over sequence-to-sequence with input attention, but also allow us to generalize to variable size output dictionaries. We show that the learnt models generalize beyond the maximum lengths they were trained on. We hope our results on these tasks will encourage a broader exploration of neural learning for discrete problems.

![An overview of Seq-Seq and Ptr-Net](https://i.ibb.co/p0pkr7t/ptr-net.png)

> refer to https://arxiv.org/abs/1506.03134 for details.

To overcome the multi-turn mapping problem, TRADE was the first work that employs copy mechanism to properly track slot states.  As such, TRADE employs soft-gated pointer-generator copying to combine a distribution over the vocabulary and a distribution over the dialogue history into a single output distribution.

## The Future: General Purpose Chat Bots?

Chatbots mimic human speech for the purposes of simulating a conversation or interaction with a real person. Chatbots are used most commonly in the customer service space, assuming roles traditionally performed by living, breathing human beings such as Tier-1 support operatives and customer satisfaction reps.

Business applications of chatbots for consumer-facing goods are growing rapidly. In fact, over 59% of millennials and 60% of Gen Xers in the United States have interacted with chatbots.

And according to a Facebook survey, 

> more than 50% of customers say they’re more likely to shop with a business that they can connect with via chat.

According to Gartner,

> “By 2020, 85% of our engagement with businesses will be done without interacting with another human. Instead, we’ll be using self-service options and chatbots.”

Additionally, according to an Oracle survey,

> “80% of businesses said they currently use or are planning to use chatbots by 2020.”

Despite of the fact that most existing commercial chatbots are made by hand-crafted rules, deep learning-based models are make a huge difference in research area. In more an more scenarios, machine is learning faster and better. Carefully-annotated large corpus is boosting this procedure but yet not dramatically enough. Real world corpus with unprecedented size is being collected for unsupervised language model training. In the near future, it is promising for mankind to actually build a brian-like machine for general purpose.

