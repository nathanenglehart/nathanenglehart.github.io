---
layout: home2
permalink: /projects/logit/
title: Logistic Regression
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

Logistic regression is commonly used in the social sciences to examine the extent to which various independent variables are related to a binary dependent variable. \
\
Logistic regression uses the sigmoid function (also known as the logistic function) to compute classification $t$ probabilities with:
\\[ \sigma(z) = \frac{1}{1 + e^{-z}} \\]
such that the probability that $t = 1$ is given by:
\\[ P(t = 1 \text{ } \vert \text{ }x, \theta) = \sigma(\theta \cdot x) = \frac{1}{1 + e^{-(\theta \cdot x)}} = \frac{1}{1 + e^{-(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}} \\]
and the probability that $t = 0$ is given by:
\\[ P(t = 0 \text{ }\vert \text{ }x, \theta) = 1 - \sigma(\theta \cdot x) = 1 - \frac{1}{1 + e^{-(\theta \cdot x)}} = \frac{e^{-(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}}{1 + e^{-(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}} \\]
where $\theta = \textbf{[}\theta_1,\theta_2,...,\theta_m\textbf{]}$ represents the logit coefficients and $x = \textbf{[}x_1,x_2,...,x_m\textbf{]}$ is a feature vector representing a single input observation. \
\
As such, using the latter equation, we can write that the predicted classification is given by:
<div align="center">
<script type="math/tex">
% <![CDATA[
\hat{t} = \begin{cases}
         1 \text{ if } 1 - P(t = 0 \text{ } | \text{ } x, \theta) \geq 0.5 \\
	 0 \text{ otherwise }
    \end{cases}
% ]]>
</script>
</div> \
<!--\\[ \hat{t} = \arg \max_{t \in [0,1]} P(t) \\]--> <!-- this isnt true -->
Like OLS and ridge regression, logistic regression seeks to find coefficients $\theta$ for the regression function which minimize prediction error. However, unlike OLS and ridge regression, logistic regression has no closed form solution. Instead, algorithms are used to compute optimal coefficients. \
\
One such algorithm is batch gradient descent (also known as vanilla gradient descent). For some $n \times m$ train matrix $X$, batch gradient descent:

1. Initiailizes $\theta$ randomly. Commonly, $\theta$ is initialized to all zeros.
2. Initializes a learning rate $\alpha$. 
3. For some number of iterations also known as epochs (often $\geq 1000$), update $\theta$ with:\
\
\\[ \theta_{i+1} = \theta_i - \frac{\alpha(X^T \cdot (P(t = 0 \text{ } | \text{ } x, \theta_i) - t))}{m} \\]

\
With each run, the algorithm updates $\theta$ to minimize prediction error. At the end of a sufficient number of iterations, $\theta$ will converge on the most optimal weights for the logit model. \
\
As such, we can write a logistic regression class using python:

```python
import numpy as np

# Nathan Englehart (Summer, 2022)

class logit_regression():
	
	def __init__(self, alpha=0.1, epoch=1000):
		
		""" Logistic regression class based on sklearn functionality 
			
			Args:
				alpha::[Float]
					Learning rate for batch gradient descent algorithm

				epoch::[Int]
					Number of iterations for batch gradient descent algorithm

		"""

		self.alpha = alpha
		self.epoch = epoch

	def fit(self,X,t):

		""" Fits logistic regression model with given regressor/train matrix and target vector 

			Args:
				X::[Numpy Array]
					Regressor/train matrix that already has column of ones for intercept

				t::[Numpy Array]
					Target vector

		"""

		self.bgd(X, t)
		self.coef_ = self.theta

		return self
	
	def bgd(self, X, t):
	
		""" Performs batch gradient descent (also known as vanilla gradient descent) to find optimal coefficients for logit model within fit function

			Args:
				X::[Numpy Array]
					Regressor/train matrix that already has column of ones for intercept
				
				t::[Numpy Array]
					Target vector

		"""
		
		self.theta = np.zeros(X.shape[1]) 

		for i in range(self.epoch):
			
			gradient = np.dot(X.T, (self.predict_proba(X) - t)) / t.size
				
			self.theta -= (self.alpha * gradient)

		return self
	
	def sigmoid(self,z):
		
		""" Sigmoid function (also called the logistic function) where P(t = 1) = sigma(coefs * x) and P(t = 0) = 1 - sigma(coefs * x) """

		return 1.0 / (1.0 + np.exp(z))

	def predict_proba(self, X):


		""" Generates probability predictions for the given matrix based on model

			Args:
				X::[Numpy Array]
					Test matrix that already has column of ones for intercept

		"""

		return 1 - self.sigmoid(np.dot(X,self.theta))

	def predict(self, X):
			
		""" Generates classification predictions for the given matrix based on the model 

			Args:
				X::[Numpy Array]
					Test matrix that already has column of ones for intercept

		"""

		return self.predict_proba(X).round()
```

Logistic regression code available at <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>.

### Computing Pseudo R-Squared

In OLS, ridge, and lasso regression, $R^2$ measures what percent of variation in the dependent variable can be explained by the independent variables collectively. Since logistic regression computes probabilities for a binary dependent variable instead of a continuous dependent variable, we cannot use the vanilla $R^2$ measure on logistic regression. However, there are many proposed pseudo $R^2$ measures for logistic regression. Like $R^2$, each pseudo $R^2$ measures what percent of variation in the dependent variable can be explained by the independent variables collectively. Common pseudo $R^2$ include:

- Efron's $R^2$
- McFadden's $R^2$ (which can be adjusted)
- Cox and Snell $R^2$
- Nagelkerke/Cragg and Uhler's $R^2$

For this writeup, we will utilize Efron's $R^2$. Efron's $R^2$ is given by:\
\
\\[ R_{\text{Efron}}^2 = \frac{\sum (t_i - \hat{\pi}_i)^2}{\sum (t_i - \overline{t})^2} \\]\
where $t$ represents a vector of respsonse variables, $\hat{\pi}$ represents a vector of prediction probabilities, and $\overline{t}$ is the mean of the vector of response variables. As such, with Python we can write:

```python
def efron_r_squared(t, t_probs):

	""" Returns Efron's psuedo R-Squared for logistic regression. 

		Args:

			t::[Numpy Array]
				Truth values

			t_probs::[Numpy Array]
				Prediction value probabilities

	"""

	return 1.0 - ( np.sum(np.power(t - t_probs, 2.0)) / np.sum(np.power((t - (np.sum(t) / float(len(t)))), 2.0)) ) 
```

### Simple Logistic Regression Example

Using data from the 1988 Pima Indians Diabetes Dataset available on Kaggle, suppose we wish to graph a logistic regression to predict whether or not a person has diabetes based on their blood glucose levels. To set up our data for the regression, like with other forms of regression, we must first build our regressor matrix $X$. In this case, our matrix should contain two columns. The first column should contain all ones. This column accounts for the intercept term. The second column should contain the blood glucose data. As such, we can write:
\\[ X = \pmatrix{
1 & x_1 \cr 
1 & x_2 \cr
\vdots & \vdots \cr
1 & x_n}\\]
where each individuals blood glucose data is represented in the vector $x = \textbf{[}x_1, x_2, ..., x_n \textbf{]}$. Then, with Python, we can write: 
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt
from sklearn import preprocessing

data = pd.read_csv('data/pima.csv',sep=",")
	
t = np.array(data['diabetes']) 
x_1 = preprocessing.scale(np.array(data['mass'], dtype=np.float128)) # , dtype=np.float128 to prevent overflow
	
X = np.array([np.ones(len(t)), x_1]).T

model = logit_regression()
model = logit_regression().fit(X,t)

t_probs = model.predict_proba(X)

plt.scatter(x_1,t, color='tab:olive')
x_1, t_probs = zip(*sorted(zip(x_1,t_probs))) # plot points in order
plt.plot(x_1,t_probs, color='tab:cyan')
plt.xlabel('x_1')
plt.ylabel('t')
plt.show()
```
As a result, this script displays:
<img src="/images/simple_logit.png" alt="/images/simple_logit.png"/> \
\
with an Efron's pseudo $R^2$ of 0.09123912933661849. Therefore, we can say that 9.1% of variance in whether or not a person has diabetes is explained by the glucose variable. Thus, a low portion of variance is explained. Not amazing - but lets see what happens when we add more independent variable predictors to the model.

### Multivarite Logistic Regression Example

Now, suppose we wish to graph a regression predicting diabetes with glucose *and* mass. In this case, we should build our regression matrix again using a constant column, a column for glucose, and a column for mass. Thus, we can write:
\\[ X = \pmatrix{
1 & x_1 & y_1 \cr 
1 & x_2 & y_2 \cr
\vdots & \vdots \cr
1 & x_n & y_n }\\]
where glucose is represented by $x = \textbf{[}x_1, x_2, ..., x_n\textbf{]}$ and mass is represented by $y = \textbf{[}y_1, y_2, ..., y_n\textbf{]}$. \
\
Then using Python we can write:

```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt
from mpl_toolkits import mplot3d
from sklearn import preprocessing

data = pd.read_csv('data/pima.csv',sep=",")

t = np.array(data['diabetes']) 
x_1 = preprocessing.scale(np.array(data['glucose'], dtype=np.float128))
x_2 = preprocessing.scale(np.array(data['mass'], dtype=np.float128))

X = np.array([np.ones(len(t)), x_1, x_2]).T

model = logit_regression()
model = logit_regression().fit(X,t)

t_probs = model.predict_proba(X)
print('prob preds',t_probs)

x_pts = np.linspace(x_1.min(), x_1.max(), 30)
y_pts = np.linspace(x_2.min(), x_2.max(), 30)
x_pairs, y_pairs = np.meshgrid(x_pts,y_pts)

# Get values for all ordered pairs in set using model

z = 1 / (1 + np.e ** (model.coef_[0] + model.coef_[1] * x_pairs + model.coef_[2] * y_pairs))

# Graph

fig = plt.figure(figsize = (100,100))
ax = plt.axes(projection='3d')
ax.plot_surface(x_pairs,y_pairs,z, rstride=1, cstride=1, color='tab:cyan', alpha=0.4, antialiased=False)
ax.scatter(x_1,x_2,t, c = 'tab:olive')
ax.set_ylabel('mass')
ax.set_title('pima', fontsize=20)
plt.xlabel('\n\n\nglucose', fontsize=18)
plt.ylabel('\n\n\nmass', fontsize=16)
plt.show()
```

Which displays:
<img src="/images/multivariate_logit.png" alt="/images/multivariate_logit.png"/> \
\
with an Efron's pseudo $R^2$ of 0.2788494451173642. As such we can say that 27.8 percent of the variance in whether or not a person has diabetes or not is explained by the glucose and mass variables. Thus, a low-to moderate portion of variance is explained. Much better than our single variable model!

### Logistic Regression as Classification

Though logistic regression is commonly used by social scientists for regression analysis, logistic regression is also widely used for classification problems. In the field of machine learning, this makes logistic regression a supervised, probabalistic, classification method. \
\
Suppose, for example, that we split the Pima Indians Diabetes Dataset into a train dataset and a test dataset such that we wish to use the train dataset to predict whether an individual in the test dataset has diabetes based. \
\
To split the dataset into train data and test data, we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

data = pd.read_csv('data/pima.csv',sep=",")

def train_test_split(data, train_filename, test_filename):
	
	df = pd.read_csv(data) 
		
	msk = np.random.rand(len(df)) < 0.8
	train = df[msk]
	test = df[~msk]

	train.to_csv(r'./data/' + train_filename, index=False)
	test.to_csv(r'./data/' + test_filename, index=False)

train_test_split('data/pima.csv', 'pima-train.csv', 'pima-test.csv')

train_data = pd.read_csv('data/train_data.csv', sep=",")
test_data = pd.read_csv('data/test_data.csv', sep=",")
```
Then to visualize the test data with its true classifications, we can write:
```python
import seaborn as sns
from matplotlib import pyplot as plt

test_data = pd.read_csv('data/pima-test.csv', sep=',')

sns.pairplot(test_data, hue='diabetes',palette=['tab:cyan','tab:olive'])
plt.show()
```
Which displays \
\
<img src="/images/true-pima-pairplot.png" alt="/images/true-pima-pairplot.png"/> \
\
Now, we can use logistic regression to predict classifications on the test dataset with:
```python
t = np.array(train_data['diabetes'])

train_data = train_data.drop(['diabetes'], axis=1)

x_1 = np.ones((len(t),1))
x_n = preprocessing.scale(np.array(train_data))	
X = np.hstack((x_1,x_n)) 

model = logit_regression()
model = logit_regression().fit(X,t)

t = np.array(test_data['diabetes'])

test_data = test_data.drop(['diabetes'], axis=1)

x_1 = np.ones((len(t),1))
x_n = preprocessing.scale(np.array(test_data))	
X = np.hstack((x_1,x_n)) 

t_hat = model.predict(X)

test_data_with_pred_classifications = pd.read_csv('data/pima-test.csv', sep=",")
test_data_with_pred_classifications['diabetes'] = t_hat
```
Then, we can visualize the results with another pairplot by writing:
```python
sns.pairplot(test_data_with_pred_classifications, hue="diabetes", palette=['lightcoral', 'skyblue'], plot_kws={'alpha':0.75})
plt.show()
```
This displays: \
\
<img src="/images/pred-pima-pairplot.png" alt="/images/pred-pima-pairplot.png"/> \
\
Which computes an error rate of ~0.24. Quite good for this dataset!

### Polynomial Logistic Regression

Under construction.

### References

Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Kaggle <a style="color: #f56a6a; !important" href="https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database">https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database</a>. San Francisco, CA.
