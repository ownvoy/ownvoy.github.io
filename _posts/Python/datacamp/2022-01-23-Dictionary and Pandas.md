---

title: "Dictonaries and Pandas"
categories: datacamp
excerpt: Intermediate Python
toc: true
toc_sticky: true

date: 2022-01-23
---
## Dictonaries and Pandas

### Dictonaries

다음을 통해 리스트의 불편한 점을 봐 보자.  
나라와 수도에 관한 리스트를 만들고, 독일의 수도를 찾는 과정이다.   


```python
countries = ['spain', 'france', 'germany', 'norway']
capitals = ['madrid', 'paris', 'berlin', 'oslo']

ind_ger = countries.index('germany')

print(capitals[ind_ger])
```

    berlin
    
<br>

과정이 조금 복잡하다. 이를 dictonary를 통해 해결할 수 있다.  
dictonary는 `"key":"Value"`로 이루어져 있다.  


```python
europe = { 'spain':'madrid', 'france':'paris','germany':'berlin','norway':'oslo'}
print(europe)
```

    {'spain': 'madrid', 'france': 'paris', 'germany': 'berlin', 'norway': 'oslo'}
    
<br>

`europe`의 key들을 살펴보기 위해서는 `keys()` 메소드를 사용하면 된다.  


```python
print(europe.keys())
```

    dict_keys(['spain', 'france', 'germany', 'norway'])
    
<br>

독일의 수도 찾아보기


```python
print(europe["germany"])
```

    berlin
    
<br>

이탈리아에 대한 정보 추가하기


```python
europe['italy']='rome'
print(europe)
```

    {'spain': 'madrid', 'france': 'paris', 'germany': 'berlin', 'norway': 'oslo', 'italy': 'rome'}
    
<br>

호주에 대한 정보 삭제하기  


```python
europe = {'spain':'madrid', 'france':'paris', 'germany':'bonn',
          'norway':'oslo', 'italy':'rome', 'poland':'warsaw',
          'australia':'vienna' }

del(europe['australia'])
print(europe)
```

    {'spain': 'madrid', 'france': 'paris', 'germany': 'bonn', 'norway': 'oslo', 'italy': 'rome', 'poland': 'warsaw'}
    
<br>
딕셔너리에 딕셔너리를 추가할 수 있다.  


```python
europe = { 'spain': { 'capital':'madrid', 'population':46.77 },
           'france': { 'capital':'paris', 'population':66.03 },
           'germany': { 'capital':'berlin', 'population':80.62 },
           'norway': { 'capital':'oslo', 'population':5.084 } }

```
<br>

프랑스의 수도 찾기


```python
print(europe['france']['capital'])
```

    paris
    
<br>
<br>

### Pandas
데이터들을 rectangular data structure 로 만들기 위한 방법으로는 2D Numpy가 있을 수 있다. 그러나 numpy는 여러 class로 된 데이터들을 잘 다루지 못한다. 그래서 Pandas를 써보도록 해보자.   


```python
# Pre-defined lists
names = ['United States', 'Australia', 'Japan', 'India', 'Russia', 'Morocco', 'Egypt']
dr =  [True, False, False, False, True, True, True]
cpc = [809, 731, 588, 18, 200, 70, 45]

# Import pandas as pd
import pandas as pd

# Create dictionary my_dict with three key:value pairs: my_dict
my_dict = {'country':names,'drives_right':dr,'cars_per_cap':cpc}

# Build a DataFrame cars from my_dict: cars
cars = pd.DataFrame(my_dict)

# Print cars
print(cars)
```

             country  drives_right  cars_per_cap
    0  United States          True           809
    1      Australia         False           731
    2          Japan         False           588
    3          India         False            18
    4         Russia          True           200
    5        Morocco          True            70
    6          Egypt          True            45
    
<br>

앞에 0,1,2,,,6으로 되어 있는게 맘에 안든다.  
`index`메소드를 이용하여 row name을 지어주도록 해보자.  


