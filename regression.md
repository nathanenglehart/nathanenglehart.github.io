---
layout: home2
permalink: /regression/
title: OLS and Ridge Regression Class Implementations
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

Multivariate regression methods are commonly used in the social sciences to examine the extent to which various independent variables are related to a dependent variable. Most commonly, when looking to analyze relationships between variables, ordered least squares regression (OLS) is the form of regression most often employed.\
\
OLS seeks to find coefficients for the regression function which minimize prediction error. Its closed-from solution does so with
\\[ \theta = (X^T X)^{-1} X^T t \\]
where $\theta$ represents the OLS estimator, $X$ is an $n \times m$ matrix containing independent variable parameters, and $t$ is a vector of response variables. \
\
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

In situations where regression equations need to be generalizable for future data, regressions that employ regularization methods to penalize overfitting with high coefficients are often utilized with cross validation. One such method is ridge regression, for which there exists a closed-form solution that computes its coefficients with
\\[ \theta = (X^T X + \lambda I)^{-1} X^T t \\]
where $\theta$ represents the ridge estimator, $X$ is an $n \times m$ matrix containing independent variable parameters, $\lambda$ is the weight penalty, $I$ is a $m \times m$ identity matrix, and $t$ is a vector of response variables. Notably, ridge regression is also highly useful with multicollinear, highly correlated independent variables.  \
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
In both regressions, the prediction vector is given by:
\\[ \hat{t} = X\theta \\]
Additionally, in both regressions, coefficients can also be computed with algorithms that seek to minimize prediction error, such as coordinate descent or gradient descent. Some regression functions, such as logit, lasso, and elastic net, have no closed-form solution and must be calculated using such algorithms. \
\
OLS and ridge code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>.
### Simple Linear Regression Example
Using data from the 1993 Auto MPG (miles per gallon) Dataset available from the UCI Machine Learning repository, suppose we wish to graph a regression to predict MPG with car weight. To set up our data for our regression requires building our regressor matrix $X$. As such, our matrix should contain two columns. The first column should contain all ones. This columns account for the intercept term. The second column should contain the car weight data. Thus, we can write:
<!--\\[ X = \begin{bmatrix} 1 & x_1 \\ 
                        1 & x_2 \\
			\vdots & \vdots \\
			1 & x_n \\ \end{bmatrix} \\] -->
\\[ X = \pmatrix{
1 & x_1 \cr 
1 & x_2 \cr
\vdots & \vdots \cr
1 & x_n}\\]
<!--\\[ X = \bmatrix{
1 & x_1 \cr 
1 & x_2 \cr
\vdots & \vdots \cr
1 & x_n}\\] -->
where car weight is represented by the vector $x = \{x_1, x_2, ..., x_n\}$. As such, with Python we can can write:
```python
#!/bin/bash python3

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

plt.scatter(np.array(data['weight']), np.array(data['mpg']), color='g')
plt.plot(np.array(data['weight']), t_hat, color='k')
plt.xlabel('weight')
plt.ylabel('mpg')
plt.show()
```
As a result, this script displays:
<img src="/images/mpg_simple.png" alt="/images/mpg_simple.png"/>
### Polynomial Regression Example
Again, utilizing the Auto MPG dataset, suppose we again wish to graph a regression predicting MPG with car weight. However, this time we wish to utilize a second order polynomial model. To do so, we can utilize the same process as before, but we must build our regressor matrix differently. In this case, our matrix will maintain the same format, except we will need to append another column of $x$ with every term squared. Therefore, we can write:
\\[ X = \pmatrix{
1 & x_1 & x_1^2 \cr 
1 & x_2 & x_2^2 \cr
\vdots & \vdots & \vdots \cr
1 & x_n & x_n^2 }\\]
Now, by slightly modifying our original Python code we can write:
```python
#!/bin/bash python3

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

plt.scatter(np.array(data['weight']), np.array(data['mpg']), color='g')
x, t_hat = zip(*sorted(zip(x,t_hat))) # plot points in order
plt.plot(np.array(data['weight']), t_hat, color='k')
plt.xlabel('weight')
plt.ylabel('mpg')
plt.show()
```
And as a result, the script displays:
<img src="/images/mpg_second_degree.png" alt="/images/mpg_second_degree.png"/>
### Mulivariate Regression Example
Now, suppose we want to graph a regression predicting MPG with car weight and displacement. In this case, we should create our regression matrix using a constant column, a column for car weight, and a column for displacement. As such, we can write:
\\[ X = \pmatrix{
1 & x_{1} & y_{1} \cr
1 & x_{2} & y_{2} \cr
\vdots & \vdots & \vdots \cr
1 & x_{n} & y_{n} } \\]
where car weight is given by $x = \{x_1, x_2, ..., x_n\}$ and displacement is given by $y = \{y_1, y_2, ..., y_n\}$. \
\
Then, using Python, we can write:
```python
#!/bin/bash python3

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
<img src="/images/mpg_multi.png" alt="/images/mpg_multi.png"/>
Cases with more variables are similar, but not able to be visualized.
### Polynomial multivariate regression
Suppose we want to again graph a regression predicting MPG with car weight and displacement. However, this time we wish to use a second order polynomial to do so. Thus, we should create a regressor matrix with a column of the form
\\[ X = \pmatrix{ 1 & x_1 & y_1 & x_1^2 & x_1 \cdot y_1 & y_1^2 \cr
                  1 & x_2 & y_2 & x_2^2 & x_2 \cdot y_2 & y_2^2 \cr
		  \vdots & \vdots & \vdots& \vdots & \vdots & \vdots \cr
		  1 & x_n & y_n & x_n^2 & x_n \cdots y_n & y_n^2
} \\]
where again, car weight is given by $x = \{x_1, x_2, ..., x_n\}$ and displacement is given by $y = \{y_1, y_2, ..., y_n\}$. \
\
So, using Python, we can write:
```python
#!/bin/bash python3

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
<img src="/images/mpg_multi_poly.png" alt="/images/mpg_multi_poly.png"/>
Again, cases with more variables are similar, but not able to be visualized. Cases with higher degrees utilize a similar regressor matrix construction. For instance, for a third degree polynomial, we should create a regressor matrix of the form
\\[ X = \pmatrix{ 1 & x_1 & y_1 & x_1^2 & x_1 \cdot y_1 & y_1^2 & x_1^3 & x_1^2 \cdot y_1 & x_1 \cdot y_1^2 & y_1^3 \cr
                  1 & x_2 & y_2 & x_2^2 & x_2 \cdot y_2 & y_2^2 & x_2^3 & x_2^2 \cdot y_2 & x_2 \cdot y_2^2 & y_2^3 \cr
	          \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \cr
		  1 & x_n & y_n & x_n^2 & x_n \cdot y_n & y_n^2 & x_n^3 & x_n^2 \cdot y_n & x_n \cdot y_n^2 & y_n^3
}
\\]
Other cases with higher degree polynomials entail the construction of similar regressor matricies. 

### Notes 

In all of the above examples, the ridge_regression() class can be used interchangably with the ols_regression() class.

### References

Rogers, Simon and Girolami, Mark. (2017).  A First Course in Machine Learning Second Edition. Routledge.\
\
Quinlan, Ross. (1983). UCI Machine Learning Repository <a style="color: #f56a6a; !important" href="https://archive.ics.uci.edu/ml/datasets/auto+mpg">https://archive.ics.uci.edu/ml/datasets/auto+mpg</a>. Irvine, CA: University of California, School of Information and Computer Science.
