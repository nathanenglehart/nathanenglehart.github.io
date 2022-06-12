---
layout: home2
permalink: /t-test/
title: Student's T-Test
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

In statistics, the Standard Score, commonly known as a Z-score, is used to calculate the number of standard deviations away from the mean for specific data points. This calculation is highly useful in ... \
\
The equation for the Z score for a raw score $x$ is computed by\\[Z = \frac{x - \mu}{\sigma}\\]
where $\sigma$ is the standard deviation of the population and $\mu$ is the population mean. \
\
As such, before calculating a Z-score, one must compute the mean, given by \\[\mu = \frac{\sum x_i}{N}\\]
where $x_i$ represents each value from the population and $N$ represents the number of terms in the population. \
\
One must also compute the the standard deviation, given by $$\sigma = \sqrt{\frac{\sum (x_i - \mu)^2}{N}}$$where, again, $x_i$ represents each value from the population and $N$ represents the number of terms in the population. \
\
Thus, to compute a Z-score, we can write

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

Code available at <a href=""></a>.
