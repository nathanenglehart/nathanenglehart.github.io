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
<!--
Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes. It is highly useful for classifying vector rows with continuous feature variables. The equation for Gaussian Naive Bayes is given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma$ represents standard deviation and $\mu$ represents mean. \
\
Gaussian Naive Bayes assumes that the probabilities associated with each class are distributed using a normal distribution. Similar to a maximum likelihood estimator of Naive Bayes, we calculate the prior density using the overall frequency of each classification in our training dataset, however, the difference lies in how we calculate our class-conditional density. We calculate the class-conditional density by drawing from a Gaussian distribution. We are then able to predict the classification in a similar way to our maximum likelihood algorithm, by multiplying our class conditional probability by our prior for each classification. We then normalize our probabilities, using the total number of classifications, and classify each test vector using the highest probability. \
\
Again, using C++ and leaving out some functions for simplicity, we can write:
```c++
std::map<int, double> calculate_classification_probabilities(std::map<int, std::vector<std::vector<double>>> summaries, Eigen::VectorXd row, int size, bool verbose)
{

  /* Calculates the classification probabilities for a single vector with P(class = y | x_1, ..., x_2) = P(x_1 | class = y) * ... * P(x_n | class = y) * P(class = y) */

  std::map<int, double> probabilities;
  std::map<int, std::vector<std::vector<double>>>::iterator it;
  int classification_value = 0;

  if(verbose == true)
  {
    std::cout << "Row " << verbose_vector_count++ << ": [ ";
    for(auto v : row)
    {
      std::cout << v << " ";
    }
    std::cout << "]\n";

  }

  for(it=summaries.begin(); it != summaries.end(); ++it)
  {

    std::vector<std::vector<double>> entry = it->second;
    probabilities[classification_value] = double_vector_list_lookup(entry,0,2) / (size); // P(class = y)

    for(int i = 1; i < row.size(); i++) // P(class = y | x_1, ..., x_n)
    {
      double mean = double_vector_list_lookup(entry,i,0);
      double standard_deviation = double_vector_list_lookup(entry,i,1);
      double x = get_eigen_index(row,i);
      probabilities[classification_value] *= gaussian_pdf(x,mean,standard_deviation); 
    }

    if(verbose == true)
    {
      std::cout << "Class: " << classification_value << " Probability: " << probabilities[classification_value] << "\n";
    }

    classification_value++;
  }

  if(verbose == true)
  {
    std::cout << "\n";
  }

  return probabilities;
}

int predict(std::map<int, std::vector<std::vector<double>>> summaries, Eigen::VectorXd row, int size, bool verbose)
{

  /* Returns classification prediction for Gaussian NB. */

  std::map<int, double> probabilities = calculate_classification_probabilities(summaries, row, size, verbose);
  std::map<int, double>::iterator it;

  int best_label = -1;
  double best_probability = -1;

  for(it=probabilities.begin(); it != probabilities.end(); ++it)
  {
    if(best_label == -1 || it->second > best_probability)
    {
      best_probability = it->second;
      best_label = it->first;
    }
  }

  return best_label;
}

std::vector<int> gaussian_naive_bayes_classifier(Eigen::MatrixXd validation, int validation_size, Eigen::MatrixXd training, int training_size, int length, bool verbose)
{

  /* Calculates the classification probabilities for each row in dataset and puts their predicted classification in a list. */

  std::map<int, std::vector<std::vector<double>>> summaries = summarize_by_classification(training, training_size, length);
  std::vector<int> predictions;

  for(int i = 0; i < validation_size; i++)
  {
    int output = predict(summaries, validation.row(i), training_size, verbose);
    predictions.push_back(output);
  }

  return predictions;
}
```
As with the previous example, after cloning and making the program, we can run Gaussian Naive Bayes. Then, using the 1936 iris data set available from the UCI Machine Learning Repository (and with my split of train data available [here]() and split of test data available [here]()), we can run: 
```bash
./naive-bayes-cli data/iris/iris.csv data/iris/iris-test.csv -g -v
```
Which, using 10 fold cross validation, returns an error rate of 10% error rate. \
\
Now, to graph the results we can first use bash and regex to save the results to a csv:
```bash
./naive-bayes-cli data/iris/iris.csv data/iris/iris-test.csv -g | sed -E 's/.*(.{1})$/\1/' | sed '$d' | awk '{print}' ORS=', ' | sed '$ s/..$//' > iris-preds.csv
```
Then, using a python script, we can write:
```
#!/usr/bin/env python3

import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

results = pd.read_csv("iris_preds.csv", header=None, sep=",")
results = results.iloc[:,0]

data = pd.read_csv("data/iris/iris-test.csv", header=None)
	
x_1 = np.array(data.iloc[:,1])
x_2 = np.array(data.iloc[:,2])
	
categories = results
colormap=np.array(['tab:blue','tab:orange','tab:green'])

plt.scatter(x_1, x_2, s=50, c=colormap[categories], alpha=0.5)
plt.xlabel('sepal length')
plt.ylabel('sepal width')
plt.show()
```
Which graphs the first categorical feature vector, sepal width, against the second categorical feature vector, sepal length:

<p align="center"><img src="/images/nb-fig-sepal_width-sepal_height.png" alt="/images/nb-fig-sepal_width-sepal_height.png"></p> \
\
-->

Full code for both implementations available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>
### References
Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling, 2013. Springer.
