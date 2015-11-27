# R Profiler

The profiler in R is a really handy tool for when you’re developing larger programs or doing big data analyses. Sometimes it taking a long time and profile can be used to find out exactly why it’s taking so much time and to suggest strategies for fixing your problem.

There are also some other tools that can help you for example to time your programs.

###### Why is My Code So Slow?

Profiling is a systematic way to examine how much time is spend in different parts of a program.

Often code runs fine once, but what if you have to put it in a loop for 1,000 iterations? 

###### General Principles of Optimization:

- Design first, then optimize
- Remember: Premature optimization is the root of all evil
- Measure (collect data), don’t guess.

[========]

## Using system.time()

Takes an arbitrary R expression as input (can be wrapped in curly braces) and returns the amount of time takes to evaluate the expression

Computes the time in seconds

If there’s an error, gives time until the error occurred

Returns an object of class proc_time

#### There are two important notion of time when you’re executing expressions on the computer:

#### user time: 
times charges to the CPU(s) for this expression

Usually, the user time and elapsed time are relatively close, for straight computing tasks. 


#### elapsed time: 
wall clock time, the time that you experience

Elapsed time may be greater than user time if the CPU spends a lot of time waiting around. Elapsed time may be smaller than the user time if your machine has multiple cores/processors (and is capable of using them)

Many of R libraries have been optimized to use multiple cores, called multi-threaded BLAS libraries, in the mac sometimes called vecLib or Accelerate (ATLAS, ACML, MKL)  
	
```r
> # Elapsed time > user time
> system.time(readLines("http://www.jhsph.edu"))
   user  system elapsed 
  0.036   0.009   1.186 

> # Elapsed time < user time
> hilbert <- function(n) { i<- 1:n; 1/outer(i-1, i, "+") }
> x <- hilbert(1000)
> system.time(svd(x))
   user  system elapsed 
  3.256   0.031   3.275 

```
## The R Profiler

The Rprof() function starts the profiler in R
The summaryRprof() function summarizes the output from Rprof()
Do NOT use system.time() and Rprof() together

Rprof() keeps track of the function call stack at regularly sampled intervals and how much time is spend in each function

Default sampling interval is 0.02 seconds

Once you see the function call stack, you know that each line of the output of function call stack is separated out by 0.02 seconds. Given that you can calculate how many seconds are spent in each of the functions.

#### There are two methods for normalizing the data:

#### by.total
divides the time spend in each function by the total run time

Gives you how much time was spent, how many times that function appeared in the calls. For example a 100% of your time is spent in the top-level function, but the reality is that often the top level functions don’t really do anything important to us, all they do is they call helper functions that do the real work. So often it’s not very interesting to know how much time is spent in these top level functions. So you really want to know how much time is spent in the top level function, but subtracting out all the functions that it calls. But most of the time you will notice that when you subtract out all lower level functions that get called there is very little time it spends in the top level function. Because all the work is being done at the lower level function. And that is where you want to focus on. 

####by.self
does the same but first subtracts out time spent in functions above in the call stack

The most interesting format to use because it tells you how much time is being spent in a given function, but after subtracting out all of the time spent in lower level functions that it calls. So it gives you more accurate picture of which functions are truly taking up the most amount of time.

