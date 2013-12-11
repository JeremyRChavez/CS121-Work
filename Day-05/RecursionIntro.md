# Recursion

You've encountered three syntax structures for repeatedly iterating a set of statements: `for(){}`, `while(){}`, and `repeat{}`.  Their similarities are strong; the differences are superficial.  Using any of these forms involves:

1. A state initialization
2. A loop with a state update.
3. A termination condition
4. (*After the termination*) some use for the state, such as `return()`.

There's a fourth structure, not exactly a sibling of for/while/repeat but a distant cousin.  This structure, **recursion**, does not involve any new syntax but a clever use of syntax that you've already been using.

A recursive function:

* Takes a state as input.  
* Then, depending on the situation, 
    * In simple situations, returns a complete answer to the problem at hand, 
    *  Or, in complicated situations, ...
        * Simplifies the problem somewhat 
        * Passes the simplified problem to one or more new invocations of itself, and, when receiving the answers it requests,
        * Assembles the answers to the simpler problem into an answer to the more complex problem.
    
For example, consider the problem of computing the $n$^th [Fibonacci number](http://en.wikipedia.org/wiki/Fibonacci_number): 
$$F_n = F_{n-1} - F_{n-2}$$
with $F_0 = 0$ and $F_1 = 1$.  As a loop, this computation can be written:

```r
fibLoop <- function(n) {
    # initialize state
    Ffirst <- 0
    Fsecond <- 1
    # for() both sets some of the state and controls termination
    for (k in 2:n) {
        # update the state
        newF <- Ffirst + Fsecond
        Ffirst <- Fsecond
        Fsecond <- newF
    }
    # Finished! Return the relevant aspect of the state
    return(newF)
}
```

Demo statements:

```r
fibLoop(5)
```

```
## [1] 5
```


#### Write another form of the loop ...

Where the state is maintained by a vector of $n+1$ slots, with the first two slots initialized to 0 and 1 respectively.

fibLoop2 <- function(n) {
  # initialize state
  vector <- c(1:n+1)
  vector[1] <- 0  
  vector[2] <- 1
  for (k in 2:n+1) { 
    vector[k] <- vector[k-1] + vector[k-2]
    }
  return( vector[n+1] )
  }

### A Recursive structure for $F_n$

The form of the formula for the Fibonacci numbers, $F_n = F_{n-1} + F_{n-2}$, is called a *recursion relationship*.  It takes the (difficult) problem of computing $F_n$ by breaking it down into two simpler problems, computing $F_{n-1}$ and $F_{n-2}$, then combines the answers to the simpler problems into an answer for the problem at hand, in this case by adding together the answers to the simpler problems.

You may well wonder why $F_{n-1}$ is simpler than $F_n$.  Understanding this requires some experience and judgment: it's not at all clear simply from the notation.  The reason, in this particular case, is that $F_{n-1}$ is one step closer to a computation whose answer we already know: $F_1$ or $F_0$.

Here's a recursive programming style implementation of the Fibonacci problem:

```r
fibRecurse <- function(n) {
    # handle the cases that are really simple
    if (n == 1) {
        return(1)
    } else {
        if (n == 0) 
            return(0)
    }
    # Break the problem down into simpler bits
    thisF <- fibRecurse(n - 1) + fibRecurse(n - 2)
    # Return the answer assembled from the simple bits
    return(thisF)
}
```


Test statements:

```r
fibRecurse(5)
```

```
## [1] 5
```


I've intentionally written `fibRecurse()` in a verbose way  There are more concise ways to write it, for instance:

```r
fib1 <- function(n) {
    if (n < 2) 
        return(n)
    return(fib1(n - 1) + fib1(n - 2))
}
```



```r
fib1(5)
```

```
## [1] 5
```


Recursion is hard and error prone. For example, the `for()` version will do  sensible even when $n$ is not an integer or is negative.  But `fibRecurse()` will go into an infinite recursion in this situation.  `fib1()` will work.

For the Fibonacci numbers, the recursive structure, even though it is a close match to the mathematical notation for specifying the recursion relationship, is a poor choice.  It is very inefficient.  To see this, compare how long it takes `fibLoop()` and `fibRecurse()` to run for a moderate value of $n$:

```r
beforeTime <- Sys.time()
fibLoop(30)
```

```
## [1] 832040
```

```r
as.numeric(Sys.time() - beforeTime)
```

```
## [1] 0.004154
```



```r
beforeTime <- Sys.time()
fibRecurse(30)
```

```
## [1] 832040
```

```r
as.numeric(Sys.time() - beforeTime)
```

```
## [1] 12.9
```


The recursive form is thousands of times slower.  And it gets much worse, very fast, for larger $n$.

There are techniques, such as **memoization** for making the recursive function faster.  But using such techniques, and recursion itself, is hard.

#### For the mathematically inclined

* Figure out why `fibRecurse()` is so slow.  Hint: time `fibRecurse(29)` and `fibRecurse(28)`.
* Make an estimate of how long `fibRecurse(40)` would take.  (But don't try it unless you are very, very patient.)

### Exercises

Write a recursive function to add up the numbers from $n$ to zero.

```r
addNSeq <- function(n) {
    # test if the answer is simple.  If so, return the simple answer.  YOUR TEST
    # GOES HERE
    
    # Simplify the problem and assemble the answer from the parts.  Your answer
    # here return( something involving addNSeq(simpler) )
}
```


Write a recursive function to add up all the numbers in a vector.  Hint: to simplify, index the vector with `[-1]`


```r
addRecursively <- function(v) {
    # Test for the simple case.  If so, return the simple answer.
    
    # Simplify the problem ans assemble the answer from the parts.
}
```


Demo your functions here:
TEST CASES



Also, write looping versions of the two computations.

Also, write functions involving simple vectorized operations (without loops) that perform the computation.

## Natural Settings for Recursion

The previous functions are "textbook" examples.  They are not of much practical use other than to present recursion in a very simple setting even though recursion isn't the simplest form of the computation.  But there are settings where recursion is a practical and important technique.

As an example, consider the integration operation in calculus.  If you studied calculus, you learned many algebraic rules for integrals in symbolic form, for instance, $\int \frac{1}{x} dx$.  You also likely encountered a textbook pictorial technique, intended to illustrate why the integral is a kind of limit.  For instance, here is an animated picture of a [Riemann Sum](http://en.wikipedia.org/wiki/Riemann_sum) showing how, in the limit of infinitesimally narrow rectangles, the sum approaches the area under the smooth curve:

![Riemann Sum](http://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Riemann_sum_%28middlebox%29.gif/120px-Riemann_sum_%28middlebox%29.gif)

The Riemann Sum is a very feasible numerical calculation.  Here's a function that does the Riemann Sum with $n$ rectangles over an interval from $a$ to $b$:

```r
simpleRiemann <- function(f, a = 0, b = 1, n = 3) {
    rectangleWidth = (b - a)/n
    midpoints <- seq(a + rectangleWidth/2, b - rectangleWidth/2, length = n)
    rectangleAreas <- sapply(midpoints, f) * rectangleWidth
    return(sum(rectangleAreas))
}
```


Simple test statements: corresponding to 
* $\int_0^{10} 3 dx$  Constant, gives $\left. 3x \right|_0^{10} = 30$.
* $\int_0^{10} 3x dx$ Ramp, gives $\left. \frac{1}{2}3x^2 \right|_0^{10} =  150$.
* $\int_0^{10} 3 x^2 dx$ Quadratic, gives $\left. \frac{3}{3}x^3 \right|_0^{10} =  1000$.

```r
# Constant function
simpleRiemann(function(x) {
    3 + 0 * x
}, a = 0, b = 10)
```

```
## [1] 30
```

```r
# Ramp function
simpleRiemann(function(x) {
    3 * x
}, a = 0, b = 10)
```

```
## [1] 150
```

```r
# Quadratic
simpleRiemann(function(x) {
    3 * x^2
}, a = 0, b = 10)
```

```
## [1] 972.2
```


Notice that the answers are exactly right for the constant and ramp functions, but not quite there for the quadratic function.  The issue is, for the quadratic, that the rectangles are not narrow enough.

The symbolic mathematical approach is to take a limit as the number of rectangles goes to $\infty$.


```r
simpleRiemann(function(x) {
    3 * x^2
}, a = 0, b = 10, n = 10)
```

```
## [1] 997.5
```

```r
simpleRiemann(function(x) {
    3 * x^2
}, a = 0, b = 10, n = 20)
```

```
## [1] 999.4
```

```r
simpleRiemann(function(x) {
    3 * x^2
}, a = 0, b = 10, n = 40)
```

```
## [1] 999.8
```

```r
# ... and so on.
```


This is a fine approach ... conceptually.  But how to know when you have enough bins?  Taking a limit $n \rightarrow \infty$ is fine as a notation, but when is enough enough?

One practical approach is to try the Riemann sum with a handful of bins, then try again with more bins.  If the answers are the same, you've got enough bins.  This could be done easily with a loop:


```r
integrateRiemann <- function(f, a = 0, b = 1) {
    nbins <- 5
    biggerBins <- simpleRiemann(f, a = a, b = b, n = nbins)
    for (k in 1:5) {
        nbins <- nbins * 10  # much smaller bins
        smallerBins <- simpleRiemann(f, a = a, b = b, n = nbins)
        if (abs(smallerBins - biggerBins) < 1e-05) 
            break
        biggerBins <- smallerBins
    }
    return(smallerBins)
}
```


Demo cases:

```r
integrateRiemann(function(x) {
    3 * x
}, a = 0, b = 10)
```

```
## [1] 150
```

```r
integrateRiemann(function(x) {
    3 * x^2
}, a = 0, b = 10)
```

```
## [1] 1000
```


Read through this program and make sure you understand the purpose of the structure.
* Why is there a `for()` loop only going to 5?
* What's the purpose of `biggerBins <- smallerBins`?
* In the worst case, how many bins will there be?

(Write your answers "in line" here.)

Another approach is to divide up the interval $[a,b]$ into parts, say in half, but use the same number of bins on the smaller interval.  This may seem like the same thing, but by structuring the program recursively, you can delve into those parts of the function's range that need smaller bins, while leaving the "simple" parts of the range with bigger bins.

Here's an approach:

```r
integrateRecursive <- function(f, a = 0, b = 1) {
    bigBins <- simpleRiemann(f, a = a, b = b, n = 5)
    smallBins <- simpleRiemann(f, a = a, b = b, n = 10)
    if (abs(bigBins - smallBins) < 1e-05) 
        return(smallBins) else {
        mid <- (a + b)/2
        total <- integrateRecursive(f, a = a, b = mid) + integrateRecursive(f, 
            a = mid, b = b)
        return(total)
    }
}
```


## Your Tasks

####  Modify `integrateRecursive()` 

* Arrange things so that there's a worst case limit on the recursion.  Once the bins get to 1/10000 of the overall range, stop.  Hint: You'll need to pass an additional argument to `integrateRecursive()`.
* Pass the tolerance (it's `0.00001` in the program above) as an argument.

#### Demo your Program

Pick some interesting looking integrals for which you know or can find the answer.  
* Test out your program on them. 
* Write out the integrals in traditional math notation (using $\LaTeX$) and give the actual and computed answer for each one.
* Find an integral for which you hit the "worst case" limit for stopping the recursion.

#### A Better Simple Integrator

Bernhard Riemann, after whom the Riemann sum is named, lived from 1826 to 1866. When Riemann was in his 20s, he studied in Göttingen under Carl Friedrich Gauss, the "Prince of Mathematicians," who lived from 1777 to 1855.  

Gauss | Riemann
------|--------
<img src='http://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Carl_Friedrich_Gauss.jpg/220px-Carl_Friedrich_Gauss.jpg' size='20%'> | <img src='http://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Georg_Friedrich_Bernhard_Riemann.jpeg/225px-Georg_Friedrich_Bernhard_Riemann.jpeg' size='20%'>

As it happens, Gauss developed a numerical method of integration that is in many ways much better than Riemann's sums.  The Gauss method, called [Gaussian Quadrature](http://en.wikipedia.org/wiki/Gaussian_quadrature) is even today the predominant numerical technique for doing integrals (over one- and two-dimensional spaces), giving great accuracy at low cost.

Here's a simple implementation.  You can't be expected to understand the motivation for this method, or why it works so well.  (It has to do with orthogonal polynomials and making the error in the integral approximately orthogonal to the approximation, so that the approximation is excellent.)


```r
gaussQuadrature <- function(f, a = 0, b = 1) {
    # Just 4 function evaluations!
    
    # 'Magic' values on z in [0,1]
    z <- c(c(-1, 1) * sqrt((3 - 2 * sqrt(6/5))/7), c(-1, 1) * sqrt((3 + 2 * 
        sqrt(6/5))/7))
    # weights (analogous to bin width)
    w <- c(rep((18 + sqrt(30))/36, 2), rep((18 - sqrt(30))/36, 2))
    # Translate to interval x in [a,b]
    x <- ((b - a)/2) * z + (a + b)/2
    # evaluate the function at x, multiply by weights, and sum
    return(((b - a)/2) * sum(w * sapply(x, f)))
}
```


Test cases:

```r
gaussQuadrature(function(x) 3 + 0 * x, 0, 10)
```

```
## [1] 30
```

```r
gaussQuadrature(function(x) 3 * x, 0, 10)
```

```
## [1] 150
```

```r
gaussQuadrature(function(x) 3 * x^2, 0, 10)
```

```
## [1] 1000
```


Even though there are just 4 "bins", the integral is exact (to the 16-digit precision of floating point arithmetic) for these functions. Amazingly, things are even easier than they appear in the function.  The values of `z` and `w` could have be written this way:

```r
z <- c(-0.339981043584856, 0.339981043584856, -0.861136311594053, 0.861136311594053)
w <- c(0.652145154862546, 0.652145154862546, 0.347854845137454, 0.347854845137454)
```

Magical numbers, indeed!

* Show that `gaussQuadrature()` works exactly for integrals of some higher powers of $x$.  For what power does it stop working.
* Test out  `gaussQuadrature()` on sine and cosine functions, with $a=0$ and $b$ successively larger, that is, $b=0.1$, $b=1.0$, and so on.  How wide can you make the interval, before the result of `gaussQuadrature()` is not accurate?
* Re-implement `integrateRecursive()` using `gaussQuadrature()` as the base integrator.  You'll have to be creative, since you can't change the number of bins as in the test-for-simple part of `integrateRecursive()`.  Show that your function works on trigonometric functions with $b$ big.

(If you like this application, you might think about doing your term project in the form of an app to demonstrate how the various forms of numerical integration differ.)



#### Plotting a function

Write a function, `plotF(f, a=0, b=1)` that plots $f(x)$ for $x \in [a,b]$.

The algorithm is very simple: 

1. Create a vector of points $x$ spanning $[a,b]$.
2. Evaluate $f()$ at each of these points to create a vector $y$.
3. Plot $y$ versus $x$, connecting the points with lines and putting a very small dot at each point.  (Hint: `pch=20,cex=.2` will set the dot size and the dot character.)
4. (This is the hard part.) Write a function, `getX(f, a=0, b=1)`, that generates a nice set of points for step (1).  Your function can use the same basic logic as in `integrateRecursive()`, but rather than returning the integral, it should return the set of points at which $f()$ is evaluated.  That is, you're going to put many $x$ points near the "wavey" parts of $f()$ and few points near the straight-line part. The criterion for deciding when you have enough points is that those points give a reasonably precise Riemann Sum. 

Your plots should look something like this, although your method for generating the `x` points will be completely different.
<img src="figure/unnamed-chunk-21.svg" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" width="40%" />
