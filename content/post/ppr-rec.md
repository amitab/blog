+++
title = "Recommendation with Personalized Page Ranking"
date = "2019-11-05T18:00:00+07:30"
tags = ["machine learning", "statistics"]
draft = false
author = "Amitabh Das"
+++

# Page Ranking Algorithm

Page Ranking Algorithm is a graph-based algorithm. It’s an interesting technique to learn of the importance of nodes in a graph. Not only that, this algorithm can be modified to do cool things like finding importance based on custom criteria.

This semester, I learnt about Page Ranking in a graduate course called “Multimedia and Web Databases”. It’s a project heavy course, and it gives a good opportunity to experiment different use cases with different algorithms.

The link to our project is [here](https://github.com/ashmgarv/MWDB).

## Recommending similar images using Page Ranking.

Let’s start with the most straight forward use case of Page Ranking. Feature vectors are used to describe images. These can be any sort of features extracted from images. In this experiment, [SIFT](https://en.wikipedia.org/wiki/Scale-invariant_feature_transform) features of the image are used.

## Building a graph from feature vectors

Our project had 6 tasks. In task 3, Personalized PageRank algorithm is to be used in order to find images similar to a given image.

First, we must construct a similarity graph. We use the SIFT features along with some metadata of the images to construct an image-feature matrix. Euclidean distance metric is used to calculate the similarity between every pair of images. This forms the adjacency matrix, which can be though of as a graph with images as nodes and the similarity values as weights of the edges between each node.

In our adjacency matrix, everything is connected to everything. That will not yield good results, since that will mean almost every node is of approximately the same importance in the graph.

Here, we define a constant set of connections outgoing from every node – `k`. Let’s take `k` to be 5. Now we have about 400 images, each image A having connections to top `k` similar images (excluding itself, so no self-loops). This graph is represented as a matrix and is used as the transition matrix.

This transition matrix is then normalized such that the outgoing weights from every vertex adds up to 1, which will help us to interpret the edge weight as probabilities. You can think of this as “_probability of going to image B from image A_” based on how similar the images are.

## The Mathematical representation of steady state

$$ \vec{\pi} = \vec{\pi} T \alpha + (1 - \alpha) \vec{s} $$

Where,
* $$ \vec{\pi} $$ is the steady state vector
* $$ T $$ is the transition matrix (or in our case, the matrix representation of the graph we created)
* $$ \alpha $$ is the probability of random walk
* $$ \vec{s} $$ is the seed vector

More information about this here. But the gist is this – imagine a person at one of the image nodes. If this person randomly moves from node to node though the edges we created between the nodes, we want to find out where the person is most likely to end up, based on the probabilities we set up earlier. That’s what the first half of the equation does. A steady state vector will be of the same size as the number of nodes (images) that we have and each value in the vector will provide us with the importance associated with each of the nodes. This is the core of Page Ranking.

To calculate the steady state probability, we can use two ways:
1. Power Iteration method (more info [here](https://en.wikipedia.org/wiki/PageRank#Power_method))
2. Using inverse of the matrix (more info [here](https://en.wikipedia.org/wiki/PageRank#Algebraic))

## That’s great, but what if our imaginary person in the graph had some preferences?

Turns out, we can take this person’s preferences into consideration. Instead of just walking from node to node, we can assign a probability of randomly “teleporting” to a node preferred by the person in our graph (in this case, an image preferred by the person). So, now we have a probability of random walk and a probability of teleporting to a preferred node. This will provide more importance to the nodes around the persons preferred nodes. Intuitively, this is like how a user might watch a preferred YouTube video, then keeps going down the suggestions rabbit hole, and then switches back to his preferred video again, repeating this process for eternity.

We can simulate the preferences using a seed vector in the equation. The seed vector is populated such that the vertices corresponding to the images preferred, have equal weightage by the walker for teleportation. Every other image preference is set to 0 in this seed vector.

$$
\vec{s_i}= 
\begin{cases}
    1, & \text{if } i = \text{index of the image provided}\\
    0, & \text{otherwise}
\end{cases}
$$

Then the math takes care of the rest.

## Interpretation of the results

Using the power iteration method, we start with the seed vector as the initial state and attempt to find a steady state based on the above formula. The steady state obtained by this process is an approximation, not just because of the iterative nature of the algorithm, but also because of floating point approximations. In fact, the stopping condition compares the previous state to the current state using a small tolerance value epsilon. Based on observations, this is much faster than trying to calculate an inverse of a large matrix, which is often the case in such situations.

The final steady state of every query has a high probability value for vertices closely connected with the input images. With an extremely high alpha value, the vertices directly connected to the preferred images have a comparatively higher probability values than the other vertices which are not directly connected to the provided images. A lower value of alpha could have increased the steady state probabilities of vertices indirectly connected to the vertices corresponding to the provided images.
The value chosen for k also influences the results. If an edge from image B directly connected to any one of the provided image A were cut off, despite having a high similarity to image A, the steady state probability of the vertex corresponding to image B would not be as strong.

Of course, the chosen feature model used to calculate the similarities also influence the results.

More experiments with Page Ranking Algorithm coming up next.
