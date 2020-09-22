+++
title = "Classification with Personalized Page Ranking"
date = "2019-12-08T18:00:00+07:30"
tags = ["machine learning", "statistics"]
draft = false
author = "Amitabh Das"
+++

# Personalized Page Ranking

Page Ranking Algorithm was briefly described in the previous [post](/post/ppr-rec).
The previous few posts experimented with the used of PPR in recommendations and relevance feedback. In this post, classification of images is attemped using PPR.

## _Can_ we even classify images using this algorithm?

PPR is a graph algorithm that can be used to find the importance of nodes in the graph. So how can this algorithm be used to classify images?

In this project, we use the [11k images dataset](https://sites.google.com/view/11khands) to try and classify unlabeled images as either dorsal or palmar. We can try to extend on the original idea of Page Ranking to find the importance of nodes in our graph. Maybe we can try to find the importance of Dorsal and Palmar of an unlabeled image somehow …?

_Turns out, we can!_

With a little inspiration from [here](https://www.cs.cmu.edu/~jypan/publications/KDD04CrossModalCorrelation.pdf), we make one small addition to the graph constructed in the previous posts.

Along with the existing nodes for each of the images, we add two new nodes to the graph corresponding to the two labels of classification - 'dorsal' and 'palmar'. Let's call them the dorsal label node and palmar label node. The idea is to see how important these dorsal label node and palmar label node are to a person randomly walking and/or teleporting in the graph based on the probability weights assigned to the edges of our graph. This idea could be expanded to multiple labels.

Then we could have a walker in the graph, with the preference to an unlabelled image (that we want to label) and see how important the dorsal label node and the palmar label node is to this walker. Or in other words, we can find the _affinity_ of an unlabelled image to each of the labelled nodes.

## But how do we connect these two nodes to the rest of the graph?

One way would be to choose equal probabilities for the outgoing edges and incoming edges between the label node and the nodes corresponding to the images of that label i.e. the dorsal label vertex connected to the dorsal images with equal probability.

Another way is to use the Page Ranking algorithm with to find the steady state vector of the labelled dorsal images. This gives us the importance of each dorsal labelled image among all the dorsal labelled images. This degree of importance can be used as the edge weight connecting to and from the dorsal image nodes to the dorsal label node. We can perform a similar calculation for palmar images.

## The affinity of nodes in a graph

We can define affinity of the label node L to an unlabeled image I as follows:
> The importance of node L with respect to node I is the steady-state probability corresponding to node L in the steady-state vector of random walk with restarts.


Which is rephrasing of the definition given in [this](https://www.cs.cmu.edu/~jypan/publications/KDD04CrossModalCorrelation.pdf) paper.
In this project, we try to find the affinity of an unlabeled image to the dorsal label vertex and the palmar label vertex.

The inverse of the matrix representation of the graph generated above is pre-calculated. Using power iteration, while faster that calculating the inverse of a large matrix, does not scale with the number of images to be labelled.

Now that we have our graph, we create a seed vector for every unlabeled image like shown below.
$$
\vec{s_i}=\begin{cases} 1, & \text{if } i = \text{index of the image to be labelled}\\\\ 0, & \text{otherwise} \end{cases}
$$

Next, the seed vector of each image is multiplied with the inverse matrix. Then, the steady state probability of the dorsal label node is compared to the steady state probability of the palmar label node. This will help us determine the dorsal affinity and the palmar affinity of that image.

## Interpretation of the classification results

The value of `k`, feature model, feature reduction technique and number of latent semantics chosen plays a huge role in the accuracy of the results.
A steady state is generated using a seed vector specific to an unlabeled image. The steady state value of the dorsal label node will provide a probability of a random walker ending up at the dorsal label node with restarts from the preferred node (in this case, this preferred node corresponds to our unlabeled image).
