---
layout: home2
permalink: /projects/probability/
title: Probability Theory 
tags: [Jekyll, theme, responsive, blog, template]
modified: 2-1-23
comments: false
---

### Sample Spaces and Probability Spaces

**Sample spaces** are denoted by $\Omega$ with elements called **sample points** $\omega \in \Omega$.  Subsets of $\Omega$ are called **events** = $\{\omega_1, ..., \omega_n\} \subseteq \Omega$. The collection of events in $\Omega$ is denoted by $\mathcal{F}$. \
\
The **probability measure** (also known as a probability distribution) $P$ is a function from $\mathcal{F}$ into the real numbers. Each **event** $A$ has a probability $\mathbb{P}(A)$. A **probability space** is defined by the triple $(\Omega, F, \mathbb{P})$.

### Kolmogorov's Axioms

Probability measures satisfy the following axioms, known as **Kolmogorov's axioms**:

- (i) $0 \leq \mathbb{P}(A) \leq 1$ for each event $A$.
- (ii) $\mathbb{P}(\Omega) = 1$ and $\mathbb{P}(\emptyset) = 0$.
- (iii) If $A_1, A_2, A_3, ...$ is a sequence of pairwise disjoint (**mutually exclusive**, meaning $A_i \cap A_j = \emptyset$ for $i \neq j$) events then \\[\mathbb{P} \bigg( \bigcup_{i=1}^{\infty} A_i \bigg) = \sum^{\infty}_{i = 1} \mathbb{P}(A_i)\\]
where (iii) also applies to finite events, meaning that if $A_1, A_2, ..., A_n$ are pairwise disjoint events, then \\[\mathbb{P}(A_1 \cup \text{ }...\text{ } \cup A_n) = \mathbb{P}(A_1) + \text{ } ... \text{ } + \mathbb{P}(A_n)\\]

### Experiments with Equally Likely Outcomes

Suppose $\Omega$ is a sample space that is finite. Let $\text{#}\Omega$ denote the **total number of possible outcomes**. If each outcome $\omega \in \Omega$ has the same probability, then: \\[ \mathbb{P}(\omega) = \frac{1}{\text{#} \Omega} \\] because probabilities must add up to 1. As such: \\[\mathbb{P}(A) = \mathbb{P}(a_1) + \mathbb{P}(a_2) + ... + \mathbb{P}(a_r) = \frac{\text{#}A}{\text{#}\Omega} \\] where $\text{#}A$ denotes the **number of elements in set** $A$. So, if the sample space $\Omega$ has finitely many elements and each outcome is equally likely, then for any event $A \subseteq \Omega$ we have: \\[\mathbb{P}(A) = \frac{\text{#}A}{\text{#}\Omega}\\]

### Conditional Probability

Let $B$ be an event in the sample space $\Omega$ such that $P(B) > 0$. Then for all events $A$ the **conditional probability** of $A$ given $B$ is defined as 
\\[ \mathbb{P}(A \text{ } \vert \text{ } B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)} \\]
If $A_1,...,A_n$ are events and all the conditional probabilities below make sense then we have 
\\[\mathbb{P}(A_1 \text{ } \cap \text{ } ... \text{ } \cap \text{ } A_n) = \mathbb{P}(A_1) \mathbb{P}(A_2 \text{ } \vert \text{  } A_1) \mathbb{P}(A_3 \text{ } \vert \text{ } A_1 \cap A_2) \text{ } \cdot \text{ } ... \text{ } \cdot \text{ } \mathbb{P}(A_n \text{ } \vert \text{ } A_1 \text{ } \cap \text{ } ... \text{ } \cap \text{ } A_{n-1} )\\]

### Bayes Formula

