---
layout: home2
permalink: /naive-bayes/
title: Naive Bayesian Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---
The Naive Bayesian Classifier Algorithm is a family of probabalistic supervised machine learning algorithms that assumes each feature is independent of other features inside a feature vector. 

### MLE Naive Bayes

One implementation of Naive Bayes uses maximum likelihood estimation (MLE). MLE Naive Bayes is useful for classifying vector rows with ordinal data as features. To compute the probability of a test vector with features $x_1 ... x_d$ belonging to classification $y$, MLE Naive Bayes uses the equation:
\\[ P(y,x_1 ... x_d) = P(y) \prod^{d}_{i=1} P_j (x_i|y) \\]
where $P(y)$ and each $P_j$ are computed using a train dataset. More specifically, MLE Naive Bayes:

1. First calculates $P(y)$ by dividing the frequency of each classification in the train data by the number of vectors in the train data
2. Computes the product sum of the conditional class probabilities of each given feature $x_i$ occuring with classification $y$
	- To do so, for each feature column $i$ in the train data: divide the frequency of vectors with the feature $x_i$ *and* classification $y$, by the length of the feature column
4. Multiply the result of the first and second steps

By running this equation for each possible classification $y$, MLE Naive Bayes is able to assigns the classification with maximal probability as the predicted classification.

### Laplace Smoothing for MLE Naive Bayes

MLE Naive Bayes faces an issue if individual features $x_i$ are missing from the classification data for some classification $y$ since this will lead to  frequency based probability estimates becoming zero. This will set our product sum to zero and greatly hinder the accuracy of the classifier. \
\
This problem can be solved using a technique called Laplace Smoothing. Laplace Smoothing is a slight modification to the Naive Bayes algorithm which eliminates the zero frequency problem by adding $1$ to each frequency of vectors with the feature $x_i$ *and* classification $y$. 

### MLE Naive Bayes Visualization

Under construction.

<!--
To perform a run of MLE Naive Bayes, let us use a synthetic dataset classifying rows as 0 or 1 using three feature vectors (train available [here](https://nathanenglehart.github.io/data/synth_cat_train-3.csv); test available [here](https://nathanenglehart.github.io/data/synth_cat_test-3.csv)). \
\
Using a python script, we can write:
```python

```







On Unix based operating systems such as any distribution of GNU + Linux or Mac OS, the full implementation can be loaded with
```bash
git clone https://github.com/nathanenglehart/naive-bayes-cpp-241
cd naive-bayes-cpp-241
make
```
A help menu is available with:
```bash
./naive-bayes-cli -h
```
Now, using a synthetic dataset classifying rows as 0 or 1 using three feature vectors (train available [here](https://nathanenglehart.github.io/data/synth_cat_train-3.csv); test available [here](https://nathanenglehart.github.io/data/synth_cat_test-3.csv)), we can run:
```bash
./naive-bayes-cli synth_cat_train-3.csv synth_cat_test-3.csv -m -v
```
Which, using 10 fold cross validation, returns an error rate of 5.2% error rate. \
\
Now, to graph the results we can first use bash and regex to save the results to a csv:
```bash
./naive-bayes-cli synth_cat_train-3.csv synth_cat_test-3.csv -m | sed -E 's/.*(.{1})$/\1/' | sed '$d' | awk '{print}' ORS=', ' | sed '$ s/..$//' > synth_cat_preds-3.csv
```
Then, using a python script, we can write:
```
#!/usr/bin/env python3

import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

results = pd.read_csv("synth_cat_preds-3.csv", header=None, sep=",")
results = results.iloc[0]

data = pd.read_csv("synth_cat_test-3.csv", header=None) 
	
x_1 = np.array(data.iloc[:,1])
x_2 = np.array(data.iloc[:,2])
	
categories = results
colormap=np.array(['tab:blue','tab:orange'])

plt.scatter(x_1, x_2, s=8, c=colormap[categories], alpha=0.5)
plt.xlabel('x_1') 
plt.ylabel('x_2')
plt.show()
```
Which graphs the first categorical feature vector $x_1$ against the second categorical feature vector $x_2$, as such:

<p align="center"><img src="/images/nb-fig-x1x2.png" alt="/images/nb-fig-x1x2.png"/></p> \
\
Similarly, we can modify the script to graph $x_2$ against $x_3$:

<p align="center"><img src="/images/nb-fig-x2x3.png" alt="/images/nb-fig-x2x3.png"/></p> 
-->
### Gaussian Naive Bayes

Under construction. \
\
Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes. It is highly useful for classifying vector rows with continuous feature variables. The equation for Gaussian Naive Bayes is given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma$ represents standard deviation and $\mu$ represents mean. \
\
Gaussian Naive Bayes assumes that the probabilities associated with each class are distributed using a normal distribution. Similar to a maximum likelihood estimator of Naive Bayes, we calculate the prior density using the overall frequency of each classification in our training dataset, however, the difference lies in how we calculate our class-conditional density. We calculate the class-conditional density by drawing from a Gaussian distribution. We are then able to predict the classification in a similar way to our maximum likelihood algorithm, by multiplying our class conditional probability by our prior for each classification. We then normalize our probabilities, using the total number of classifications, and classify each test vector using the highest probability. \
\

### Gaussian Visualization

The 1936 Iris dataset, previously mentioned in the KNN writeup, contains 150 flowers classified by species and their respective sepal and petal measurements (test available [here](https://github.com/nathanenglehart/nathanenglehart.github.io/blob/gh-pages/data/iris-test.csv) and train available [here](https://github.com/nathanenglehart/nathanenglehart.github.io/blob/gh-pages/data/iris-train.csv) where 0 represents iris-setosa, 1 represents iris-versicolor, and 2 represents iris-virginica). \
\
Then, using a python script, we can write:
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
Now, we can view a graph of the whole test dataset's information with its true classifications
<img src="/images/pairplot-iris-true.png" alt="/images/pairplot-iris-true.png"/>
Then, using the previously mentioned C++ implementation, we can generate classification predictions for the test set with Gaussian Naive Bayes. To do so, we can run:
```bash
./naive-bayes-cli iris-train.csv iris-test.csv -g -v
```
Then, by storing the predicting classifications in a csv file to use on the test dataset we can write another python script:
```python
#!/usr/bin/env python3

import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv("iris-test.csv", sep=',')

preds = pd.read_csv("iris-pred-classifications.csv", sep=',')
preds = preds.iloc[:,0]

data.iloc[:,0] = preds
data.columns=["classifications", "sepal length", "sepal width", "petal length", "petal width"]

sns.pairplot(data, hue="classifications", palette=['tab:blue','tab:orange','tab:green'])
plt.show()
```

Full code for both implementations available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>
### References
Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling, 2013. Springer.
