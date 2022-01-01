---

title: "7 Exploratory Data Analysis"
categories: R4DS

toc: true
toc_sticky: true

date: 2021-12-30
---

## Exploratory Data Analysis

__Exploratory Data Analysis(EDA)__ 는 반복적인 작업이다 
1. __data__ 에 관한 물음을 가진다.  
2. __data__ 를 __visualizing__, __transforming__, __modeling__ 해서 답을 찾는다.  
3. 질문을 가다듬거나, 새로운 질문을 만든다  
<br>

이런 과정을 통해 우리는 data에 대한 이해를 높일 수 있다.  
높은 이해를 위해서는 좋은 질문들이 필수적이다.   
질문에 대한 규칙은 없지만 다음 두가지의 질문은 언제나 용이하다. 
<br>

1. __variable__(변수)에 무슨 변화가 일어 났는지?
2. __variable__(변수) 사이에 무슨 상관관계가 있는지?
<br>
<br>
<br>

### 7-1 용어 정리
- __variable__(변수): 측정할 수 있는 __quantity, quality, property__  
<br>
- __value__(값): __variable__ 의 상태  
<br> 
- __observation__(관측값): 비슷한 조건 하에 측정된 값들의 모임  
(~~관측값은 서로 다른 __variable__(변수)가 조합된 다양한 __value__(값)을 포함한다~~)  
<br>
- __tabular data__: __value__(값)들의 모임이다
<br>

-  __tidy__ : 각각의 __value__ 들이 __cell__ 에,각각의 __variable__ 들이 __column__ 에, 각각의  __observation__ 들이 __row__ 에 있는 것 
<br>
<br>
<br>

### 7-2 Variation
__variation__ 은 __variable__ 의 __value__ 들이 변화하는 경향성이다.  
각각의 __variable__ 들은 __variation__ 을 가지고 있는데, 이를 이해 할 수 있는 방법은 __value__ 의 __distribution__ 을 살펴 보는 것이다. 
<br>
<br>

#### 7-2-1 Visualising distributions
__variable__ 이 _categorical_ 하냐 _continuous_ 하냐에 따라 __distribution__ 이 달라진다. 
_categorical_ variable은 __bar_chart__ 를 통해 표현한다
```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147641215-7dba83fe-03cf-4928-955a-f8a6b6ec9196.png)
y는 x value에 대해 얼마나 많은 observation이 일어났는지 표현한다. 
`dyplr`에 있는 `count()` 함수에서 역시 이를 확인 할 수 있다.  
```r
 diamonds %>% 
+   count(cut)
```
```r
# A tibble: 5 x 2
  cut           n
  <ord>     <int>
1 Fair       1610
2 Good       4906
3 Very Good 12082
4 Premium   13791
5 Ideal     21551
```
<br>

만약 variable이 __continuous__ 하면 수많은 집합을 가진다.   
~~시간이나 수같은 경우가 continuous variable이다~~ 
<br>
이들의 __distriution__ 를 확인하기 위해서는 __histogram__ 을 사용하기로 한다

```r
ggplot(data = diamonds) +
  geom_histogram(mapping = aes(x = carat), binwidth = 0.5)
```
![Rplot01](https://user-images.githubusercontent.com/96481582/147706703-844dcce8-73fa-4bc5-a7a5-3525dfd00dae.png)
이 역시 `dyplr`에 있는 `count()`함수를 통해 확인할 수 있지만 continuous하기 떄문에 구간을 정해 주어야 한다. 
구간을 정해주기 위해서는 `ggplot2` 에 있는 `cut_width()`를 이용한다
```r
diamonds %>% 
  count(cut_width(carat, 0.5))
  ```
  
```r
  # A tibble: 11 x 2
   `cut_width(carat, 0.5)`     n
   <fct>                   <int>
 1 [-0.25,0.25]              785
 2 (0.25,0.75]             29498
 3 (0.75,1.25]             15977
 4 (1.25,1.75]              5313
 5 (1.75,2.25]              2002
 6 (2.25,2.75]               322
 7 (2.75,3.25]                32
 8 (3.25,3.75]                 5
 9 (3.75,4.25]                 4
10 (4.25,4.75]                 1
11 (4.75,5.25]                 1
```
`bindwith`을 통해 이 간격을 정할 수 있다.  
다양한 `bindwith`을 설정하는 것이 좋은데 각각이 다른 패턴들을 보여 주기 때문이다. 
```r
smaller <- diamonds %>% 
  filter(carat < 3)
  
ggplot(data = smaller, mapping = aes(x = carat)) +
  geom_histogram(binwidth = 0.1)
