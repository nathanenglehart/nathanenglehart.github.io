---
layout: home2
permalink: /chi-squared-test/
title: Pearson's Chi-squared Test
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-16-22
comments: false
---

<!--## Pearson's Chi-squared Test-->

The equation for Pearson's Chi-squared Test, commonly known as the Chi-squared Test, is used by social scientists and staticians to determine whether there is a statistically significant relationship between two nominal variables by accepting or rejecting a null hypothesis.  

### Implementation

The equation for Pearson's Chi-squared Test is given by \\[x^2 = \sum \frac{(O_i - E_i)^2}{E_i}\\]where $x^2$ is chi squared, $O_i$ is the observed value and $E_i$ is the expected value. \
\
Similar to the Student's T-Test, degrees of freedom are needed to interpret the findings Chi-squared Tests. The equation for degrees of freedom for the Chi-squared Test is given by \\[ \text{df} = (r-1)(c-1) \\]
where $r$ is the number of categories of the independent variable and $c$ is the number of categories of the dependent variable. \
\
Suppose we suspect that women will be more likely than men to think that the government should do more to address climate change. Then, using data from the 2016 American National Election Study Pilot (ANES), we find that between the male and female sexes, respondents felt that

<!--\\[\begin{array} {|r|r|}\hline  & \text{Male} & \text{Female} & \text{Total} \\ \hline \text{Do Less} & 163 & 101 & 264 \\ \hline \text{Do More} & 309 & 395 & 704 \\ \hline \text{Total} & 472 & 496 & 968 \\ \hline  \end{array}\\] -->

| Gov. should do more about rising temps. | Male | Female | Total |
| :---- | :----: | :----: | :----: |
| Do Less | 163 | 101 | 264 |
| Do More | 309 | 395 | 704 |
| Total | 472 | 496 | 968 |

where "Do Less" indicates that the respondent feels the government should do less about climate change and "Do More" indicates that the respondent feels the government should do more about climate change. As such, we can see that approximately 65% of men selected "Do More" compared to 79% of women. To test whether these results are statistically significant, or merely due to chance alone, we must formulate a null hypothesis: "Women will be no more likely than men to think that the government should do more to address climate change." \
\
Then, to perform our Chi-squared Test, using C we can write

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
In this case, the sample's chi squared value is 24.486299 with 1 degree of freedom.

### Interpreting Results
To interpret the results of a Chi-squared Test, one needs to know three details:

- The chi squared value.
- The degrees of freedom of the Chi-squared Test.
- The level of significance of the Chi-squared Test.

 Then, given a Chi-squared distribution table (such as the one available at <a style="color: #f56a6a; !important" href="https://people.richland.edu/james/lecture/m170/tbl-chi.html">https://people.richland.edu/james/lecture/m170/tbl-chi.html</a>) and these three details, one can determine the meaning of the Chi-squared Test results and whether to accept or reject the null hypothesis. \
\
If one's determined chi squared value is less than its corresponding value on the Chi-squared distribution table, one should accept the null hypothesis, meaning that no relationship was found between variables. Conversely, if one's determined chi squared value is greater than its corresponding value on the Chi-squared distribution table, one should reject null the hypothesis.\
\
In the case of our previous example using ANES data, given that we found the sample's chi squared value is 24.486299 with 1 degree of freedom, one can see that results from the ANES data were significant at the .001 level. This means that the probability that the observed results are due to chance alone is less than .1%. Thus, we can reject the null hypothesis and consequently. Likewise, since a greater percentage of women than men responded "Do More" when asked if the government should do more to combat climate change, we can conclude the chance that women in the sample are more likely than men to think that the government should do more to address climate change is due to chance alone is less than .1%. \
\
Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/chi-squared-test">https://github.com/nathanenglehart/chi-squared-test</a>.

### References 
Miller, Melissa. (2021, June). Chi-squared Test [Lecture]. Bowling Green State University: POLS 2900, Bowling Green, OH, United States.\
\
American National Election Studies. (2016). <i>2016 American National Election Study Pilot</i> [Data File]. Retrieved from <a style="color: #f56a6a; !important" href="https://electionstudies.org/data-center/anes-2016-pilot-study/">https://electionstudies.org/data-center/anes-2016-pilot-study/</a>.
