---
layout: home2
permalink: /projects/chi-squared-test/
title: Pearson's Chi-squared Test
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

The equation for Pearson's Chi-squared Test, commonly known as the Chi-squared Test, is used to determine whether there is a statistically significant relationship between two nominal variables.

### Implementation

The equation for Pearson's Chi-squared Test is given by \\[\chi^2 = \sum \frac{(O_i - E_i)^2}{E_i}\\]where $\chi^2$ is chi squared, $O_i$ is the observed value and $E_i$ is the expected value. \
\
Similar to the Student's T-Test, degrees of freedom are needed to interpret the findings Chi-squared Tests. The equation for degrees of freedom for the Chi-squared Test is given by \\[ \text{df} = (r-1)(c-1) \\]
where $r$ is the number of categories of one variable and $c$ is the number of categories of the other variable. \
\
Suppose we hypothesize that: "Women will be more likely than men to think that the government should do more to address climate change." \
\
Using data from the 2016 American National Election Study Pilot (ANES), we find the following:

| Gov. should do more about rising temps. | Male | Female | Total |
| :---- | :----: | :----: | :----: |
| Do Less | 163 | 101 | 264 |
| Do More | 309 | 395 | 704 |
| Total | 472 | 496 | 968 |

In the table, "Do Less" indicates that the respondent feels the government should do less about climate change and "Do More" indicates that the respondent feels the government should do more about climate change. We can see in the table that approximately 65% of men selected "Do More" compared to 79% of women. \
\
To perform our Chi-squared Test using C, we can write the following:

```c
/* Nathan Englehart, (Summer, 2022) */

#include <stdio.h>
#include <math.h>

double cross_tabs [3][3] = {
	{163, 101, 264},
	{309, 395, 704},
	{472, 496, 968}
};

int length = sizeof(cross_tabs) / sizeof(cross_tabs[0]);
int width = sizeof(cross_tabs[0])/sizeof(cross_tabs[0][0]);

double chi_squared_test(double cross_tabs[length][width]) {

	/* Returns chi-squared value using given inputs. */

	double chi_squared = 0;

	for(int i = 0; i < length-1; i++)
	{
		for(int j = 0; j < width-1; j++)
		{
			double e = cross_tabs[i][width-1] * (cross_tabs[length-1][j] / cross_tabs[length-1][width-1]);
			chi_squared += (pow(cross_tabs[i][j] - e, 2)) / e;
		}
	}

	return chi_squared;
}

int main() {

	double chi_squared = chi_squared_test(cross_tabs);

	printf("chi_squared = %f.\n",chi_squared);
	printf("df = %d.\n",(length - 2)* (width - 2));	

	return 0;
}
```
In this case, the sample's chi-squared value is 24.486299 with 1 degree of freedom.

### Interpreting Results
To interpret the results of a Chi-squared Test, one needs to know three details:

- The chi-squared value
- The degrees of freedom of the Chi-squared Test
- The level of significance or alpha level of the Chi-squared Test e.g. 0.05, 0.01, 0.001 (similar to the T-Test)

Given a Chi-squared distribution table (such as the one available at <a style="color: #f56a6a; !important" href="https://people.richland.edu/james/lecture/m170/tbl-chi.html">https://people.richland.edu/james/lecture/m170/tbl-chi.html</a>) and these three details, one can determine the meaning of the Chi-squared Test results and whether our hypothesis was significant or the results were likely due to chance alone. \
\
If one's computed chi-squared value is less than its corresponding value on the chi-squared distribution table for the given degrees of freedom and an alpha level of 0.05 or less, the relationship between the two variables is not statistically significant. In other words, the results may have been due to chance alone. \
\
Conversely, if one's computed chi squared value is greater than its corresponding value on the chi-squared distribution table for the given degrees of freedom and an alpha level of 0.05 or less, the relationship between variables is statistically significant. \
\
In our previous example using ANES data, the computed chi-squared value is 24.486299 with 1 degree of freedom. Therefore, the results are statistically significant at the 0.001 level, since 24.486299 exceeds the value of chi-squared in the distribution table for 1 degree of freedom at the 0.001 level. This means that the probability that the observed results are due to chance alone is less than .1%. \
\
We know that the results are statistically significant, but the results must be consistent with the hypothesis in order for the hypothesis to be supported. Recall that a greater percentage of women than men responded "Do More" when asked if the government should do more to combat climate change. As such, we can conclude that the hypothesis is supported. \
\
For the Chi-squared Test and other significance tests, the results must be statistically significant and consistent with the hypothesis in order for the hypothesis to be supported. \
\
Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/chi-squared-test">https://github.com/nathanenglehart/chi-squared-test</a>.

### References 
American National Election Studies. (2016). <i>2016 American National Election Study Pilot</i> [Data File]. Retrieved from <a style="color: #f56a6a; !important" href="https://electionstudies.org/data-center/anes-2016-pilot-study/">https://electionstudies.org/data-center/anes-2016-pilot-study/</a>. \
\
Miller, Melissa. (2021, June). Chi-squared Test [Lecture]. Bowling Green State University: POLS 2900, Bowling Green, OH, United States.
