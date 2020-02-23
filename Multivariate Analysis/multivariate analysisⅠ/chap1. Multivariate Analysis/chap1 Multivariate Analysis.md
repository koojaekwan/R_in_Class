---
title: "chap 1. Multivariate data analysis"
author: "Jae Kwan Koo"
output:
  github_document:
    toc: yes
    toc_depth: 4
  word_document: default
  html_document:
    fig_height: 6
    fig_width: 10
    highlight: textmate
    toc: yes
    toc_depth: 4
    toc_float: yes
---



[R과 함께하는 다변량 자료분석] 의 연습문제 1.7에 해당하는 분석이다.  




```r
library(data.table)  
library(corrplot)    # making the correlation plot.
library(tidyverse)

library(MVN)         # Multivariate normality test
```


```
## [1] "D:/Jae Kwan/github/R_in_class/Multivariate Analysis/data"
```



```r
data<-fread("klpga.txt", drop="V1") # delete the number column.
```

```
## Error in fread("klpga.txt", drop = "V1"): File 'klpga.txt' does not exist or is non-readable. getwd()=='D:/Jae Kwan/github/R_in_class/Multivariate Analysis/multivariate analysisⅠ/chap1. Multivariate Analysis'
```

```r
data[1:6]  
```

```
## Error in data[1:6]: object of type 'closure' is not subsettable
```


```r
str(data)
```

```
## function (..., list = character(), package = NULL, lib.loc = NULL, 
##     verbose = getOption("verbose"), envir = .GlobalEnv, overwrite = TRUE)
```

```r
dim(data)
```

```
## NULL
```

```r
colSums(is.na(data))
```

```
## Error in colSums(is.na(data)): 'x' must be an array of at least two dimensions
```


```r
summary(data)
```

```
## Error in object[[i]]: object of type 'closure' is not subsettable
```


```r
data %>% 
  select_if(is.numeric) %>%
  cor(use="complete.obs") %>%
  corrplot.mixed(tl.cex=0.85)
```

```
## Error in UseMethod("tbl_vars"): no applicable method for 'tbl_vars' applied to an object of class "function"
```




### Find mean vector, covariance matrix S, correlation matrix R  


```r
X<-as.matrix(data)
```

```
## Error in as.vector(x, mode): cannot coerce type 'closure' to vector of type 'any'
```

```r
n<-nrow(X)
```

```
## Error in nrow(X): object 'X' not found
```

```r
xbar<-t(X)%*%matrix(1,n,1)/n
```

```
## Error in t(X): object 'X' not found
```

```r
I<-diag(n)
J<-matrix(1,n,n)
```

```
## Error in matrix(1, n, n): non-numeric matrix extent
```

```r
H<-I-1/n*J
```

```
## Error in eval(expr, envir, enclos): object 'J' not found
```

```r
Y<-H%*%X
```

```
## Error in eval(expr, envir, enclos): object 'H' not found
```

```r
S<-t(Y)%*%Y/(n-1)
```

```
## Error in t(Y): object 'Y' not found
```

```r
D<-diag(1/sqrt(diag(S)))
```

```
## Error in diag(S): object 'S' not found
```

```r
Z<-H%*%X%*%D
```

```
## Error in eval(expr, envir, enclos): object 'H' not found
```

```r
R<-t(Z)%*%Z/(n-1)
```

```
## Error in t(Z): object 'Z' not found
```

```r
colnames(xbar)<-c("Mean")
```

```
## Error in colnames(xbar) <- c("Mean"): object 'xbar' not found
```

```r
xbar; S; R
```

```
## Error in eval(expr, envir, enclos): object 'xbar' not found
```

```
## Error in eval(expr, envir, enclos): object 'S' not found
```

```
## Error in eval(expr, envir, enclos): object 'R' not found
```

- Another way to solve the problem.  


```r
colMeans(data)
cov(data)
cor(data)
```

### Find two kinds of multivariate variations, and interpret them.  



```r
det(S)   #generalized variance
```

```
## Error in determinant(x, logarithm = TRUE, ...): object 'S' not found
```

```r
det(R) 
```

```
## Error in determinant(x, logarithm = TRUE, ...): object 'R' not found
```