Let $A$, $B$ be events in the sample space $\Omega$ such that $\mathbb{P}(B) > 0$. **Bayes formula** for two random variables is given by \\[\mathbb{P}(A \text{ } \vert \text{ } B) = \frac{\mathbb{P}(B \text{ } \vert \text{ } A)\mathbb{P}(A)}{\mathbb{P}(B)}\\]In addition, **Bayes formula** has a general version. Let $B_1,..., B_n$ be a partition of the sample space $\Omega$ such that each $\mathbb{P}(B_i) > 0$. Then for any event $A$ with $\mathbb{P}(A) > 0$, and any $k = 1,...,n$, \\[\mathbb{P}(B_k \text{ } \vert \text{ } A) = \frac{\mathbb{P}(A \cap B_k)}{\mathbb{P}(A)} = \frac{\mathbb{P}(A \text{ } \vert \text { } B_k) \mathbb{P}(B_k)}{\sum^n_{i=1} \mathbb{P}(A \text{ } \vert \text{ } B_i) \mathbb{P}(B_i)}\\]

### Independence

Events $A_1, A_2,..., A_n$ are **independent** if and only if \\[\mathbb{P}(A_1 \text{ }\cap\text{ } ...\text{ } \cap \text{ }A_n) = \mathbb{P}(A_1) \text{ } \cdot \text{ } ... \text{ } \cdot \text{ } \mathbb{P}(A_n)\\]

### Random Variable

Let $\Omega$ be a sample space. A **random variable** is a function from $\Omega$ into the real numbers which represents numerical values derived from the outcomes of an experiment. Any random variable $X$ has a probability distribution which is the collection of probabilities $\mathbb{P}(X \in B)$ for sets $B$ of real numbers.

### Discrete Random Variable

A random variable $X$ is a discrete random variable if there exists a finite or countably infinite set $\{k_1, k_2, k_3,...\}$ of real numbers such that \\[\sum_i \mathbb{P}(X = k_i) = 1\\]where the sum ranges over the entire set of points $\{k_1, k_2, k_3,...\}$.

### Continuous Random Variable

A random variable $X$ is a continuous random variable if it satisfies

1. $\int_{-\infty}^\infty f(x) dx = 1$ for its probability density function $f$
2. $f(x) \geq 0$ for all values of $X$, denoted $x$.

### Probability Mass Function

Probability mass functions are only defined for discrete random variables. The probability mass function of a discrete random variable $X$ is the function $p$ (or $p_X$) is defined by \\[p(k) = \mathbb{P}(X = k)\\]for all possible values $k$ of $X$.\
\
The **conditional probability mass function** of a discrete random variable $X$ and an event $B$ with $\mathbb{P}(B) > 0$ is the function $p_{X|B}$ which is defined by 
\\[p_{X|B}(k) = \mathbb{P}(X = k \text{ } \vert \text{ } B) = \frac{\mathbb{P}(\{X=k\} \cap B)}{\mathbb{P}(B)}\\]

### Probability Density Function

Probability density functions are only defined for continuous random variables. The probability density function of a continuous random variable $X$ is a function $f$ that satisfies \\[\mathbb{P}(X \leq b) = \int_{-\infty}^b f(x)dx\\]for all real values $b$. \
\
Let $X$ and $Y$ be jointly continuous random variables with joint density function $f_{X,Y}(x, y)$. Then, **conditional probability density function** of $X$ given $Y$ is the function $f_{X|Y}(x \text{ } | \text{ } y)$ which is defined by  
\\[f_{X|Y}(x \text{ } | \text{ } y) = \frac{f_{X,Y}(x,y)}{f_Y(y)}\\]
for those $y$ such that $f_Y(y) > 0$. 

### Cumulative Probability Function

The cumulative distributon function of a random variable $X$ is defined by \\[F(s) = \mathbb{P}(X \leq s) \text{ } \text{ } \forall \text{ } s \in \mathbb{R}\\]

### Bernoulli Random Variable

Let $0 \leq p \leq 1$. A random variable $X$ has the Bernoulli distribution with success probability $p$ if $X$ is $\{0,1\}$-valued and satisfies $\mathbb{P}(X = 1) = p$ and $\mathbb{P}(X = 0) = 1 - p$. Abbreviate this by $X \sim \text{Ber}(p)$. In plain English, a Bernoulli random variable models a trial with two possibilites. This is a discrete random variable.

