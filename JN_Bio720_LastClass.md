---
title: "JN_Bio720_LastClass"
author: "Jalees Nasir"
date: "December 10, 2018"
output: 
  html_document:
    keep_md: yes
---

For robust function:
stop
break
stopifnot




2 functions:
1) function to compute ratios (x, y)

Example:
x1/y1
x2/y2

2) function - coeeficient of variation
coef_var = sd(x)/mean(x)


```r
# Version 1
ratio <- function(x, y){
  x/y
}

ratio(3,2)
```

```
## [1] 1.5
```

```r
ratio(c(1,2,3), c(2,3,4))
```

```
## [1] 0.5000000 0.6666667 0.7500000
```

```r
variation <- function(x){
  sd(x)/mean(x)
}
```



```r
# Version 2
ratio <- function(x,y)
  if (y==0){
    return(print("y can't be 0"))
    break
  } else {return(x/y)}
```



```r
# Version 3
ratio(3,0)
```

```
## [1] "y can't be 0"
```



```r
# Version 2 
variation <- function(x) {
  if (class(x) != "numeric"){
    warning("No strings, only numbers.")
  } else if (length(x) < 2){
    warning("Input must be a vector of at least 2 elements")
  } else if (mean(x) == 0){
    warning("Cannot divide by 0")
  } else {return(sd(x)/mean(x))}
}

x<-c("hi", "bye")
variation(x)
```

```
## Warning in variation(x): No strings, only numbers.
```


```r
# Version 3 
variation <- function(x) {
  if (class(x) != "numeric"){
    stop("No strings, only numbers.")
  } else if (length(x) < 2){
    stop("Input must be a vector of at least 2 elements")
  } else if (mean(x) == 0){
    stop("Cannot divide by 0")
  } else {return(sd(x)/mean(x))}
}

x<-c("hi", "bye")
#y<-list(2, 3)
#variation(x)
```


