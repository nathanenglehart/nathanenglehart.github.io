---
layout: home2
permalink: /projects/regression/
title: OLS, Ridge, and Lasso Regression
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

Multivariate regression methods are commonly used in the social sciences to examine the extent to which various independent variable(s) are related to a dependent variable. This first section will examine OLS, ridge, and lasso regression. The following section will examine a fundamentally different form of regression, logistic regression.\
\
When looking to analyze relationships between independent variable(s) and a continuous dependent variable, ordered least squares (OLS) regression, ridge regression, and lasso regression are all commonly utilized. While OLS regression is most commonly used in regression analysis by social scientists, ridge regression and lasso regression are highly useful in situations where regression models need to be generalizable for future data. This is because ridge regression and lasso regression employ regularization methods to penalize overfitting with high coefficients (using l2 and l1 regularization respectively). 

### OLS Regression

OLS seeks to find coefficients for the regression function which minimize prediction error. Its closed-from solution does so with
\\[ \theta = (X^T X)^{-1} X^T t \\]
where $\theta$ represents the OLS estimator (also known as coefficients or weights), $X$ is an $m \times n$ matrix containing independent variable parameters, and $t$ is a vector of response variables. Then, OLS regression generates predictions with:
\\[ \hat{t} = X\theta \\]
As such, we can write an OLS class using Python with:

```python
import numpy as np
import pandas as pd

# Nathan Englehart (Summer, 2022)

class ols_regression():

  def __init__(self):
      
      """ OLS regression class based on sklearn functionality (for modular use) """

  def fit(self, X, t):

      """ Fits ridge regression model with given train matrix and target vector
		Args:
			
			X::[Numpy Array]
				Train matrix (build before putting into function)
			
			t::[Numpy Array]
				Target vector
      """

      theta = np.linalg.inv(X.T.dot(X)).dot(X.T).dot(t)
      
      self.theta = theta
      self.coef_ = theta
      
      return self

  def predict(self, X):
      
      """ Generates predictions for the given matrix based on model.
      		Args:
			
			X::[Numpy Array]
				Test matrix (build before putting into function)
      """

      self.predictions = X.dot(self.theta)

      return self.predictions
```

### Ridge Regression

As previously noted, there are situations where regression models need to be generalizable for future data (a common machine learning problem). In such situations, regressions that use regularization to penalize overfitting with high coefficients are often utilized. One such method is ridge regression which uses l2 regularization. There exists a closed-form solution of ridge regression that computes its coefficients with
\\[ \theta = (X^T X + \lambda I)^{-1} X^T t \\] 
where $\theta$ represents the ridge estimator (also known as coefficients or weights), $X$ is an $n \times m$ matrix containing independent variable parameters, $\lambda$ is the weight penalty, $I$ is a $m \times m$ identity matrix, and $t$ is a vector of response variables. As in OLS regression, predictions are generated with:
\\[ \hat{t} = X\theta \\]
Notably, ridge regression is highly useful with multicollinear, highly correlated independent variables.  \
\
Thus, similar to OLS, we can again write a ridge class using Python with:

```python
import numpy as np
import pandas as pd

# Nathan Englehart (Summer, 2022)

class ridge_regression():

  def __init__(self, lam=1.0):
      
      """ Ridge regression class based on sklearn functionality (for modular use)
		
		Args:
			
			lam::[Float]
				Weight penalty for high order polynomials
      """

      self.lam = lam

  def fit(self, X, t):

      """ Fits ridge regression model with given train matrix and target vector
		Args:
			
			X::[Numpy Array]
				Train matrix (build before putting into function)
			
			t::[Numpy Array]
				Target vector
      """

      I = np.identity(X.shape[1])
      I[0, 0] = 0
      lam_matrix = self.lam * I
      theta = np.linalg.inv(X.T.dot(X) + lam_matrix).dot(X.T).dot(t)
      
      self.theta = theta
      self.coef_ = theta
      
      return self

  def predict(self, X):
      
      """ Generates predictions for the given matrix based on model.
      		Args:
			
			X::[Numpy Array]
				Test matrix (build before putting into function)
      """

      self.predictions = X.dot(self.theta)

      return self.predictions
```

