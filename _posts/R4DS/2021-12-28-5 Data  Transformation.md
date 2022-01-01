---

title: "5 Data Transformation"
categories: R4DS

toc: true
toc_sticky: true

date: 2021-12-28
---
## 5 Data Transformation
_data transformation_  을 하는 이유=> 더 작업하기 좋게 위해서
<br>
<br>

### 약어

- int: integer  
- dbl: double(real number)  
- chr: character  
- dttm: date+time  
- lgl: logical  
- fctr: factor  
- date: date  
<br>

### 5가지 dplyr function

- filter(): 값을 통해 관측치를 고른다  
- arrange(): row을 정렬한다  
- select(): 이름을 기준으로 속성을 고른다  
- mutate(): 존재하는 속성을 가지고 새로운 속성을 만든다  
- summarise(): 여러 속성을 하나로 정리  
<br>

#### dplyr function 사용 방법

(__first__ , __second__, __third__)  
__first__ = data frame  
__second__ := 변수 이름을 통해 data frame 한테 뭐 할지  
__third__ = new data frame  
<br>
<br>

### 1.filter()

__(__first__ , __second__, __third__)__  
__first__ = data frame  
__second__= 속성  
__third__= 속성  

```r
(dec25 <- filter(flights, month == 12, day == 25))
```
  
```r 
# A tibble: 719 x 19
    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
 1  2013    12    25      456            500        -4      649            651
 2  2013    12    25      524            515         9      805            814
 3  2013    12    25      542            540         2      832            850
 4  2013    12    25      546            550        -4     1022           1027
 5  2013    12    25      556            600        -4      730            745
 6  2013    12    25      557            600        -3      743            752
 7  2013    12    25      557            600        -3      818            831
 8  2013    12    25      559            600        -1      855            856
 9  2013    12    25      559            600        -1      849            855
10  2013    12    25      600            600         0      850            846
```

가로를 쳐주어야지 dec25에 저장을 하고 출력을 동시에 할 수 있다  
<br>
filters()는 TRUE 값들만 사용하기 때문에 NA는 못쓴다 NA를 쓰고 싶으면
is.na(x)를 사용해야 한다  
<br>
<br>

### 2.arrange() 
__(__first__ , __second__, __third__)__  
__first__ = data frame  
__second__= 속성  
__third__= 속성  
```r
arrange(flights, year, month, day)
```

```r
# A tibble: 719 x 19
    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
 1  2013    12    25      456            500        -4      649            651
 2  2013    12    25      524            515         9      805            814
 3  2013    12    25      542            540         2      832            850
 4  2013    12    25      546            550        -4     1022           1027
 5  2013    12    25      556            600        -4      730            745
 6  2013    12    25      557            600        -3      743            752
 7  2013    12    25      557            600        -3      818            831
 8  2013    12    25      559            600        -1      855            856
 9  2013    12    25      559            600        -1      849            855
10  2013    12    25      600            600         0      850            846
# ... with 709 more rows, and 11 more variables: arr_delay <dbl>,
#   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

dept_time를 기준으로 내림차순 정리하고 싶으면 desc(dept_time)를 쓴다

```r 
arrange(flights,desc(dept_time))
```
<br>
<br>

### 3.select() 
column들을 선택한다  
```r
select(flights,year,month,day)
```
```r
# A tibble: 336,776 x 3
    year month   day
   <int> <int> <int>
 1  2013     1     1
 2  2013     1     1
 3  2013     1     1
 4  2013     1     1
 5  2013     1     1
 6  2013     1     1
 7  2013     1     1
 8  2013     1     1
 9  2013     1     1
10  2013     1     1
# ... with 336,766 more rows
```
```r
select(flights,year:day)
select(flights,-(year:day))
```
__year:day__ 는 year와 day 사이에 있는 column들을 선택한다  
__-(year:day)__ 는 year와 day 사이에 있는 column들을 __제외__ 하고 선택한다  
<br>

__select()안에 쓸 수 있는 그 외 함수들__  
```starts_with("abc")```: abc로 시작하는 거 선택  

```ends_with("abc")  ```: abc로 끝나는 거 선택  

```contains("ijk") ```: ijk포함하는 거 선택  

```num_range("x", 1:3)```:x1,x2,x3에 매칭  
<br>
<br>

### 4.mutate()  
```r
flights_sml <- select(flights, 
  year:day, 
  ends_with("delay"), 
  distance, 
  air_time
  )
