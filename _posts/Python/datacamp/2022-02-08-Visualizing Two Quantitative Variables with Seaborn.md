---

title: "Visualizing Two Quantitative Variables with Seaborn"
categories: datacamp
excerpt: Introduction to Data Visualization with Seaborn
toc: true
toc_sticky: true

date: 2022-02-08
---

### Introduction of Seaborn

Seborn library는 visualization을 간편하게 해준다. 또한 pandas data structure을 갖고 용이하게 작업을 할 수 있으며, matplotlib에 기반을 두고 있다.  


```python.....

import seaborn as sns
```

다음과 같이 불러 올 수 있다.  pandas data frame을 다루기 전에 list를 갖고 plot들을 만들어 보도록 하자.  


```python
import pandas as pd
import matplotlib.pyplot as plt

countries = pd.read_csv("countries-of-the-world.csv")
young_survey = pd.read_csv("young-people-survey-responses.csv")
student = pd.read_csv("student.csv")
mpg= pd.read_csv("mpg.csv")

gdp=countries["GDP ($ per capita)"]
phones=countries["Phones (per 1000)"]
region=countries["Region"]

gdp_list = gdp.values.tolist()
phones_list = phones.values.tolist()
region_list = region.values.tolist()

plt.style.use("ggplot")

```
<br>

`gdp`와 `phones`의 관계를 scatter point를 통해 살펴보도록 하자.  


