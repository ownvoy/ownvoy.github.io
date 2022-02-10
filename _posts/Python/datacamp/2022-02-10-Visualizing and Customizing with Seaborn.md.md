---

title: "Visualizing and Customizing with Seaborn"
categories: datacamp
excerpt: Introduction to Data Visualization with Seaborn
toc: true
toc_sticky: true

date: 2022-02-10
---


## Visualizing a Categorical and a Quantitative Variable
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

countries = pd.read_csv("countries-of-the-world.csv")
young_survey = pd.read_csv("young-people-survey-responses.csv")
student = pd.read_csv("student.csv")
mpg= pd.read_csv("mpg.csv")
plt.style.use("ggplot")
```
<br>

### Count Plot and Bar Plot
인터넷을 아이들이 얼마나 쓰는지에 대해 count plot를 통해 알아 보자.  


```python
sns.catplot(x="Internet usage", data= young_survey, kind='count')
plt.show()
```


    
![output_2_0](https://user-images.githubusercontent.com/96481582/153471348-e3799567-0f83-41fa-8d1f-e99e8b895c35.png)
    
<br>


variable name이 겹쳐서 이런식으로 수정할 수 있다.  


```python
sns.catplot(y="Internet usage", data= young_survey, kind='count')
plt.show()
```


    
![output_4_0](https://user-images.githubusercontent.com/96481582/153471350-00d34219-ba9c-4464-9a5c-527dea573c00.png)
    

<br>

또 다른 변수 `"Gender"`를 추가함으로써 plot를 세분화 할 수 있다.  


```python
sns.catplot(col="Gender", y="Internet usage", data= young_survey,
            kind="count")
plt.show()
```


    
![output_6_0](https://user-images.githubusercontent.com/96481582/153471353-b3c5c31c-7629-4ac2-acf3-c10ebccc02e6.png)
    

<br>

bar plot은 category에 따라 quantative variable의 평균을 알려준다.  seaborn은 자동적으로 95% 신뢰구간을 준다.  

학생들이 몇 percentage 수학에 관심 있는지 또 그것이 성별에 따라 다른지 bar plot를 통해 보자.


```python
sns.catplot(col="Gender", y="Mathematics", data= young_survey,
            kind="bar")
plt.show()
```


    
![output_8_0](https://user-images.githubusercontent.com/96481582/153471354-6cbb61b2-8645-4b90-906f-4a4d3dcb1ec4.png)
    

<br>

bar plot를 통해 study time 과 G3의 평균의 관계를 보도록 하자.  


```python
category_order = ["<2 hours", 
                  "2 to 5 hours", 
                  "5 to 10 hours", 
                  ">10 hours"]

sns.catplot(x="study_time", y="G3",
            data=student,
            kind="bar",
            order=category_order)

plt.show()
```


    
![output_10_0](https://user-images.githubusercontent.com/96481582/153471356-515447c5-b64d-470b-86e0-604dc562f78a.png)
    

<br>

ci를 지울수도 있다.  


```python
category_order = ["<2 hours", 
                  "2 to 5 hours", 
                  "5 to 10 hours", 
                  ">10 hours"]

sns.catplot(x="study_time", y="G3",
            data=student,
            kind="bar",
            order=category_order,ci=None)

plt.show()
```


    
![output_12_0](https://user-images.githubusercontent.com/96481582/153471357-ff2f0700-28fe-43b7-90e7-181f7892e4c4.png)
    

<br>
<br>

### Box Plot and Point Plot
box plot으로도 이를 표현할 수 있다.  


```python
sns.catplot(x="study_time", y="G3",data= student, 
            kind="box" ,
            order= category_order )

plt.show()
```


    
![output_14_0](https://user-images.githubusercontent.com/96481582/153471359-eb5ffc43-d53b-49d5-b2ec-45360e4bd4a0.png)
    

<br>

`0.0`에 표시된 것은 `outlier`인데 `sym=""`를 통해 이를 없앨 수 있다.  


```python
sns.catplot(x="study_time", y= "G3", 
            data= student, 
            kind='box',
            hue="location",
            sym="")
