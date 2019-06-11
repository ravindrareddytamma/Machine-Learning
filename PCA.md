Principal Component Analysis
================
Ravindra Reddy Tamma
11 June 2019

Principal Component Analysis
----------------------------

The main goals of principal component analysis is :

1.  To identify hidden pattern in a data set

2.  To reduce the dimensionnality of the data by removing the noise and redundancy in the data

3.  To identify correlated variables

PCA is traditionally performed on covariance matrix or correlation matrix.

Lets perform PCA step by step on Iris Dataset

``` r
data("iris")
head(iris)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

Building a Covariance matrix
----------------------------

``` r
df <- iris[,-5]
cov.df <- round(cov(df),2)
cov.df
```

    ##              Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## Sepal.Length         0.69       -0.04         1.27        0.52
    ## Sepal.Width         -0.04        0.19        -0.33       -0.12
    ## Petal.Length         1.27       -0.33         3.12        1.30
    ## Petal.Width          0.52       -0.12         1.30        0.58

Interpretention of the covariance matrix
----------------------------------------

The diagonal elements are the variances of the different variables. A large diagonal values correspond to strong signal.

``` r
diag(cov.df)
```

    ## Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
    ##         0.69         0.19         3.12         0.58

The off-diagonal values are the covariances between variables.

They reflect distortions in the data (noise, redundancy). Large off-diagonal values correspond to high distortions in our data.

The aim of PCA is to minimize this distortions and to summarize the essential information in the data

How to minimize the distortion in the data ?
--------------------------------------------

In the covariance table above, the off-diagonal values are different from zero. This indicates the presence of redundancy in the data. In other words, there is a certain amount of correlation between variables.

This kind of matrix, with non-zero off-diagonal values, is called "non-diagonal" matrix.

We need to redefine our initial variables (x, y, z, ..) in order to diagonalize the covariance matrix.

This means that we want to change the covariance matrix so that the off-diagonal elements are close to zero (i.e, zero correlation between pairs of distinct variables).

The new variables (x', y', z', .) are a linear combination of the old ones :

X'=a1X+a2Y+a3Z,...

Y'=b1X+b2Y+b3Z,...

In PCA, the constants a1, a2, an, b1, b2, bn are calculated such that the covariance matrix is diagonal.

PCA terminologies : Eigenvalues / eigenvectors
----------------------------------------------

Eigenvalues : The numbers on the diagonal of the diagonalized covariance matrix are called eigenvalues of the covariance matrix. Large eigenvalues correspond to large variances.

Eigenvectors : The directions of the new rotated axes are called the eigenvectors of the covariance matrix

Steps for principal component analysis
--------------------------------------

The procedure includes 5 simple steps :

Prepare the data :

1.  Center the data : subtract the mean from each variables. This produces a data set whose mean is zero.

2.  Scale the data : If the variances of the variables in your data are significantly different, it's a good idea to scale the data to unit variance. This is achieved by dividing each variables by its standard deviation.

3.  Calculate the covariance/correlation matrix

4.  Calculate the eigenvectors and the eigenvalues of the covariance matrix

5.  Choose principal components : eigenvectors are ordered by eigenvalues from the highest to the lowest.

6.  The number of chosen eigenvectors will be the number of dimensions of the new data set.

7.  eigenvectors = (eig\_1, eig\_2,., eig\_n)

compute the new dataset :
-------------------------

1.  Transpose eigeinvectors : rows are eigenvectors

2.  Transpose the adjusted data (rows are variables and columns are individuals)

3.  New.data = eigenvectors.transposed X adjustedData.transposed

Compute principal component analysis (step by step)
---------------------------------------------------

``` r
df <- iris[, -5]
head(df)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1          5.1         3.5          1.4         0.2
    ## 2          4.9         3.0          1.4         0.2
    ## 3          4.7         3.2          1.3         0.2
    ## 4          4.6         3.1          1.5         0.2
    ## 5          5.0         3.6          1.4         0.2
    ## 6          5.4         3.9          1.7         0.4

Center & Scale the Data
-----------------------

