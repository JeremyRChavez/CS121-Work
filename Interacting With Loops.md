
```r
testLatency <- function(x) {
    if (missing(x)) {
        tests <- rexp(20, rate = 1/2)
        result <- vector(length = length(tests))
    } else {
        tests <- rexp(x, rate = 1/2)
        result <- vector(length = length(tests))
    }
    for (n in 1:length(tests)) {
        before <- Sys.time()
        readline("Press return: ")
        after <- Sys.time()
        delay <- as.numeric(after - before)
        cat(rep("\n", 20))
        result[n] <- delay
        Sys.sleep(tests[n])
    }
    return(result)
}

report <- function() {
    load("JeremyData.RData")
}

blastoff <- function(time) {
    while (time != 0) {
        Sys.sleep(1)
        cat(time, "\n")
        time <- time - 1
    }
    cat("Blastoff!")
}
blastoff(5)
```

```
## 5 
## 4 
## 3 
## 2 
## 1 
## Blastoff!
```
