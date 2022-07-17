---
layout: home2
permalink: /naive-bayes/
title: Naive Bayesian Classifier Algorithm
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---
The Naive Bayesian Classifier Algorithm is a family of probabalistic supervised machine learning algorithms that assumes each feature is independent of other features inside a feature vector. Each implementation is based on Bayes Rule, which is given by:
\\[  P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)} \\]
One such implementation uses maximum likelihood estimation (MLE), given by:
\\[ P(y,x_1 … x_d) = q(y) \prod^{d}_{j=1} q_j (x_j|y) \\]
Breaking down the algorithm more specifically, using the training dataset we first calculate the frequency of each feature in the train dataset in relation to each classification in the dataset. These frequencies allow us to calculate the prior probability for each feature or $q(y)$. We can then calculate the class conditional probability of the test vector belonging to a certain class by taking the product sum of the corresponding probabilities for each feature in the test set belonging to the classification label y. We then apply Bayes' rule by multiplying the product sum by the overall probability of the classification label occurring in the test set $q(y)$ to get our posterior probability. We then normalize these probabilities by dividing by the number of potential classes. The Naive Bayes classifier thus classifies each test vector using the label y which returned the highest probability. \
\
Another implementation of the Naive Bayes algorithm is Gaussian Naive Bayes, given by:
\\[ P(x_i|y) = \frac{1}{\sqrt{2\pi\sigma^2_y}}exp\bigg(- \frac{(x_i - \mu_y)^2}{2\sigma^2_y} \bigg) \\]
where $\sigma$ represents standard deviation and $\mu$ represents mean. \
\
Gaussian Naive Bayes assumes that the probabilities associated with each class are distributed using a normal distribution. Similar to a maximum likelihood estimator of Naive Bayes, we calculate the prior density using the overall frequency of each classification in our training dataset, however, the difference lies in how we calculate our class-conditional density. We calculate the class-conditional density by drawing from a Gaussian distribution. We are then able to predict the classification in a similar way to our maximum likelihood algorithm, by multiplying our class conditional probability by our prior for each classification. We then normalize our probabilities, using the total number of classifications, and classify each test vector using the highest probability. \
\
Code for both implementations available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/naive-bayes-cpp-241">https://github.com/nathanenglehart/naive-bayes-cpp-241</a>
### References
Barber, David. (2016). Bayesian Reasoning and Machine Learning. Cambridge University Press. \
\
Kuhn, Max and Johnson, Kjell. (2013). Applied Predictive Modeling, 2013. Springer.
