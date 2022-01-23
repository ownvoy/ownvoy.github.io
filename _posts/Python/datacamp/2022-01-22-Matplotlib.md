---

title: "Matplotlib"
categories: datacamp
excerpt: Intermediate python
toc: true
toc_sticky: true

date: 2022-01-23
---

## Matplotlib

일단 `gapminder` 가져오기 뒤에서 배울 내용을 앞으로 당겨썼다.


```python
import pandas as pd
```


```python
gapminder = pd.read_csv("C:/Users/wj527/Python/Gapminder.csv")

```
<br>


```python
print(gapminder.shape)
```

    (142, 7)
    
<br>


```python
gapminder
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>country</th>
      <th>year</th>
      <th>population</th>
      <th>cont</th>
      <th>life_exp</th>
      <th>gdp_cap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>Afghanistan</td>
      <td>2007</td>
      <td>31889923.0</td>
      <td>Asia</td>
      <td>43.828</td>
      <td>974.580338</td>
    </tr>
    <tr>
      <th>1</th>
      <td>23</td>
      <td>Albania</td>
      <td>2007</td>
      <td>3600523.0</td>
      <td>Europe</td>
      <td>76.423</td>
      <td>5937.029526</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35</td>
      <td>Algeria</td>
      <td>2007</td>
      <td>33333216.0</td>
      <td>Africa</td>
      <td>72.301</td>
      <td>6223.367465</td>
    </tr>
    <tr>
      <th>3</th>
      <td>47</td>
      <td>Angola</td>
      <td>2007</td>
      <td>12420476.0</td>
      <td>Africa</td>
      <td>42.731</td>
      <td>4797.231267</td>
    </tr>
    <tr>
      <th>4</th>
      <td>59</td>
      <td>Argentina</td>
      <td>2007</td>
      <td>40301927.0</td>
      <td>Americas</td>
      <td>75.320</td>
      <td>12779.379640</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>137</th>
      <td>1655</td>
      <td>Vietnam</td>
      <td>2007</td>
      <td>85262356.0</td>
      <td>Asia</td>
      <td>74.249</td>
      <td>2441.576404</td>
    </tr>
    <tr>
      <th>138</th>
      <td>1667</td>
      <td>West Bank and Gaza</td>
      <td>2007</td>
      <td>4018332.0</td>
      <td>Asia</td>
      <td>73.422</td>
      <td>3025.349798</td>
    </tr>
    <tr>
      <th>139</th>
      <td>1679</td>
      <td>Yemen, Rep.</td>
      <td>2007</td>
      <td>22211743.0</td>
      <td>Asia</td>
      <td>62.698</td>
      <td>2280.769906</td>
    </tr>
    <tr>
      <th>140</th>
      <td>1691</td>
      <td>Zambia</td>
      <td>2007</td>
      <td>11746035.0</td>
      <td>Africa</td>
      <td>42.384</td>
      <td>1271.211593</td>
    </tr>
    <tr>
      <th>141</th>
      <td>1703</td>
      <td>Zimbabwe</td>
      <td>2007</td>
      <td>12311143.0</td>
      <td>Africa</td>
      <td>43.487</td>
      <td>469.709298</td>
    </tr>
  </tbody>
</table>
<p>142 rows × 7 columns</p>
</div>

<br>



```python
life_exp= gapminder.loc[:,['life_exp']]
gdp_cap = gapminder.loc[:,['gdp_cap']]
pop = gapminder.loc[:,['population']]
print(life_exp)
print(gdp_cap)
```

         life_exp
    0      43.828
    1      76.423
    2      72.301
    3      42.731
    4      75.320
    ..        ...
    137    74.249
    138    73.422
    139    62.698
    140    42.384
    141    43.487
    
    [142 rows x 1 columns]
              gdp_cap
    0      974.580338
    1     5937.029526
    2     6223.367465
    3     4797.231267
    4    12779.379640
    ..            ...
    137   2441.576404
    138   3025.349798
    139   2280.769906
    140   1271.211593
    141    469.709298
    
    [142 rows x 1 columns]
    

<br>

### plot 과 scatter point

이제 준비가 됐고 plot를 써보도록 하자.  


```python
import matplotlib.pyplot as plt
```


```python
plt.plot(gdp_cap,life_exp)
plt.show()
```


    
![output_8_0](https://user-images.githubusercontent.com/96481582/150666868-34054bc6-7c00-4e51-971b-dacaee0e3674.png)
    


두 variable의 correlation을 보기 위해서는 scatter가 더 나을 수 있다.  


```python
plt.scatter(gdp_cap,life_exp)
plt.show()
```


    
![output_10_0](https://user-images.githubusercontent.com/96481582/150666871-d8fbc0e6-c945-4412-ae7b-3a17ba934967.png)
    


GDP per capita에  logarithmic scale를 쓰면 correlation을 더 선명하게 볼 수 있다.  


```python
plt.scatter(gdp_cap, life_exp)
plt.xscale('log')
plt.show()
```


    
![output_12_0](https://user-images.githubusercontent.com/96481582/150666872-77db772c-b4a1-47c6-82b7-75a05e587a67.png)
    


양의 상관관계를 보이는 것을 알 수 있다.  
이번에는 life expancy와 population의 관계를 살펴보도록 하자.  


```python
plt.scatter(pop, life_exp)
plt.show()
```


    
![output_14_0](https://user-images.githubusercontent.com/96481582/150666873-a10c03da-05f9-4e41-af53-e8c44b3147bd.png)
    


별다른 relationship이 없다.  
<br>

### customization

이제 customization을 해 볼 것이다.  
처음으로 x축과 y축의 이름, 제목을 붙여 보도록 하자.  
이를 위해서는 `matplotlib.pyplot` 에 있는 `xlabel()`, `ylabel()`, `title()`를 써 보자.  


```python
plt.scatter(gdp_cap, life_exp)
plt.xscale('log')
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')
plt.show()
```

    
![output_17_0](https://user-images.githubusercontent.com/96481582/150666874-f88bbedc-777f-4e60-938c-3ce32d1d8f06.png)
    


x 축에 써져 있는 `1000`, `10000`, `100000`을 `xticks()`을 이용해서 `1k`, `10k`,`100k`로 바꿀 수도 있다.  


```python
plt.scatter(gdp_cap, life_exp)
plt.xscale('log')
tick_val = [1000, 10000, 100000]
tick_lab = ['1k', '10k', '100k']
plt.xticks(tick_val,tick_lab)
plt.show()
```


    
![output_19_0](https://user-images.githubusercontent.com/96481582/150666875-14de6fb3-8aec-45bc-9768-5bd79f338bb5.png)
    


지금까지는 일정한 크기의 blue dot인데, 이제는 population에 따라 사이즈를 다르게 해보자.


```python
import numpy as np
np_pop= np.array(pop)
np_pop = np_pop * 0.0000002
plt.scatter(gdp_cap, life_exp, s = np_pop)
plt.xscale('log') 
plt.xlabel('GDP per Capita [in USD]')
plt.ylabel('Life Expectancy [in years]')
plt.title('World Development in 2007')
plt.xticks([1000, 10000, 100000],['1k', '10k', '100k'])
plt.show()
```


    
![output_21_0](https://user-images.githubusercontent.com/96481582/150666876-07769a4b-9f98-4931-8b2b-82053d38837f.png)
    
<br>

### histogram

`life_exp`는 나라마다 기대수명에 대한 data를 가지고 있다.  
histogram을 사용해서 이를 보도록 하자.  


```python
plt.hist(life_exp)
plt.show()
```


    
![output_23_0](https://user-images.githubusercontent.com/96481582/150666877-98a961bb-98fc-40b2-a485-63ca635b7f7f.png)
    


histogram의 bin은 10개로 default로 정해져있다.  
이를 따로 bin argument를 사용하여 설정할 수도 있다.  


```python
plt.hist(life_exp,bins=5)
plt.show()
```


    
![output_25_0](https://user-images.githubusercontent.com/96481582/150666878-d35083a3-2d88-4760-8b4c-31f59ba50b4b.png)
    



```python
plt.hist(life_exp,bins=20)
plt.show()
```


    
![output_26_0](https://user-images.githubusercontent.com/96481582/150666879-b63333a5-394c-45f9-a859-2adade2d07d2.png)
    