```
![plot](https://user-images.githubusercontent.com/96481582/147707411-07ebc47d-fbb4-4169-bff6-b1f7e8952591.png)
<br>
<br>
만약 같은 plot에 여러 개의 histogram을 그리고 싶다면 `geom_freqpoly()`를 쓰면 된다! 
대신 bar 가 line으로 대체 된다
 ```r
 ggplot(data = smaller, mapping = aes(x = carat, colour = cut)) +
  geom_freqpoly(binwidth = 1)
  ```
![plot](https://user-images.githubusercontent.com/96481582/147707744-eb52774b-30ae-4ff0-bd9b-65746ad2d106.png)
<br>
<br>

#### 7-2-2 Unusual values
__outliers(이상치)__ 는 패턴에 맞지 않는 data point이다.  
<br>
outliers는 단순히 잘못 입력된 값일 수 있지만 결정적인 정보가 될 수 도 있다. 
데이터가 너무 많으면 outliers를 찾아내기 어렵다.
<br>

y가 변수인 다음과 같은 상황을 보자. 
~~여기서 y는 x축 y축의 y가 아니고 width이다. 궁금하면 `?diamonds`에서 확인할 수 있다~~  

```r
ggplot(diamonds) + 
  geom_histogram(mapping = aes(x = y), binwidth = 0.5)
  ```
![plot](https://user-images.githubusercontent.com/96481582/147708399-f98ddbd1-af89-478c-a629-554ab622240e.png)

common bins에 너무 많은 observation이 있기에 __unusual values__ 을 확인하기 어렵다. 

~~bin은 쉽게 똑같이 나눠진 구역이라 생각하면 된다~~ 

따라서 y축을 조정해 보겠다. 
이를 위해서는 `coord_cartesian()`를 사용하면 된다
```r
ggplot(diamonds) + 
  geom_histogram(mapping = aes(x = y), binwidth = 0.5) +
  coord_cartesian(ylim = c(0, 50))
```
![plot](https://user-images.githubusercontent.com/96481582/147708694-f92147d6-e011-47f0-a386-b3f2db972a0c.png)
<br>

~~x축 역시 `coord_cartesian()`의 `xlim()`을 통해 조정할 수 있다~~  

이제야 __unusual values__ 를 확인해 볼 수 있다  
0, (20,40), (40,60)에서 보인다.

이제 `dyplr` 함수들을 통해 걸러 보도록 하자.  
```r
unusual <- diamonds %>% 
  filter(y < 3 | y > 20) %>% 
  select(price, x, y, z) %>%
  arrange(y)
unusual
```
```r
# A tibble: 9 x 4
  price     x     y     z
  <int> <dbl> <dbl> <dbl>
1  5139  0      0    0
2  6381  0      0    0
3 12800  0      0    0
4 15686  0      0    0
5 18034  0      0    0
6  2130  0      0    0
7  2130  0      0    0
8  2075  5.15  31.8  5.12
9 12210  8.09  58.9  8.06
```
이제 다음과 같은 해석을 할 수 있다. 

길이가 0인 다이아몬드는 존재 할 수 없으니 이상한 값이다.  
또한 (20,60) 에 있는 data 역시 인정하기 어렵다. 큰 것에 비해서 가격이 싸기 때문이다.  
<br>
<br>

### 7-3 Missing values
__unusual values__ 를 만났을 떄 우리가 할 수 있는 것은 2가지 있다
> 1. 이상한 값이 있는 모든 row를 지운다
```r
diamonds2 <- diamonds %>% 
  filter(between(y, 3, 20))
```
그러나 이 방법은 추천 하지 않는다. 
measurement 하나가 이상하다고 해서 전체 measurement가 이상한 것이 아닐 뿐더러 이렇게 삭제하기만 하다면 작은 데이터에서는 아무 데이터가 남지 않을 수 있다!
  2. unusual values를 missing values로 바꿔준다

가장 쉬운 방법은 `mutate()`와 `ifelse()` 함수를 사용하여 unusual values를 `NA`로 바꿔 주는 것이다.
```r
diamonds2 <- diamonds %>% 
  mutate(y = ifelse(y < 3 | y > 20, NA, y))
