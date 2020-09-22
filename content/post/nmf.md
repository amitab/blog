+++
title = "Non-Negative Matrix Factorization"
date = "2019-10-02T18:00:00+07:30"
tags = ["machine learning", "statistics"]
draft = false
author = "Amitabh Das"
+++

# Non-Negative Matrix Factorization

I recently took the course “_Multimedia and Web Databases_” and we learnt to use feature reduction techniques to reduce the dimensionality of the multimedia data. This is done so that we can scan the entire database (_relatively_) fast for approximately similar data.

One of our project tasks involved using __NMF__ to find top ‘k’ images closest to a given image in the Euclidean space. Our project can be found [here](https://github.com/ashmgarv/MWDB).

The image feature we used for this task was Color Moments (More information about that [here](https://en.wikipedia.org/wiki/Color_moments)).

Color Moments contains `[mean, std, skew]` for every channel _YUV_ color model of an image. So, an image would have `3 × 3 = 9` features. If the image is divided into windows of `100 × 100`, for every image of size `1600 × 1200`, there would be `1728` features.

Now all we had to do is apply feature reduction techniques on it.

However, there was a small problem. Color moments uses skew as a part of its feature set. Skew could be negative. _NMF_ is Non-Negative Matrix factorization. But that’s not a problem, right? Just normalize and we should be good, right?

I tried different ways:
1. Shift all the values in the feature vector by the least negative value in the database, so that all the values in the database is >= 0.
2. Shift only the skew column by the least negative value in the database.
3. Normalize only the skew column of the database.
4. Normalize all the values.
5. Ignore the skew values (missing out on a whole lot of discrimination power)

The approaches I tried gave me completely different results for my task. Of course, one could argue that similar images can be subjective to the user. But I had hard evidence – the metadata of the image database. We were using the 11k Hand image [dataset](https://sites.google.com/view/11khands). Logically, given an image of a hand of a subject, the hand images of the same subject should show up as the top closest. This suggested that approach 1 had the closest matching results to the metadata among all. Even so, it was far from ideal.

So even shifting the entire database linearly changes the results. I also noticed that the smaller the shift, the closer to the ideal result.

Then my goal is to try and reduce the shift. Which means, I somehow needed to reduce how negative the skew was.

_Why is there a negative skew?_

In our case, that’s because there is a hand on a white surface. The transition from the hand to white background causes the values to be skewed to the right since the RGB value for white is 255.  So, in a window, if there is a transition from a part of the hand to a very large amount of white surface, there will be a large negative skew.

_Can we try to linearly transform the features so that the distances of the data are preserved and gets rid of the negative values?_

As in approach 1, I tried to simply add the absolute of the lowest negative value to all features such that the lowest negative value present in the data matrix becomes a zero. The coordinates of the data points are shifted but the relative distances are preserved.
However, NMF transformation does some sort of operation that amplifies this linear transformation. The larger the shift, the more the results change.

Now the challenge is - _how to reduce the shift to transform the negative values to zero?_

Inverting the image would change all the 255 values to 0. This would help reduce the number of negative skew values. Based on the results, I saw that the number of negative skew values and the lowest negative values have also reduced. So, the value to be added to transform the dataset to convert the lowest negative value to zero also has reduced.

Minimal shift. Closer to the ideal result. But never close enough. 
