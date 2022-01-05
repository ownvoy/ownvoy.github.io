---

title: "3 Data visualisation"
categories: R4DS
excerpt: 데이터 시각화
toc: true
toc_sticky: true

date: 2021-12-24
---
## 3 Data visualisation
__ggplot2__ 에 있는 `mpg` __data frame__ 을 사용  
 ```r
 <br>
 mpg
 ```
```r
# A tibble: 234 x 11
   manufacturer model      displ  year   cyl trans drv     cty   hwy fl    classv
   <chr>        <chr>      <dbl> <int> <int> <chr> <chr> <int> <int> <chr> <chr>
 1 audi         a4           1.8  1999     4 auto~ f        18    29 p     comp~
 2 audi         a4           1.8  1999     4 manu~ f        21    29 p     comp~
 3 audi         a4           2    2008     4 manu~ f        20    31 p     comp~
 4 audi         a4           2    2008     4 auto~ f        21    30 p     comp~
 5 audi         a4           2.8  1999     6 auto~ f        16    26 p     comp~
 6 audi         a4           2.8  1999     6 manu~ f        18    26 p     comp~
 7 audi         a4           3.1  2008     6 auto~ f        18    27 p     comp~
 8 audi         a4 quattro   1.8  1999     4 manu~ 4        18    26 p     comp~
 9 audi         a4 quattro   1.8  1999     4 auto~ 4        16    25 p     comp~
10 audi         a4 quattro   2    2008     4 manu~ 4        20    28 p     comp~
# ... with 224 more rows
```
<br>
<br>

### 3-1 mpg 용어
- __displ__: 엔진 사이즈  
- __hwy__: 연비 on highway  
- __cyl__: 엔진 aka cylinder  
- __drv__: 구동 방식 (f= front 전륜구동, r = rear 후륜구동, 4= 4륜구동) 
<br>  

### 3-2 creating a ggplot
```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```
![ggplot 젤 기본적인 산점도](https://user-images.githubusercontent.com/96481582/147457389-99fe3bf1-0f0f-49ce-8e1d-f7cb50fb4e43.png){: width=""70" height="70"}

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
  ```
![class 별로 색깔](https://user-images.githubusercontent.com/96481582/147457752-ba188764-7b22-498f-855b-165e18b966d4.png){: width=""110" height="50"}
<br>
<br>

### 3-3 facets
__categorical value__ 를 잘 다루는 방법으로는 __facets__ 으로 __plot__ 을 나눌 수 있다  
```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
```
`( ,class)` 는 __collumn__  별로 나눈 것이며 , `nrow=2`는 __row__ 개수 2개로 한것이다 
<br>
![facet표](https://user-images.githubusercontent.com/96481582/147458523-67d87bc2-340e-41e5-820f-0f3ddf93d71a.png)

__row__ 별로도 나눌 수 있다
```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(drv ~ cyl)
```
![씽facet](https://user-images.githubusercontent.com/96481582/147459285-e32e04c7-ec92-42a2-9ce7-2ca71b4d4a45.png)
<br>
<br>

### 3-4 geom_point와 geom_smooth
각각은 `geom_point` ,`geom_smooth`로 표현된 plot이다 
```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```
![산점도 기본](https://user-images.githubusercontent.com/96481582/147462349-b974e7b4-7d28-432a-9858-7c8f7722ad37.png)
```r
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy))
```
![스무스 dios](https://user-images.githubusercontent.com/96481582/147462576-0a608b61-319b-4820-9525-634939542629.png)

__drv__ 마다 `geom_smooth`를 나눌 수 있다
```r
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv))
  ```
![drvdrv](https://user-images.githubusercontent.com/96481582/147462801-7c1b698e-ea9d-453a-b5e5-32bcb20ecfc7.png)

`geom_point`와 `geom_smooth`를 같이 쓸 수도 있다
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
  ```
