# Generating Random Numbers

Functions for probability distributions in R

- ###### rnorm
generate random Normal variates with a given mean and standard deviation

- ###### dnorm 
evaluate the Normal probability density (with a mean/SD) at a point (or vector of points)

- ###### pnorm 
evaluate the cumulative distribution function for a Normal distribution

- ######  rpois 
generate random Poisson variates with a given rate

Probability distribution functions have four functions associated with them. The functions are prefixed with a 

- ###### d	for density
- ###### r	for random number generation
- ###### p	for cumulative distribution
- ###### q	for quantile function

Every distribution has these four types of functions. For example for gamma distribution, there is the dgamma, rgamma, pgamma, and qgamma function. And for Poisson distribution there is the rpoise, dpoise, ppoise, and qpoise function.

So working with the normal distribution requires these four functions which they each take a number of different parameters. All the functions have required that you specify the mean and the standard deviation, because that’s what specifies the actual probability distribution. If you do not specify them, then default values are a distribution, a standard normal distribution, which has mean zero and standard deviation one.

###### dnorm (x, mean = 0, sd = 1, log = FALSE )
###### pnorm (q, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE )
###### qnorm (p, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE )
###### rnorm (n, mean = 0, sd = 1 )

## Normal Random Distribution

```r
> x <- rnorm(10)
> x
 [1] -0.6723882  2.0625886 -1.5874398 -0.6072721  0.9261632  0.1209405
 [7]  0.6287053 -1.8561197  0.6125349 -0.1858938

```
```r
> x <- rnorm(10, 20, 2)
> x
 [1] 20.62214 20.14612 18.26644 21.56712 20.21596 20.68025 21.06344
 [8] 20.14623 18.27095 20.67179

```
```r
> summary(x)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  18.27   20.15   20.42   20.17   20.68   21.57 

```
## Poisson Distribution 

```r
> rpois(10,1)   # This generates 10 poisson variables with a rate of 1
 [1] 0 0 1 1 2 1 1 4 1 2
> rpois(10,2)
 [1] 4 1 2 0 1 1 0 1 4 1
> rpois(10,20)
 [1] 19 19 24 23 22 24 23 20 11 22

```
For the Poisson distribution, the mean is equal to the rate.

```r
> ppois(2,2)  # Cumulate distribution
[1] 0.6766764	# Pr(x<=2)

```
It shows the probability that a Poisson random variable is less than or equal to two if the rate is two.

```r
> ppois(4,2)  
[1] 0.947347  # Pr(x<=4)
> ppois(6,2)
[1] 0.9954662 # Pr(x<=6)

```