---

title: "16 Date and Times"
categories: R4DS
excerpt: 시간과 날짜
toc: true
toc_sticky: true

date: 2022-01-17
---

## 16 Dates and times

이번 파트에서는 날짜와 시간을 다뤄보도록 할 것이다.  
__lubridate__ 에 집중을 해보도록 하자.  


```R
library(tidyverse)

library(lubridate)
library(nycflights13)
```

    Registered S3 methods overwritten by 'ggplot2':
      method         from 
      [.quosures     rlang
      c.quosures     rlang
      print.quosures rlang
    Registered S3 method overwritten by 'rvest':
      method            from
      read_xml.response xml2
    -- Attaching packages --------------------------------------- tidyverse 1.2.1 --
    √ ggplot2 3.1.1       √ purrr   0.3.2  
    √ tibble  2.1.1       √ dplyr   0.8.0.1
    √ tidyr   0.8.3       √ stringr 1.4.0  
    √ readr   1.3.1       √ forcats 0.4.0  
    -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    x dplyr::filter() masks stats::filter()
    x dplyr::lag()    masks stats::lag()
    
    Attaching package: 'lubridate'
    
    The following object is masked from 'package:base':
    
        date
    
    Warning message:
    "package 'nycflights13' was built under R version 3.6.3"
<br>
<br>

### 16-1 Creating date/times

날짜 시간 데이터는 세가지 유형이 있다.  

- __date__ : Tibble은 <date>로 출력한다.  
- __time__ : Tibble은 <time>로 출력한다.  
- __date-time__: Tibble은 <dttm>로 출력한다.  

현재의 시간 정보를 얻기 위해서는 다음과 같은 코드를 쓰면 된다.  


```R
today()
```


<time datetime="2022-01-18">2022-01-18</time>



```R
now()
```


    [1] "2022-01-18 11:00:38 KST"


이거 아니면 시간 데이터를 만드는 3가지 방법이 또 있다.  
- From a string.
From individual date-time components.
From an existing date/time object.
<br>
<br>

#### 16-1-1 From strings

lubritate의 도움을 받아 string 형식의 data를 만들 수 있다.  
그렇기 위해서는 일정한 형식을 따라야 한다.  


```R
ymd("2017-01-31")
```


<time datetime="2017-01-31">2017-01-31</time>


```R
mdy("January 31st, 2017")
```


<time datetime="2017-03-01">2017-03-01</time>



```R
dmy("31-Jan-2017")
```


<time datetime="2017-01-31">2017-01-31</time>

<br>

이 함수는 심지어 unquoted 숫자까지 쓸 수 있다.  


```R
ymd(20170131)
```


<time datetime="2017-01-31">2017-01-31</time>

<br>

언더바 이후에 시간(h),분(m),초(s)를 추가할 수 도 있다.  


```R
ymd_hms("2017-01-31 20:11:59")
```


    [1] "2017-01-31 20:11:59 UTC"



```R
mdy_hm("01/31/2017 08:01")
```


    [1] "2017-01-31 08:01:00 UTC"

<br>

timezone을 추가할 수도 있다.  


```R
ymd(20170131, tz = "UTC")
```


    [1] "2017-01-31 UTC"

<br>
<br>

### 16-1-2 From individual components
앞에서 배운 single string 대신에 여러개의 collumn에 걸친 date-time 요소들을 쓸 수도 있다.  
이는 flight data에 있다.  


```R
flights %>% 
  select(year, month, day, hour, minute) %>%
    head()
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>hour</th><th scope=col>minute</th></tr></thead>
<tbody>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>5   </td><td>15  </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>5   </td><td>29  </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>5   </td><td>40  </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>5   </td><td>45  </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>6   </td><td> 0  </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>5   </td><td>58  </td></tr>
</tbody>
</table>


<br>

시간 정보를 만들기 위해, `make_date()`와 `make_datetime()`를 써볼 수 있다.  


```R
flights %>% 
  select(year, month, day, hour, minute) %>% 
  mutate(departure = make_datetime(year, month, day, hour, minute)) %>%
  head()
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>departure</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>5                  </td><td>15                 </td><td>2013-01-01 05:15:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>5                  </td><td>29                 </td><td>2013-01-01 05:29:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>5                  </td><td>40                 </td><td>2013-01-01 05:40:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>5                  </td><td>45                 </td><td>2013-01-01 05:45:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>6                  </td><td> 0                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>5                  </td><td>58                 </td><td>2013-01-01 05:58:00</td></tr>
</tbody>
</table>


<br>
<br>


```R
make_datetime_100 <- function(year, month, day, time) {
  make_datetime(year, month, day, time %/% 100, time %% 100)
}# 사용자 정의 함수

flights_dt <- flights %>% 
  filter(!is.na(dep_time), !is.na(arr_time)) %>% 
  mutate(
    dep_time = make_datetime_100(year, month, day, dep_time),
    arr_time = make_datetime_100(year, month, day, arr_time),
    sched_dep_time = make_datetime_100(year, month, day, sched_dep_time),
    sched_arr_time = make_datetime_100(year, month, day, sched_arr_time)
  ) %>% 
  select(origin, dest, ends_with("delay"), ends_with("time")) 

flights_dt %>%
  head()

```


<table>
<thead><tr><th scope=col>origin</th><th scope=col>dest</th><th scope=col>dep_delay</th><th scope=col>arr_delay</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>air_time</th></tr></thead>
<tbody>
	<tr><td>EWR                </td><td>IAH                </td><td> 2                 </td><td> 11                </td><td>2013-01-01 05:17:00</td><td>2013-01-01 05:15:00</td><td>2013-01-01 08:30:00</td><td>2013-01-01 08:19:00</td><td>227                </td></tr>
	<tr><td>LGA                </td><td>IAH                </td><td> 4                 </td><td> 20                </td><td>2013-01-01 05:33:00</td><td>2013-01-01 05:29:00</td><td>2013-01-01 08:50:00</td><td>2013-01-01 08:30:00</td><td>227                </td></tr>
	<tr><td>JFK                </td><td>MIA                </td><td> 2                 </td><td> 33                </td><td>2013-01-01 05:42:00</td><td>2013-01-01 05:40:00</td><td>2013-01-01 09:23:00</td><td>2013-01-01 08:50:00</td><td>160                </td></tr>
	<tr><td>JFK                </td><td>BQN                </td><td>-1                 </td><td>-18                </td><td>2013-01-01 05:44:00</td><td>2013-01-01 05:45:00</td><td>2013-01-01 10:04:00</td><td>2013-01-01 10:22:00</td><td>183                </td></tr>
	<tr><td>LGA                </td><td>ATL                </td><td>-6                 </td><td>-25                </td><td>2013-01-01 05:54:00</td><td>2013-01-01 06:00:00</td><td>2013-01-01 08:12:00</td><td>2013-01-01 08:37:00</td><td>116                </td></tr>
	<tr><td>EWR                </td><td>ORD                </td><td>-4                 </td><td> 12                </td><td>2013-01-01 05:54:00</td><td>2013-01-01 05:58:00</td><td>2013-01-01 07:40:00</td><td>2013-01-01 07:28:00</td><td>150                </td></tr>
</tbody>
</table>


<br>

위 분포를 the year동안 departure time이 몇 번 일어났는지 visualization할 수 있다.  


```R
flights_dt %>% 
  ggplot(aes(dep_time)) + 
  geom_freqpoly(binwidth = 86400) # 86400 seconds = 1 day
```


    
![output_23_0](https://user-images.githubusercontent.com/96481582/149863175-b0b324e5-daa5-47e8-a7b6-427c1494f736.png)
    


아래는 하루동안 이다.  


```R
flights_dt %>% 
  filter(dep_time < ymd(20130102)) %>% 
  ggplot(aes(dep_time)) + 
  geom_freqpoly(binwidth = 600)
```


    
![output_25_0](https://user-images.githubusercontent.com/96481582/149863176-746b00a1-cfd1-4660-b5f3-841179737849.png)
    
<br>
<br>

### 16-1-3 From other types

date-time과 date 사이에서 형식을 바꾸고 싶다면, `as_datetime()` 와 `as_date()` 를 사용하면 된다.  


```R
as_datetime(today())
```


    [1] "2022-01-18 UTC"



```R
as_date(now())
```


<time datetime="2022-01-18">2022-01-18</time>

<br>
<br>

### 16-2 Date-time components

이제 date-time data를 갖는 방법을 배웠으니, 이거 갖고 뭐를 할 수 있는 지 봐보자.  

#### 16-2-1 Getting components

다음 함수를 통해 date를 쪼갤 수 있다.  
`year()`, `month()`, `mday()` (day of the month), `yday()` (day of the year), `wday()` (day of the week), `hour()`, `minute()`, and `second()`


```R
datetime <- ymd_hms("2016-07-08 12:34:56")

year(datetime)
```


2016



```R
month(datetime)
```


7



```R
mday(datetime)
```


8


```R
yday(datetime)
```


190



```R
wday(datetime)
```


6
<br>


`wday()`는 `label = TRUE` 를 같이 쓰면, 달의 이름으로 바뀌고, `abbr = FALSE`를 추가할시 약어에서 풀네임으로 바뀐다.  


```R
wday(datetime, label = TRUE)
```


금
<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'일'</li>
		<li>'월'</li>
		<li>'화'</li>
		<li>'수'</li>
		<li>'목'</li>
		<li>'금'</li>
		<li>'토'</li>
	</ol>
</details>



```R
wday(datetime, label = TRUE, abbr = FALSE)
```


금요일
<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'일요일'</li>
		<li>'월요일'</li>
		<li>'화요일'</li>
		<li>'수요일'</li>
		<li>'목요일'</li>
		<li>'금요일'</li>
		<li>'토요일'</li>
	</ol>
</details>

<br>

주말보다 평일에 flight가 많은 것을 알 수 있다.  


```R
flights_dt %>% 
  mutate(wday = wday(dep_time, label = TRUE)) %>% 
  ggplot(aes(x = wday)) +
    geom_bar()
```


    
![output_40_0](https://user-images.githubusercontent.com/96481582/149863165-f82c38af-0c56-4e9e-852c-b68d8ae8819f.png)
    

<br>

한편, dep_time이 20~30분, 50~60분인 시간대가 딴 시간대에 비해서 delay가 적은 것을 알 수 있다.   


```R
flights_dt %>% 
  mutate(minute = minute(dep_time)) %>% 
  group_by(minute) %>% 
  summarise(
    avg_delay = mean(arr_delay, na.rm = TRUE),
    n = n()) %>% 
  ggplot(aes(minute, avg_delay)) +
    geom_line()
```


    
![output_42_0](https://user-images.githubusercontent.com/96481582/149863170-5d17a624-61b7-43c1-8334-6f370c5f358f.png)
    
<br>
<br>

### 16-2-2 Setting components

위의 함수들을 이용하여, date/time data를 정할 수 있다.  


```R
(datetime <- ymd_hms("2016-07-08 12:34:56"))
```


    [1] "2016-07-08 12:34:56 UTC"



```R
year(datetime) <- 2020
datetime
```


    [1] "2020-07-08 12:34:56 UTC"



```R
month(datetime) <- 01
datetime
```


    [1] "2020-01-08 12:34:56 UTC"



```R
hour(datetime) <- hour(datetime) + 1
datetime
```


    [1] "2020-01-08 13:34:56 UTC"

<br>


`update()`를 이용하여 이를 한 번에 할 수도 있다.  


```R
update(datetime, year = 2020, month = 2, mday = 2, hour = 2)
```


    [1] "2020-02-02 02:34:56 UTC"

<br>

만약 value가 너무 크면 이월된다. 


```R
ymd("2015-02-01") %>% 
  update(mday = 30)
```


<time datetime="2015-03-02">2015-03-02</time>



```R
ymd("2015-02-01") %>% 
  update(hour = 400)
```


    [1] "2015-02-17 16:00:00 UTC"



```R
flights_dt %>% 
  mutate(dep_hour = update(dep_time, yday = 3)) %>% 
  ggplot(aes(dep_hour)) +
    geom_freqpoly(binwidth = 300)
```


    
![output_53_0](https://user-images.githubusercontent.com/96481582/149863172-d899716c-2913-4e90-92a9-934eb6e75751.png)
    


### 16-3 Time span
이제는 date를 가지고 뺴기,더하기,나누기 이런것을 해보도록 할 것이다.  
<br>
<br>

#### 16-3-1 Time spans

difftime class는 시,분,초,달,주 이런식으로 저장되어있어서 빼거나 더할 때 모호성이 있다.  


```R
h_age <- today() - ymd(19791014)
h_age
```


    Time difference of 15437 days

<br>

이를 해결하기 위해 항상 seconds로 표현해주는 __duration__ 을 쓴다.  


```R
as.duration(h_age)
```


1333756800s (~42.26 years)

<br>

다음은 duration의 예시이다.  


```R
dseconds(15)
```


15s



```R
dminutes(10)
```


600s (~10 minutes)



```R
dhours(c(12, 24))
```


<ol class=list-inline>
	<li>43200s (~12 hours)</li>
	<li>86400s (~1 days)</li>
</ol>




```R
ddays(0:5)
```


<ol class=list-inline>
	<li>0s</li>
	<li>86400s (~1 days)</li>
	<li>172800s (~2 days)</li>
	<li>259200s (~3 days)</li>
	<li>345600s (~4 days)</li>
	<li>432000s (~5 days)</li>
</ol>




```R
dweeks(3)
```


1814400s (~3 weeks)



```R
dyears(1)
```


31536000s (~52.14 weeks)

<br>

더하거나 곱하기 역시 할 수 있다.  


```R
2 * dyears(1)
```


63072000s (~2 years)



```R
dyears(1) + dweeks(12) + dhours(15)
```


38847600s (~1.23 years)



```R
tomorrow <- today() + ddays(1)
tomorrow
```


<time datetime="2022-01-19">2022-01-19</time>

<br>

그러나 __duration__ 은 정확하게 second여서 예기치 못한 상황이 발생할 수도 있다.  


```R
one_pm <- ymd_hms("2016-03-12 13:00:00", tz = "America/New_York")
one_pm
one_pm + ddays(1)
```


    [1] "2016-03-12 13:00:00 EST"



    [1] "2016-03-13 14:00:00 EDT"


하루를 더했더니 1시에서 2시가 되었다. time zone이 바뀐 것을 알 수 있는데, DST 떄문에, 3월12일은 하루가 23시간이다.  
이런 문제를 해결하기 위해 직관적으로 쓸 수 있는 __periods__ 가 있다.  
<br>
<br>

#### 16-3-2 Periods
duration처럼 쉽게 시간을 만들 수 있다.  


```R
seconds(15)
```


15S



```R
minutes(10)
```


10M 0S



```R
hours(c(12, 24))
```


<ol class=list-inline>
	<li>12H 0M 0S</li>
	<li>24H 0M 0S</li>
</ol>




```R
days(7)
```


7d 0H 0M 0S



```R
months(1:6)
```


<ol class=list-inline>
	<li>1m 0d 0H 0M 0S</li>
	<li>2m 0d 0H 0M 0S</li>
	<li>3m 0d 0H 0M 0S</li>
	<li>4m 0d 0H 0M 0S</li>
	<li>5m 0d 0H 0M 0S</li>
	<li>6m 0d 0H 0M 0S</li>
</ol>




```R
weeks(3)
```


21d 0H 0M 0S



```R
years(1)
```


1y 0m 0d 0H 0M 0S

<br>

periods 역시 연산을 할 수 있다.  


```R
10 * (months(6) + days(1))
```


60m 10d 0H 0M 0S



```R
days(50) + hours(25) + minutes(2)
```


50d 25H 2M 0S


위의 문제들을 해결할 수 있다.  


```R
ymd("2016-01-01") + dyears(1)
```


<time datetime="2016-12-31">2016-12-31</time>



```R
ymd("2016-01-01") + years(1)
```


<time datetime="2017-01-01">2017-01-01</time>



```R
one_pm + ddays(1)
```


    [1] "2016-03-13 14:00:00 EDT"



```R
one_pm + days(1)
```


    [1] "2016-03-13 13:00:00 EDT"

<br>

이제 periods를 써서 flight에 있는 이상한 값을 고쳐보도록하자.  
어떤 값들은 도착시간이 출발시간보다 빠른데 다음날로 바꿔줘야한다.  


```R
flights_dt %>% 
  filter(arr_time < dep_time) %>%
    head()
```


<table>
<thead><tr><th scope=col>origin</th><th scope=col>dest</th><th scope=col>dep_delay</th><th scope=col>arr_delay</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>air_time</th></tr></thead>
<tbody>
	<tr><td>EWR                </td><td>BQN                </td><td>  9                </td><td> -4                </td><td>2013-01-01 19:29:00</td><td>2013-01-01 19:20:00</td><td>2013-01-01 00:03:00</td><td>2013-01-01 00:07:00</td><td>192                </td></tr>
	<tr><td>JFK                </td><td>DFW                </td><td> 59                </td><td> NA                </td><td>2013-01-01 19:39:00</td><td>2013-01-01 18:40:00</td><td>2013-01-01 00:29:00</td><td>2013-01-01 21:51:00</td><td> NA                </td></tr>
	<tr><td>EWR                </td><td>TPA                </td><td> -2                </td><td>  9                </td><td>2013-01-01 20:58:00</td><td>2013-01-01 21:00:00</td><td>2013-01-01 00:08:00</td><td>2013-01-01 23:59:00</td><td>159                </td></tr>
	<tr><td>EWR                </td><td>SJU                </td><td> -6                </td><td>-12                </td><td>2013-01-01 21:02:00</td><td>2013-01-01 21:08:00</td><td>2013-01-01 01:46:00</td><td>2013-01-01 01:58:00</td><td>199                </td></tr>
	<tr><td>EWR                </td><td>SFO                </td><td> 11                </td><td>-14                </td><td>2013-01-01 21:08:00</td><td>2013-01-01 20:57:00</td><td>2013-01-01 00:25:00</td><td>2013-01-01 00:39:00</td><td>354                </td></tr>
	<tr><td>LGA                </td><td>FLL                </td><td>-10                </td><td> -2                </td><td>2013-01-01 21:20:00</td><td>2013-01-01 21:30:00</td><td>2013-01-01 00:16:00</td><td>2013-01-01 00:18:00</td><td>160                </td></tr>
</tbody>
</table>

<br>


```R
flights_dt <- flights_dt %>% 
  mutate(
    overnight = arr_time < dep_time,
    arr_time = arr_time + days(overnight * 1),
    sched_arr_time = sched_arr_time + days(overnight * 1)
  ) 

flights_dt
```


<table>
<thead><tr><th scope=col>origin</th><th scope=col>dest</th><th scope=col>dep_delay</th><th scope=col>arr_delay</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>air_time</th><th scope=col>overnight</th></tr></thead>
<tbody>
	<tr><td>EWR                </td><td>IAH                </td><td> 2                 </td><td> 11                </td><td>2013-01-01 05:17:00</td><td>2013-01-01 05:15:00</td><td>2013-01-01 08:30:00</td><td>2013-01-01 08:19:00</td><td>227                </td><td>FALSE              </td></tr>
	<tr><td>LGA                </td><td>IAH                </td><td> 4                 </td><td> 20                </td><td>2013-01-01 05:33:00</td><td>2013-01-01 05:29:00</td><td>2013-01-01 08:50:00</td><td>2013-01-01 08:30:00</td><td>227                </td><td>FALSE              </td></tr>
	<tr><td>JFK                </td><td>MIA                </td><td> 2                 </td><td> 33                </td><td>2013-01-01 05:42:00</td><td>2013-01-01 05:40:00</td><td>2013-01-01 09:23:00</td><td>2013-01-01 08:50:00</td><td>160                </td><td>FALSE              </td></tr>
	<tr><td>JFK                </td><td>BQN                </td><td>-1                 </td><td>-18                </td><td>2013-01-01 05:44:00</td><td>2013-01-01 05:45:00</td><td>2013-01-01 10:04:00</td><td>2013-01-01 10:22:00</td><td>183                </td><td>FALSE              </td></tr>
	<tr><td>LGA                </td><td>ATL                </td><td>-6                 </td><td>-25                </td><td>2013-01-01 05:54:00</td><td>2013-01-01 06:00:00</td><td>2013-01-01 08:12:00</td><td>2013-01-01 08:37:00</td><td>116                </td><td>FALSE              </td></tr>
	<tr><td>EWR                </td><td>ORD                </td><td>-4                 </td><td> 12                </td><td>2013-01-01 05:54:00</td><td>2013-01-01 05:58:00</td><td>2013-01-01 07:40:00</td><td>2013-01-01 07:28:00</td><td>150                </td><td>FALSE              </td></tr>
</tbody>
</table>

<br>



```R
flights_dt %>% 
  filter(overnight, arr_time < dep_time)
```


<table>
<thead><tr><th scope=col>origin</th><th scope=col>dest</th><th scope=col>dep_delay</th><th scope=col>arr_delay</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>air_time</th><th scope=col>overnight</th></tr></thead>
<tbody>
</tbody>
</table>

<br>
<br>

### 16-3-3 Intervals
`dyears(1)`를 `ddays(365)`로 나누면 둘 다 정확한 second로 나누니 1이 나올 것이다.  
그러면  `years(1)` 를 `days(1)`로 나누면??  

어떤년도는 365일이고 어떤년도는 366일이다.  


```R
years(1) / days(1)
```

    estimate only: convert to intervals for accuracy
    


365.25

<br>

__interval__ 를 쓰면 더 명확하게 할 수 있다.  
starting point를 정해주는 것이다.  


```R
next_year <- today() + years(1)
(today() %--% next_year) / ddays(1)
```


365

<br>
<br>

~~공부하다보니까 date와 datetime을 구별 안한 거 같다.. 다음에 복습할 떄 기울여서~~
