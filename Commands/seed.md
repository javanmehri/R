# seed

Any time you simulate random numbers from any distribution, for any purpose, it’s very important that you set the random number generator seed. It allows you to reproduce random numbers that you generate.

```r
> set.seed(1)
> rnorm(5)
[1] -0.6264538  0.1836433 -0.8356286  1.5952808  0.3295078
> rnorm(5)
[1] -0.8204684  0.4874291  0.7383247  0.5757814 -0.3053884
> set.seed(1)
> rnorm(5)
[1] -0.6264538  0.1836433 -0.8356286  1.5952808  0.3295078

```
These are the exact result of the first call of rnorm(5)!

So why we want to reproduce random numbers?!

In many applications you want to generate the same random numbers so that people can reproduce what you’ve done, and in particular if there are some errors or problems i what you’ve done, you want to be able to go back to the exact situation that produced those problems.
