Chapter 2.1 Generation of probability distribution
================
Jae Kwan Koo

-   [generation of distribution](#generation-of-distribution)
-   [Random number generator](#random-number-generator)
    -   [recursive formula](#recursive-formula)
    -   [conditions of random number generator](#conditions-of-random-number-generator)
-   [linear conguential](#linear-conguential)
    -   [Code](#code)
    -   [Conditions](#conditions)
-   [Standard Uniform distribution test](#standard-uniform-distribution-test)

### generation of distribution

### Random number generator

0과 1사이 균일분포로부터 하나의 값을 뽑아내야 한다. 어떻게 0과 1사이 값을 찾아낼까?
여러번 뽑아내면 똑같은 확률을 가지게 되더라.

``` r
knitr::include_graphics('https://raw.githubusercontent.com/koojaekwan/jaekwan-s-R/d2a9dd03991f12a752fe05bcf4948b79ee7f4bff/Computerized_Statistics/Chapter2/chap2-1.PNG')
```

<img src="https://raw.githubusercontent.com/koojaekwan/jaekwan-s-R/d2a9dd03991f12a752fe05bcf4948b79ee7f4bff/Computerized_Statistics/Chapter2/chap2-1.PNG" style="display: block; margin: auto;" />

#### recursive formula

뽑았던 공에 번호를 붙이면 영향을 받게 되는가?
함수의 값을 결정짓는데 직전 함수의 값이 직후의 함수값에 영향을 미치는지가 순환공식이다.

-   난수는 확률이 아니라 관측값을 찾아내는 것이다.
-   순환했기 때문에 애매하다. (직전에 뽑았던 구슬에 영향받으므로 독립은 아니다.)
-   난수이되, 독립인 난수가 아니라 서로 영향을 받을 수 있다. `유사 난수`라고 한다.(pseudo random number) 하지만 유사 난수는 난수와 역할, 특징이 거의 비슷하다. (서로 독립이 아닐 수 있다는 것만 빼면 비슷하다)
    지금부터는 그냥 이 것을 난수라고 생각하자.

~~*요즘은 그냥 난수생성 알고리즘이라고 말하는거 같다*~~

#### conditions of random number generator

-   난수같은 경우 균일분포를 따라야 한다. (균일분포의 이론을 활용했기 때문이다.)
-   초기값이 순환공식에 의해 필요하다.
-   이후에 뽑을 공은 이전에 뽑은 공에 영향을 받게 된다.

모의실험하는 입장에서 불가능하지만 컨트롤 할 수 있다. 첫번째 뽑은 공을 설정할 수 있다.
순환공식에 의해 만들어진 난수는 균일분포를 따르게 될 수 밖에 없다. 특별한 조건을 이야기 하자면 `자기상관`이 없어야 한다.

`시드값(seed)`을 고정시켜 동일한 난수를 생성가능해야 한다. 따라서, 모의실험 시 시드값을 부여하는 것이 가장 좋다.

### linear conguential

가장 단순한 방법이다.

Xn값이 크면 당연히 Xn+1값이 커지게 된다. 이 경우 자기상관이 나타날 수 밖에 없다.
이 방법을 이용했을 시 재생성이 가능하거나 자기상관이 없다는 등 말할 수 없지만 유사난수의 역할을 할 수 있다.

~~*요즘은 유사난수도 그냥 난수라고 부른다.*~~

x0, a, c, m은 초기에 필요한 값이다. 이 것들은 고정된 값으로 역할하게 된다.
m은 나눠주게 되는 큰 숫자이다. Xn은 일반적으로 m보다는 작게 나타나는 것이 일반적이다.

{0,...}은 0과 1사이에 있는 균일분포라고 가정하게 되고 1/m에 대응되는 x의 값 등 확률이라고 생각할 수 있다. 1/m만큼 간격, 등간격으로 나타나므로 균일분포 특성을 반영한 것 같이 보인다.
이산형 균일분포같은 경우 듬성듬성하게 되어 있다. 하지만, 잘게 쪼갠다면 거의 이어진 것 처럼 나타낼 수 있을 것이다.

Ex) 간단한 예를 보자.
먼저, m=8로 고정시켰다.
1) 초기치의 값(a, c, x0 등)을 부여했다. x1 = 3\*x0+1 / 8 로 나눈 나머지를 이야기하게 된다. x2 ,x3 등등 마찬가지로 순서대로 구한 후 8로 나눈 나머지의 값을 가진다.

하고 나면, 일정하게 동일한 수가 반복되는 하나의 난수열이라고 볼 수 있다.
가질 수 있는 값이 4개밖에 없다. -&gt;이 것은 굉장히 국한되어 있는 형태이다.

2번 예제도 마찬가지이다.
이 예제에서 우리는 모의실험은 4가지 8가지 밖에 나타나지 않는다는 사실을 알 수 있다.
즉, 너무 가질 수 있는 값이 적고 여러 이유 때문에 선형합동법은 전통적인 방법이라 잘 사용되지는 않는다. 이 방법은 특정상황에서만 사용가능하므로 요즘 사용빈도는 떨어진다.

``` r
knitr::include_graphics('https://raw.githubusercontent.com/koojaekwan/jaekwan-s-R/master/Computerized_Statistics/Chapter2/chap2-2.PNG')
```

<img src="https://raw.githubusercontent.com/koojaekwan/jaekwan-s-R/master/Computerized_Statistics/Chapter2/chap2-2.PNG" style="display: block; margin: auto;" />

#### Code

순환 공식을 사용하기 때문에 직전 자료값으로부터 직후 자료값이 영향받는다.
즉, 초깃값이 필요하다. java, c, python 같은 경우 벡터를 참조할 때 0부터 시작하지만 R 같은 경우 1부터 시작한다는 점에서 R은 프로그래밍 언어로써 약간 차이가 있다.
즉, x0는 3이라는 것을 부여한 것이 x\[1\]&lt;-3이다.
간단하게 주기가 4로 나열되는 것을 확인 가능할 것이다.

한번 돌려보고 결과를 확인해보자.

``` r
m <- 8; a <- 3; c <- 1; x <- 3

for(i in 2:10) x[i] <- (a*x[i-1]+c)%%m

x
```

    ##  [1] 3 2 7 6 3 2 7 6 3 2

``` r
i <- 1; y <- 3

while(1){
  y[i+1] <- (a*y[i]+c)%%m
  
  if(length(y)>=1000) break;
  i <- i+1 }

y
```

    ##    [1] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##   [35] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##   [69] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [103] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [137] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [171] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [205] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [239] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [273] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [307] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [341] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [375] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [409] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [443] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [477] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [511] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [545] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [579] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [613] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [647] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [681] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [715] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [749] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [783] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [817] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [851] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [885] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [919] 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6
    ##  [953] 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2 7 6 3 2
    ##  [987] 7 6 3 2 7 6 3 2 7 6 3 2 7 6

#### Conditions

-   m의 값이 매우 클수록 좋다. 나누는 값이 커질수록 앞의 나머지 값이 다양한 형태로 나타나게 되므로 난수의 형태가 다양해진다.
-   `완전주기`인 경우 훨씬 더 좋은경우라고 볼 수 있다.
    -   c와 m은 공통인수를 가지지 않는 정수여야 한다.
    -   a는 m의 모든 인수 p에 대하여 a = 1(mod p) 이어야 한다.

초기값은 내 마음대로 설정가능하다.
m은 클수록 좋은데 그렇다면 a, c는 어떻게 설정하는게 좋은가?
증가량이 m보다 더 커지게 된다면 난수생성에 시간이 더 오래걸리고 계산도 많이 해야되기 떄문에 일반적으로 root m보다 크고 m - root m보다 작도록 잡아준다.

-   c는 절편의 역할이다.
-   c가 있는 경우를 `혼합식`, c가 없는 경우를 `승산식`이라고 한다.
-   c가 없는 경우는 계산이 더 빠르다.(당연하다.)
-   c가 없는 경우는 완전조건이라고 할 수 없다. (c와 m의 공통인수가 없어야 한다는 완전주기 조건충족 못함)

승산식은 쉬우므로 혼합식보다 더 많은 연구가 이루어져 있다.
여러 면을 종합할 때, 승산식은 완전조건 만족하지 않아도 된다는 장점이 있다.
또한, c가 0이 아닐때 더 효율적이라고 판단한다.

~~*혼합식이 나쁜 것은 아니다. 단지, 더 계산해야되기도 하고 복잡하기도 할 뿐이다.*~~

### Standard Uniform distribution test

그냥 여러가지의 가설검정들이 있다. 난수들의 독립성 검정을 해야할 필요도 있고, 그래프도 그려볼 수 있다.
딱히 표준일 필요는 없다. 그냥 균일분포를 따르는지에 대한 검정이다.
모수적인 방법, 비모수적인 방법, 그래프를 이용한 방법 등 여러가지 방법이 있다.

카이제곱분포를 이용한 적합성 검정 경우 모수적인 방법이다.
콜모고로프-스미르노프는 비모수 방법이다. 비모수적인 것은 모집단 특성을 나타내고 있는 모수가 포함되어 있지 않는 경우를 말한다. 이것은 경험적인 분포를 사용한다고 한다.

자기상관이 없어야 한다고 이야기했었다. 그래프를 통해 난수사이 독립성 검정 해볼 수 있다.
만약 1번부터 100까지 난수번호를 알고 있다면, 순차적으로 첫번째 부터 100까지 그래프를 그려볼 수 있다. 만약 특정한 경향성을 가질 때, 난수간의 연관이 있다고 말한다. 즉, 자기상관이 있다고 말한다.
이 경우, 특정 패턴이 반복된다고 볼 수 있는데, 비모수에서 배울 런검정으로 패턴이 반복되는 경우 확인이 가능하다.
