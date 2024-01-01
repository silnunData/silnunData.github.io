---
title: "(R) markdown 문법"
categories: R
tag: ['R','Rmd']
use_math: true
---


# echo 옵션

코드의 부분이 보여지는지의 여부

echo = TRUE 일떄, 코드부분과 결과부분이 모두 보여짐

``` r
a <- c(1:5)
a
```

    ## [1] 1 2 3 4 5

echo = FALSE 일 떄, 결과부분만 보여짐

    ## [1] 1 2 3 4 5

<br/>

# eval 옵션

코드를 저장하는지의 여부 (echo = TRUE 이기 떄문에 보여지긴 한다.)

eval = TRUE 일 떄, result1을 불러와진다.

``` r
result1 <- mean(a)
```

eval = FALSE 일 떄, result2를 불러오려고 하면 err가 난다. (저장이 안됬기
떄문)

``` r
result2 <- median(a)
```

<br/>

# results 옵션

결과가 보여지는지의 여부 (eval = T 이니깐 변수가 저장은 됨)

``` r
b <- 1:100
b
```

<br/>

# 본문안에 변수 삽입

result1 의 값은 3 이다. (’r 변수’이다.) <br/><br/>

# 수식 입력

- 본문 내에 삽입하는 방법 (Inline mode).  
  `$` 기호를 사용하여 묶어준다. $y= f(x)$

- 본문과 독립적으로 삽입하는 방법 (Display mode).  
  `$$` 기호를 사용하여 묶어준다. $$
  y= f(x)
  $$

- 수식을 여러줄 입력할 경우에는 `aligned` 라는 레이텍 환경을 이용한다.

  1.  `$$` 기호로 display mode를 만든다.

  2.  `aligned` 환경을 만들어준다.  
      시작할떄 ₩begin{aligned}, 끝날떄 ₩end{aligned}

  3.  `\\` 을 사용하여 줄바꿈.

  4.  정렬기준 정하기  
      `&` 을 사용하여 정해주면, 그 곳을 기준으로 정렬이 된다.

$$
\begin{aligned}
f(x,y) &= x^2 + \sqrt{y^3} \\
g(x) &= x^3 + x^2 + 3x + 5
\end{aligned}
$$

- 입력한 수식에 번호 부여하기  
  `equation` 환경과 `aligned` 를 사용하여 수식 입력  
  `(\#eq:myequ)` 태그를 사용하여 라벨링

- `align` 환경에서의 수식 번호

- 본문에서 수식 언급하기  
  `\@ref` 태그를 이용하여 `\@ref(eq:myequ2)` 로 사용한다.  
  수식의 라벨은 꼭 `eq:`로 시작해야 한다.

<br/><br/>
<hr>

# TIP

1.  cirl + alt + i 를 하면 바로 code chuck가 나온다.  
    (window의 경우 cirl + shift + i)

2.  code chunck의 설정에 들어가면 warning, message 등을 키고 끌 수 있다.

3.  개별 chunck에서 설정이 귀찮다면 맨위 r setup 에 FALSE 로 추가 할 수
    있다.  
    ex. knitr::opts_chunk\$set(message = F)
