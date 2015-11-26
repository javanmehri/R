
# Random Sampling

The sample function draws randomly from a specified set of (scalar) objects allowing you to sample from arbitrary distributions.

```r
> sample(1:10, 4)
[1] 3 4 5 7
> sample(1:10, 4)
[1] 3 9 8 5

> sample(letters, 5)
[1] "q" "b" "e" "x" “p"

```
If I don’t set how many sample I want, It will give me a permutation, same vector but in random order.

```r
> sample(1:10)
 [1]  4  7 10  6  9  2  8  3  1  5
> sample(1:10)
 [1]  2  3  4  1  9  5 10  8  6  7

```
In all these cases we won’t get repeated numbers because we are not sampling with replacement.

But if I set replace = TRUE, we might get one:

```r
> sample(1:10, replace = TRUE)
 [1] 2 9 7 8 2 8 5 9 7 8

```

Credit: Roger D. Peng, Associate Professor of Biostatistics at the Johns Hopkins Bloomberg School of Public Health 