```
`ifelse()`는 다음과 같이 사용된다.  
<br>
__ifelse(`test`, `yes`, `no`)__   
`test`는 logical vector 여야 한다.    
`test`가 참이면 `yes`로  
`test`가 거짓이면 `no`로 이동한다.  

~~ifelse() 대신에 dplyr에 있는 case_when()을 사용하기도 한다.~~  
<br>

ggplot2는 missing values 가 조용히 사라져서는 안된다고 여긴다. 그런데 이 missing values를 plot에 나타낼 방법이 없다. 그래서 ggplot2는 이 값을 포함하지 않으며 그들이 삭제되었다는 메세지를 준다. 
```r
ggplot(data = diamonds2, mapping = aes(x = x, y = y)) + 
  geom_point()
  ```
```
Removed 9 rows containing missing values (geom_point). 
```
![plot](https://user-images.githubusercontent.com/96481582/147710545-8ff5e6bd-ed1f-41cb-b3ed-db44886a85fd.png)

이 메세지를 없애기 위해서는 `na.rm = TRUE`을 쓰면 된다. 
```r
ggplot(data = diamonds2, mapping = aes(x = x, y = y)) + 
  geom_point(na.rm = TRUE)
  ```
<br>
<br>
때로는 missing values와 그렇지 않은 값을 비교해보고 싶을 수도 있다.  

예를들어 `nycflights13::flights`에 있는 `dep_time`에서 missing values가 났다는 것은 비행이 취소 됐다는 것을 의미한다.  
<br>
취소된 시간대와 그렇지 않은 시간대를 알고 싶을 수 있다! 

```r
nycflights13::flights %>% 
  mutate(
    cancelled = is.na(dep_time),
    sched_hour = sched_dep_time %/% 100,
    sched_min = sched_dep_time %% 100,
    sched_dep_time = sched_hour + sched_min / 60
  ) %>% 
  ggplot(mapping = aes(sched_dep_time)) + 
    geom_freqpoly(mapping = aes(colour = cancelled), binwidth = 1/4)
```
![plot](https://user-images.githubusercontent.com/96481582/147711291-36097c00-87fd-4a2d-bc8d-bc4b1cab8a42.png)
<br>
<br>
<br>

### 7-4 Covariation 
__variation__ 이 하나의 variable 안에서의 모습을 묘사한 거라면 __covariation__ 은 variable 들끼리의 관계를 묘사한다.  
이들의 관계를 포착하기 위한 가장 쉬운 방법은 시각화 하는 것이다.  
<br>
<br>

#### 7-4-1A categorical and continuous variable
_continuous variable_ 의 distribution을 살펴 보기 위해 앞서 했듯이, `geom_freqpoly()`를 사용할 수 있다.  
<br>
그러나 이는 그다지 좋은 방법이 아니다. y축이 count로 제시 되어 있기에 count가 적은 그룹 같은 경우 변화를 관측하기 어렵다.  
```r
ggplot(data = diamonds, mapping = aes(x = price)) + 
  geom_freqpoly(mapping = aes(colour = cut), binwidth = 500)
```
![plot](https://user-images.githubusercontent.com/96481582/147716702-f16a6df1-af89-44e3-b6cb-4f59dc35ec87.png)
<br>

비교를 쉽게 하기 위해 frequency polygon의 영역을 1로 표준화한 __density__ 를 사용하는 것도 하나의 해결책이다. 
```r
ggplot(data = diamonds, mapping = aes(x = price, y = ..density..)) + 
  geom_freqpoly(mapping = aes(colour = cut), binwidth = 500)
  ``` 
![plot](https://user-images.githubusercontent.com/96481582/147716883-4ce5365b-1828-4587-b963-fff0011cf314.png)
<br>

또 다른 시각화 방법은 __boxplot__ 이다.  
> boxplot에 대한 설명
1. interquartile range (IQR)의 아래선은 25% 윗선은 75%을 나타낸다.
2. IQR 안에 있는 선은 median이다
3. IQR의 1.5 밖에 있는 점들은 outliers 이다
4. IQR 바깥으로 퍼져나가는 선들은 non-outlier이다.
![제목 없음](https://user-images.githubusercontent.com/96481582/147717316-1137d554-f9aa-4f68-9388-9f34e55ddd84.png)


<br>

geom_boxplot()을 이용하여 `cut` 에 따른 `price`의 distribution을 알아보자. 
```r
ggplot(data = diamonds, mapping = aes(x = cut, y = price)) +
  geom_boxplot()
  ``` 
  ![plot](https://user-images.githubusercontent.com/96481582/147717467-cfa0281e-f517-4d71-906d-9dccd61eec26.png)  
<br>
boxplot을 통해 얻을 수 있는 정보는 적지만 더 간단하게 볼 수 있다. 
<br> 

`cut`은 __ordered factor__ 이다. (fair<good,<verygood<premium<ideal)  
그러나 많은 경우 이렇게 명시적으로 정렬되어 있지 않기 때문에  
reorder의 필요성이 있다!  
<br>
```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) +
  geom_boxplot()
```

![plot](https://user-images.githubusercontent.com/96481582/147717741-6a34432e-12b8-40af-a151-f265333f7819.png)
<br>
경향을 쉽게 보기 위해서 `median` 값을 중심으로 reorder 할 수 있다.  
```r
ggplot(data = mpg) +
  geom_boxplot(mapping = aes(x = reorder(class, hwy, FUN = median), y = hwy))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147718040-94be7e59-0472-4733-9ce0-9cd813b4a030.png)
