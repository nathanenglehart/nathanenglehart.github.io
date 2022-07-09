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
where $\overline{x}$ is the observed mean of the sample, $\mu$ is the theoretical mean of the population, $s$ is the standard deviation of the sample, and $n$ is the sample size. For the One Sample T-Test, degrees of freedom are given by \\[\text{df} = n - 1 \\]
<!--Suppose the observed sample mean is 74, the theoretical population mean is 78, the standard deviation of the sample is 3.5, and the sample size is 10. Then, we can write:-->
According to U.S. News & World Report, in 2021, the average American watched 3.1 hours of tv per day (<a style="color: #f56a6a; !important" href="https://www.usnews.com/news/best-states/articles/2021-07-22/americans-spent-more-time-watching-television-during-covid-19-than-working">link</a>). As such, using data from the 2021 General Social Survey (GSS), we can hypothesize that there is no difference in the tv watching habits of the average American according to U.S. News & World Report and the average General Social Survey respondent. Then, using the tvhours variable from the 2021 GSS, we can find that the observed sample mean is 3.476400, the standard deviation of the sample is 3.101692, and the sample size is 3780. Additionally, as previously noted, the theoretical population mean is 3.1. Then, we can write using C:

```c
/* Nathan Englehart (Summer, 2022) */

#include <stdio.h>
#include <math.h>

double one_sample(double x_bar, double mu, double s, int n) {

	/* Returns t value for one tailed t-test using given variables. */

	return (x_bar - mu) / (s / sqrt(n));
}

int main() {

	double x_bar = 3.476400;
	double mu = 3.1;
	double s = 3.10192;
	int n = 3780;
	
	double t = one_sample(x_bar, mu, s, n);

	printf("absolute t = %f.\n",t);
	printf("df = %d.\n",n-1);

	return 0;
}
```
In this case, the sample's T value is 7.460448 with degree of freedom of 3779. \
\
To avoid so many manual calculations, this process can be simplified using R:

```r
#!/usr/bin/env Rscript

### Nathan Englehart (Summer 2022)

library(haven) # to read sav

gss_data <- read_sav("data.sav")

one_sample_t_test <- function(sample,theoretical_mean) {

   ### Returns the t value and degrees of freedom with one sample t test using given sample and theoretical mean. 
   ###	
   ###   Args:
   ###           
   ###	      sample::[List]
   ###	         Given sample 
   ###	      
   ###	      theoretical_mean::[Double]
   ###	         Theoretical mean of sample.
   ###
   ###

   t = (mean(sample,na.rm = TRUE)-theoretical_mean)/(sd(sample, na.rm = TRUE)/(sqrt(length(!is.na(sample)))))
   df = length(sample) - 1 

   return(list(t,df))
}

sample = as.numeric(unlist(gss_data[,"tvhours"])) # "how many hours per day do you watch tv?"
tm = 3.1

result = one_sample_t_test(sample,tm)
result
```
And this script returns the same results.

### Two Sample

The two sample T-Test equation is given by \\[t = \frac{\overline{x}_1-\overline{x}_2}{\sqrt{\frac{s_1^2}{n_1-1} + \frac{s_2^2}{n_2-1}}} \\] where $\overline{x}_1$ is the observed mean of the 1st sample and $\overline{x}_2$ is the observed mean of the 2nd sample, $s_1$ is the standard deviation of the 1st sample and $s_2$ is the standard deviation of the 2nd sample, and $n_1$ is the size of the first sample whereas $n_2$ is the size of the second sample. In addition, for the Two Sample T-Test, degrees of freedom are given by \\[ \text{df} = n_1 + n_2 + 2 \\]
<!--Suppose the observed mean of the 1st sample is 3392.00, the observed mean of the 2nd sample is 16610.86, the standard deviation of the first sample is 3848.102, the standard deviation of the second sample is 3725.971, the size of the first sample is 84, and the size of the second sample is 21. Then, we can write:-->
Suppose we hypothesize that between the male and female sexes, their is no difference that the average amount of tv watched per day. Let males represent the first sample and females represent the second sample. Then, again, using 2021 GSS survey data, the observed mean of the first sample is 3.379852, the standard deviation of the first sample is 3.155964, and the size of the first sample is 1082. Similarly, the observed mean of the second sample is 3.557818, the standard deviation of the second sample is 3.071581, and the size of the second sample is 1375. Then, using C, we can write:

<!--

[1] "men:"
[1] "mean: 3.379852"
[1] "sd: 3.155964"
[1] "n: 1082"
[1] ""
[1] "women:"
[1] "mean: 3.557818"
[1] "sd: 3.071581"
[1] "n 1375"



