# Oct 1, 2013

## Reverser


```r
# Definition here

```


Test statements:

```r
# Test statement here. Should return TRUE or FALSE
Reverser <- function(x) {
    return(rev(strsplit(x, split = character(0))[[1]][1:nchar(x)]))
}
```

## Scrambler
 Scrambler<-function(x){return(sample(strsplit(x,split=character(0))[[1]]
