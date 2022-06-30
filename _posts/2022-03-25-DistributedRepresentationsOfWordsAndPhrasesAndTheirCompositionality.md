---
layout: post
title: Distributed Representations of Words and Phrases and their Compositionality
subtitle: 论文笔记
categories: ArtificialIntelligence
tags: book-report artificial-intelligence machine-learning natural-language-processing
---

# Paper Reading: Distributed Representations of Words and Phrases and their Compositionality

> Mikolov, T., Sutskever, I., Chen, K., Corrado, G. S., & Dean, J. (2013). Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems, 26.

## Summary

### Innovations

* Present several **extensions** of the original Skip-gram model.
  1. Identify a large number of phrases using a data driven approach
  2. Treat the phrases as individual tokens during the training
* Present a simplified variant of **Noise Contrastive Estimation (NCE)** for training the Skip-gram model.

### Experiment Result

#### Improve speed and accuracy

* **Sub-sampling** of frequent words during training results in a significant speed up.
* Improves accuracy of the **representations of less frequent words**.

#### Other foundings

* Simple vector addition can often produce meaningful results.

### Difficulty

* **Idiomatic phrases**
  > Word representations are limited by their inability to represent idiomatic phrases that are compositions of the individual words.

### Evaluation techniques

Developed a test set of analogical reasoning tasks that contains **both words and phrases**.

## The Skip-gram Model

### Training objective

Find word representations that are useful for predicting the surrounding words in a sentence of a document.

Given a sequence of **training words** $w_1, w_2, w_3, ..., w_T$, the objective of the Skip-gram model is to **maximize the average log probability**

$$
\frac{1}{T}\sum_{t=1}^{T}\sum_{-c\le j\le c,j\not ={0}}\log p(w_{t+j}|w_t)
$$

* $c$: the size of the training context
  * Larger $c$ $\rightarrow$ accuracy $\uparrow$, training time $\uparrow$
* $w_t$: the center word

The basic Skip-gram formulation defines $p(w_{t+j}|w_t)$ using the softmax function

$$
p(w_O|w_I) = \frac{\exp(v_{w_O}'^\top v_{w_I})}{\sum_{w=1}^W\exp(v_w'^\top v_{w_I})}
$$

* $v_w$ and $v_w'$ are the "input" and "output" vector representations of $w$
* $W$: the number of words in the vocabulary

## Hierarchical Softmax: To Speed up

### Advantage

Instead of evaluating $W$ output nodes in the neural network to obtain the probability distribution, it is needed to evaluate only about $\log_2(W)$ nodes.

### Method

![](/assets/images/post/paper/bi-tree.png)

Use a **binary tree** representation of the **output** layer with the $W$ words as its leaves and, for each node, explicitly represents the relative probabilities of its child nodes. We use a **binary Huffman tree**.

The hierarchical softmax defines $p(w_O|w_I)$ as follows:

$$
p(w|w_I) = \prod_{j=1}^{L(w)-1}\sigma(\llbracket n(w,j+1) = \mathrm{ch}(n(w,j)) \rrbracket \cdot v'_{n(w,j)}\top v_{w_I})
$$

* $\sigma(x) = 1/(1+\exp(-x))$
* $v_w$, $v'_n$: the hierarchical softmax formulation has one representation $w_w$ for each word $w$ and one representation $v'_n$ for every inner node $n$ of the binary tree.

## Negative Sampling

### Background

**Noise Contrastive Estimation**: An alternative to the hierarchical softmax is Noise Contrastive Estimation (NCE).

Define **Negative sampling (NEG)** by the objective, which is used to replace every $\log P(w_O|w_I)$ term in the Skip-gram objective. The task is to distinguish the target word $w_O$ from draws from the noise distribution $P_n(w)$ using logistic regression, where there are $k$ negative samples for eah data sample.

$$
\log\sigma (v'_{w_O}\top v_{w_I}) + \sum_{i=1}^k\mathbb{E}_{w_i\sim P_n(w)} [\log\sigma(-v'_{w_i}\top v_{w_I})]
$$

Both NCE and NEG have the noise distribution $P_n(w)$ as a free parameter. The unigram distribution $U(w)$ raised to the 3/4rd power outperformed significantly the unigram and the uniform distributions, for both NCE and NEG on every task we tried including language modeling.

## Subsampling of Frequent Words

**Imbalance between the rare and frequent words**: frequent words provide less information value than the rare words.

**Subsampling approach**: each word $w_i$ in the training set is discarded with probability computed by the formula

$$
P(w_i) = 1 - \sqrt{\frac{t}{f(w_i)}}
$$

* $f(w_i)$: the frequency of word $w_i$
* $t$: a chosen threshold, typically around $1-^{-5}$

**Result**: **accelerated** learning and even significantly improves the **accuracy** of the learned vectors of the rare words.