
```r
frameImage <- function(img, framecount) {
    framed <- cbind(imag[, rev(1:framecount)], img, img[, rev(197:216)])
    canvas(x = c(1, 40 + 216), y = c(1, 198), asp = 1)
    rasterImage(framed, 1, 1, 40 + 216, 198)
}
puppyFace <- puppy[160:200, 1:60, ]
```

```
## Error: object 'puppy' not found
```

```r
puppyPaw <- puppy[160:198, 1:60, ]
```

```
## Error: object 'puppy' not found
```

```r
puppyTag <- puppy[120:140, 100:120, ]
```

```
## Error: object 'puppy' not found
```

