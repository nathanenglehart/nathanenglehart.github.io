---
layout: home2
permalink: /projects/regression/
title: OLS, Ridge, and Lasso Regression
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

Multivariate regression methods are commonly used in the social sciences to examine the extent to which various independent variable(s) are related to a dependent variable. \
\
When looking to analyze relationships between independent variable(s) and a continuous dependent variable, ordered least squares (OLS) regression, ridge regression, and lasso regression are all commonly utilized. While OLS regression is most commonly used in regression analysis by social scientists, ridge regression and lasso regression are highly useful in situations where regression models need to be generalizable for future data. This is because ridge regression and lasso regression 
 employ regularization methods to penalize overfitting with high coefficients (using l2 and l1 regularization respectively). 

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
\\[ R_{adj}^2 = 1 - \frac{(1 - R^2)(m - 1)}{m - p} \\]
\
where $n$ is the number of rows in the dataset and $p$ is the number of features/predictors in the dataset. With Python, we can write:

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
Extensive OLS, ridge, and lasso code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>. This repository also contains code for logistic regression with documentation available <a style="color: #f56a6a; !important" href="/projects/logit">here</a>.

### References

Rogers, Simon and Girolami, Mark. (2017).  A First Course in Machine Learning Second Edition. Routledge.\
\
Quinlan, Ross. (1983). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/auto+mpg">https://archive.ics.uci.edu/ml/datasets/auto+mpg</a>. Irvine, CA: University of California, School of Information and Computer Science.