plt.show()
```


    
![output_16_0](https://user-images.githubusercontent.com/96481582/153471361-5e0f8d64-a9e1-40f9-894c-2c65337dcb76.png)
    

<br>

whiskers의 길이를 조절 할 수도 있다. IQR의 0.5배 수염을 그려보자.  


```python
sns.catplot(x="study_time", y= "G3", 
            data= student, 
            kind='box',
            hue="location",
            sym="",
            whis= 0.5)
plt.show()
```


    
![output_18_0](https://user-images.githubusercontent.com/96481582/153471362-c7819a15-9642-4ccb-88de-880088b6f83a.png)
    

<br>

whiskers를 5퍼센트에서 95퍼센트까지 표현 하면 다음과 같다.  


```python

sns.catplot(x="study_time", y= "G3", 
            data= student, 
            kind='box',
            hue="location",
            sym="",
            whis= [5,95])
plt.show()
```


    
![output_20_0](https://user-images.githubusercontent.com/96481582/153471298-139dcc15-224f-46bc-b574-fe03b540f0d4.png)
    

<br>

point plots은 각 categorical 별로 quantitative variable의 mean을 보여준다. vertival bar은 mean의 95% confidence intervals 를 보여준다.   


```python
sns.catplot(x="famrel",y= "absences", data= student, kind="point")
plt.show()
```


    
![output_22_0](https://user-images.githubusercontent.com/96481582/153471307-d8a14ffb-6fd0-4fe4-8b92-c134f8a7fd2a.png)
    

<br>

끝에 cap을 추가할 수도 있다.  


```python
sns.catplot(x="famrel", y="absences",
			data=student,
            kind="point",capsize=0.2)
        
# Show plot
plt.show()
```


    
![output_24_0](https://user-images.githubusercontent.com/96481582/153471310-f296d1ef-30cc-4c99-9490-68f39fa5dc5b.png)
    

<br>

point를 join하는 line을 제거할 수도 있다.  


```python
sns.catplot(x="famrel", y="absences",
			data=student,
            kind="point",
            capsize=0.2,
            join= False)
        
plt.show()
```


    
![output_26_0](https://user-images.githubusercontent.com/96481582/153471311-847042d0-60c6-4af9-ad22-607ae15d03c3.png)
    

<br>

다음과 같이 subgroup을 만들 수 있다.  


```python
sns.catplot(x= "romantic", y= "absences", 
            data= student,
            kind= 'point',
            hue="school")

plt.show()
```


    
![output_28_0](https://user-images.githubusercontent.com/96481582/153471313-c5461a30-cd66-46e4-88e9-d842c47aa9ec.png)
    

<br>

위에 했던거처럼 ci를 없에려면 `ci=None`를 해주면 된다.  


```python
sns.catplot(x= "romantic", y= "absences", 
            data= student,
            kind= 'point',
            hue="school",
            ci= None)

plt.show()
```


    
![output_30_0](https://user-images.githubusercontent.com/96481582/153471317-04b586ee-1780-422b-9fb2-2d670203ee28.png)
    
<br>


`numpy`를 이용해 평균 대신에 median을 나타낼수도 있다.  


```python

from numpy import median

sns.catplot(x="romantic", y="absences",
			data=student,
            kind="point",
            hue="school",
            ci=None,
            estimator=median)