`geom_point`에 `class`별로 색깔 추가 역시 할 수 있다.
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth()
```
![스무스+ 포인트](https://user-images.githubusercontent.com/96481582/147463737-58550a6a-091f-49e2-91db-bdb6458ed4d1.png)
<br>

`geom_point` 에 선형회귀식을 추가 할 수 도 있다  
선형식의 95% 신뢰 구간이 __default__ 로 주어진다  
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(method=lm)
```
![linear model](https://user-images.githubusercontent.com/96481582/147464669-790ab347-5adc-432d-a2d7-f09eabcb00b7.png)

신뢰 구간이 없는 회귀식을 넣고 싶다면 `se= FALSE`를 활용하면 된다
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(method=lm, se= FALSE)
  ```

비선형 회귀식을 넣고 싶다면 `geom_smooth(method="loess")`를 사용하면 된다  
`loess`는 _local polynomial regression_의 줄임말이다  
<br>
<br>

__class__ 의 __subcompact__ 만을 노리고 `geom_smooth`를 표시 할 수 있다
```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE)
```
![filter](https://user-images.githubusercontent.com/96481582/147467610-3384a3a5-1c81-4583-bd68-217d4b1e41db.png)
<br>
<br>

### 3-5 statistical transformations
__ggplot2__ 에 있는 `diamonds` 데이터를 이용해보도록 한다 
`diamonds`는 `price`, `carrot`, `color`, `clarity` 등의 정보가 있다
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut))
```
![Rplot](https://user-images.githubusercontent.com/96481582/147472621-66cec64c-2421-484b-bb7c-3c46073b3322.png)

>y축 __count__ 에 대한 정보는 __diamonds__ 에 존재하지 않는다

__bar_chart__ 가 새로운 변수(__count__)을 만든 것이다
이렇게 새로운 변수가 만들어지는 알고리즘을 __stat__ 이라고 한다
~~실제로 `geom_bar()` 을 잘 살펴보면 `stat_count`를 이용한다~~
<br>
```r
ggplot(data = diamonds) + 
  stat_count(mapping = aes(x = cut))
```
똑같은 결과를 내놓는다

이렇게 내부에 있는 stat을 명시적으로 써야되는 이유가 있는데   
다음과 같다   
<br>
<br>

__1.stat을 새롭게 바꾸고 싶을 수 있기 때문이다__

```r
demo <- tribble(
  ~cut,         ~freq,
  "Fair",       1610,
  "Good",       4906,
  "Very Good",  12082,
  "Premium",    13791,
  "Ideal",      21551
)
ggplot(data = demo) +
  geom_bar(mapping = aes(x = cut, y = freq), stat = "identity")
```
__geom_bar__ 의 stat을 __count__ 에서 __identity__ 로 변경하였다
이로써 __$y$ 의 frequency__ 값으로 표현 할 수 있게 됐다  
<br>
<br>

__2.  statistical transformation에 집중을 시키기기 위해서이다__
```r
ggplot(data = diamonds) + 
  stat_summary(
    mapping = aes(x = cut, y = depth),
    fun.min = min,
    fun.max = max,
    fun = median
  )
```
![Rplot01](https://user-images.githubusercontent.com/96481582/147493029-5348fe98-394c-4ba6-8268-b402e77c76ca.png)
~~근데 이게 왜 집중을 시키는 건지 모르겠다. 또 그게 왜 stat을 명시적으로 써야 하는지 잘 모르겠다~~  
<br>
<br>

### 3-6 Position adjustment
__bar_chart__ 를 색칠 할 수도 있다
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, colour = cut))
  ```
![Rplot02](https://user-images.githubusercontent.com/96481582/147493645-e3e6534c-153f-4a82-a480-fe52da3cd137.png)

```r 
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut))
```
__이러면 속까지 채워진다__  
<br>
__cut__ 이 아닌 다른 변수들로 채울 수도 있다
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity))
```
![Rplot03](https://user-images.githubusercontent.com/96481582/147494860-ffe4a266-9469-4ebe-b1f3-021f25d2df65.png)
<br>
__position adjustment__ 에 의해 bar들이 쌓아진다
다른 방식으로도 쌓기가 가능한데 `"Identity"`,`"Dodge"`, `"fill"`이 있다  
<br>

#### 3-6-1 "Identity"
`position = "identity"`는 실제 값에 대응하게 한다.
그래서 __bar chart__ 에는 좋지 못하다.
영역들이 겹칠 수 있기 때문이다.
이는 다음과 같이 확인 할 수 있다
```r
ggplot(data = diamonds, mapping = aes(x = cut, colour = clarity)) + 
  geom_bar(fill = NA, position = "identity")
  ```
![Rplot01](https://user-images.githubusercontent.com/96481582/147549691-9aef9b19-c755-4864-9df8-ee501b1ff547.png)
<br>
<br>

#### 3-6-2 "Dodge" 
`position = "dodge"`는 각각을 옆에 배치 한다.  
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "dodge")
  ```
   ![Rplot04](https://user-images.githubusercontent.com/96481582/147550288-19abf134-3ed6-4c9f-a625-5bf873b6b178.png)
<br>
<br>

#### 3-6-3 "Fill"
`position = "fill"`은 각각의 비율을 알기 쉽게 해준다.
```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill")
  ```
![Rplot05](https://user-images.githubusercontent.com/96481582/147550991-583dd36b-d785-4548-a2c7-4d09bc83c1a8.png)

### 3-7 Coordinate systems
```r
bar <- ggplot(data = diamonds) + 
  geom_bar(
    mapping = aes(x = cut, fill = cut), 
    show.legend = FALSE,
    width = 1
  ) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)


bar + coord_flip()
bar + coord_polar()
```
<br>

__1. bar + coord_ploar()에서는 theme(aspect.ratio =1) 가 theme(aspect.ratio=10392) 결과물은 달라지지 않는다. 심지어 없어도 말이다. 무슨 역할을 하지?__  

 aspect ratio는 가로 세로비이다. polor는 가로세로가 없으니까 영향을 안 받나 보다

bar + coord_flip() 같은 경우 flip 됐으니 세로가로비인거다 
<br>
<br>

__2. labs()가 하는 역할은?__ 
 x와 y, title의 이름을 지어준다
 ~~null이니까 있으나 마나~~
<br>
<br>

__3. 두 코드가 어떻게 구현 될까?__
```R
ggplot(mpg, aes(x = factor(1), fill = drv)) +
  geom_bar(width = 1) +
  coord_polar(theta = "y")
  ```

```r
ggplot(mpg, aes(x = factor(1), fill = drv)) +
  geom_bar(width = 1) +
  coord_polar()
  ```


첫번째 코드는 파이모양으로 나타나고 두번째 코드는 양궁모양이다. 
둘의 차이는 __coord_poloar()__ 에 __theta='y'__ 유무이다 


