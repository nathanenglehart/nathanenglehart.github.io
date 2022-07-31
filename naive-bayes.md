---
layout: home2
permalink: /naive-bayes/
title: Naive Bayesian Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---
The Naive Bayesian Classifier Algorithm is a family of probabalistic supervised machine learning algorithms that assumes each feature is independent of other features inside a feature vector. 

### Multinomial Naive Bayes

Perhaps the most common implementation of Naive Bayes is Multinomial Naive Bayes. Multinomial Naive Bayes is useful for classifying vector rows with categorical data (nominal or ordinal) as features. To compute the probability of a test vector with features $x_1 ... x_d$ belonging to classification $y$, Multinomial Naive Bayes uses the equation:
\\[ P(y,x_1 ... x_d) = P(y) \prod^{d}_{i=1} P_j (x_i|y) \\]
where $P(y)$ and each $P_j$ are computed using a train dataset. More specifically, Multinomial Naive Bayes:

1. First calculates $P(y)$ by dividing the frequency of each classification in the train data by the number of vectors in the train data
2. Computes the product sum of the conditional class probabilities of each given feature $x_i$ occuring with classification $y$
	- To do so, for each feature column $i$ in the train data: divide the frequency of vectors with the feature $x_i$ *and* classification $y$, by the length of the feature column
4. Multiply the result of the first and second steps

By running this equation for each possible classification $y$, Multinomial Naive Bayes is able to assigns the classification with maximal probability as the predicted classification.

### Laplace Smoothing for Multinomial Naive Bayes

Multinomial Naive Bayes faces an issue if individual features $x_i$ are missing from the classification data for some classification $y$ since this will lead to  frequency based probability estimates becoming zero. This will set our product sum to zero and greatly hinder the accuracy of the classifier. \
\
This problem can be solved using a technique called Laplace Smoothing. Laplace Smoothing is a slight modification to the Naive Bayes algorithm which eliminates the zero frequency problem by adding $1$ to each frequency of vectors with the feature $x_i$ *and* classification $y$. 

### Multinomial Naive Bayes Visualization

Under construction.

### Gaussian Naive Bayes

Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes. It is highly useful for classifying vector rows with continuous feature variables. The equation for Gaussian Naive Bayes is given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma$ represents standard deviation and $\mu$ represents mean.

### Gaussian Visualization

To perform a run of Gaussian Naive Bayes, let us use a synthetic dataset classifying rows as 0, 1, 2, 3, or 4 using five feature vectors (train available [here](https://nathanenglehart.github.io/data/synth-train-blobs-5.csv); test available [here](https://nathanenglehart.github.io/data/synth-test-blobs-5.csv.csv)). \
\
To visualize the data, using python we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv('synth-test-blobs-5.csv', sep=',', header=None)

data.columns = ['classification','x_1','x_2','x_3','x_4','x_5']
sns.pairplot(data, hue='classification',palette=['tab:blue','tab:orange','tab:green','tab:red','tab:purple'])
plt.show()
```
Which displays: \
\
<img src="/images/pairplot-synth-nb_ex.png" alt="/images/pairplot-synth-nb_ex.png"> \
\
Then, again utilizing `naive-bayes-cli`, to run Gaussian Naive Bayes, we can write:
```bash
$ ./naive-bayes-cli synth-train-blobs-5.csv synth-test-blobs-5.csv -g -v # Verbose
$ ./naive-bayes-cli synth-train-blobs-5.csv synth-test-blobs-5.csv -g > preds.csv # Store results
```
By storing the results in a csv file to use on the test dataset we can write another python script:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv('synth-test-blobs-5.csv', sep=',', header=None)
results = pd.read_csv("preds.csv", header=None, sep=",")
results = results.iloc[:,0]
data.iloc[:,0] = results

data.columns = ['classification','x_1','x_2','x_3','x_4','x_5']
sns.pairplot(data, hue='classification',palette=['tab:blue','tab:orange','tab:green','tab:red','tab:purple'])
plt.show()
```
Which displays: \
\
<img src="/images/pairplot-synth-nb_ex-preds.png" alt="/images/pairplot-synth-nb_ex-preds.png"> \
\
As such, we can see that the predicted classifications are very close to the true classifications (in fact, running in verbose shows an error rate of ~0.1% on any given run)! 

### Notes

Full code for both implementations available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>.

### References
Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling, 2013. Springer.
