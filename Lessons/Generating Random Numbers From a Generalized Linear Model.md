# Generating Random Numbers From a Generalized Linear Model

Suppose you might want to simulate some outcome data that are count variables, instead of continuous variables. So we have to use a slightly more complicated approach, because the error distribution is not going to be normal, It’s going to be a Poisson distribution.

Lets assume that the outcome y has a Poisson distribution with mean mu and that the log of mu follows a linear model with a intercept beta knot and a slope beta one:

###### 	log mu = Beta0 + (Beta 1) x

And lets assume that Beta0 = 0.5 and Beta1 = 0.3

To simulate this we need rpois function:

```r
> set.seed(1)
> x <- rnorm(100)
> log.mu <- 0.5 + 0.3*x
> y <- rpois(100, exp(log.mu))
> summary(y)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   0.00    1.00    1.00    1.55    2.00    6.00 
> plot(x, y)

```
![](03.png)
