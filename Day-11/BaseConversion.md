
```r

toBase <- function(Z, b) {
    output <- as.character(c())
    while (Z > 0) {
        r <- Z%%b
        output <- as.character(c(r, output))
        Z <- (Z - r)/b
    }
    return(output)
}

# Want a baseToNumeric()? How bout 4!

# This one takes a number and a base as an argument and returns the printed
# number as a numeric. It has a dependency on the toBase() function.
baseToNumeric <- function(Z, b) {
    return(as.numeric(paste(toBase(Z, b), collapse = "")))
}

# This one takes a string as an argument
baseToNumeric <- function(string) {
    return(as.numeric(paste(string, collapse = "")))
}

baseToNumeric <- function(Z, b) {
    output <- as.character(c())
    while (Z > 0) {
        r <- Z%%b
        output <- as.character(c(r, output))
        Z <- (Z - r)/b
    }
    return(as.numeric(paste(output, collapse = "")))
}

baseToNumeric <- function(Z, b, output = c(), hasRecursed = FALSE) {
    if (Z == 0 && !hasRecursed) 
        return(0) else if (Z == 0 && hasRecursed) 
        return(as.numeric(paste(output, collapse = ""))) else {
        hasRecursed <- TRUE
        r <- Z%%b
        output <- as.character(c(r, output))
        baseToNumeric((Z - r)/b, b, output, hasRecursed)
    }
}
```