In both OLS and ridge regressions, instead of using their closed form solutions, coefficients can also be computed with algorithms that seek to minimize prediction error. Such algorithms include coordinate descent and gradient descent. Some regression functions, such as logit, lasso, and elastic net, have no closed-form solution and must be calculated using such algorithms. 

### Lasso Regression

Lasso regression uses l1 regularization to penalize high coefficients. As in OLS and ridge regression, lasso regression also generates predictions with:
\\[ \hat{t} = X\theta \\]
However, as lasso regression has no closed form solution to compute an optimal lasso estimator (coefficients/weights), an algorithm must be employed. Batch gradient descent (also known as vanilla gradient descent) is one such algorithm. For some $m \times n$ regressor matrix $X$, batch gradient descent:

1. Initializes $\theta$ randomly. Often $\theta$ is initialized to all zeros though initializing $\theta$ as the OLS or ridge estimator is sometimes used.
2. Initializes a learning rate $\alpha$.
3. Initializes a weight penalty $\lambda$
4. For some number of iterations, also known as epochs (often $\geq 1000$), update $\theta$ with:

\\[ \theta_{i+1} = \theta_{i} - \frac{\alpha(X^T \cdot (\hat{t} - t))}{n} + \lambda \sum \vert \theta \vert \\] 
\
With each run, the algorithm updates $\theta$ to minimize prediction error. At the end of a sufficient number of iterations, $\theta$ will converge on the most optimal lasso estimator for the model. \
\
Therefore, we can write a lasso class using Python:

```python
import numpy as np

# Nathan Englehart (Summer, 2022)

class lasso_regression():
	
	def __init__(self, alpha=0.01, epoch=2500, lam=0.1):
		
		""" Lasso regression class based on sklearn functionality 
			
			Args:
				alpha::[Float]
					Learning rate for batch gradient descent algorithm

				epoch::[Int]
					Number of iterations for batch gradient descent algorithm

				lam::[Float]
					L1 penalty for lasso regression

		"""

		self.alpha = alpha
		self.epoch = epoch
		self.lam = lam

	def fit(self,X,t):

		""" Fits lasso regression model with given regressor/train matrix and target vector 

			Args:
				X::[Numpy Array]
					Regressor/train matrix that already has column of ones for intercept

				t::[Numpy Array]
					Target vector

		"""

		self.gd(X, t)
		self.coef_ = self.theta

		return self
	
	def gd(self, X, t):
	
		""" Performs batch gradient descent to find optimal coefficients for lasso model within fit function

			Args:
				X::[Numpy Array]
					Regressor/train matrix that already has column of ones for intercept
				
				t::[Numpy Array]
					Target vector

		"""

		self.theta = np.zeros(X.shape[1]) 

		for i in range(self.epoch):
			
			t_hat = self.predict(X)

			gradient = (np.dot(X.T, (t_hat - t)) / t.size) + (self.lam * np.sign(self.theta))
				
			self.theta = self.theta - (self.alpha * gradient)

		return self
	
	def predict(self, X):


		""" Generates predictions for the given matrix based on lasso model

			Args:
				X::[Numpy Array]
					Test matrix that already has column of ones for intercept

		"""

		return X.dot(self.theta) 

```

### Computing R-Squared

