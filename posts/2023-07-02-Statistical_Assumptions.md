---
layout: post
title: "Commonly Abused Assumptions"
subtitle: ""
background: 'img/posts/Irving_Geis.jpg'
---

## Application 5

The data in Table 5.2 are taken from Hand et. al. and gives scores on
the Vocabulary subtest of the WAIS-R intelligence test for 54 university
students. Solely for the purpose of this exercise, we’ll assume the
students were randomly sampled from a large university population, and
within this population, the vocabulary scores have an approximately normal cumulative distribution function (cdf).
However, in reality there is no reason to believe that either of these
assumptions are true. Let's proceed and observe the consequences of these erroneous assumptions.  

``` r
data5.2 = read.delim("Table5_2.txt", header = T)
tab1 = as.matrix(data5.2)
tab5.2 = matrix(tab1, nrow = 3)
colnames(tab5.2) = rep(" ",18)
rownames(tab5.2) = rep(" ", 3)
```


    ## [1] "Table 5.2: Vocabulary Scores"

    ##                                                        
    ##   14 13 11 13 13 14 16 11 13 11 14 17 11 12 11 13 11 16
    ##   11 13 16 14 12 10 14 11 12 11 16  9 19 12 12 14 15 15
    ##   13 15 10 11 10 14 14 11 13 15 12 16 14 10 13 11 12 11



## 1. Find an equal tails exculsion 95% confidence interval for the mean of the university population distribution of WAIS-R vocabulary scores using the sample data in Table 5.2. Find a 95% lower bound for the mean from the same data. Interpret this bound

Since we are assuming that we have a random sample from a large,
normally distributed population of universities, we will use a
confidence interval for a Normal Population mean. This is of the form:  
$$
          (1-2\alpha)100\%\ \text{CI for }\mu:\ \bar{x} \pm \sqrt{\hat{\sigma}^2/n}\ \cdot\ F_t^{-1}(\alpha)
                        $$ We know that

``` r
n1 = length(data5.2$Scores)
print(paste("n =", n1))
```

    ## [1] "n = 54"

``` r
alpha1 = 0.025
print(paste("alpha=", alpha1))
```

    ## [1] "alpha= 0.025"



We now find $\mu, \&\ \hat{\sigma}^2$ from our sample data. 

``` r
xbar1 = mean(data5.2$Scores)
varx1 = var(data5.2$Scores)
print(paste("X bar = ", xbar1))
```

    ## [1] "X bar =  12.8703703703704"

``` r
print(paste("Sigma^2 = ", varx1))
```

    ## [1] "Sigma^2 =  4.37910552061495"



And the quantile score for $F^{-1}_t(0.025)$ from a t-distribution, with
$54-1 = 53$ degrees of freedom. 

``` r
Ft1 = qt(alpha1,53,lower.tail = F)
Ft1
```

    ## [1] 2.005746



Putting this all together, we have the 95% CI for $\mu$ to be 

``` r
UB1 = xbar1 + sqrt(varx1/n1)*Ft1
LB1 = xbar1 - sqrt(varx1/n1)*Ft1

print(paste("[",round(LB1,5),",",round(UB1,5),"]"))
```

    ## [1] "[ 12.29919 , 13.44155 ]"



Thus, we conclude that 95% of the confidence intervals in this exact
same manner on any sample of the population will contain the true value
of the population mean. 

We will now determine a 95% lower bound for the mean of the same data.


``` r
LB11 = xbar1 - sqrt(varx1/n1)*qt(2*alpha1, 53, lower.tail = F)
print(paste("[",round(LB11,5),",","infty",")"))
```

    ## [1] "[ 12.39363 , infty )"



We can interpret this as “we are 95% confident that the true population
mean is greater than 12.3936”. 

## 2. Find a 95% equal tails exclusion CI for the variance of the population distribution of vocabulary scores sampled in Table 5.2

Again, because we’re assuming that we have a random sample from a
population that is normally distributed, we construct a confidence
interval about the variance as: With the same alpha and degrees of
freedom, we have:

``` r
LB2 = varx1*(53/qchisq(0.975,53))
UB2 = varx1*(53/qchisq(0.025,53))
print(paste("[",round(LB2,5),",",round(UB2,5),"]"))
```

    ## [1] "[ 3.09449 , 6.67387 ]"

    ##                                                       
    ## Refereed Publications  0  1  2  3  4  5  6  7  8  9 10
    ## Faculty Members       28  4  3  4  4  2  1  0  2  1  1



**a. Plot the data using a histogram. Describe the shape of the data.**


![](test1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

The data display a positive skewness. The majority of the observations
are a value of 0. In other words, most of the staff in this sample did
not have any refereed publication. 

**b. Estimate the mean number of publications per faculty member, and
give the SE for your estimate.** 

Here, our population is the $N= 807$ faculty members at this university.
Hence, this sample is a simple random sample *without* replacement
(SRS). We first estimate the population mean, $\bar{y}*{U}$, with the
sample mean $$
                                  \bar{y} = \sum*{i\in \mathcal{S}} y_i
                                        $$ So we have

``` r
dat_66 = c(rep(0,28),rep(1,4),rep(2,3),rep(3,4),rep(4,4),rep(5,2),rep(6,1),rep(7,0),rep(8,2),rep(9,1),rep(10,1))
mean(dat_66)
```

    ## [1] 1.78



So we estimate
$\bar{y} = 1.78 \frac{\text{publications}}{\text{faculty member}}$. 

We have the estimated variance of $\bar{y}$ is $$
                    \hat{V}[\bar{y}] = \Big(1 - \frac{n}{N}\Big)\frac{s^2}{n}
                                    $$ where $s^2$ is the usual sample
variance $s^2 = \frac{1}{n-1}\sum_{i\in \mathcal{S}}(y_i - \bar{y})^2$.
However, we usually report the *standard error*, which is the square
root of the estimated variance. $$
                                    SE[\bar{y}] = \sqrt{\Big(1 - \frac{n}{N}\Big)\frac{s^2}{n}}
                                          $$

``` r
sqrt((1-(50/807))*(var(dat_66)/50))
```

    ## [1] 0.3674151

$SE[\bar{y}] = \sqrt{\big(1 - \frac{50}{807}\big)\frac{7.1955}{50}} \approx 0.37$.

**c. Do you think that $\bar{y}$ from (b) will be approximately normally
distributed? Why or why not?** 

Given that the original sample is so skewed, it is unlikely that the
sample mean will be normally distributed. 

**10. Which of the following SRS designs will give the most precision
for estimating a population mean? Assume that each population has the
same value of the population variance** $S^2$.

1. An SRS of size 400 from a population of size 4000.

2. An SRS of size 30 from a population of size 300.

3. An SRS of size 3000 from a population of size 300,000,000. 

By “most precision”, we mean having the smallest variance. Lets examine
each samples variance and compare. Note that we are excluding the $S^2$
from the equation, since it is assumed to be the same for all three
sample cases. 

``` r
V1 = (1 - (400/4000))*(1/400)
V2 = (1 - (30/300))*(1/300)
V3 = (1 - (3000/300000000))*(1/3000)
tab_1 = data.frame("Sample" = c(1:3), "Variance Proportion" = c(V1,V2,V3))
gt(tab_1)
```
