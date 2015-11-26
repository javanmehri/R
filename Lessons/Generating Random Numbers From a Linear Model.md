# Generating Random Numbers From a Linear Model

Suppose we want to simulate from the following linear model

y = (B0) + (B1) x + e

where B0 = 0.5, B1 = 2
and e is normal random distribution with mean =0 and sd = 2

```r
> set.seed(20)
> x <- rnorm(100)
> e <- rnorm(100, 0, 2)
> y <- 0.5 + 2*x + e
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-6.4080 -1.5400  0.6789  0.6893  2.9300  6.5050 
> plot(x, y)

```
![](01.png)

What if x is binary random variable? For example maybe it represents gender or maybe it’s some treatment versus control.

```r
> set.seed(10)
> x <- rbinom(100, 1, 0.5)

```
It generates 100 binomial random variables with n = 1 and p = 0.5 so probability of 1 is equal to 0.5

```r
> e <- rnorm(100, 0, 2)
> y <- 0.5 + 2*x + e
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-3.4940 -0.1409  1.5770  1.4320  2.8400  6.9410 
> plot(x, y)

```
![](02.png)