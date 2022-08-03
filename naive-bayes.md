---
layout: home2
permalink: /naive-bayes/
title: Naive Bayesian Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The Naive Bayesian Classifier Algorithm is a family of probabalistic supervised machine learning algorithms that assumes each feature is independent of other features inside a feature vector. \
\
To compute the probability of a test vector with features $x_1 ... x_d$ belonging to classification $y \in C$ where $C$ is the set containing all possible classifications, using a $n \times m$ train matrix, Naive Bayes uses the equation:
\\[ P(y,x_1 ... x_m) = P(y) \prod^{m}_{i=1} P_i (x_i|y) \\]

Then, by running this equation for each possible classification $y$, Naive Bayes assigns the classification with maximal probability as the predicted classification. As such, to compute the predicted classification $\hat{y}$, we can write:
\\[ \hat{y} = \arg \max_{y \in C} P(y,x_1 ... x_m) = \arg \max_{y \in C} P(y) \prod^{m}_{i=1} P_i (x_i|y) \\]
Implementations of Naive Bayes are unique in how they compute the prior and likelihood. This writeup will explore two varieties of Naive Bayes: Categorical Naive Bayes and Gaussian Naive Bayes.

### Note

To see my full code behind the mathematical notation for Categorical Naive Bayes and Gaussian Naive Bayes implementations (which will be used below in demonstrations), please see: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>.

### Categorical Naive Bayes

One of the most common implementations of Naive Bayes is Categorical Naive Bayes. Categorical Naive Bayes is useful for classifying vectors with categorical data (nominal or ordinal) as features. Respectively, Categorical Naive Bayes computes the prior and likelihood with: 
<div align="center">
$P(y) = \frac{\sum^n_{j=1} I(y_j = y)}{n}$ and $P_i (x_i|y) = \frac{\sum^n_{j=1} I(x_i = x_j \land y_j = y)}{\sum^n_{j=1} I(y_j = y)}$
</div> \
In plain English, Categorical Naive Bayes:

1. First calculates $P(y)$ by dividing the frequency of each classification in the train data by the number of vector rows in the train data $n$ with classification $y$
2. Compute the likelihood by taking the product sum of conditional class probabilities where conditional class probabilities are calculated for each $x_i$ within feature column $i$ in the train data by:
	- Dividing the frequency of the feature $x_i$ with classification $y$ by the total number of rows in the train data $n$ with classification $y$
4. Multiply the result of the first and second steps

By running this equation for each possible classification $y$, Categorical Naive Bayes is able to assigns the classification with maximal probability as the predicted classification.

### Laplace Smoothing for Categorical Naive Bayes

Categorical Naive Bayes faces an issue if individual categorical features labels are missing from the train data for some classification $y$ since this will lead to frequency based probability estimates becoming zero. This will set our product sum to zero and hinder the accuracy of the classifier. \
\
This problem can be solved using a technique called Laplace Smoothing. Laplace Smoothing is a slight modification to the Naive Bayes algorithm which solves the zero frequency problem by modifying the conditional class probability equation with:
\\[ P_i (x_i|y) = \frac{(\sum^n_{j=1} I(x_i = x_j \land y_j = y)) + \alpha}{(\sum^n_{j=1} I(y_j = y)) + (\alpha m)} \\]
for some $\alpha \geq 1$. This ensures that conditional class probabilities will never become zero. Laplace smoothing can also be applied to other forms of Naive Bayes. For example, Laplace Smoothing is often applied to Multinomial Naive Bayes.

### Categorical Naive Bayes Visualization

The 1993 University of Wisconsin Breast Cancer Dataset contains information on cells gathered from digitized images of fine need aspirate (FNA) breast mass. Cells are classified as benign (0) or malignant (1). Feature include cell radius, texture, perimeter, area, smoothness, compacteness, concavity, concave points, symmetry, and fractal dimension such that each features is ordinal on a scale of 1 to 10 (train available [here](https://nathanenglehart.github.io/data/bc-train.csv); test available [here](https://nathanenglehart.github.io/data/bc-test.csv)). \
\
To visualize the data, with python we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv('bc-test.csv', sep=',', header=None)
data.columns = ['classification','radius','texture','perimeter','area','smoothness','compactness','concavity','concave points','symmetry','fractal dimension']
sns.pairplot(data, hue='classification',palette=['tab:cyan','tab:olive'])
plt.show()
```
Which displays: \
\
<img src="/images/bc-pairplot-true.png" alt="/images/bc-pairplot-true.png"> \
\
Now, to generate predictions we can use the previously mentioned C++ implementation. To clone and compile the repository, we must first run:

```bash
git clone https://github.com/nathanenglehart/naive-bayes-cpp-241
cd naive-bayes-cpp-241
make
```

This compiles the program `naive-bayes-cli`. Next, to run Categorical Naive Bayes on the dataset, we can run:

```bash
./naive-bayes-cli bc-train.csv bc-test.csv -c -v # Verbose
./naive-bayes-cli bc-train.csv bc-test.csv -c > preds.csv # Store results
```

By storing the results in a csv file to use on the test dataset we can write another python script:

```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

data = pd.read_csv('bc-test.csv', sep=',', header=None)
data.columns = ['classification','radius','texture','perimeter','area','smoothness','compactness','concavity','concave points','symmetry','fractal dimension']
results = pd.read_csv('preds.csv',header=None,sep=',')
results = results.iloc[:,0]
data.iloc[:,0] = results

sns.pairplot(data, hue='classification',palette=['tab:cyan','tab:olive'])
plt.show()
```

Which displays: \
\
<img src="/images/bc-pairplot-preds.png" alt="/images/bc-pairplot-preds.png"> \
\
Very similar to our original graph of true predictions! Further, by running in verbose, we can see that this run of Categorical Naive Bayes had an error rate of ~4%.

### Gaussian Naive Bayes

Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes. It is highly useful for classifying vector rows with continuous feature variables. As in Categorical Naive Bayes, in Gaussian Naive Bayes, the prior probability is given by:
\\[ P(y) = \frac{\sum^n_{j=1} I(y_j = y)}{n} \\]
On the other hand, the equation for the Gaussian Naive Baye likelihood is given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma_y$ represents standard deviation computed using features of column $i$ with classification $y$ and $\mu_y$ represents mean computed using features of column $i$ with classification $y$.

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
./naive-bayes-cli synth-train-blobs-5.csv synth-test-blobs-5.csv -g -v # Verbose
./naive-bayes-cli synth-train-blobs-5.csv synth-test-blobs-5.csv -g > preds.csv # Store results
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
As such, we can see that the predicted classifications are very close to the true classifications. In fact, running in verbose shows an error rate of ~1% on any given run!

### References

Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling. Springer. \
\
Wolberg, William et al. (1995). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic)">https://archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic)</a>. Irvine, CA: University of California, School of Information and Computer Science.