R-Squared ($R^2$) is a statistical measure for regression models that represents the amount of variance in the dependent variable that can be explained by the independent variables collectively. The equation for $R^2$ is given by: \
\
\\[ R^2 = 1 - \frac{\sum (t_i - \hat{t}_i)^2}{\sum (t_i - \overline{t})^2} \\]
\
where $t$ represents a vector of response variables, $\hat{t}$ represents a vector of predictions, and $\bar{t}$ is the mean of the vector of response variables. As such, with Python, we can write:
```python
import numpy as np

def r_squared(t, t_hat):
	
	""" Returns R-Squared for model with given truth values and prediction values. 

		Args:
			
			t::[Numpy Array]
				Truth values

			t_hat::[Numpy Array]
				Prediction values

	"""

	t_bar = t.mean()
	return 1 - ((((t-t_hat)**2).sum())/(((t-t_bar)**2).sum()))
```

### Computing Adjusting R-Squared

In multivariate regression models, plain $R^2$ is slightly problematic. This is because $R^2$ increases or stays the same as new predictors are added to the model. To account for this issue, adjusted $R^2$ only increases if newly added predictors improve the model's predictive power. Similarly, adjusted $R^2$ decreases if irrelevant predictors are added to the model. The equation for adjusted $R^2$ is given by: \
\
\\[ R_{adj}^2 = 1 - \frac{(1 - R^2)(m - 1)}{m - k} \\]
\
where $m$ is the number of rows in the dataset and $k = n - 1$ is the number of features/predictors in the dataset. With Python, we can write:

```python
import numpy as np

def adj_r_squared(t,t_hat,p):

	""" Returns adjusted R-Squared for model with given truth values and prediction values.

		Args:
				
			t::[Numpy Array]
				Truth values

			t_hat::[Numpy Array]
				Prediction values
			
			p::[Integer]
				Number of features in the dataset (not including constant ones column)
	
	"""

	m = len(t)

	return 1 - ((1 - r_squared(t,t_hat)) * ((m-1) / (m-p))) 
```

### Simple Linear Regression Example

Using data from the 1993 Auto MPG (miles per gallon) dataset available from the UCI Machine Learning repository, suppose we wish to graph a regression to predict MPG with car weight. To set up our data for our regression requires building our regressor matrix $X$. As such, our matrix should contain two columns. The first column should contain all ones. This column accounts for the intercept term. The second column should contain the car weight data. Thus, we can write:
\\[ X = \pmatrix{
1 & x_1 \cr 
1 & x_2 \cr
\vdots & \vdots \cr
1 & x_m}\\]
where car weight is represented by the vector $x = \textbf{[}x_1, x_2, ..., x_m\textbf{]}$. As such, with Python we can can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt

data = pd.read_csv("mpg.csv", sep=",")

t = np.array(data['mpg'])
x = np.array(data['weight'])

X = np.array([np.ones(len(t)), x]).T

model = ols_regression()
model = ols_regression().fit(X,t)

t_hat = model.predict(X)
print('R-Squared:',r_squared(t, t_hat))

plt.scatter(np.array(data['weight']), np.array(data['mpg']), color='g')
plt.plot(np.array(data['weight']), t_hat, color='k')
plt.xlabel('weight')
plt.ylabel('mpg')
plt.show()
```
As a result, this script displays:
<img src="/images/mpg_simple.png" alt="/images/mpg_simple.png"/> \
\
with an $R^2$ of 0.6917929800341573 and an adjusted $R^2$ of 0.6917929800341573. Thus, using adjusted $R^2$, we can say that 69.2 percent of the variance in MPG is explained by weight and a high portion of the variance is explained.

### Polynomial Regression Example

Again, utilizing the Auto MPG dataset, suppose we again wish to graph a regression predicting MPG with car weight. However, this time we wish to utilize a second order polynomial model. To do so, we can utilize the same process as before, but we must build our regressor matrix differently. In this case, our matrix will maintain the same format, except we will need to append another column of $x$ with every term squared. Therefore, we can write:
\\[ X = \pmatrix{
1 & x_1 & x_1^2 \cr 
1 & x_2 & x_2^2 \cr
\vdots & \vdots & \vdots \cr
1 & x_m & x_m^2 }\\]
where car weight is represented by $x = \textbf{[}x_1,x_2, ..., x_m\textbf{]}$. Now, by slightly modifying our original Python code we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt

data = pd.read_csv("mpg.csv", sep=",")

t = np.array(data['mpg'])
x = np.array(data['weight'])

X = np.array([np.ones(len(t)), x, np.square(x)]).T

model = ols_regression()
model = ols_regression().fit(X,t)

t_hat = model.predict(X)
print('R-Squared:',r_squared(t, t_hat))

plt.scatter(np.array(data['weight']), np.array(data['mpg']), color='g')
x, t_hat = zip(*sorted(zip(x,t_hat))) # plot points in order
plt.plot(np.array(data['weight']), t_hat, color='k')
plt.xlabel('weight')
plt.ylabel('mpg')
plt.show()
```
And as a result, the script displays:
<img src="/images/mpg_second_degree.png" alt="/images/mpg_second_degree.png"/> \
\
with an $R^2$ of 0.714788125827216 and an adjusted $R^2$ of 0.7140678938217291. Now, with adjusted $R^2$, we can say that 71.4 percent of the variance in MPG is explained by weight. Compared to the previous linear model, an even high portion of variance is explained.