plt.show()
```


    
![output_32_0](https://user-images.githubusercontent.com/96481582/153471318-116ca58d-714d-47ac-b181-184150e7a577.png)
    

<br>
<br>

## Customizing
### Changing Styles and Scales


`sns.set_style()`를 이용해여 위의 그래프의 style를 바꿀 수 있다.  


```python
sns.set_style("whitegrid")

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_34_0](https://user-images.githubusercontent.com/96481582/153471319-61bf7a28-cadc-4503-950b-8854f1a6c06f.png)
    

<br>

다음과 같이 palette를 설정하여 bar의 색을 정할 수 있다.  


```python
sns.set_style('whitegrid')
sns.set_palette('Purples')

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_36_0](https://user-images.githubusercontent.com/96481582/153471320-9ec1adb7-c67e-49e0-bb1c-a235baa8953a.png)
    



```python
sns.set_style('whitegrid')
sns.set_palette('RdBu')

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_37_0](https://user-images.githubusercontent.com/96481582/153471325-7908f3dc-1a32-48be-9c65-1a9775d6d53b.png)
    

<br>

scale을 설정할 수도 있다.  `paper`,`notebook`,`talk`,`poster`순으로 scale이 커진다.  


```python
sns.set_style('whitegrid')
sns.set_palette('RdBu')
sns.set_context("paper")

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_39_0](https://user-images.githubusercontent.com/96481582/153471329-bb5445ce-cc72-4700-817b-b300bee74125.png)
    

<br>

```python
sns.set_style('whitegrid')
sns.set_palette('RdBu')
sns.set_context("notebook")

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_40_0](https://user-images.githubusercontent.com/96481582/153471332-415a940a-a147-498e-bedf-018cef0703ba.png)
    

<br>

```python
sns.set_style('whitegrid')
sns.set_palette('RdBu')
sns.set_context("poster")

sns.catplot(x="Parents' advice", 
            data=young_survey, 
            kind="count")

plt.show()
```


    
![output_41_0](https://user-images.githubusercontent.com/96481582/153471334-72e8cd6d-6fb7-4d2c-b70a-370df613ae73.png)
    
<br>
<br>

### Adding Titles and Labels

Seaborn's plot functions 은 <span style="color:violet">FacetGrids</span> 혹은 <span style="color:orange">AxesSubplots</span>을 만든다. <span style="color:violet">"relplot()","catplot()"</span>이 subplot을 만든것을 상기해보자. 이는 <span style="color:violet">FacetGrids</span> objects임을 뜻한다. 반대로 single-type plot functions <span style="color:orange">"scatterplot()" ,"countplot()"</span> 같은 single-type plot functions는 single <span style="color:orange">AxesSubplot</span> object를 만든다. 


```python
g = sns.relplot(x="weight", 
                y="horsepower", 
                data=mpg,
                kind="scatter")

type_of_g = type(g)

print(type_of_g)
```

    <class 'seaborn.axisgrid.FacetGrid'>
    


<br>


제목은 `fig.suptitle`를 통해 작성할 수 있다.  


```python
sns.set_context("notebook")

g = sns.relplot(x="weight", 
                y="horsepower", 
                data=mpg,
                kind="scatter")

g.fig.suptitle("Car Weight vs. Horsepower")

plt.show()
```


    
![output_45_0](https://user-images.githubusercontent.com/96481582/153471338-7e14d4b1-9132-4e08-8dd5-f6b285998334.png)
    
<br>


이제 AxesSubplot의 제목을 짓는 법을 살펴보도록 하겠다. 제목을 짓기 위해서는 `set_title()`를 사용하면 된다.  


```python

g = sns.lineplot(x="model_year", y="mpg", 
                 data=mpg,
                 hue="origin")

g.set_title("Average MPG Over Time")

plt.show()
```


    
![output_47_0](https://user-images.githubusercontent.com/96481582/153471340-c81c3a26-0e76-43e7-b5d6-24c576ee57c0.png)
    
<br>


x라벨과 y라벨의 이름을 지을 수도 있다.  


```python
g = sns.lineplot(x="model_year", y="mpg",
                 data=mpg,
                 hue="origin")

g.set(xlabel="Car Model Year",ylabel="MPG")

plt.show()
```


    
![output_49_0](https://user-images.githubusercontent.com/96481582/153471341-4d3b3cdf-80ad-4d08-a8f4-fde85bf31512.png)
    

