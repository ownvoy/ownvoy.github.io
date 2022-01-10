---

title: "13 Relational Data"
categories: R4DS
excerpt: 관계형 데이터
toc: true
toc_sticky: true

date: 2022-01-10
---
### 11-1 Relational Data
하나의 data frame만을 갖고 해석 하는 것은 드문 일이다. 대개 여러 개의 data frame을 합쳐서 분석한다. data들은 연결되어 있기 때문에, 여러 개의 data frame을 __relational data__ 라 부른다.  

relational data를 다루기 위한 3개의 동사가 있다.

- __Mutating joins__ : 다른 data frame에 있는 observation을 가져와 해당 variable을 새로 추가한다.  
- __Filtering joins__ : data frame에 있는 observation을 걸러낸다. 그 observation이 다른 data framedml observation과 맞는지 아닌지에 따라서
- __Set operations__ : observation을 set elements인것 처럼 다룬다.  
<br>
<br>
<br>

### 11-2 nycflights13
relational data에 대해 배우기 위해 nycflights13을 사용할 것이다. nycflight은 `flights`와 관련된 4개의 table을 가지고 있다. 
 
`airlines`는 약어로 전체 항공사명을 볼 수 있다.    
```r
airlines
```
```
# A tibble: 16 x 2
   carrier name
   <chr>   <chr>
 1 9E      Endeavor Air Inc.
 2 AA      American Airlines Inc.
 3 AS      Alaska Airlines Inc.
 4 B6      JetBlue Airways
 5 DL      Delta Air Lines Inc.
 6 EV      ExpressJet Airlines Inc.
 7 F9      Frontier Airlines Inc.
 8 FL      AirTran Airways Corporation
 9 HA      Hawaiian Airlines Inc.
10 MQ      Envoy Air
11 OO      SkyWest Airlines Inc.
12 UA      United Air Lines Inc.
13 US      US Airways Inc.
14 VX      Virgin America
15 WN      Southwest Airlines Co.
16 YV      Mesa Airlines Inc.
```
`faa`로 identified된 `airports`는 airport에 대한 정보를 제공한다.  
```r
airports
```
```r
# A tibble: 1,458 x 8
   faa   name                             lat    lon   alt    tz dst   tzone    
   <chr> <chr>                          <dbl>  <dbl> <dbl> <dbl> <chr> <chr>
 1 04G   Lansdowne Airport               41.1  -80.6  1044    -5 A     America/~
 2 06A   Moton Field Municipal Airport   32.5  -85.7   264    -6 A     America/~
 3 06C   Schaumburg Regional             42.0  -88.1   801    -6 A     America/~
 4 06N   Randall Airport                 41.4  -74.4   523    -5 A     America/~
 5 09J   Jekyll Island Airport           31.1  -81.4    11    -5 A     America/~
 6 0A9   Elizabethton Municipal Airport  36.4  -82.2  1593    -5 A     America/~
 7 0G6   Williams County Airport         41.5  -84.5   730    -5 A     America/~
 8 0G7   Finger Lakes Regional Airport   42.9  -76.8   492    -5 A     America/~
 9 0P2   Shoestring Aviation Airfield    39.8  -76.6  1000    -5 U     America/~
10 0S9   Jefferson County Intl           48.1 -123.    108    -8 A     America/~
# ... with 1,448 more rows
```
`tailnum`으로 identified 된 `planes`는 각각의 plane에 대한 정보를 제공한다.
```r
planes
```
```r
# A tibble: 3,322 x 9
   tailnum  year type          manufacturer   model  engines seats speed engine
   <chr>   <int> <chr>         <chr>          <chr>    <int> <int> <int> <chr>
 1 N10156   2004 Fixed wing m~ EMBRAER        EMB-1~       2    55    NA Turbo-~
 2 N102UW   1998 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 3 N103US   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 4 N104UW   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 5 N10575   2002 Fixed wing m~ EMBRAER        EMB-1~       2    55    NA Turbo-~
 6 N105UW   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 7 N107US   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 8 N108UW   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
 9 N109UW   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
10 N110UW   1999 Fixed wing m~ AIRBUS INDUST~ A320-~       2   182    NA Turbo-~
# ... with 3,312 more rows
```
`weather`는 nyc 공항의 매 시각 날씨를 제공한다.  
```r
# A tibble: 26,115 x 15
   origin  year month   day  hour  temp  dewp humid wind_dir wind_speed
   <chr>  <int> <int> <int> <int> <dbl> <dbl> <dbl>    <dbl>      <dbl>
 1 EWR     2013     1     1     1  39.0  26.1  59.4      270      10.4 
 2 EWR     2013     1     1     2  39.0  27.0  61.6      250       8.06
 3 EWR     2013     1     1     3  39.0  28.0  64.4      240      11.5 
 4 EWR     2013     1     1     4  39.9  28.0  62.2      250      12.7
 5 EWR     2013     1     1     5  39.0  28.0  64.4      260      12.7
 6 EWR     2013     1     1     6  37.9  28.0  67.2      240      11.5
 7 EWR     2013     1     1     7  39.0  28.0  64.4      240      15.0 
 8 EWR     2013     1     1     8  39.9  28.0  62.2      250      10.4
 9 EWR     2013     1     1     9  39.9  28.0  62.2      260      15.0
10 EWR     2013     1     1    10  41    28.0  59.6      260      13.8
# ... with 26,105 more rows, and 5 more variables: wind_gust <dbl>,
#   precip <dbl>, pressure <dbl>, visib <dbl>, time_hour <dttm>
```
이런식으로 정리가 된다!!!
![제목 없음](https://user-images.githubusercontent.com/96481582/148690922-56c09d14-a314-49ae-8628-172c2cb60771.png)
<br>
<br>
<br>

### 11-3 Keys
각각의 table을 연결짓는 variable를 __key__ 라 한다. key는 unique하게 observation을 identify 한다. 예를 들어 각각의 plane은 tailnum으로 identified 되었다. 또  `weather`에 있는 observation은 `year`,`month`,`day`,`hour`,`origin`으로 identifiy 되었다.  
<br>

key는 __primary key__ 와 __foreign key__ 로 두 가지가 있다.

- __primary key__ : 자신의 table에 있는 observation을 unique하게 identify 한다.  
(`planes$tailnum`은 primary key이다. 그 이유는 `planes` table에 있는 plane을 identify하기 때문이다.)    
- __foreign key__ : 다른 table에 있는 observation을 unique하게 identify 한다.  
(`flights$tailnum`는 foreign key이다.`flights` table에 있는 flight을 identify하기 때문이다.
~~flight이랑 plane을 잇는 느낌~~)
<br>

variable은 primary key이면서 foreign key일 수 있다. 예를들어  `origin`은 `weather` primary key의 일부이면서, `airports` table의 foreign key이다.  
<br>

table의 primary key를 발견했으면 그게 진짜 각각의 observation을 unique하게 identify하는지 확인해볼 필요가 있다. `count()`를 통해서 말이다.  
```r
planes %>% 
  count(tailnum) %>% 
  filter(n > 1)
  ```
```r
# A tibble: 0 x 2
# ... with 2 variables: tailnum <chr>, n <int>
```
왜 이런 결과가 나올지 생각 해봤는데 tailnum으로 묶으면 count가 1이어서 `filter()`로 걸러진 것이다.  
__즉 identify 된 것이다.__
```r
weather %>% 
  count(year, month, day, hour, origin) %>% 
  filter(n > 1)
  ```
```r
# A tibble: 3 x 6
   year month   day  hour origin     n
  <int> <int> <int> <int> <chr>  <int>
1  2013    11     3     1 EWR        2
2  2013    11     3     1 JFK        2
3  2013    11     3     1 LGA        2
```
identify 되지 못했다.  
<br>

때로는 명시적으로 primary key를 발견하지 못할 수 있다. 예를 들어 어떤 variable의 조합들도 observation을 identify 못할 수도 있다. 예를 들어 `flights`의 primary key는 뭘까? date+ flight 아니면 date+ tailnumber? 둘 다 아니다.  

```r
flights %>% 
  count(year, month, day, flight) %>% 
  filter(n > 1)
  ```
```r
# A tibble: 29,768 x 5
    year month   day flight     n
   <int> <int> <int>  <int> <int>
 1  2013     1     1      1     2
 2  2013     1     1      3     2
 3  2013     1     1      4     2
 4  2013     1     1     11     3
 5  2013     1     1     15     2
 6  2013     1     1     21     2
 7  2013     1     1     27     4
 8  2013     1     1     31     2
 9  2013     1     1     32     2
10  2013     1     1     35     2
# ... with 29,758 more rows
```
```r
flights %>% 
  count(year, month, day, tailnum) %>% 
  filter(n > 1)
```
```r
# A tibble: 64,928 x 5
    year month   day tailnum     n
   <int> <int> <int> <chr>   <int>
 1  2013     1     1 N0EGMQ      2
 2  2013     1     1 N11189      2
 3  2013     1     1 N11536      2
 4  2013     1     1 N11544      3
 5  2013     1     1 N11551      2
 6  2013     1     1 N12540      2
 7  2013     1     1 N12567      2
 8  2013     1     1 N13123      2
 9  2013     1     1 N13538      3
10  2013     1     1 N13566      3
# ... with 64,918 more rows
```

만약에 table이 primary key가 없으면 `mutate()`와 `row_number()`를 통해 하나 추가하는것도 방법이다. 이는 filter를 하고나서 원래의 data의 observation를 보다 쉽게 match 해준다. 이를 __surrogate key__ 라 한다.  

primary key와 다른 table에 있는 foreign key는 __relation__ 을 형성한다.  
대게  relation은 1-to-many이다. 예를들어 각각의 flight은 하나의 plane을 가지지만, plane은 많은 flight을 가진다. 
many-to-1과 1-to-many를 합쳐서 many to many relation을 형성하는 것도 가능하다.  
<br>
<br>

### 11-4 Mutating joins
table들을 합치는 tool 중 처음 볼 것은 __mutating join__ 이다. key를 통해 observation을 match 한 다음에 다른 table의 variable을 가져온다.  

`mutate`처럼 variable을 오른쪽에 추가한다.  

```r
flights2 <- flights %>% 
  select(year:day, hour, origin, dest, tailnum, carrier)
flights2
```
```r
# A tibble: 336,776 x 8
    year month   day  hour origin dest  tailnum carrier
   <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>
 1  2013     1     1     5 EWR    IAH   N14228  UA     
 2  2013     1     1     5 LGA    IAH   N24211  UA
 3  2013     1     1     5 JFK    MIA   N619AA  AA
 4  2013     1     1     5 JFK    BQN   N804JB  B6
 5  2013     1     1     6 LGA    ATL   N668DN  DL
 6  2013     1     1     5 EWR    ORD   N39463  UA     
 7  2013     1     1     6 EWR    FLL   N516JB  B6
 8  2013     1     1     6 LGA    IAD   N829AS  EV
 9  2013     1     1     6 JFK    MCO   N593JB  B6
10  2013     1     1     6 LGA    ORD   N3ALAA  AA     
# ... with 336,766 more rows
```
`flights2`에 airline의 full name을 넣고 싶다면, `left_join()`를 통해 `airlines`와 `flights2`를 합치면 된다.  
```r
flights2 %>%
  select(-origin, -dest) %>% 
  left_join(airlines, by = "carrier")
  ```
```r
# A tibble: 336,776 x 7
    year month   day  hour tailnum carrier name
   <int> <int> <int> <dbl> <chr>   <chr>   <chr>
 1  2013     1     1     5 N14228  UA      United Air Lines Inc.   
 2  2013     1     1     5 N24211  UA      United Air Lines Inc.
 3  2013     1     1     5 N619AA  AA      American Airlines Inc.
 4  2013     1     1     5 N804JB  B6      JetBlue Airways
 5  2013     1     1     6 N668DN  DL      Delta Air Lines Inc.
 6  2013     1     1     5 N39463  UA      United Air Lines Inc.
 7  2013     1     1     6 N516JB  B6      JetBlue Airways
 8  2013     1     1     6 N829AS  EV      ExpressJet Airlines Inc.
 9  2013     1     1     6 N593JB  B6      JetBlue Airways
10  2013     1     1     6 N3ALAA  AA      American Airlines Inc.
# ... with 336,766 more rows
```
`mutate()`와 비슷하기때문에 mutating joing이라 부른다.  
<br>
<br>

#### 11-4-1 Understanding joins
이제 그림과 함께 어떻게 join이 work하는지 쉽게 보여줄 것이다.  
```r
x <- tribble(
  ~key, ~val_x,
     1, "x1",
     2, "x2",
     3, "x3"
)
y <- tribble(
  ~key, ~val_y,
     1, "y1",
     2, "y2",
     4, "y3"
)
```
![제목 없음](https://user-images.githubusercontent.com/96481582/148715692-1fdc090b-60f2-4bb3-9aa9-99d457db570b.png){: width="50%" height="50%"}

색칠된 collumn은 "key" variable 이다.  
회색 collumn은 value이다.  

join이라는 작업은 `x` 있는 row를 `y`에 있는 row에 연결 하는 것이다. 
<br>
<br>

#### 11-4-2 Inner join
inner join은 key가 같을 때만 match한다.  
![제목 없음](https://user-images.githubusercontent.com/96481582/148716131-f5bbfdd4-0461-41c0-8960-484ac0f045fe.png)
```r
x %>% 
  inner_join(y, by = "key")
```
```r
# A tibble: 2 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1 x1    y1
2     2 x2    y2
```
가장 중요한 특징은 unmatched row는 결과에 포함되지 않는다.  
<br>
<br>

#### 11-4-3 Outer joins
inner join이 observation이 양쪽 둘 다 있어야지만 살려줬다면, outer join은 한쪽에만 있어도 살려준다. 
세가지 outer join이 있다.  

- left join: `x`의 observation을 유지한다.  
- right join: `y`의 observation을 유지한다.  
- full join: `x`와 `y`의 observation을 유지한다. 

대응하는 key가 없다면 `NA`를 채워준다.  

![제목 없음](https://user-images.githubusercontent.com/96481582/148716656-6b649640-f5bb-48ae-bfe2-d6c5b41f1745.png)
<br>
<br>

#### 11-4-4 Duplicate keys
지금까지 table에서의 key는 unique하였지만 여기서 다룰 table은 그렇지 않다. 두 가지 table을 보도록 하자.   

1. table 1개가 duplicate keys를 가지고 있다. 전형적인 1-to-many relation이다.  
```r
x <- tribble(
  ~key, ~val_x,
     1, "x1",
     2, "x2",
     2, "x3",
     1, "x4"
)
y <- tribble(
  ~key, ~val_y,
     1, "y1",
     2, "y2"
)
left_join(x, y, by = "key")
```

```r
# A tibble: 4 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1 x1    y1
2     2 x2    y2
3     2 x3    y2
4     1 x4    y1
```

2. table 2개가 duplicate keys를 가지고 있다. 보통의 경우 error이다. 어느 table 하나 key가 observation을 identify하지 못했기 때문이다.(~~그게 왜 error인데~~)

```r
x <- tribble(
  ~key, ~val_x,
     1, "x1",
     2, "x2",
     2, "x3",
     3, "x4"
)
y <- tribble(
  ~key, ~val_y,
     1, "y1",
     2, "y2",
     2, "y3",
     3, "y4"
)
left_join(x, y, by = "key")
```

```r
# A tibble: 6 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1 x1    y1
2     2 x2    y2
3     2 x2    y3
4     2 x3    y2
5     2 x3    y3
6     3 x4    y4   
```

<br>
<br>

#### 11-4-5 Defining the key columns
지금까지는 두 개의 공통으로 있는 한 개의 variable의 name으로 joing하였는데 이는 `by = "key"`로 가능한 것이었다. 

`by`를 사용하여 다른 방식으로 이을 수 있다. 

- `by = NULL`은 default로 모든 table의 variable을 쓴다. 이를 __natural join__ 이라 부른다.
```r
flights2 %>% 
  left_join(weather)
  ```
```r
Joining, by = c("year", "month", "day", "hour", "origin")
# A tibble: 336,776 x 18
    year month   day  hour origin dest  tailnum carrier  temp  dewp humid
   <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>   <dbl> <dbl> <dbl>
 1  2013     1     1     5 EWR    IAH   N14228  UA       39.0  28.0  64.4
 2  2013     1     1     5 LGA    IAH   N24211  UA       39.9  25.0  54.8
 3  2013     1     1     5 JFK    MIA   N619AA  AA       39.0  27.0  61.6
 4  2013     1     1     5 JFK    BQN   N804JB  B6       39.0  27.0  61.6
 5  2013     1     1     6 LGA    ATL   N668DN  DL       39.9  25.0  54.8
 6  2013     1     1     5 EWR    ORD   N39463  UA       39.0  28.0  64.4
 7  2013     1     1     6 EWR    FLL   N516JB  B6       37.9  28.0  67.2
 8  2013     1     1     6 LGA    IAD   N829AS  EV       39.9  25.0  54.8
 9  2013     1     1     6 JFK    MCO   N593JB  B6       37.9  27.0  64.3
10  2013     1     1     6 LGA    ORD   N3ALAA  AA       39.9  25.0  54.8
# ... with 336,766 more rows, and 7 more variables: wind_dir <dbl>,
#   wind_speed <dbl>, wind_gust <dbl>, precip <dbl>, pressure <dbl>,
#   visib <dbl>, time_hour <dttm>
```
- `by = "x"`는 natural join과 비슷하지만, common variable의 일부만 사용한다.  
예를 들어, `flights`와 `planes`는 `year` variable을 가지고 있지만, 서로 다른 의미를 가지고 있기에 `tailnum`으로만 join을 해보도록 하자.  
```r
flights2 %>% 
  left_join(planes, by = "tailnum")
```
```r
# A tibble: 336,776 x 16
   year.x month   day  hour origin dest  tailnum carrier year.y type
    <int> <int> <int> <dbl> <chr>  <chr> <chr>   <chr>    <int> <chr>
 1   2013     1     1     5 EWR    IAH   N14228  UA        1999 Fixed wing mult~
 2   2013     1     1     5 LGA    IAH   N24211  UA        1998 Fixed wing mult~
 3   2013     1     1     5 JFK    MIA   N619AA  AA        1990 Fixed wing mult~
 4   2013     1     1     5 JFK    BQN   N804JB  B6        2012 Fixed wing mult~
 5   2013     1     1     6 LGA    ATL   N668DN  DL        1991 Fixed wing mult~
 6   2013     1     1     5 EWR    ORD   N39463  UA        2012 Fixed wing mult~
 7   2013     1     1     6 EWR    FLL   N516JB  B6        2000 Fixed wing mult~
 8   2013     1     1     6 LGA    IAD   N829AS  EV        1998 Fixed wing mult~
 9   2013     1     1     6 JFK    MCO   N593JB  B6        2004 Fixed wing mult~
10   2013     1     1     6 LGA    ORD   N3ALAA  AA          NA NA
# ... with 336,766 more rows, and 6 more variables: manufacturer <chr>,
#   model <chr>, engines <int>, seats <int>, speed <int>, engine <chr>
```

### 11-5 Set operation
이 operation은 모든 variable의 value를 비교하면서 전체 row에 적용된다. x와 y가 똑같은 variable을 가진다면, observation은 set처럼 다루어진다.  

```r
df1 <- tribble(
  ~x, ~y,
   1,  1,
   2,  1
)
df2 <- tribble(
  ~x, ~y,
   1,  1,
   1,  2
)
```

- `intersect(x, y)`: `x`와 `y` 둘 다에 있는 observation을 반환한다.  
```r
intersect(df1, df2)
```
```r
# A tibble: 1 x 2
      x     y
  <dbl> <dbl>
1     1     1
```
- `union(x,y)`: `x`와`y`에 있는 unique한 observation을 반환한다.  
```r
union(df1, df2)
```
```r
# A tibble: 3 x 2
      x     y
  <dbl> <dbl>
1     1     1
2     2     1
3     1     2
```
`setdiff(x, y)`: `x`에는 있지만 `y`에는 없는 observation을 반환한다.  
```r
setdiff(df1, df2)
```
```r
# A tibble: 1 x 2
      x     y
  <dbl> <dbl>
1     2     1
```