``` r
df.scaled <- scale(df, center = TRUE, scale = TRUE)
head(df.scaled)
```

    ##      Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## [1,]   -0.8976739  1.01560199    -1.335752   -1.311052
    ## [2,]   -1.1392005 -0.13153881    -1.335752   -1.311052
    ## [3,]   -1.3807271  0.32731751    -1.392399   -1.311052
    ## [4,]   -1.5014904  0.09788935    -1.279104   -1.311052
    ## [5,]   -1.0184372  1.24503015    -1.335752   -1.311052
    ## [6,]   -0.5353840  1.93331463    -1.165809   -1.048667

Compute the correlation matrix :
--------------------------------

``` r
res.cor <- cor(df.scaled)
round(res.cor, 2)
```

    ##              Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## Sepal.Length         1.00       -0.12         0.87        0.82
    ## Sepal.Width         -0.12        1.00        -0.43       -0.37
    ## Petal.Length         0.87       -0.43         1.00        0.96
    ## Petal.Width          0.82       -0.37         0.96        1.00

Eigen Values & Eigen Vectors
----------------------------

``` r
res.eig <- eigen(res.cor)
res.eig$values
```

    ## [1] 2.91849782 0.91403047 0.14675688 0.02071484

``` r
res.eig$vectors
```

    ##            [,1]        [,2]       [,3]       [,4]
    ## [1,]  0.5210659 -0.37741762  0.7195664  0.2612863
    ## [2,] -0.2693474 -0.92329566 -0.2443818 -0.1235096
    ## [3,]  0.5804131 -0.02449161 -0.1421264 -0.8014492
    ## [4,]  0.5648565 -0.06694199 -0.6342727  0.5235971

Compute the new dataset :
=========================

``` r
# Transpose eigeinvectors
eigenvectors.t <- t(res.eig$vectors)
eigenvectors.t
```

    ##            [,1]       [,2]        [,3]        [,4]
    ## [1,]  0.5210659 -0.2693474  0.58041310  0.56485654
    ## [2,] -0.3774176 -0.9232957 -0.02449161 -0.06694199
    ## [3,]  0.7195664 -0.2443818 -0.14212637 -0.63427274
    ## [4,]  0.2612863 -0.1235096 -0.80144925  0.52359713

``` r
# Transpose the adjusted data
df.scaled.t <- t(df.scaled)
df.scaled.t[,1:6]
```

    ##                    [,1]       [,2]       [,3]        [,4]      [,5]
    ## Sepal.Length -0.8976739 -1.1392005 -1.3807271 -1.50149039 -1.018437
    ## Sepal.Width   1.0156020 -0.1315388  0.3273175  0.09788935  1.245030
    ## Petal.Length -1.3357516 -1.3357516 -1.3923993 -1.27910398 -1.335752
    ## Petal.Width  -1.3110521 -1.3110521 -1.3110521 -1.31105215 -1.311052
    ##                   [,6]
    ## Sepal.Length -0.535384
    ## Sepal.Width   1.933315
    ## Petal.Length -1.165809
    ## Petal.Width  -1.048667

``` r
df.new <- eigenvectors.t %*% df.scaled.t
df.new <- t(df.new)
colnames(df.new) <- c("PC1", "PC2", "PC3", "PC4")
head(df.new)
```

    ##            PC1        PC2         PC3          PC4
    ## [1,] -2.257141 -0.4784238  0.12727962  0.024087508
    ## [2,] -2.074013  0.6718827  0.23382552  0.102662845
    ## [3,] -2.356335  0.3407664 -0.04405390  0.028282305
    ## [4,] -2.291707  0.5953999 -0.09098530 -0.065735340
    ## [5,] -2.381863 -0.6446757 -0.01568565 -0.035802870
    ## [6,] -2.068701 -1.4842053 -0.02687825  0.006586116

Resources
---------

Correlation vs Covariance

<https://www.linkedin.com/pulse/covariance-vs-correlation-kumar-p/>

<https://www.wallstreetmojo.com/correlation-vs-covariance/>

Eigen Values & Eigen Vectors

<https://www.scss.tcd.ie/~dahyotr/CS1BA1/SolutionEigen.pdf>