```
flights_sml은 year:day, delay로 끝나는 것들,distance,air_time을 고른 거다
여기서  
__gain = dep_delay - arr_delay__  
__speed = distance / air_time * 60__  
새로운 변수를 만들고 싶음

```r
mutate(flights_sml,
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60)
```
결과
```r
# A tibble: 336,776 x 9
    year month   day dep_delay arr_delay distance air_time  gain speed
   <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl> <dbl>
 1  2013     1     1         2        11     1400      227    -9  370.
 2  2013     1     1         4        20     1416      227   -16  374.
 3  2013     1     1         2        33     1089      160   -31  408.
 4  2013     1     1        -1       -18     1576      183    17  517.
 5  2013     1     1        -6       -25      762      116    19  394.
 6  2013     1     1        -4        12      719      150   -16  288.
 7  2013     1     1        -5        19     1065      158   -24  404.
 8  2013     1     1        -3       -14      229       53    11  259.
 9  2013     1     1        -3        -8      944      140     5  405.
10  2013     1     1        -2         8      733      138   -10  319.
# ... with 336,766 more rows
```
__gain__ 과 __speed__ column들만 뽑고 싶으면
mutate 대신 
```r  
transmute(flights,
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60
)
```
를 사용하면 된다
<br>
<br>

### 5.summarise()
__summarise()__ 는 __group_by()__ 와 짝꿍  
__group_by()__ 를 통해 전체 데이터에 적용하는 것이 아닌 그룹별로 적용 가능!  

```r
by_month <- group_by(flights, month)
```
데이터를 __month(월)__ 로  __그룹화__
이제 __month(월)__ 별로 __요약__
```r
summarise(by_month, delay = mean(dep_delay, na.rm = TRUE),n = n())
```
```r
# A tibble: 12 x 3
   month delay     n
   <int> <dbl> <int>
 1     1 10.0  27004
 2     2 10.8  24951
 3     3 13.2  28834
 4     4 13.9  28330
 5     5 13.0  28796
 6     6 20.8  28243
 7     7 21.7  29425
 8     8 12.6  29327
 9     9  6.72 27574
10    10  6.24 28889
11    11  5.44 27268
12    12 16.6  28135
```
__n()__ 의 역할은 행이 몇개 인지 알려준다  
~~예를 들어 1월달은 27004개 있다~~  
<br>

__결측 값__ 을 제거 하는 방법으로는
__summarise()__ 안에 `na.rm= TRUE`
__filter()__ 안에 `!is.na()`를 사용한다
```r
not_cancelled <- filter(flights, !is.na(dep_delay))
by_month <- group_by(not_cancelled, month)
summarise(by_month, delay = mean(dep_delay))
```
코드가 점점 복잡해지고 있다
`%>%`(__파이프__)로 해결
```r
flights %>%
  filter(!is.na(dep_delay)) %>%
  group_by(month) %>%
  summarise(delay = mean(dep_delay), n = n())
```
위의 코드와 똑같은 결과가 나온다  
<br>

#### Grouping by multiple variables
```r
daily <- group_by(flights, year, month, day)
(per_day   <- summarise(daily, flights = n()))
```
```r
# A tibble: 365 x 4
# Groups:   year, month [12]
    year month   day flights
   <int> <int> <int>   <int>
 1  2013     1     1     842
 2  2013     1     2     943
 3  2013     1     3     914
 4  2013     1     4     915
 5  2013     1     5     720
 6  2013     1     6     832
 7  2013     1     7     933
 8  2013     1     8     899
 9  2013     1     9     902
10  2013     1    10     932
# ... with 355 more rows
```
여기서 또 그룹핑 할 수 있을까?
day까 없어지는 걸로
```r
(per_month <- summarise(per_day, flights = sum(flights)))
```
```r
# A tibble: 12 x 3
# Groups:   year [1]
    year month flights
   <int> <int>   <int>
 1  2013     1   27004
 2  2013     2   24951
 3  2013     3   28834
 4  2013     4   28330
 5  2013     5   28796
 6  2013     6   28243
 7  2013     7   29425
 8  2013     8   29327
 9  2013     9   27574
10  2013    10   28889
11  2013    11   27268
12  2013    12   28135
```
또 그루핑? month가 없어지는걸루
```r
(per_year  <- summarise(per_month, flights = sum(flights)))
```
```r
# A tibble: 1 x 2
   year flights
  <int>   <int>
1  2013  336776
```
~~왜 flights = sum(flights)를 하는지 잘 모르겠다~~
<br>
<br>

#### ungrouping
그룹푸는 법
```r
daily %>% 
  ungroup() %>% 
  summarise(flights = n())
```
```r
# A tibble: 1 x 1
  flights
    <int>
1  336776
```