```python
sns.scatterplot(x=gdp_list, y=phones_list)

plt.show()
```


    
![output_5_0](https://user-images.githubusercontent.com/96481582/152991634-ca2eedb3-9b77-43b4-9694-a273ce2bb8db.png)
    

<br>

이번에는 list를 가지고 countplot를 만들어보자. 이 `countries`는 227개 국가에 대한 정보를 가지고 있다. 우리가 알아볼 것은 얼마나 많은 나라가 어떤 region에 얼만큼 속해있는지 이다.


```python
sns.countplot(y=region_list)
plt.show
```



    
![output_7_1](https://user-images.githubusercontent.com/96481582/152991636-720571ce-63ec-43ef-8360-39a8b67449cd.png)
    

<br>

이제는 pandas dataframe을 가지고 plot를 만들어 볼 것이다.  `young_survey`은 학생들의 선호에 대한 정보를 가지고 있는데 예를 들어 `Spiders`는 거미에 대한 선호를 나타낸다. 5는 극도로 싫어하는 거고 1은 극도로 좋아하는 거다.  


```python
sns.countplot(x='Spiders', data=young_survey)
plt.show()
```


    
![output_9_0](https://user-images.githubusercontent.com/96481582/152991639-17c69106-da10-4275-bd08-a9957e8d60b6.png)
    
<br>
<br>

###  Visualizing Two Quantitative Variables

색칠을 통해 제 3의 정보를 추가할 수 있다. 이번에 그릴 plot은 scatter plot으로  `young_survey`를 사용할 것이다. x 축을 결석의 수(`"absences"`)로 y 축을 최종 성적(`"G3"`)으로 그린 다음, `location`으로 색칠 해보자.  


```python
sns.scatterplot(x= "absences", y= "G3",data=student, hue= "location")
plt.show()
```


    
![output_11_0](https://user-images.githubusercontent.com/96481582/152991642-bf57cb9d-6d1f-4c66-9600-42c675fc408a.png)
    

<br>

hue의 order를 바꿀 수도 있다.  


```python
sns.scatterplot(x="absences", y="G3", 
                data=student, 
                hue="location",hue_order=["Rural","Urban"])
plt.show()
```


    
![output_13_0](https://user-images.githubusercontent.com/96481582/152991644-0f8e04e2-7426-4b91-bd0d-afe5f1ad6555.png)
    
<br>


색깔을 직접 지정할 수도 있다.  


```python
palette_colors = {"Rural": "green", "Urban": "blue"}

sns.scatterplot(x="absences", y="G3", 
                data=student, 
                hue="location",hue_order=["Rural","Urban"],
                palette=palette_colors)
plt.show()
```


    
![output_15_0](https://user-images.githubusercontent.com/96481582/152991646-f4b25c0d-9250-41e3-be9f-1222fd75c696.png)
    

<br>

결석이 많은 학생일 수록 final grade(G3)이 낮은 경향이 있는 것을 알 수 있다. 이제 subplot를 만들어 보도록 하자.  


```python
sns.relplot(x="absences", y="G3", 
                data=student ,kind= "scatter",
                row="study_time")

plt.show()
```


    
![output_17_0](https://user-images.githubusercontent.com/96481582/152991647-f4aa4c05-1f23-4a47-8f01-acd32af8e701.png)
    
<br>


two factor subplot도 가능하다.  first semester grade ("G1")을 잘 맞으면 their final grade ("G3")가 높을까?  extra educational support from their school ("schoolsup") 이나 from their family ("famsup")가 성적에 영향이 있을까?


```python
sns.relplot(x="G1", y="G3", 
            data=student,
            kind="scatter", 
            col="schoolsup",
            col_order=["yes", "no"],
            row="famsup",
            row_order=["yes","no"])

plt.show()
```


    
![output_19_0](https://user-images.githubusercontent.com/96481582/152991649-80321ab0-753d-4240-9633-7b708d15528d.png)
    

<br>

이번에 다루어볼 dataset은 `mpg`로 차에 대한 정보로 생산년도, 마력, 생산국, mpg에 대한 정보를 담고 있다. 마력과 fuel efficiency(mpg)에 대한 관계를 봐보도록 하자. 그리고 이 관계가 cylinders의 숫자에 따라 바뀌는지 말이다.  


```python
sns.relplot(x="horsepower", y="mpg", 
            data=mpg, kind="scatter", 
            size="cylinders",hue="cylinders")

plt.show()
```


    
![output_21_0](https://user-images.githubusercontent.com/96481582/152991651-10d06d49-76ae-4d6d-b878-3526d913f8f1.png)
    

<br>

"acceleration" 과 its fuel efficiency ("mpg")의 관계를 봐보도록 하자. 이들의 관계가 country of origin ("origin")에 따라 변하는지도 말이다.  


```python
sns.relplot(x="acceleration",y="mpg",data=mpg, kind="scatter",
            hue="origin", style="origin")

plt.show()
```


    
![output_23_0](https://user-images.githubusercontent.com/96481582/152991607-22199570-f5d0-4a8b-a90b-368443e3b49f.png)
    
<br>


이번에는 시간에 따라 mpg가 어케 변했는지 보자 시간의 흐름은 line plot으로 나타내면 좋다.  


```python
sns.relplot(x="model_year",y="mpg",data=mpg, kind="line")

plt.show()
```


    
![output_25_0](https://user-images.githubusercontent.com/96481582/152991610-d165a4c9-3001-4b3f-aa5d-804a171c3a3e.png)
    
<br>


위 그림은 confidence interval(신뢰구간)의 평균을 shaded area로 표현한 것이다. 이번에는 standard deviation 내에서 shaded area를 plot해보도록 하자. `ci = sd`를 추가해주면 된다.  


```python
sns.relplot(x="model_year", y="mpg",
            data=mpg, kind="line",ci="sd")

plt.show()
```


    
![output_27_0](https://user-images.githubusercontent.com/96481582/152991612-f2b793cd-4166-4b4b-a6fa-7a39e896a619.png)
    
<br>


이번에는 mpg 말고 마력이 바꿨는지 봐보자. shaded area를 없게 하려면 `ci=None`을 하면 된다.  


```python
sns.relplot(x="model_year", y="horsepower",
            data=mpg,kind="line",ci=None)

plt.show()
```


    
![output_29_0](https://user-images.githubusercontent.com/96481582/152991613-5afb32b0-dba9-4f98-8df3-a0ad8211c49c.png)
    

<br>

`origin`에 따라 line style과 color를 다르게 해보자.  


```python
sns.relplot(x="model_year", y="horsepower", 
            data=mpg, kind="line", 
            ci=None,style='origin',hue='origin')

plt.show()
```


    
![output_31_0](https://user-images.githubusercontent.com/96481582/152991617-28b23eb8-c333-482d-be0e-8e39e8a08e9b.png)
    

<br>

마커를 추가하고, 점선을 그냥 선으로 바꿀 수도 있다.  


```python
sns.relplot(x="model_year", y="horsepower", 
            data=mpg, kind="line", 
            ci=None, style="origin", 
            hue="origin",markers=True, dashes=False)

plt.show()
```


    
![output_33_0](https://user-images.githubusercontent.com/96481582/152991620-22d6cec5-830d-4dd5-92d1-666907a0835c.png)
    