### Mulivariate Regression Example

Now, suppose we want to graph a regression predicting MPG with car weight and displacement. In this case, we should create our regression matrix using a constant column, a column for car weight, and a column for displacement. As such, we can write:
\\[ X = \pmatrix{
1 & x_{1} & y_{1} \cr
1 & x_{2} & y_{2} \cr
\vdots & \vdots & \vdots \cr
1 & x_{m} & y_{m} } \\]
where car weight is given by the vector $x = \textbf{[}x_1, x_2, ..., x_m\textbf{]}$ and displacement is given by the vector $y = \textbf{[} y_1, y_2, ..., y_m \textbf{]}$. \
\
Then, using Python, we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt
from mpl_toolkits import mplot3d

plt.style.use('seaborn-poster')

data = pd.read_csv("mpg.csv", sep=",")

t = np.array(data['mpg'])
x = np.array(data['weight'])
y = np.array(data['displacement'])

X = np.array([np.ones(len(t)), x, y]).T

model = ols_regression()
model = ols_regression().fit(X,t)

t_hat = model.predict(X)
print('R-Squared:',r_squared(t, t_hat))

# Create set of ordered pairs to work with on graph

x_pts = np.linspace(x.min(), x.max(), 30)
y_pts = np.linspace(y.min(), y.max(), 30)
x_pairs, y_pairs = np.meshgrid(x_pts,y_pts)

# Get values for all ordered pairs in set using model

z = model.coef_[0] + model.coef_[1] * x_pairs + model.coef_[2] * y_pairs

# Graph

fig = plt.figure(figsize = (100,100))
ax = plt.axes(projection='3d')
ax.plot_surface(x_pairs,y_pairs,z, rstride=1, cstride=1, color='teal', alpha=0.4, antialiased=False)
ax.scatter(x,y,t, c = 'r')
ax.set_ylabel('displacement')
ax.set_title('mpg', fontsize=20)
plt.xlabel('\n\n\nweight', fontsize=18)
plt.ylabel('\n\n\ndisplacement', fontsize=16)
plt.show()
```
As such, this script displays:
<img src="/images/mpg_multi.png" alt="/images/mpg_multi.png"/> \
\
with an $R^2$ of 0.6979764850769228 and an adjusted $R^2$ of 0.6972137994331776. As such, using adjusted $R^2$, we can say that 69.7 percent of the variance in MPG is explained by weight and displacement. A high portion of variance is explained (but not quite as much as in our last example). \
\
Cases with more variables are similar, but not able to be visualized.

### Polynomial multivariate regression

Suppose we want to again graph a regression predicting MPG with car weight and displacement. However, this time we wish to use a second order polynomial to do so. Thus, we should create a regressor matrix with a column of the form
\\[ X = \pmatrix{ 1 & x_1 & y_1 & x_1^2 & x_1 \cdot y_1 & y_1^2 \cr
                  1 & x_2 & y_2 & x_2^2 & x_2 \cdot y_2 & y_2^2 \cr
		  \vdots & \vdots & \vdots& \vdots & \vdots & \vdots \cr
		  1 & x_m & y_m & x_m^2 & x_m \cdot y_m & y_m^2
} \\]
where again, car weight is given by the vector $x = \textbf{[}x_1, x_2, ..., x_m\textbf{]}$ and displacement is given by the vector $y = \textbf{[} y_1, y_2, ..., y_m \textbf{]}$. \
\
So, using Python, we can write:
```python
#!/usr/bin/env python3

