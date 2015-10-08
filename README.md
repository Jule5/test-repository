# test-repository

---
title: "Central Limit Theorem and Large Law of Numbers."
author: "Julio Cesar Ruiz"
date: "September 17, 2015"
output: pdf_document
---

The Central Limit Theorem is an important theorem in the field of Statistics, it helps to obtain a good approximation of a probability of a distribution when the random sample is big enough. The Central Limit Theorem sates that if we subtract the mean of a variable of the same variable and then divided it by its standard deviation this new variable has a Normal Standard Distribution.

\begin{center}
$\frac{X-E(X)}{\sqrt{Var(X)}} \sim N(0,1)$
\end{center}

Other important result is the Law of Large Numbers, this law states that the average of a large random sample will converge to the population mean.

\begin{center}
$\bar{X} \rightarrow \mu$
\end{center}

In this project I will illustrate the Central Limit Theorem, the Law of Large Numbers, and verify that the Sample Variance $S^2$ is a consistent estimator for the variance. In order to do this, I will use an exponential random variable setting $\lambda$ = 0.2 and perform a 1000 simulations.

First I will illustrate the Law of Large Numbers using the following settings: n sample size, numsim the number of simulations, and means1 the averages. The following is the R code. The function $set.seed(1)$ makes the results reproducible.
```{r, echo = FALSE}
library(ggplot2)

```

```{r, echo = TRUE}
set.seed(1)
n <- 40
numsimu <- 1000
means1 <- cumsum(rexp(n, rate = 0.2))/(1:n)
f <- ggplot(data.frame(x = 1 : n, y = means1), aes(x = x, y = y)) 
f <- f + geom_hline(yintercept = 5) + geom_line(size = 2) 
f <- f + labs(x = "Number of Observations", y = "Cumulative Mean")
```

```{r, echo = FALSE}
f
```

The graph shows that the averages approximately converge to 5, which is the mean of a exponential random variable with rate 0.2. This also states that the Law of Large Numbers is fulfilled.

Now let's illustrate that $S^2$ is a good estimator for the population variance $\sigma ^2$ . For that purpose $x$ stores the simulation of a random sample of size 40 and $sdx$ stores the standard deviation of $x$.
```{r echo = TRUE}
for(i in 1:numsimu){
set.seed(1)  
x <- rexp(n, rate = 0.2)
}
sdx <- sd(x)
```
The standard deviation of a exponential random variable is $\frac{1}{\lambda}$, in this case the standard deviation is 5. The following value is the standard deviation of the simulation:
```{r, echo = FALSE}
sdx
```
The standard deviation of the simulation is approximately 5, which confirms that $S^2$ is a consistent estimator for $\sigma ^2$.

Finally I will illustrate the Central Limit Theorem, for this purpose I will compare the histograms of sample of sizes 20, 30, and 40.
```{r, echo =TRUE}
stand <- function(x, n) sqrt(n) * (mean(x) - 5) / 5
dat <- data.frame(
 x = c(apply(matrix(rexp(numsimu * 20, rate = 0.2), 
                      numsimu), 1, stand, 20),
         apply(matrix(rexp(numsimu * 30, rate = 0.2), 
                      numsimu), 1, stand, 30),
         apply(matrix(rexp(numsimu * 40, rate = 0.2), 
                      numsimu), 1, stand, 40)
         ),
   size = factor(rep(c(20, 30, 40), rep(numsimu, 3))))
```
The variable $stand$ stores the application of the Central Limit Theorem.
```{r, echo = FALSE}
f1 <- ggplot(dat, aes(x = x, fill = size)) + geom_histogram(alpha = .20, binwidth=.3, colour = "black", aes(y = ..density..)) 
f1 <- f1 + stat_function(fun = dnorm, size = 2, colour = "red")
f1 <- f1 + facet_grid(. ~ size)
```

```{r, echo = FALSE}
f1
```

From the graph above we can see that as the size of the random sample increases the histogram begins to center at zero and has an approximate standard normal distribution. So this result illustrates the Central Limit Theorem.

