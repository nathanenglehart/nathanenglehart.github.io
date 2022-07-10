---
layout: home2
permalink: /knn/
title: K-Nearest Neighbors Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The K nearest neighbors (KNN) classifier is a highly useful and popular tool for applications of data mining. KNN is a relatively simple, supervised machine learning algorithm that classifies vector data points, each of which stores data and a classification, by computing the distance between an input vector and all other vectors. Upon examining all vectors in the dataset, KNN determines the plurality class of the K nearest vectors and assigns this ideal classification to the input vector. In KNN, distance can be measured using various $L^\mathcal{p}$ norms such as the Manhattan distance formula, the Chebyshev distance formula, and most commonly, the Euclidean distance formula which is given by: \\[ d(p,q) = \sqrt{\sum_{i=1}^n (q_i - p_i)^2} \\]
Next, in order to draw more accurate predictions than those one might come to by using an arbitrary K value, running N-fold cross-validation on different values of K and comparing the results in can be used to compute the K with the smallest error value. This will be the K that best fits the data. In other words, for each K value from KNN, one should split the data into N folds of data. Then, for N runs, assign one of these folds to be the test/validation data and the rest of the N-1 folds to be the training data, alternating the test/validation data on each run so that every fold will serve as the validation data once. For each of these tests, run KNN on all of the training folds, using the validation data as input. Then calculate the misclassification rate for each train fold output on the sealed validation fold. Averaging these misclassification rates give the misclassification rate for the K value tested in this run of N-fold cross-validation. Miscalculation rate can be computed with:
\\[ \text{Misclassification Rate} = \frac{1}{N} \sum_{n} I (\hat{t}_n \neq t_n) \\]
where $\hat{t}_n$ represents the classifier's output for a training point $n$, and $I(A)$ returns $1$ if $A$ is true and $0$ if $A$ is false. Whichever K value has the least average misclassification rate across N folds is the ideal K. This K is the one that should be used when adding a new vector to assign a classification. \
\
Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/knn-cpp-241">https://github.com/nathanenglehart/knn-cpp-241</a>.
