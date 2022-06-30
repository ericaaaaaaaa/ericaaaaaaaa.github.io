---
layout: post
title: Improving Distributional Similarity with Lessons Learned from Word Embeddings
subtitle: 论文笔记
categories: ArtificialIntelligence
tags: book-report artificial-intelligence machine-learning natural-language-processing
---

# Paper Reading: Improving Distributional Similarity with Lessons Learned from Word Embeddings

> Levy, O., Goldberg, Y., & Dagan, I. (2015). Improving distributional similarity with lessons learned from word embeddings. Transactions of the association for computational linguistics, 3, 211-225.

## Findings

* Much of the performance gains of word embeddings are due to certain system design choices and hyperparameter optimizations, rather than the embedding algorithms themselves.
* Hyperparameter tuning significantly improves performance in every task

## Notation

* $w\in V_w$: a collection of words
* $c\in V_c$: contexts of words
* $V_w$ and $V_c$: the word and context vocabularies
* $D$: the collection of observed word-context pairs
  * D is commonly obtained by taking a corpus $w_1, w_2,..., w_n$ and defining the contexts of word $w_i$ as the words surrounding it in an $L$-sized window $w_{i-L},...,w_{i-1},w_{i+1},...,w_{i+L}$
* $\#(w,c)$: the number of times the pair $(w,c)$ apprears in $D$
* $\#(w) = \sum_{c'\in V_c}\#(w,c')$: the number of times $w$ occurred in $D$
* $\#(c) = \sum_{w'\in V_w}\#(w',c)$: the number of times $c$ occures in $D$
* $d$: words and contexts are embedded in a space of $d$ dimensions
* $\vec{w}, \vec{c}\in \mathbb{R}^d$: word vector of $w$ and $c$, sometimes refer to the vector $\vec{w}$ as rows in a $|V_W|\times d$ matrix $W$, and to the vectors $\vec{c}$ as rows in a $|V_C|\times d$ matrix $C$

## Background

### Explicit Representations (PPMI Matrix)

**PMI**: a popular measure of the association between the word $w_i$ and the context $c_j$

