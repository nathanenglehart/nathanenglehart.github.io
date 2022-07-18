---
layout: home2
permalink: /z-score/
title: Standard Score
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The Standard Score, commonly known as a Z-score, is used to calculate how far away from the mean a specific data point is in terms of standard deviations. This calculation is highly useful for various applications, including the difference in proportions test. \
\
The equation for the Z score for a raw score $x$ is computed by \\[Z = \frac{x - \mu}{\sigma}\\]
where $\sigma$ is the standard deviation of the population and $\mu$ is the population mean. \
\
Before calculating a Z-score, one must compute the mean, given by \\[\mu = \frac{\sum x_i}{N}\\]
where $x_i$ represents each value from the population and $N$ represents the size of the population. \
\
One must also compute the the standard deviation, given by \\[\sigma = \sqrt{\frac{\sum (x_i - \mu)^2}{N}} \\] 
where, again, $x_i$ represents each value from the population and $N$ represents the size of the population. \
\
Thus, to compute the Z-score of 1 in a population of 1, 2, 3, 4, and 5, we can write

```c
/* Nathan Englehart (Summer, 2022) */

#include <stdio.h>
#include <math.h>

double mean(double * population, int population_length) {
	
	/* Returns population mean. */

	double sum = 0;

	for(int i = 0; i < population_length; i++) {
		sum += population[i];	
	}

	return sum;
}

double standard_deviation(double * population, int population_length) {

	/* Returns population standard deviation. */

	double sum = 0;
	double mu = mean(population, population_length);

	for(int i = 0; i < population_length; i++) {
		sum += pow(population[i] - mu, 2);
	}

	return sqrt((sum / population_length));
}


int main() {

	double population [5] = {1,2,3,4,5};
	int population_length = sizeof population / sizeof population[0];

	double x = population[0];
	double mu = mean(population, population_length);
	double sigma = standard_deviation(population, population_length);

	double z = (x - mu) / sigma;

	printf("z = %f\n.",z);

	return 0;
}
```

Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/z-score">https://github.com/nathanenglehart/z-score</a>.

### References

Kreyszig, Erwin. (1979). Advanced Engineering Mathematics (Fourth ed.). Wiley.
