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
OLS seeks to find coefficients for the regression function which minimize prediction error. It does so with
\\[ \theta = (X^T X)^{-1} X^T t \\]
where $\theta$ represents the OLS estimator, $X$ is an n $\times$ m matrix containing independent variable parameters, and $t$ is a vector of response variables. \
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

In situations where regression equations need to be generalizable for future data, regressions that employ regularization methods to penalize overfitting with high coefficients are often utilized with cross validation. One such method is ridge regression, which computes its coefficients with
\\[ \theta = (X^T X + \lambda I)^{-1} X^T t \\]
where $\theta$ represents the ridge estimator, $X$ is an n $\times$ m matrix containing independent variable parameters, $\lambda$ is the weight penalty, $I$ is a m $\times$ m identity matrix, and $t$ is a vector of response variables. Notably, ridge regression is also highly useful with multicollinear, highly correlated independent variables.  \
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

Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/regression">https://github.com/nathanenglehart/regression</a>.
