---
layout: post
title: GloVe Global Vectors for Word Representation
subtitle: 论文笔记
categories: ArtificialIntelligence
tags: book-report artificial-intelligence machine-learning natural-language-processing
---

# Paper Reading: GloVe: Global Vectors for Word Representation

> Pennington, J., Socher, R., & Manning, C. D. (2014, October). Glove: Global vectors for word representation. In Proceedings of the 2014 conference on empirical methods in natural language processing (EMNLP) (pp. 1532-1543).

New global logbilinear regression model that combines the advantages of the two major model families in the literature: **global matrix factorization** and **local context window** methods.

**Advantages**

* Efficiently leverages statistical information by training only on the nonzero elements in a word-word co-occurrence matrix.
* Produces a word vector space with meaningful substructure

## Related Work

There are two main model families for learning word vectors:

<table>
  <tr>
    <th>model family</th>
    <th>example</th>
    <th>advantages</th>
    <th>disadvantages</th>
  </tr>
  <tr>
    <th>Global matrix factorization methods</th>
    <td>latent semantic analysis(LSA)</td>
    <td>efficiently leverage statistical information</td>
    <td>do poorly on the word analogy task</td>
  </tr>
  <tr>
    <th>Local context window methods</th>
    <td>the skip-gram model</td>
    <td>do better on the word analogy task</td>
    <td>poorly utilize the statistics of the corpus</td>
  </tr>
</table>

## The GloVe Model(GloVe, for Global Vectors)

> The statistics of word occurrences in a corpus is the primary source of information available to all unsupervised methods for learning word representations.

### Notation

* $X$: the matrix of word-word co-occurrence counts
  * $X_{ij}$ tabulate the number of times word $j$ occurs in the context of word $i$
  * $\sum_k X_{ik}$: the number of times any word appears in the context of word $i$
* $P_{ij} = P(j|i) = X_{ij}/X_i$: the probability that word $j$ appear in the context of word $i$

### Advantage

* Compared to the raw probabilities
  * the ratio is better able to distinguish **relevant** words from **irrelevant** words
  * the ratio is better able to discriminate between the two **relevant** words

### Method

#### Staring point

The appropriate starting point of word vector learning should be with **ratios of co-occurrence probabilities** rather than the probabilities themselves.

The ratio $P_{ik}/P_{jk}$ depends on three words $i$, $j$, and $k$:

$$
F(w_i, w_j, \tilde{w_k}) = \frac{P_{ik}}{P_{jk}}
$$

* $w\in \mathbb{R}^d$: word vectors
* $\tilde{w}\in \mathbb{R}^d$: separate context word vectors
* $F$: may depend on some as-of-yet unspecified parameters
* $\frac{P_{ik}}{P_{jk}}$: extracted from the corpus

#### Selecting $F$

1. let $F$ to encode the information present the ratio $P_{ik}/P_{jk}$ in the word vector space
  Restrict our consideration to those functions $F$ that depend only on the **difference** of the two target words
  $$
  F(w_i-w_j, \tilde{w_k}) = \frac{P_{ik}}{P_{jk}}
  $$

2. Prevent MF from mixing the vector **dimensions**
  $$
  F((w_i-w_j)^T \tilde{w_k}) = \frac{P_{ik}}{P_{jk}}
  $$

3. Require $F$ be a **homomorphism** between the groups ($\mathbb{R}$, +) and ($\mathbb{R}_{>0}$, $\times$)
  $$
  F((w_i-w_j)^T \tilde{w_k}) = \frac{F(w_i^T \tilde{w_k})}{F(w_j^T \tilde{w_k})}
  $$
  Therefore,
  $$
  F(w_i^T\tilde{w_k}) = P_{ik} = \frac{X_{ik}}{X_i}
  $$
  Thus, $F = \exp$, or, $w_i^T\tilde{w_k} = \log(P_{ik})=\log(X_{ik}) - \log(X_i)$

4. Adding an additional bias $\tilde{b_k}$ for $\tilde{w_k}$ restores the symmetry
  $$
  w_i^T\tilde{w_k} + b_i + \tilde{b_k} = \log(X_{ik})
  $$

5. Address problem (the logarithm diverges whenever its argument is zero) by proposing a new weighted least square regression model.
  $$
  J = \sum_{i,j=1}^{V} f(X_{ij})(w_i^T\tilde{w_j}+b_i+\tilde{b_j}-\log X_{ij})^2
  $$
  where $V$ is the size of the vocabulary.
  Regulations of the weighting function $f$:
  - $f(0)=0$. If $f$ is viewed as a continuous function, it should vanish as $x\rightarrow 0$ fast enough that the $\lim_{x\rightarrow 0} f(x)\log^2 x$ is finite
  - $f(x)$ should be non-decreasing so that rare co-occurrences are not overweighted
  - $f(x)$ should be relatively small for large values of $x$, so that frequent co-occurrences are not overweighted
  $$
  f(x) = 
    \left\{
      \begin{array}{ll}
      (x/x_{\max})^\alpha & \mathrm{if}\quad x < x_{\max}\\
      1 & \mathrm{otherwise}
      \end{array}
    \right.
  $$
  > $\alpha = 3/4$ gives a modest improvement over a linear version with $\alpha = 1$
  ![](/assets/images/post/paper/weightingF.png)