$$
\mathrm{PMI}(w,c) = \log\frac{\hat{P}(w,c)}{\hat{P}(w)\hat{P}(c)} = \log\frac{\#(w,c)\cdot |D|}{\#(w)\cdot \#(c)}
$$

if $\#(w,c) = 0$, $\log 0\rightarrow -\infin$, so

$$
\mathrm{PPMI}(w,c) = \max(\mathrm{PMI}(w,c), 0)
$$

**Drawback**: a rare context $c$ that co-occurred with a targe word $w$ even once, will often yield relatively high PMI score because $\hat{P}(c)$, which is in PMI's denominator, is very small.

### Singular Value Decomposition (SVD)

> Popularized in NLP via Latent Semantic Analysis (LSA)
> Working with **dense low-dimensional vectors**
> * Improved **computational efficiency**
> * Better **generalization**

Finds the optimal rank $d$ factorization with respect to $L_2$ loss

$$
W^{\mathrm{SVD}} = U_d\cdot \Sigma_d \\
C^{\mathrm{SVD}} = V_d
$$

* $U$ and $V$ are orthonormal
* $\Sigma$ is a diagonal matrix of eigenvalues in decreasing order
* $M_d = U_d\cdot \Sigma_d\cdot V_d^\top$

### Skip-Gram with Negative Sampling (SGNS)

Represent each word $w\in V_W$ and each context $c\in V_C$ as $d$-dimensional vectors $\vec{w}$ and $\vec{c}$., such that words that are "similar" to each other will have similar vector representations.

**Objective**:
* **Maximize** a function of the product $\vec{w}\cdot\vec{c}$ for ($w$, $c$) pairs that occur in $D$
* **Minimize** a function of the product $\vec{w}\cdot\vec{c_N}$ for ($w$, $c$) pairs that not occur in $D$

#### SGNS as Implicit Matrix Factorization

SGNS's corpus level objective achieves its optimal value when

$$
\vec{w}\cdot\vec{c} = \mathrm{(w, c)}-\log k
$$

Hence, SGNS is implicitly factorizing a word-context matrix whose cell's value are PMI, shifted by a global constant ($\log k$)

$$
W\cdot C^\top = M^{PMI} - \log k
$$

### Global Vectors (GloVe)

Present each word $w\in V_W$ and each context $c\in V_C$ as $d$-dimensional vectors $\vec{w}$ and $\vec{c}$ such that

$$
\vec{w}\cdot\vec{c} + b_w + b_c = \log(\#(w, c)) \qquad \forall(w, c)\in D
$$

* $b_w$ and $b_c$(scalars) are word/context-specific biases

**Objective**: a factorization of the log-count matrix, shifted by the entire vocabularies' bias terms

$$
M^{\log(\#(w, c))}\approx W\cdot C^\top + \vec{b^w} + \vec{b^c}
$$

* $\vec{b^w}$: $|V_W|$ dimensional row vector
* $\vec{b^c}$: $|V_C|$ dimensional column vector

Minimize a weighted least square loss, giving more weight to frequent $(w, c$ pairs.

## Transferable Hyperparameters

### Pre-processing Hyperparameters

#### Dynamic Context Window (dyn)

| Approach | Window Size | Window Weight | Example |
| :---: | :---: | :---: | :---: |
| Traditional Approaches | Constant | Un-weighted | for window size of 5, then a word five tokens apart from the target is treated the same as an adjacent word |
| GloVe |  | harmonic function | a context word three tokens away will be counted as $\frac{1}{3}$ |
| word2vec | uniformly sampling the actual window size between 1 and $L$ for each token | weight = distance / window size | a size-5 window will weigh its contexts by $\frac{5}{5}, \frac{4}{5}, \frac{3}{5}, \frac{2}{5}, \frac{1}{5}$

#### Subsampling (sub)

**Subsampling** is a method of:
- diluting very **frequent** words
- akin to removing **stop-words**


Randomly removes words that are more frequent than some **threshold $t$** with a probability of $p$, where $f$ marks the word's corpus frequency.

$$
p = 1 - \sqrt{\frac{t}{f}}
$$

We use $t=10^{-5}$ in our experiments.

> - **dirty** subsampling: remove tokens **before** the corpus is processed into word-context pairs
> -**clean** subsampling: remove tokens **after** the corpus is processed into word-context pairs
> The performances of dirty and clean subsampling are comparable

#### Deleting Rare Words (del)

Word2vec removes these tokens from the corpus **before** creating context windows.

This variation **narrows** the distance between tokens

> Small effect on performance $\rightarrow$ not investigate its effect in this paper

### Association Metric Hyperparameters

Two variations of the PMI association metric:

#### Shifted PMI (neg)

$$
\mathrm{SPPMI}(w, c) = \max(\mathrm{PMI}(w,c) - \log k, 0)
$$

$k$ acts as a prior on the probability of observing a positive example versus a negative example.

> e.g. A higher $k$ means that negative examples are more probable

#### Context Distribution Smoothing (cds)

In word2vec, negative examples (contexts) are sampled according to a **smoothed** unigram distribution.

All context counts are raised to the **power of $\alpha$**

$$
\mathrm{PMI}_\alpha(w, c) = \log\frac{\hat{P}(w, c)}{\hat{P}(w)\hat{P}_\alpha(c)} \\
\hat{P}\alpha(c) = \frac{\#(c)^\alpha}{\sum_c\#(c)^\alpha}
$$

Enlarging the probability of sampling a rare context, which in turn reduces the PMI of $(w, c)$ for any $w$ co-occurring with the rare context $c$.

> This novel variant of PMI is very effective

## Post-processing Hyperparameters

### Addiing Context Vectors (w+c)

==To be continued...==