import numpy as np
import pandas as pd

from matplotlib import pyplot as plt
from sklearn.preprocessing import PolynomialFeatures
from mpl_toolkits import mplot3d

plt.style.use('seaborn-poster')

data = pd.read_csv("mpg.csv", sep=",")

t = np.array(data['mpg'])
x = np.array(data['weight'])
y = np.array(data['displacement'])
X = np.array([x, y]).T

degree = 2
poly = PolynomialFeatures(degree)
X = poly.fit_transform(X)

model = ols_regression()
model = ols_regression().fit(X,t)

t_hat = model.predict(X)
print('R-Squared:',r_squared(t, t_hat))

# Create set of ordered pairs to work with on graph

x_pts = np.linspace(x.min(), x.max(), 30)
y_pts = np.linspace(y.min(), y.max(), 30)
x_pairs, y_pairs = np.meshgrid(x_pts,y_pts)

# Get values for all ordered pairs in set using model

z = model.coef_[0] + model.coef_[1] * x_pairs + model.coef_[2] * y_pairs + model.coef_[3] * x_pairs**2 + (model.coef_[4] * y_pairs * x_pairs) + (model.coef_[5] * y_pairs**2)

# Graph

fig = plt.figure(figsize = (100,100))
ax = plt.axes(projection='3d')
ax.plot_surface(x_pairs,y_pairs,z, rstride=1, cstride=1, color='teal', alpha=0.4, antialiased=False)
ax.scatter(x,y,t, c = 'r')
ax.set_ylabel('displacement')
ax.set_title('mpg', fontsize=20)
plt.xlabel('\n\n\nweight', fontsize=18)
plt.ylabel('\n\n\ndisplacement', fontsize=16)
plt.show()
```
Then, this script displays:
<img src="/images/mpg_multi_poly.png" alt="/images/mpg_multi_poly.png"/> \
\
with an $R^2$ of 0.7264034513441128 with an adjusted $R^2$ of 0.7236187536478695. Now, using adjusted $R^2$, we can write that 72.4 percent of variance is explained by weight and displacement. In addition, a high portion of variance is explained. Our best model!   \
\
Again, cases with more variables are similar, but not able to be visualized. Cases with higher degrees utilize a similar regressor matrix construction. For instance, for a third degree polynomial, we should create a regressor matrix of the form
\\[ X = \pmatrix{ 1 & x_1 & y_1 & x_1^2 & x_1 \cdot y_1 & y_1^2 & x_1^3 & x_1^2 \cdot y_1 & x_1 \cdot y_1^2 & y_1^3 \cr
                  1 & x_2 & y_2 & x_2^2 & x_2 \cdot y_2 & y_2^2 & x_2^3 & x_2^2 \cdot y_2 & x_2 \cdot y_2^2 & y_2^3 \cr
	          \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \cr
		  1 & x_m & y_m & x_m^2 & x_m \cdot y_m & y_m^2 & x_m^3 & x_m^2 \cdot y_m & x_m \cdot y_m^2 & y_m^3
}
\\]
Other cases with higher degree polynomials entail the construction of similar regressor matricies. 

### Notes 

In all of the above examples, the `ols_regression()` class can be used interchangably with the `ridge_regression()` and `lasso_regression()` classes with only slightly different results. \
\
Extensive OLS, ridge, and lasso code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>. The next section will detail logistic regression. 

<!--This repository also contains code for logistic regression with documentation available <a style="color: #f56a6a; !important" href="/projects/logit">here</a>.-->

# Logit and Probit Regression

Logistic regression (also known as logit regression) and probit regression - like OLS regression - are commonly used in the social sciences. Logit and probit regression models are used to examine the extent to which various independent variables are related to a binary dependent variable. \
\
Probit and logit regression models take the form:
\\[ \mathbb{P}(t = 1 \text{ } \vert \text{ } \boldsymbol x, \theta) = G(\theta \cdot \boldsymbol x) \\]
where $\theta = \textbf{[}\theta_1,\theta_2,...,\theta_n \textbf{]}$ represents the logit coefficients, $\boldsymbol x = \textbf{[}x_1,x_2,...,x_n \textbf{]}$ is a feature vector representing a single input observation, and $G$ is a function that takes on values between zero and one. \
\
<!--It follows that the probability that $t=0$ is given by
\\[ P(t = 0 \text{ }\vert \text{ } \boldsymbol x, \theta) = 1 - P(t = 1 \text{ } \vert \text{ } \boldsymbol x, \theta) = 1 - G(\theta \cdot \boldsymbol x) \\]
-->
<!--
such that the probability that $t = 1$ is given by:
\\[ P(t = 1 \text{ } \vert \text{ }x, \theta) = \sigma(\theta \cdot x) = \frac{1}{1 + e^{-(\theta \cdot x)}} = \frac{1}{1 + e^{-(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}} \\]
and the probability that $t = 0$ is given by:
\\[ P(t = 0 \text{ }\vert \text{ }x, \theta) = 1 - \sigma(\theta \cdot x) = 1 - \frac{1}{1 + e^{-(\theta \cdot x)}} = \frac{e^{(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}}{1 + e^{(\theta_1 \cdot x_1 + \theta_2 \cdot x_2 + ... + \theta_n \cdot x_n)}} \\]
where $\theta = \textbf{[}\theta_1,\theta_2,...,\theta_n \textbf{]}$ represents the logit coefficients and $x = \textbf{[}x_1,x_2,...,x_n \textbf{]}$ is a feature vector representing a single input observation. \
\
-->

As such, we can write that the predicted classification is given by:
<div align="center">
<script type="math/tex">
% <![CDATA[
\hat{t} = \begin{cases}
         1 \text{ if } \mathbb{P}(t = 1 \text{ } \vert \text{ } \boldsymbol x, \theta) \geq 0.5 \\
	 0 \text{ otherwise }
    \end{cases}
% ]]>
</script>
</div> \
Logistic regression uses the sigmoid function (also known as the logistic function) to compute classification $t$ probabilities with:
\\[ G(z) = \sigma(z) = \frac{1}{1 + e^{-z}} = \frac{e^z}{1+e^z} \\]
Probit regression uses the standard normal cumulative distribution function to compute classification probabilities $t$ with:
\\[ G(z) = \Phi(z) = \int^z_{-\infty} \phi (v)dz = \frac{1}{\sqrt{2\pi}} \int^z_{-\infty} e^{\frac{-z^2}{2}} dz  \\]
<!--\\[ \hat{t} = \arg \max_{t \in [0,1]} P(t) \\]--> <!-- this isnt true -->
Like OLS and ridge regression, logistic regression seeks to find coefficients $\theta$ for the regression function which minimize prediction error. However, unlike OLS and ridge regression, logistic regression has no closed form solution. Instead, algorithms are used to compute optimal coefficients. \
\
One such algorithm is batch gradient descent (also known as vanilla gradient descent). For some $m \times n$ train matrix $X$, batch gradient descent:

1. Initiailizes $\theta$ randomly. Commonly, $\theta$ is initialized to all zeros.
2. Initializes a learning rate $\alpha$. 
3. For some number of iterations also known as epochs (often $\geq 1000$), update $\theta$ with:\
\
\\[ \theta_{i+1} = \theta_i - \frac{\alpha(X^T \cdot ((1 - \mathbb{P}(t = 1 \text{ } | \text{ } \boldsymbol x, \theta_i)) - t))}{n} \\]

\
With each run, the algorithm updates $\theta$ to minimize prediction error. At the end of a sufficient number of iterations, $\theta$ will converge on the most optimal weights for the logit model. Note, however, that the formal derivation for this method is outside the scope of this writeup.  \
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

    def log_likelihood(self,X,t,theta):
		
		""" Compute log likelihood given inputs. """

		y_star = np.dot(X,theta)
		return np.sum(-np.log(1+np.exp(y_star))) + np.sum(t * y_star) 

```