```r
sum(diag(S))  #total variance
```

```
## Error in diag(S): object 'S' not found
```

```r
sum(diag(R))
```

```
## Error in diag(R): object 'R' not found
```

Two methods were obtained: covariance matrix and correlation matrix.  
generalized variances can be obtained as a matrix of covariance or as a matrix of correlation.  
since the determinant of R(correlation matrix) is near zero, the correlation between the variables is very high.  
it is obvious that the total variance of the correlation matrix is 6. because we can see the number of variables is 6.  

#### refer  

| type | description |  
|--| :- | 
| generalized variance | determinant of covariance matrix | 
| total variance | sum of diag element of covariance matrix |  

that is, generalized variance use all the information of the variance and covariance.  
while, |S| = 0 indicates that the columns in the centralization data matrix Y are linear dependents, indicating that there is co-linearity among the variables.  

In particular, since |S| and tr(S) are heavily influenced by the size of the variance, it is desirable to use a correlation R with variance 1 as diagonal elements.  


### Between centering & standardizing , which one is more appropriate for the data?  explain it.  

It seems more appropriate to standardize each variable because of its different units.  
in particular, the variance of the prize rate is larger than others.  


###Is it possible to group variables in the data into two parts of skill factor group and award factor group?  



```r
plot(data)
```

```
## Error in curve(expr = x, from = from, to = to, xlim = xlim, ylab = ylab, : 'expr' did not evaluate to an object of length 'n'
```

```r
round(cor(data),3)
```

```
## Error in cor(data): supply both 'x' and 'y' or a matrix-like 'x'
```

When checking the correlation coefficient with the prize rate within the variables of a group of technology factor variables, only the average number of putts is negative, so it cannot be divided into a group of technical factors and a group of performance factors based on the association.  

### Check the multivariate normality based on , and kurtosis & skewness.  


```r
result<-mvn(data, multivariatePlot =  "qq")
```

```
## Error in uniNorm(data, type = "SW"): Input must be one of classes "vector", "data frame" or "matrix"
```

```r
result
```

```
## Error in eval(expr, envir, enclos): object 'result' not found
```


Since p-value about skewness and kurtosis is lower than alpha=0.05, we reject the null hypothesis H0.  
That is , Multivariate data are not normally distributed.

| hypothesis |  |  
|--| :- | 
| H0 | "Multivariate data are normally distributed." | 
| H1 | "Multivariate data are not normally distributed." |  


```r
n<-dim(data)[[1]]
p<-dim(data)[[2]]


s<-cov(data)
```

```
## Error in cov(data): supply both 'x' and 'y' or a matrix-like 'x'
```

```r
xbar<-colMeans(data)
```

```
## Error in colMeans(data): 'x' must be an array of at least two dimensions
```

```r
m<-mahalanobis(data,xbar,s)
```

```
## Error in as.vector(x, mode): cannot coerce type 'closure' to vector of type 'any'
```

```r
m<-sort(m)
```

```
## Error in sort(m): object 'm' not found
```

```r
id<-seq(1,n)
```

```
## Error in seq.default(1, n): 'to' must be of length 1
```

```r
pt<-(id-0.5)/n
```

```
## Error in id - 0.5: non-numeric argument to binary operator
```

```r
q<-qchisq(pt,p)
```

```
## Error in qchisq(pt, p): 수치형이 아닌 인자가 수학함수에 이용되었습니다
```

```r
plot(q,m,pch='*')
```

```
## Error in plot.function(q, m, pch = "*"): object 'm' not found
```

```r
abline(0,1)
```

```
## Error in int_abline(a = a, b = b, h = h, v = v, untf = untf, ...): plot.new has not been called yet
```

The horizontal axis in this plot indicates quantile of chi square distribution and verticle axis represents  Mahalanobis distance.  




```r
rq<-cor(cbind(q,m))[1,2]
```

```
## Error in cbind(q, m): object 'm' not found
```

```r
rq
```

```
## Error in eval(expr, envir, enclos): object 'rq' not found
```

Almost of coordinate points are not out of linearity.  
Since correlation coefficient of quantile and mahalanobis distance is almost 1, the linearity of the chi-square plot is highly appreciated.  