```python
row_labels = ['US', 'AUS', 'JPN', 'IN', 'RU', 'MOR', 'EG']
cars.index= row_labels
print(cars)
```

               country  drives_right  cars_per_cap
    US   United States          True           809
    AUS      Australia         False           731
    JPN          Japan         False           588
    IN           India         False            18
    RU          Russia          True           200
    MOR        Morocco          True            70
    EG           Egypt          True            45
    
<br>

매번 딕셔너리를 만들어서 Data Frame으로 만드는 것은 여간 쉬운 일이 아니다.  
csv 파일을 `pd.read_csv()`를 통해 <span style="color:orange">Pandas Data Frame</span>으로 만들어 보자.  


```python
import pandas as pd
cars= pd.read_csv('cars.csv')
print(cars)
```

      Unnamed: 0  cars_per_cap        country  drives_right
    0         US           809  United States          True
    1        AUS           731      Australia         False
    2        JAP           588          Japan         False
    3         IN            18          India         False
    4         RU           200         Russia          True
    5        MOR            70        Morocco          True
    6         EG            45          Egypt          True
    
<br>

옆에 있는 0,1,2,,,,6 이 맘에 안 든다. `index_col = 0`을 설정함으로써 없애보도록 하자.  


```python
cars = pd.read_csv('cars.csv', index_col = 0)
print(cars)
```

         cars_per_cap        country  drives_right
    US            809  United States          True
    AUS           731      Australia         False
    JAP           588          Japan         False
    IN             18          India         False
    RU            200         Russia          True
    MOR            70        Morocco          True
    EG             45          Egypt          True
    
<br>
<br>

### Pandas data select
square brackets을 통해 select 해보자.  
single square brackets를 쓰면 Pandas Series가 나오고
double square brackets를 쓰면 Pandas DataFrame이 나온다.  


```python
print(cars['country'])
```

    US     United States
    AUS        Australia
    JAP            Japan
    IN             India
    RU            Russia
    MOR          Morocco
    EG             Egypt
    Name: country, dtype: object
    


```python
print(cars[['country']])
```

               country
    US   United States
    AUS      Australia
    JAP          Japan
    IN           India
    RU          Russia
    MOR        Morocco
    EG           Egypt
    
<br>

3개의 observation 보기
```python
print(cars[0:3])
```

         cars_per_cap        country  drives_right
    US            809  United States          True
    AUS           731      Australia         False
    JAP           588          Japan         False
    

4,5,6번쨰 observation 보기
```python
print(cars[3:6])
```

         cars_per_cap  country  drives_right
    IN             18    India         False
    RU            200   Russia          True
    MOR            70  Morocco          True
    
<br>

<span style="color:orange">loc</span>과  <span style="color:orange">iloc</span>을 쓰면 거의 모든 data selection을 할 수 있다.  
<span style="color:orange">loc</span>은 문자 기반으로 row와 column을 뽑는 것이며, <span style="color:orange">iloc</span>은 숫자 기반으로 row와 column을 뽑는다.


```python
print(cars.iloc[2])
```

    cars_per_cap      588
    country         Japan
    drives_right    False
    Name: JAP, dtype: object
    


```python
print(cars.loc[["AUS","EG"]])
```

         cars_per_cap    country  drives_right
    AUS           731  Australia         False
    EG             45      Egypt          True
    


```python
print(cars.iloc[[3, 4], 0])
```

    IN     18
    RU    200
    Name: cars_per_cap, dtype: int64
    


```python
# Print sub-DataFrame
print(cars.loc[["RU","MOR"],["country","drives_right"]])
```

         country  drives_right
    RU    Russia          True
    MOR  Morocco          True
    
<br>

한 개의 column만을 뽑을 수도 있다.  


```python
print(cars.loc[:, 'country'])
```

    US     United States
    AUS        Australia
    JAP            Japan
    IN             India
    RU            Russia
    MOR          Morocco
    EG             Egypt
    Name: country, dtype: object
    


```python
print(cars.loc[:,['drives_right']])
```

         drives_right
    US           True
    AUS         False
    JAP         False
    IN          False
    RU           True
    MOR          True
    EG           True
    