Similarly, we can write a probit class using python with:

```python
import numpy as np
from scipy.stats import norm

# Nathan Englehart (Spring, 2023)

class probit_regression():
	
	def __init__(self, alpha=0.1, epoch=1000):
		
		""" Probit regression class based on sklearn functionality 
			
			Args:
				alpha::[Float]
					Learning rate for batch gradient descent algorithm

				epoch::[Int]
					Number of iterations for batch gradient descent algorithm

		"""

		self.alpha = alpha
		self.epoch = epoch

	def fit(self,X,t):

		""" Fits probit regression model with given regressor/train matrix and target vector 

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

	def Phi(self, z):
		
		""" Cumulative distribution function for the standard normal distribution. """
		
		return norm.cdf(-z) 

	def predict_proba(self, X):


		""" Generates probability predictions for the given matrix based on model

			Args:
				X::[Numpy Array]
					Test matrix that already has column of ones for intercept

		"""

		return 1 - self.Phi(np.dot(X,self.theta))

	def predict(self, X):
			
		""" Generates classification predictions for the given matrix based on the model 

			Args:
				X::[Numpy Array]
					Test matrix that already has column of ones for intercept

		"""

		return self.predict_proba(X).round()

	def log_likelihood(self,X,t,theta):
		
		""" Compute log likelihood given inputs. """

		y_star = np.dot(X,theta)
		return np.sum(-np.log(1+np.exp(y_star))) + np.sum(t * y_star) 

```

