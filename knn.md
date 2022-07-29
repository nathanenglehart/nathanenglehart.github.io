---
layout: home2
permalink: /knn/
title: K Nearest Neighbors Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The K nearest neighbors (KNN) classifier is a highly useful and popular tool for applications of data mining. KNN is a non-probabalistic supervised machine learning algorithm that classifies vectors containing data and a classification. 

## KNN Classification Algorithm

To run, the KNN algorithm requires three inputs:

1. A set of test vectors for which to determine classifications
2. A set of train vectors for which to compare test vectors against
3. Some pre-determined $1 \leq K \leq$ length(train vectors)

Then, for each test vector, KNN: 

1. Computes the distance between a test vector and all vectors in the train dataset
2. Determines which train vectors are the $K$ closest vectors to the test vector
3. Determine the plurality class (the most frequently occuring class) of the $K$ closest vectors
4. Assign the plurality class to the test vector

In KNN, distance can be measured using various $L^\mathcal{p}$ norms. Most commonly, Euclidean Distance is used, which is given by: \\[ d(p,q) = \sqrt{\sum_{i=1}^n (q_i - p_i)^2} \\]
However, Chebyshev Distance and Manhattan Distance are also frequently used.

## KNN with N-Fold Cross Validation

In order to draw more accurate predictions than those one might come to by using an arbitrary K value, we can utilize N-fold cross validation to determine the error for an array of $K$ values. Then, finding the $K$ value which minimizes error shows us the optimal $K$ value which best fits the data. \
\
For each run, N-fold cross validation requires the same three inputs as KNN. It also requires an $N$ value, which represents the number of folds that we will split the train data into. $N = 10$ is a common choice. Then, N-fold cross validation: 

1. Randomly splits the set of train vectors into $N$ groups.
2. Assigns $N-1$ of the groups to be a *new* train set and $1$ of the sets to be a *new* test set
3. Put error $= 0$.
4. Then, until every set has served as a *new* test set ($N$ runs):
	- Runs KNN on the *new* train and *new* test
	- Computes the misclassification rate of the computed *new* test classifications and the true *new* test classifications  
	- Adds the computed misclassification rate to the error term
	- Assigns the *new* test to be a different vector
5. Return mean error $=$ error / $N$

where miscalculation rate can be computed with:
\\[ \text{Misclassification Rate} = \frac{1}{N} \sum_{n} I (\hat{t}_n \neq t_n) \\]
with $\hat{t}$ representing computed classifications and $t$ representing true classifications.\
\
Full code for implementation written in C++ available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/knn-cpp-241">https://github.com/nathanenglehart/knn-cpp-241</a>.

## Visualization

The 1936 Iris dataset, a classic dataset used for classification algorithms, contains 150 flowers classified by species and their respective sepal and petal measurements (test available [here](); train available [here]()). \
\
Using a python script, we can write:
```python
#!/usr/bin/env python3

import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv("iris-test.csv", sep=',')
data.columns=["classifications", "sepal length", "sepal width", "petal length", "petal width"]
sns.pairplot(data, hue="classifications", palette=['tab:blue','tab:orange','tab:green'])
plt.show()
```
Which graphs the whole test dataset's information with its true classifications:
<img src="/images/pairplot-iris-true.png" alt="/images/pairplot-iris-true.png"/> \
\
Then, utilizing the aforementioned C++ implementation above, we can run KNN. First, to clone and compile KNN, we can run:
```bash
git clone https://github.com/nathanenglehart/knn-cpp-241
cd knn-cpp-241
make
```
Then, to test run KNN on our datasets, we can run:
```bash
./knn-cli iris-train.csv iris-test.csv -e -v
```
For $K = 1$ to $K = 135$, N-fold cross validation computed the following misclassification rates with $N = 10$: \
\
<img src="/images/misclassification_rate_across_folds_iris.png" alt="/images/misclassification_rate_across_folds_iris.png"/> \
\
choosing $K = 3$ (with an average misclassification rate of 0.03) as the optimal value for minimizing the misclassification rate across folds. \
\
Then, using the predicted classifications on the test dataset 
<img src="/images/pairplot-iris-true.png" alt="/images/pairplot-iris-preds.png"/> \
\
We can see that the predicted classifications are very similar to the true classifications!

### References

Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Fisher, R.A. (1988). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/iris">https://archive.ics.uci.edu/ml/datasets/iris</a>. Irvine, CA: University of California, School of Information and Computer Science.