### Binomial Random Variable

Let $n$ be a positive integer and $0 \leq p \leq 1$. A random variable $X$ has the binomial distribution with parameters $n$ and $p$ if the possible values of $X$ are $\{0,1,...,n\}$ and the probabilities are \\[\mathbb{P}(X = k) = {n \choose k} p^k (1-p)^{n-k}\\]Abbreviate this by $X \sim \text{Bin}(n,p)$. In plain English, a Binomial random variable models the number of successes in $n$ trials with success probability $p$. This is a discrete random variable.

### Geometric Random Variable

Let $0 < p \leq 1$. A random variable $X$ has the geometric distribution with success parameter $p$ if the possible values of $X$ are $\{1,2,3,...\}$ and $X$ satisfies \\[\mathbb{P}(X = k) = (1-p)^{k-1}p\\]for positive integers $k$. Abbreviate this by $X \sim \text{Geom}(p)$. The geometric distribution gives the probability that the first occurence of success requires $k$ independent trials, each with success probability $p$. This is a discrete random variable.

### Negative Binomial Random Variable

Let $k$ be a positive integer and $0 < p < 1$. A random variable $X$ has the negative binomial distribution with parameters $(k,p)$ if the set of possible values of $X$ is the set of integers $\{k,k+1,k+2,...\}$ and the probability mass function is \\[ \mathbb{P}(X = n) = {n - 1 \choose k - 1} p^k(1-p)^{n-k} \text{ for } n \geq k\\]Abbreviate this by $X \sim \text{Negbin}(k,p)$. The negative binomial distribution represents the number of trials needed to get exactly $k$ successes of $n$ Bernoulli trials. This is discrete random variable.

### Uniform Random Variable

Let $[a,b]$ be a bounded interval on the real line. A random variable $X$ has the uniform distribution on the interval $[a,b]$ if $X$ has density function 
\\[f(x) = \begin{aligned} \begin{cases} \frac{1}{b-a}, &\text{ if } x \in [a,b] \\\\ 0, &\text{ if } x \not\in [a,b] \end{cases} \end{aligned} \\]
Abbreviate this by $X \sim \text{Unif}[a,b]$. This is a continuous random variable.

### Gaussian Random Variable

A random variable $X$ has the normal distribution (the Gaussian distribution) if $X$ has the density function 
\\[ f(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}  \\]
on the real line. Abbreviate this by $X \sim \mathcal{N}(\mu,\sigma)$.\ 
\
A random variable $Z$ has the standard normal distribution (the standard Gaussian distribution) if $Z$ has density function 
\\[f(z) = \frac{1}{\sqrt{2\pi}}e^{\frac{-z^2}{2}}\\]
on the real line. Abbreviate this by $Z \sim \mathcal{N}(0,1)$. This is a continuous random variable.

### Poisson Random Variable

Let $\lambda > 0$. A random variable $X$ has the Poisson distribution with parameter $\lambda$ if $X$ is nonnegative integer valued and has the probability mass function \\[\mathbb{P}(X = k) = e^{-\lambda} \frac{\lambda^k}{k!}, k\in \{0,1,2,...\}\\]Abbreviate this by $X \sim \text{Poisson}(\lambda)$. This is a continuous random variable.

### Exponential Random Variable

Let $0 < \lambda < \infty$. A random variable $X$ has the exponential distribution with parameter $\lambda$ if $X$ has the density function 
\\[f(x) = \begin{aligned} \begin{cases} \lambda e^{-\lambda x}, & x \geq 0 \\\\ 0, & x < 0 \end{cases} \end{aligned} \\]
on the real line. Abbreviate this by $X \sim \text{Exp}(\lambda)$. The $\text{Exp}(\lambda)$ distribution is also called the exponential distribution with rate $\lambda$. The exponential distribution is the continuous counterpart to the geometric distribution. The exponential distribution models continuous waiting times, for example, the first customer arrival at a post office. This is a continuous random variable.

### Expectation