-->
```c
/* Nathan Englehart (Summer, 2022) */

#include <stdio.h>
#include <math.h>

long double two_sample(double x_bar_1, double x_bar_2, double s_1, double s_2, double n_1, double n_2) {

	/* Returns t value for two tailed t-test using given variables. */

	return (x_bar_1 - x_bar_2) / sqrt(((long double) ((long double) ((pow(s_1,2))) / (n_1-1))) + ((long double) ((long double) ((pow(s_2,2))) / (n_2-1)))); 

}

int main() {

	/* Variables */

	double x_bar_1 = 3.379852;
	double x_bar_2 = 3.557818;
	double s_1 = 3.155964;
	double s_2 = 3.071581;
	int n_1 = 1082;
	int n_2 = 1375;

	double t = two_sample(x_bar_1, x_bar_2, s_1, s_2, n_1, n_2);

	printf("t = %f.\n",t);
	printf("df = %d.\n",n_1 + n_2 - 2);

	return 0;
}
```
In this case, the sample's T value is -1.403427 with 2455 degrees of freedom. \
\
Again, to avoid so many manual calculations, we can use R:

```r
#!/usr/bin/env Rscript

### Nathan Englehart (Summer 2022)

library(haven) # to read sav

gss_data <- read_sav("data.sav")

two_sample_t_test <- function(sample_1,sample_2) {

   ### Returns the t value and degrees of freedom with two sample t test using given samples. 
   ###	
   ###   Args:
   ###          
   ###	      sample_1::[List]
   ###	         First given sample 
   ###
   ###	      sample_2::[List]
   ###	         Second given sample
   ###
   ###

   t = (mean(sample_1, na.rm = TRUE) - mean(sample_2, na.rm = TRUE))/sqrt((((sd(sample_1, na.rm = TRUE))^2)/(sum(!is.na(sample_1))-1)) + (((sd(sample_2, na.rm = TRUE))^2)/(sum(!is.na(sample_2))-1)))
   df = sum(!is.na(sample_1)) + sum(!is.na(sample_2)) - 2

   return(list(t,df))
}

s <- as.numeric(unlist(gss_data[,"sex"]))
t <- as.numeric(unlist(gss_data[,"tvhours"]))

sample <- data.frame(s,t)

m <- subset(sample, sample$s == 1)
w <- subset(sample, sample$s == 2)

m_hours <- m$t
w_hours <- w$t

result <- two_sample_t_test(m_hours,w_hours)
result
```

### Interpreting Results

To interpret the results of a one or two sample T-Test, one needs to know four details:

- The T value.
- The degrees of freedom of the T-Test.
- The number of tails of the T-Test.
	- For a directional hypothesis, one should conduct a one-tailed T-Test. For a non-directional hypothesis, one should conduct a two tailed T-Test. 
- The level of significance or alpha level of the T-Test, e.g. 0.05, 0.01, or 0.10.

As such, given a T distribution table (such as the one available at <a style="color: #f56a6a; !important" href="https://t-tables.net/">https://t-tables.net/</a>) and these four details, one can determine the meaning of T-Test results and whether to accept or reject the null hypothesis. \
\
If one's determined T value is greater than its corresponding value on the T distribution table, one should reject the null hypothesis. If one's determined T value is less than its correpsonding value on the T distribution table, one should accept the null hypothesis, meaning that our test found no significant relationship between variables. \
\
For example, in the One Sample T-Test example, we found a T value of 7.4096 which is greater than 3.291, the value for the 99.9% confidence interval (with alpha = 0.0005) for more than 1000 degrees of freedom. As such, we can reject the null hypothesis, meaning there is a statistically significant difference in the average amount of time Americans watch tv per day according to U.S. News & World Report and the average amount of time respondents watch tv per day according to the 2021 GSS. \
\
Similarly, in the Two Sample T-Test example, we found a T value of -1.403427, the absolute value of which is greater than the value for alpha = 0.2 on the 80% confidence interval for more than 1000 degrees of freedom. Therefore, for the 0.20 level of significance, we can reject the null hypothesis meaning we can be 80% confident that there is a statistically significant difference in the average amount of time Americans of the male and female sex watch tv per day. However, for lower levels of significance, we cannot reject the null hypothesis. \
\
Code available at: <a style="color: #f56a6a; !important" href="https://github.com/nathanenglehart/t-test">https://github.com/nathanenglehart/t-test</a>.

### References

Miller, Melissa. (2021, June). T-Test [Lecture]. Bowling Green State University: POLS 2900, Bowling Green, OH, United States.\
\
GSS General Social Survey. (2021). 2021 General Social Survey [Data File]. Retrieved from <a style="color: #f56a6a; !important" href="https://gss.norc.org/">https://gss.norc.org/</a>.
\
<!--T Table. (2022). <a style="color: #f56a6a; !important" href="https://www.ztable.net/">https://www.ztable.net</a>-->
