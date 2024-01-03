---
title: '(R) 통계검정의 2가지 근본맥락 구간추정과 가설검정'
categories: R
tag: ['R','statistic']
---



# 통계검정의 2가지 근본

## 1. 통계적 추정 (신뢰구간 추정)

모평균을 모를때, 표본을 통하여 모평균을 추정하고자 한다.

### 1) 모분산을 알때 (Z 검정통계량 사용)

$$
\bar{X} \pm Z_{a/2} \frac{\sigma_n}{\sqrt{n}}
$$

### 2) 모분산을 모를때, 표본분산을 사용 (t 검정통계량 사용)

t 검정통계량을 통하여 직접 신뢰구간 계산

$$
\bar{X} \pm t_{a/2, n-1} \frac{S_n}{\sqrt{n}}
$$

``` r
data <- c(4.3,4.1,5.2,4.9,5,4.5,4.7,4.8,5.2,4.6)

n <- length(data)
mean <- mean(data)
sd <- sd(data)

lower = round(mean - qt(0.975,n-1)*sd/sqrt(n),3)
upper = round(mean + qt(0.975, n-1)*sd/sqrt(n),3)

c(lower,upper)
```

    ## [1] 4.469 4.991

<br/>

t.test를 통하여 신뢰구간 추정하는 법

``` r
t.test(data, mu = mean(data), alternative = "two.sided")
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  data
    ## t = 0, df = 9, p-value = 1
    ## alternative hypothesis: true mean is not equal to 4.73
    ## 95 percent confidence interval:
    ##  4.46868 4.99132
    ## sample estimates:
    ## mean of x 
    ##      4.73

## 2. 가설검정 (귀무가설 채택/기각 여부)

귀무가설에서 주어지는 모평균추정치를 알때, 그 추정치가 나올 확률을
계산하고 비교

귀무가설($H_0$) : 기존에 참이라고 받아들여지고 있는 가정  
대립가설($H_A$, $H_1$) : 귀무가설의 반대 상황 (주장하고자 하는 바)

$H_A : \mu \neq 3$ 양측검정(two-tailed test)  
$H_A : \mu < 3$ 단측검정 (one-tailed test)  
$H_A : \mu > 3$ 단측검정 (one-tailed test)

예제) $H_0 : \mu = 7$ vs. $H_A : \mu \neq 7$

``` r
data2 <- c(4.62,4.09,6.2,8.24,0.77,5.55,3.11,11.97,2.16,3.24,10.91,11.36,0.87,9.93,2.9)

shapiro.test(data2)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  data2
    ## W = 0.91163, p-value = 0.1434

``` r
n <- length(data2)
mean <- mean(data2)
sd <- sd(data2)

t_value <- (mean - 7) / (sd/sqrt(n)) 

pt(t_value, n-1) * 2
```

    ## [1] 0.2216002

data2는 shapiro-wilk 의 귀무가설을 기각하지 못하므로, 정규성을
만족한다.  
0.05의 유의수준으로 양측검정을 해볼때, 해당 p-value는 기각역에 속하지
않으므로, 귀무가설을 기각하지 못한다.
