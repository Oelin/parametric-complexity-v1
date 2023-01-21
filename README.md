# PCM

First attempt at estimating the parametric complexity (minimum descrption length) of binary classifier models. The method used here requires further development to reduce variance in produced results. Nonetheless, it provides a first approximation of parametric complexity, which is a useful metric for model selection.


## Introduction

The *parametric complexity* of a model family $\mathcal{M}$ is a measure of how much information it tends to *memorise* when fitted using a particular estimator. In the case of MLE, it measures how susceptible the *most likley* model in the family is to overfitting. Then it can be used to make sound comparisons between the *probabilities* of different model families as follows. Let

### $$L_{\text{nml}}(\mathcal{M}|D) = L_{M^*}(D) + L_\mathcal{M}(\mathcal{M})$$

where:

- $\mathcal{M}$ is a model family amoung some set of candiates $\mathcal{M}_1,\dots,\mathcal{M}_n$.

- $D$ is a dataset from some family $\mathcal{D}$.

- $L_{\text{nml}}$ is the *posterior* length of $\mathcal{M}$ given $D$.

- $L_{M^\*}$ is the *likelihood* length of $D$ under the MLE estimate $M^*$. 

- $L_\mathcal{M}$ is the *prior* length of $\mathcal{M}$, i.e. its parametric complexity.

Then the *most probable* model family is the one with shortest posterior length. To compute this length, we first need to compute $L_{M^*}$ and $L_\mathcal{M}$. Luckily, this is rather straightforward and avoids the arbitrariness of crude MDL.

To compute $L_{\mathcal{M}}$, we use the following algorithm:

1. Pick a dataset from $\mathcal{D}$, such that each data point is chosen uniformly at random. Neccessarily, the chance that at least $k$ bits can be removed from this dataset is exponentially small in $k$.

2. Compute $L_{M^\*}$ for this dataset and subtract it from the dataset's original size in bits. Since the dataset cannot feasibly be compressed, this measures how much information is simply *memorised* in $M^*$.

3. Repeat steps 1-2 several times to obtain an average for how many bits are memorised.

4. Return this average as the parametric complexity of $\mathcal{M}$.

> Note 1: for step 2, we assume that the number of bits removed from the dataset is sufficiently large, such that the probability that those bits *were memorised* is very close to 1. For example, if 50 bits are removed, the probability that they were memorised is $1 - 2^{-50} =  0.9999999999999991$. It's exeedingly likely that those bits were simply transfered to the model rather than genuinly compressed.
>
> Note 2: this algorithm estimates parametric complexity w.r.t. a particular dataset family $\mathcal{D}$. This is a specification for how many features and examples are in the dataset. It's important that $D \in \mathcal{D}$ when we calculate $L_M^*$, i.e. it must have the same number of features and examples. 


## Applications

Parametric complexity (and approximations to it) play a key role in so called *refined MDL* model selection. Unlike other most other model selection criteria, parametric complexity allows for sound comparisons between different model families and takes into account more than just *the number of parameters* used. Rather, it considers essentially how flexible a model (and learning algorithm) are as a whole; which is more useful for actually estimating generalization performance. 
