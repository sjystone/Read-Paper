# Token Merging: Your  ViT But Faster

## Introduction

Since ViT appeared, Transformer-based vision models have been pushed to an unprecedented height. However, the calculation efficiency is always a trouble for training and deploying. Previous works proposed token pruning[1,2,3,4] to enable a faster model due to the input-agnostic nature of Transformers. Yet, it has some restrictions, such as the number of tokens we can prune is limited by the information loss, and some models can't be applied to speed up training. Other works, such as Token Pooling[5] which uses K-means, are slow and bring additional calculations. In this paper, the authors propose Token Merging (ToMe), a method to combine tokens, rather than prune them, which increases the training and inferencing speed.

## Approach

The Token Merging is applied between the attention and MLP branches of each Transformer block. First, the authors attempt to calculate the token's similarity between K/Q/V through different distance functions and apply different head aggregations to fuse the tokens. The results show that using a cosine similarity between the keys of each token and applying concatenation (which has close performance with mean function but faster) to fuse the similar tokens is the best choice. Then, the authors propose Token Merging based on Bipartite Soft Matching, a gradual and fast way to reduce *r* tokens in each Transformer layer. 

<p align="center">
<img src="./figures/fig1.jpg" alt="Bipartite Soft Matching" width="500" style="zoom:10%;" />
</p>



The algorithm is as follows:

1. Partition the tokens into two sets **A** and **B** of roughly equal size.
2. Draw one edge from each token in **A** to its most similar token in **B**.
3. Keep the *r* most similar edges.
4. Merge tokens that are still connected (e.g., by averaging their features).
5. Concatenate the two sets back together.

After merging, some tokens which consist of no less than one token should be given more weight before softmax. Thus the authors optimize the attention operation. The mathematical expression is as followed, *s* represents the size of each token (number of patches the token represents). 
$$
A=softmax({QK^T}/\sqrt{d}+logs)
$$

## Experiments

TBW

## Reference

[1] DynamicViT: Efficient Vision Transformers with Dynamic Token Sparsification

[2] AdaViT: Adaptive Vision Transformers for Efficient Image Recognition

[3] A-ViT: Adaptive tokens for efficient vision transformer

[4] SPViT: Enabling Faster Vision Transformers via Latency-aware Soft Token Pruning

[5] Token pooling in vision transformers