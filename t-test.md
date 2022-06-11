---
layout: home2
permalink: /t-test/
title: Student's T-Test
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The Student's T-Test, commonly known as the T-Test, is frequently used by social scientists and staticians to determine whether to reject or accept a null hypothesis. 

### One Sample

The one sample T-Test equation is given by \\[t = \frac{\overline{x} - \mu}{\frac{s}{\sqrt{n}}}\\]
where $\overline{x}$ is the observed mean of the sample, $\mu$ is the theoretical mean of the population, $s$ is the standard deviation of the sample, and $n$ is the sample size. For the One Sample T-Test, degrees of freedom are given by \\[df = n - 1 \\]
Suppose the observed sample mean is 74, the theoretical population mean is 78, the standard deviation of the sample is 3.5, and the sample size is 10. Then, we can write:

```c
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

	double t = one_sample(x_bar, mu, s, n);

	printf("absolute t = %f.\n",t);
	printf("df = %d.\n",n-1);

	return 0;
}
```
In this case, the sample's T value is 3.61 with degree of freedom of 9. 

### Two Sample

The two sample T-Test equation is given by \\[t = \frac{\overline{x}_1-\overline{x}_2}{\sqrt{\frac{s_1^2}{n_1-1} + \frac{s_2^2}{n_2-1}}} \\] where $\overline{x}_1$ is the observed mean of the 1st sample and $\overline{x}_2$ is the observed mean of the 2nd sample, $s_1$ is the standard deviation of the 1st sample and $s_2$ is the standard deviation of the 2nd sample, and $n_1$ is the size of the first sample whereas $n_2$ is the size of the second sample. In addition, for the One Sample T-Test, degrees of freedom are given by \\[ df = n_1 + n_2 + 2 \\]
Suppose the observed mean of the 1st sample is 3392.00, the observed mean of the 2nd sample is 16610.86, the standard deviation of the first sample is 3848.102, the standard deviation of the second sample is 3725.971, the size of the first sample is 84, and the size of the second sample is 21. Then, we can write:

```c
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

	double t = two_sample(x_bar_1, x_bar_2, s_1, s_2, n_1, n_2);

	printf("t = %f.\n",t);
	printf("df = %d.\n",n_1 + n_2 - 2);

	return 0;
}
```
In this case, the sample's T value is -14.445582 with 103 degrees of freedom. 

### Interpreting Results

To interpret the results of a one or two sample T-Test, one needs to know four details:

- The T value.
- The degrees of freedom of the T-Test.
- The number of tails of the T-Test.
	- For a directional hypothesis, one should conduct a one-tailed T-Test. For a non-directional hypothesis, one should conduct a two tailed T-Test. 
- The level of significance or alpha level of the T-Test, e.g. 0.05, 0.01, or 0.10.

As such, given a T distribution table (shown below) and these four details, one can determine the meaning of T-Test results and whether to accept or reject the null hypothesis. 

|           |<p align="left">P</p>      |       |        |        |        |         |         |
|-----------|-------|-------|--------|--------|--------|---------|---------|
| one-tail  | 0.1   | 0.05  | 0.025  | 0.01   | 0.005  | 0.001   | 0.0005  |
| two-tails | 0.2   | 0.1   | 0.05   | 0.02   | 0.01   | 0.002   | 0.001   |
| df        |       |       |        |        |        |         |         |
| 1         | 3.078 | 6.314 | 12.706 | 31.821 | 63.656 | 318.289 | 636.578 |
| 2         | 1.886 | 2.92  | 4.303  | 6.965  | 9.925  | 22.328  | 31.6    |
| 3         | 1.638 | 2.353 | 3.182  | 4.541  | 5.841  | 10.214  | 12.924  |
| 4         | 1.533 | 2.132 | 2.776  | 3.747  | 4.604  | 7.173   | 8.61    |
| 5         | 1.476 | 2.015 | 2.571  | 3.365  | 4.032  | 5.894   | 6.869   |
| 6         |       |       |        |        |        |         |         |
| 7         |       |       |        |        |        |         |         |
| 8         |       |       |        |        |        |         |         |
| 9         |       |       |        |        |        |         |         |
| 10        |       |       |        |        |        |         |         |

If one's determined T value is greater than its corresponding value on the T distribution table, one should reject the null hypothesis. If one's determined T value is less than its correpsonding value on the T distribution table, one should accept the null hypothesis, meaning that our test found no significant relationship between variables. \
\
Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/t-test">https://github.com/nathanenglehart/t-test</a>.

### References

Miller, Melissa. (2021, June). T-Test [Lecture]. Bowling Green State University: POLS 2900, Bowling Green, OH, United States.