Let $X$ be a random variable. The **expectation** of $X$ is a weighted average of all possible values of $X$. The weights are determined by the probability mass/density function of $X$.\
\
The expectation or mean of a discrete random variable $X$ is defined by \\[\mathbb{E}(X) = \sum_k k \mathbb{P}(X = k) \\]where the sum ranges over all the possible values $k$ of $X$.\
\
The conditional expectation or mean of a discrete random variable $X$ given event $B$ is denoted by $\mathbb{E}[X \vert B]$ and is defined as 
\\[\mathbb{E}[X \vert B] = \sum_k k p_{X|B}(k) = \sum_k k \mathbb{P}(X = k \text{ } \vert \text{ } B)\\]
where the sum ranges over all all possible values $k$ of $X$.\
\
The expectation or mean of a continuous random variable $X$ with density function $f$ is \\[\mathbb{E}(X) = \int_\infty^{-\infty} x f(x) dx\\]An alternative symbol is $\mu = \mathbb{E}(X)$.\
\
The conditional expectation of a continuous random variable $X$ given another random variable $Y$ denoted by $\mathbb{E}[X \vert Y = y]$ is defined as 
\\[\mathbb{E}[X \text{ } \vert \text{ } Y = y] = \int^\infty_{-\infty} x f_{X|Y}(x \vert y)dx\\]
where $y$ is defined such that $f_Y(y) > 0$.\
\
Let $g$ be a real-valued function defined on the range of a random variable $X$. If $X$ is a discrete random variable then \\[\mathbb{E}(g(X)) = \sum_k g(k)\mathbb{P}(X = k)\\]while if $X$ is a continuous random variable with density function $f$ then \\[\mathbb{E}(g(X)) = \int_{-\infty}^\infty g(x)f(x)dx\\]The *n*th moment of the random variable $X$ is the expectation $\mathbb{E}(X^n)$. In the discrete case the *n*th moment is calculated by \\[\mathbb{E}(X^n) = \sum_k k^n \mathbb{P}(X = k)\\]If $X$ has density function $f$ its *n*th moment is given by \\[\mathbb{E}(X^n) = \int^\infty_{-\infty}x^nf(x)dx\\]Let $X$ be a random variable and $a$ and $b$ real numbers. Then \\[\mathbb{E}(aX + b) = a\mathbb{E}(X) + b\\]

### Variance

The **variance** of a random variable $X$ measures the expected distance from $X$ to its expected value.\
\
Let $X$ be a random variable with mean $\mu$. The variance of $X$ is defined by \\[\text{Var}(X) = \mathbb{E}[(X - \mu)^2]\\]An alternative symbol is $\sigma^2 = \text{Var}(X)$.\
\
Let $X$ be a random variable with mean $\mu$. Then \\[\text{Var}(X) = \sum_k (k- \mu)^2 \mathbb{P}(X = k)\\]if $X$ is discrete and \\[\text{Var}(X) = \int^\infty_{-\infty} (x - \mu)^2f(x)dx\\]if $X$ has density function $f$.\
\
An alternative formula for variance is given by \\[\text{Var}(X) = \mathbb{E}(X^2) - (\mathbb{E}(X))^2\\]Let $X$ be a random variable and $a$ and $b$ real numbers. Then \\[\text{Var}(aX+b) = a^2 \text{Var}(X)\\]

### Standard Deviation

The standard deviation of a random variable measures the amount of variation or dispersion of a set of values. A low standard deviation indicates that the values tend to be close to the expected value of the set, while a high standard deviation indicates that the values are spread out over a wider range. The standard deviation of a random variable, denoted $\text{sd}(X)$, is simply the positive square root of the variance: \\[\text{sd}(X) \equiv +\sqrt{\text{Var}(X)}\\]The standard devition is sometimes denoted $\sigma$ when the random variable is understood.   

### Covariance

