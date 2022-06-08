---
layout: home2
permalink: /t-test/
title: Student's T-Test
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

### One Sample T-Test

The one sample T-Test equation is given by \\[t = \frac{\overline{x} - \mu}{\frac{s}{\sqrt{n}}}\\]
where $x$ is the observed mean of the sample, $\mu$ is the theoretical mean of the population, $s$ is the standard deviation of the sample, and $n$ is the sample size. \
\
Suppose the observed sample mean is 74, the theoretical population mean is 78, the standard deviation of the sample is 3.5, and the sample size is 10. Then, we can write:

```C

/* Nathan Englehart (Summer, 2022) */

#include <stdio.h>
#include <math.h>

double one_sample(double x_bar, double mu, double s, int n) {

	/* Returns t value for one tailed t-test using given variables. */

	return (x_bar - mu) / (s / sqrt(n));

}

int main() {

	double x_bar = 74.0;
	double mu = 78.0;
	double s = 3.5;
	int n = 10;

	double t = fabs(one_sample(x_bar, mu, s, n));

	printf("absolute t = %f.\n",t);
	printf("df = %d.\n",n-1);

	return 0;

}


```
In this case, the sample's absolute T-Test value is 3.61 with degree of freedom of 9. 

### Two Sample Tailed T-Test

The two sample T-Test equation is given by \\[t = \frac{\overline{x}_1-\overline{x}_2}{\sqrt{\frac{s_1^2}{n_1-1} + \frac{s_2^2}{n_2-1}}}\\]
where $\overline{x}_1$ is the observed mean of the 1st sample and $\overline{x}_2$ is the observed mean of the 2nd sample, $s_1$ is the standard deviation of the 1st sample and $s_2$ is the standard deviation of the 2nd sample, and $n_1$ is the size of the first sample whereas $n_2$ is the size of the second sample.  

```C

/* Nathan Englehart (Summer, 2022) */

#include <stdio.h>
#include <math.h>

double two_sample(double x_bar_1, double x_bar_2, double s_1, double s_2, int n_1, int n_2) {

	/* Returns t value for two tailed t-test using given variables. */

	return (x_bar_1 - x_bar_2) / (sqrt((pow(s_1,2)/n_1-1) + ((pow(s_2,2)/n_2-1))));

}

int main() {

	/* Variables */

	double x_bar_1 = 3392.00;
	double x_bar_2 = 16610.86;
	double s_1 = 3848.102;
	double s_2 = 3725.971;
	int n_1 = 84;
	int n_2 = 21;

	double t = fabs(two_sample(x_bar_1, x_bar_2, s_1, s_2, n_1, n_2));

	printf("absolute t = %f.\n",t);
	printf("df = %d.\n",n_1 + n_2 - 2);

	return 0;

}

```
In this case, the sample's absolute T-Test value is 14.445582 with 103 degrees of freedom. 

### Interpreting Results

To interpret the results of a T-Test, one needs to know four values:

- The T value.
- The degrees of freedom of the T-Test.
- The number of tails of the T-Test.
	- For a directional hypothesis, one should conduct a one-tailed T-Test. For a non-directional hypothesis, one should conduct a two tailed T-Test. 
- The level of significance or alpha level of the T-Test, e.g. 0.01, 0.05, or 0.10.

As such, given a T distribution table, one can determine the meaning of T-Test results.
