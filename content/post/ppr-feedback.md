+++
title = "Relevance Feedback with Personalized Page Ranking"
date = "2019-11-20T18:00:00+07:30"
tags = ["machine learning", "statistics"]
draft = false
author = "Amitabh Das"
+++

# Page Ranking Algorithm

Page Ranking Algorithm was briefly described in the previous [post](/post/ppr-rec)
This post details experimenting with relevance feedback using PPR.

## Using feedback from the user to fine tune our Recommendations

We already have our graph from before.

In this scenario, we assume we have some sort of preferences of the user. Maybe the user has viewed some images before and marked some of the images as relevant and some as irrelevant.

We want to show the user the images that are more relevant to him and try to avoid the irrelevant ones. But the whole process is imprecise, since user feedback depends on various factors for example – trends and patterns change over time. Also, similarity of images is very subjective. Creating an objective algorithm to satisfy a subjective criterion is hard. But maybe we can get close.

In personalized page ranking, we create a seed vector with equal importance to the vertices of corresponding images preferred by the users. In this case, we have two labels - "relevant" and "irrelevant". We have a third implicitly provided label - "everything else". The users prefer the relevant images, so we gave them a 100% weightage. They definitely don't want the irrelevant images, so we give them a 0% weightage. Everything else however, is uncertain. The users may or may not prefer them. We give all the other images a 50% weightage because they all have a 50% chance of being picked as relevant or irrelevant.

We then use the power iteration method to find the steady state of the images. The images are ordered based on the probabilities contained in the steady state and then provided as results.

## Interpretation of results from feedback to PPR

A score is associated with every image ranked by this PPR feedback system. The score associated with an image A represents the steady state probability that a random walker with high preference to relevant images, medium preference to unknown images and zero preference to irrelevant images, would reach the vertex associated with image A i.e. the affinity of relevant images and unknown images (to a certain degree) with respect to image A.

Another post coming up with results from another experiment with PPR.