<br>
만약 variable 이름이 길다면 90도 회전하는 것도 하나의 방법이다.  
```r
ggplot(data = mpg) +
  geom_boxplot(mapping = aes(x = reorder(class, hwy, FUN = median), y = hwy)) +
  coord_flip()
  ```
  ![plot](https://user-images.githubusercontent.com/96481582/147718149-ad23539e-8a75-469a-9117-f2506d4bc95f.png)  
  <br>
  <br>

#### 7-4-2 Two categorical variables
두 개의 categorical variables의 covariation 을 시각화하기 위해 각 조합에서 만들어지는 관측치들을 세어 볼 필요가 있다.  
<br>

이를 위해 `geom_count()`를 사용해 볼 것이다.
```r
ggplot(data = diamonds) +
  geom_count(mapping = aes(x = cut, y = color))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147718683-c1d727ee-fc4b-45d9-8d8a-71fc88c0221f.png)
<br>
점들의 크기는 variable들의 조합에 해당하는 observation들이 얼마나 많은지 표현 한 것이다. 
<br>

`dyplr`을 통해서도 표현할 수 있다.
```r
diamonds %>% 
  count(color, cut)
```
```r
# A tibble: 35 x 3
   color cut           n
   <ord> <ord>     <int>
 1 D     Fair        163
 2 D     Good        662
 3 D     Very Good  1513
 4 D     Premium    1603
 5 D     Ideal      2834
 6 E     Fair        224
 7 E     Good        933
 8 E     Very Good  2400
 9 E     Premium    2337
10 E     Ideal      3903
# ... with 25 more rows
```
그리고 이를 `geom_tile()`를 통해 시각화 할 수 있다.
```r
diamonds %>% 
  count(color, cut) %>%  
  ggplot(mapping = aes(x = color, y = cut)) +
    geom_tile(mapping = aes(fill = n))
```

![plot](https://user-images.githubusercontent.com/96481582/147718927-f8b56440-1bd2-450f-b383-17c59588092d.png)
<br>

~~만약 categorical variables이 unordered 하면 seriation package를 통해 reorder 할 수 있으며  
플롯이 이보다 더 클 경우, `d3heatmap` 이나 `heatmaply` packages를 사용해 볼 수 있다.~~
<br>
<br>

#### 7-4-3 two continuous variable
이미 우리는 two continuous variable의 covariation을 시각화하는 방법을 안다.  
`geom_plot`이 그 예이다.  

```r
ggplot(data = diamonds) +
  geom_point(mapping = aes(x = carat, y = price))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147848982-88064e69-157c-407f-bc14-a4bbd1a0ac2b.png)  

__scatterplot__ 은 데이터의 사이즈가 커질 수록 유용하지 않을 수 있다.  
데이터들이 겹칠 수 있기 때문이다.  
해결 방법은 전에 한 번 다루었다.  
__transparency__ 를 추가하는 것이다.
```r
ggplot(data = diamonds) + 
  geom_point(mapping = aes(x = carat, y = price), alpha = 1 / 100)
  ```
  ![plot](https://user-images.githubusercontent.com/96481582/147849046-95a3ea3a-d2a1-4c93-9b65-47fe81ec0793.png)  
<br>
<br>
2d bin 을 색칠하는 방법으로도 시각화 할 수 있다.
`geom_bin2d()`는 정사각형 bins를 만들고
`geom_hex()`는 육각형 bins를 만든다.  
```r
ggplot(data = diamonds) +
  geom_bin2d(mapping = aes(x = carat, y = price))

# install.packages("hexbin")
ggplot(data = diamonds) +
  geom_hex(mapping = aes(x = carat, y = price))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147849206-311e9124-a928-44aa-8847-772de872e289.png)

<br>
또 다른 하나의 방법은 하나의 continuous value를 categorical value 취급하는 것이다!  

그럼 7-4-1에서 배운 것을 써먹을 수 있다.
예를들어 `carat`을 bin해서 boxplot을 보일 수도 있다.
```r
ggplot(data = diamonds, mapping = aes(x = carat, y = price)) + 
  geom_boxplot(mapping = aes(group = cut_width(carat, 0.1)))
  ```
![plot](https://user-images.githubusercontent.com/96481582/147849852-42aaa74f-ddaa-48d8-a6b3-c95810259076.png)

  이 그래프의 문제는 각 구간의 observation이 얼마나 있는지 보여주지 못한다는 것이다.  
  이를 해결하기 위해서는 bin에 있는 대략적인 숫자를 표시하는 것이다. 
  `cut_number`를 이용해서
  ```r
  ggplot(data = diamonds, mapping = aes(x = carat, y = price)) + 
  geom_boxplot(mapping = aes(group = cut_number(carat, 20))) 
  ```
![plot](https://user-images.githubusercontent.com/96481582/147849834-e023b773-beef-409e-b6a7-e83fda19a061.png)
<br>
<br>