**Covariance** measures the amount of linear dependence between two random variables $X$ and $Y$. $X$ and $Y$ are positively correlated if $\text{Cov}(X,Y) > 0$. This means that the occurence of $X$ increases the chances of $Y$. $X$ and $Y$ are negatively correlated if $\text{Cov} < 0$. This means that the occurence of $X$ decreases the chances of $Y$. $X$ and $Y$ are uncorrelated if $\text{Cov}(X,Y) = 0$.\
\
Let $X$ and $Y$ be random variables defined on the sample space with expectations $\mu x$ and $\mu y$. The covariance of $X$ and $Y$ is defined by \\[\text{Cov}(X,Y) = \mathbb{E}((X - \mu x)(Y - \mu y))\\]if the expectation on the right is finite.\
\
An alternative formula for the covariance is given by \\[\text{Cov}(X,Y) = \mathbb{E}(XY) - \mathbb{E}(X)\mathbb{E}(Y)\\]

Covariance has the following rules associated with it. 
1. The covariance of the same random variable $X$ is $\text{Cov}(X,X) = \text{Var}(X)$. 
2. The covariance of anything with a constant $a \in \mathbb{R}$ is zero: $\text{Cov}(a,X) = 0$. 
3. The covariance of a random variable multiplied by a constant $a \in \mathbb{R}$ is zero: $\text{Cov}(aX,Y) = a \text{Cov}(X,Y)$.

### Sample Mean, Variance, Standard Deviation

Given a sample of data $(x_i,y_i)$ for $i = 1,2,...,N$, we can get the sample mean, sample variance, and the sample covariance respectively. These are random variables with their own sampling distribution.
\\[\begin{aligned} \bar{x} &= \frac{1}{N} \sum_{i=1}^N x_i \\\\ s^2 &= \frac{1}{N-1} \sum^N_{i=1} (x_i - \bar{x})^2  \\\\ \text{Cov}(x,y) &= \frac{1}{N-1} \sum^N_{i=1} (x_i - \bar{x})(y_i - \bar{y})\end{aligned}\\]

### Law of Large Numbers

The **weak Law of Large Numbers** (w-LLN) states the following. Suppose $(X_j)$ with $j \in \mathbb{N}$ is a sequence of independent and identically distributed random variables with finite $\mathbb{E}(X_1) := \mu$ and $\text{Var}(X_1) =: \sigma^2$. Denote $S_n := \sum^n_{i=1} X_j$. Now $\frac{1}{n} S_n \xrightarrow{n \to \infty} \mu$ in probability if and only if for all $\varepsilon > 0$, \\[\mathbb{P} \bigg( \bigg\lvert \frac{S_n}{n} - \mu \bigg\rvert < \varepsilon \bigg) \xrightarrow{n \to \infty} 1\\]Less formally, this tells us that no matter how small an interval $(\mu - \varepsilon, \mu + \varepsilon)$ you put around $\mu$, as $n$ becomes larger, the observed mean $\frac{S_n}{n}$ will lie inside the interval with overwhelming probability.\
\
The **strong Law of Large Numbers** (s-LLN) states the following. Suppose that we have independently identically distributed random variables $X_1, X_2,...$ with finite mean $\mathbb{E}(X_1) = \mu$. Let $S_n = X_1 + ... + X_n$. Then \\[\mathbb{P} \bigg( \lim_{n \to \infty} \frac{X_1 + ... + X_n}{n} = \mu\bigg) = 1\\]

### Central Limit Theorem

The **Central Limit Theorem** (CLT) states the following. Suppose that we have independent identically distributed random variables $X_1,X_2,X_3,...$ with finite mean $\mathbb{E}(X_1) = \mu$ and finite variance $\text{Var}(X_1) = \sigma^2$. Let $S_n = X_1 + ... + X_n$. Then for any fixed $-\infty \leq  a \leq b \leq \infty$ we have \\[\lim_{n \to \infty} \mathbb{P}\bigg( a \leq \frac{S_n - n\mu}{\sigma \sqrt{n}} \leq b \bigg) = \Phi(b) - \Phi(a) = \int^b_a \frac{1}{\sqrt{2\pi}} e^{\frac{-y^2}{2}}dy\\]