### Computing Pseudo R-Squared

In OLS, ridge, and lasso regression, $R^2$ measures what percent of variation in the dependent variable can be explained by the independent variables collectively. In models which predict binary dependent variables, however, we cannot use vanilla $R^2$ to measure goodness of fit. However, there are many proposed pseudo $R^2$ measures. Common pseudo $R^2$ include:

- Efron's $R^2$
- McFadden's $R^2$ (which can be adjusted)
- Cox and Snell $R^2$
- Nagelkerke/Cragg and Uhler's $R^2$

For this writeup, we will examine Efron's $R^2$ *and* McFadden's $R^2$. \
\
Efron's $R^2$ is given by:\
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

McFadden's $R^2$ is computed as follows.
<!-- \\[R^2_{\text{McFadden}} = 1 - \frac{L_{ur}}{L_{0}} = \frac{\sum^N_{i=1} \bigg((1-y_i) \log [1 - G(\boldsymbol x_i\hat\theta)] + y_i \log[G(\boldsymbol x_i\hat\theta)]\bigg)}{\sum^N_{i=1} \bigg((1-y_i) \log [1 - G(\boldsymbol x_i\hat\theta_0)] + y_i \log[G(\boldsymbol x_i\hat{\theta}_{0})]\bigg)} \\]
-->

\\[R_{\text{McFadden}}^2 = 1 - \frac{L_F}{L_0} = \frac{\sum^N_{i=1} \bigg((1-y_i) \log [1 - G(\boldsymbol x_i\hat\theta)] + y_i \log[G(\boldsymbol x_i\hat\theta)]\bigg)}{\sum^N_{i=1} \bigg((1-y_i) \log [1 - G(\boldsymbol x_i\hat \theta_0)] + y_i \log[G(\boldsymbol x_i\hat \theta_0)]\bigg)} \\]

where $G$ is the sigmoid function (logit) or the cumulative distribution function of a standard normal random variable (probit) and $\boldsymbol x_i$ are rows of the regressor matrix.\
\
Notice that the numerator and denominator functions are log likelihood functions. The log likelihood of function gives the likelihood of observing a sample with given function parameters. In McFadden's $R^2$, the numerator and denominator are log likelihoods with coefficients $\hat{\theta}$ and $\hat \theta_0$ which maximize the likelihood of observing the given data. The difference between the two is that $\hat{\theta}$ represents the full vector of coefficients while $\hat\theta_0$ represents only the intercept term (first coefficient) while setting the rest of the coefficients to zero. \
\
Note that since $G$ is between $0$ and $1$, and the log of a number less than $1$ is negative, that both log-likelihoods are negative. If the $\boldsymbol x$'s  has no predictive power, then the log likelihood will be the same. So: \\[ L_F = L_0 \implies R^2_{\text{McFadden}} = 0 \\]
We can write McFadden's $R^2$ in python as follows:

```python
def mcfadden_r_squared(theta, X, t, model):

	""" Returns McFadden's psuedo R-Squared for logistic regression 
	
		Args:
			
			theta::[Numpy Array]
				Weights/coefficients for the given logistic regression model
			
			X::[Numpy Array]
				Regressor matrix

			t::[Numpy Array]
				Truth values corresponding to regressor matrix

	"""

	L_ul = model.log_likelihood(X,t,theta)
	theta_0 = np.zeros(theta.size)
	theta_0[0] = theta[0]
	L_0 = model.log_likelihood(X, t, theta_0)

	return 1 - (L_ul / L_0)
```

### Simple Logistic Regression Example

Using data from the 1988 Pima Indians Diabetes Dataset available on Kaggle, suppose we wish to graph a logistic regression to predict whether or not a person has diabetes based on their blood glucose levels. To set up our data for the regression, like with other forms of regression, we must first build our regressor matrix $X$. In this case, our matrix should contain two columns. The first column should contain all ones. This column accounts for the intercept term. The second column should contain the blood glucose data. As such, we can write:
\\[ X = \pmatrix{
1 & x_1 \cr 
1 & x_2 \cr
\vdots & \vdots \cr
1 & x_m}\\]
where each individuals blood glucose data is represented in the vector $x = \textbf{[} x_1, x_2, ..., x_m \textbf{]}$. Then, with Python, we can write: 
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
1 & x_m & y_m }\\]
where glucose is represented by $x = \textbf{[}x_1, x_2, ..., x_m \textbf{]}$ and mass is represented by $y = \textbf{[}y_1, y_2, ..., y_m \textbf{]}$. \
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

### Notes 

In all of the above examples, the `logit_regression()` class can be used interchangably with the `probit_regression()` class with only *slightly* different results. \
\
Logit and probit regression code also available at <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>.

### References

Jurafsky, D. & Martin, J. (2021). <i>Speech and Language Processing (3rd ed. draft)</i> <a style="color: #f56a6a; !important" href="https://web.stanford.edu/~jurafsky/slp3/">https://web.stanford.edu/~jurafsky/slp3/</a>. Stanford, CA. \
\
Quinlan, R. (1983). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/auto+mpg">https://archive.ics.uci.edu/ml/datasets/auto+mpg</a>. Irvine, CA: University of California, School of Information and Computer Science. \
\
Rogers, S. & Girolami, M. (2017).  A First Course in Machine Learning Second Edition. Routledge.\
\
Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Kaggle <a style="color: #f56a6a; !important" href="https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database">https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database</a>. San Francisco, CA. \
\
Wooldridge, J. (2020). Introductory econometrics (7e). Cengage.
