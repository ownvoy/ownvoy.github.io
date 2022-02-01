---

title: "Transforming and Aggregating df"
categories: datacamp
excerpt: Data Manipulation with pandas
toc: true
toc_sticky: true

date: 2022-01-29
---

## Slicing and Indexing Df


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
temperatures = pd.read_csv("temperatures.csv",index_col=0)
print(temperatures)
```

                 date     city        country  avg_temp_c
    0      2000-01-01  Abidjan  Côte D'Ivoire      27.293
    1      2000-02-01  Abidjan  Côte D'Ivoire      27.685
    2      2000-03-01  Abidjan  Côte D'Ivoire      29.061
    3      2000-04-01  Abidjan  Côte D'Ivoire      28.162
    4      2000-05-01  Abidjan  Côte D'Ivoire      27.547
    ...           ...      ...            ...         ...
    16495  2013-05-01     Xian          China      18.979
    16496  2013-06-01     Xian          China      23.522
    16497  2013-07-01     Xian          China      25.251
    16498  2013-08-01     Xian          China      24.528
    16499  2013-09-01     Xian          China         NaN
    
    [16500 rows x 4 columns]
    
<br>

### Explicit indexes

`.set_index()`를 통해 variable을 index로 만들 수 있다.  


```python
temperatures_ind = temperatures.set_index("city")
print(temperatures_ind)
```

                   date        country  avg_temp_c
    city                                          
    Abidjan  2000-01-01  Côte D'Ivoire      27.293
    Abidjan  2000-02-01  Côte D'Ivoire      27.685
    Abidjan  2000-03-01  Côte D'Ivoire      29.061
    Abidjan  2000-04-01  Côte D'Ivoire      28.162
    Abidjan  2000-05-01  Côte D'Ivoire      27.547
    ...             ...            ...         ...
    Xian     2013-05-01          China      18.979
    Xian     2013-06-01          China      23.522
    Xian     2013-07-01          China      25.251
    Xian     2013-08-01          China      24.528
    Xian     2013-09-01          China         NaN
    
    [16500 rows x 3 columns]
    
<br>

`city`가 index가 된 것을 볼 수 있다. 이를 취소하려면 `.reset_index()`를 사용하면 된다. `city`를 없애고 싶다면 `drop = True`를 사용하면 된다.  


```python
print(temperatures_ind.reset_index())
```

              city        date        country  avg_temp_c
    0      Abidjan  2000-01-01  Côte D'Ivoire      27.293
    1      Abidjan  2000-02-01  Côte D'Ivoire      27.685
    2      Abidjan  2000-03-01  Côte D'Ivoire      29.061
    3      Abidjan  2000-04-01  Côte D'Ivoire      28.162
    4      Abidjan  2000-05-01  Côte D'Ivoire      27.547
    ...        ...         ...            ...         ...
    16495     Xian  2013-05-01          China      18.979
    16496     Xian  2013-06-01          China      23.522
    16497     Xian  2013-07-01          China      25.251
    16498     Xian  2013-08-01          China      24.528
    16499     Xian  2013-09-01          China         NaN
    
    [16500 rows x 4 columns]
    


```python
print(temperatures_ind.reset_index(drop = True))
```

                 date        country  avg_temp_c
    0      2000-01-01  Côte D'Ivoire      27.293
    1      2000-02-01  Côte D'Ivoire      27.685
    2      2000-03-01  Côte D'Ivoire      29.061
    3      2000-04-01  Côte D'Ivoire      28.162
    4      2000-05-01  Côte D'Ivoire      27.547
    ...           ...            ...         ...
    16495  2013-05-01          China      18.979
    16496  2013-06-01          China      23.522
    16497  2013-07-01          China      25.251
    16498  2013-08-01          China      24.528
    16499  2013-09-01          China         NaN
    
    [16500 rows x 3 columns]
    

<br>

모스크바와 상트페테르부르크에 대한 정보를 뽑고 싶다고 하자. 옛날 같았으면 이렇게 해야했다.  


```python
cities = ["Moscow", "Saint Petersburg"]
print(temperatures[temperatures["city"].isin(cities)])
```

                 date              city country  avg_temp_c
    10725  2000-01-01            Moscow  Russia      -7.313
    10726  2000-02-01            Moscow  Russia      -3.551
    10727  2000-03-01            Moscow  Russia      -1.661
    10728  2000-04-01            Moscow  Russia      10.096
    10729  2000-05-01            Moscow  Russia      10.357
    ...           ...               ...     ...         ...
    13360  2013-05-01  Saint Petersburg  Russia      12.355
    13361  2013-06-01  Saint Petersburg  Russia      17.185
    13362  2013-07-01  Saint Petersburg  Russia      17.234
    13363  2013-08-01  Saint Petersburg  Russia      17.153
    13364  2013-09-01  Saint Petersburg  Russia         NaN
    
    [330 rows x 4 columns]
    
<br>

`.loc`를 사용함으로써 보다 직관적으로 가능하다.  


```python
print(temperatures_ind.loc[["Moscow", "Saint Petersburg"]])
```

                            date country  avg_temp_c
    city                                            
    Moscow            2000-01-01  Russia      -7.313
    Moscow            2000-02-01  Russia      -3.551
    Moscow            2000-03-01  Russia      -1.661
    Moscow            2000-04-01  Russia      10.096
    Moscow            2000-05-01  Russia      10.357
    ...                      ...     ...         ...
    Saint Petersburg  2013-05-01  Russia      12.355
    Saint Petersburg  2013-06-01  Russia      17.185
    Saint Petersburg  2013-07-01  Russia      17.234
    Saint Petersburg  2013-08-01  Russia      17.153
    Saint Petersburg  2013-09-01  Russia         NaN
    
    [330 rows x 3 columns]
    
<br>

multi level의 index를 설정하는 것도 가능하다.  


```python
temperatures_ind = temperatures.set_index(["country","city"])
rows_to_keep = [("Brazil","Rio De Janeiro"),("Pakistan","Lahore")]
print(temperatures_ind.loc[rows_to_keep])
```

                                   date  avg_temp_c
    country  city                                  
    Brazil   Rio De Janeiro  2000-01-01      25.974
             Rio De Janeiro  2000-02-01      26.699
             Rio De Janeiro  2000-03-01      26.270
             Rio De Janeiro  2000-04-01      25.750
             Rio De Janeiro  2000-05-01      24.356
    ...                             ...         ...
    Pakistan Lahore          2013-05-01      33.457
             Lahore          2013-06-01      34.456
             Lahore          2013-07-01      33.279
             Lahore          2013-08-01      31.511
             Lahore          2013-09-01         NaN
    
    [330 rows x 2 columns]
    
<br>

이제는 sort하는 것을 봐보도록 하자. 앞에서 `.sort_values()`로 sort한 것을 기억하는가? 이번에는 `.sort_index()`를 통해 sort 해 볼 것이다.  


```python
print(temperatures_ind.sort_index())
```

                              date  avg_temp_c
    country     city                          
    Afghanistan Kabul   2000-01-01       3.326
                Kabul   2000-02-01       3.454
                Kabul   2000-03-01       9.612
                Kabul   2000-04-01      17.925
                Kabul   2000-05-01      24.658
    ...                        ...         ...
    Zimbabwe    Harare  2013-05-01      18.298
                Harare  2013-06-01      17.020
                Harare  2013-07-01      16.299
                Harare  2013-08-01      19.232
                Harare  2013-09-01         NaN
    
    [16500 rows x 2 columns]
    
그냥하면 상위 level로 sort 되는 것을 볼 수 있다.  

<br>
```python
print(temperatures_ind.sort_index(level=["city"]))
```

                                 date  avg_temp_c
    country       city                           
    Côte D'Ivoire Abidjan  2000-01-01      27.293
                  Abidjan  2000-02-01      27.685
                  Abidjan  2000-03-01      29.061
                  Abidjan  2000-04-01      28.162
                  Abidjan  2000-05-01      27.547
    ...                           ...         ...
    China         Xian     2013-05-01      18.979
                  Xian     2013-06-01      23.522
                  Xian     2013-07-01      25.251
                  Xian     2013-08-01      24.528
                  Xian     2013-09-01         NaN
    
    [16500 rows x 2 columns]
    

`level`를 지정함으로써 하위 level로 sort 할 수도 있다.  
<br>


```python
print(temperatures_ind.sort_index(level=["country","city"],ascending= [False,True]))
```

                              date  avg_temp_c
    country     city                          
    Zimbabwe    Harare  2000-01-01      22.119
                Harare  2000-02-01      21.569
                Harare  2000-03-01      22.370
                Harare  2000-04-01      19.999
                Harare  2000-05-01      17.703
    ...                        ...         ...
    Afghanistan Kabul   2013-05-01      21.245
                Kabul   2013-06-01      25.859
                Kabul   2013-07-01      26.805
                Kabul   2013-08-01      24.974
                Kabul   2013-09-01         NaN
    
    [16500 rows x 2 columns]
    
<br>
<br>

### Slicing index
`loc`과 `iloc`를 이용하여 slicing 해보도록 하자.  


```python
temperatures_srt = temperatures_ind.sort_index()
```
<br>

파키스탄에서 러시아 까지의 정보를 뽑아보도록 하자.  


```python
print(temperatures_srt.loc["Pakistan":"Russia"])
```

                                     date  avg_temp_c
    country  city                                    
    Pakistan Faisalabad        2000-01-01      12.792
             Faisalabad        2000-02-01      14.339
             Faisalabad        2000-03-01      20.309
             Faisalabad        2000-04-01      29.072
             Faisalabad        2000-05-01      34.845
    ...                               ...         ...
    Russia   Saint Petersburg  2013-05-01      12.355
             Saint Petersburg  2013-06-01      17.185
             Saint Petersburg  2013-07-01      17.234
             Saint Petersburg  2013-08-01      17.153
             Saint Petersburg  2013-09-01         NaN
    
    [1155 rows x 2 columns]
    
<br>

하위 레벨의 도시 라호르에서 모스크바의 정보를 뽑아보도록 하자.  


```python
print(temperatures_srt.loc["Lahore":"Moscow"])
```

                              date  avg_temp_c
    country city                              
    Mexico  Mexico      2000-01-01      12.694
            Mexico      2000-02-01      14.677
            Mexico      2000-03-01      17.376
            Mexico      2000-04-01      18.294
            Mexico      2000-05-01      18.562
    ...                        ...         ...
    Morocco Casablanca  2013-05-01      19.217
            Casablanca  2013-06-01      23.649
            Casablanca  2013-07-01      27.488
            Casablanca  2013-08-01      27.952
            Casablanca  2013-09-01         NaN
    
    [330 rows x 2 columns]
    

이러면 안되는 것을 알 수 있다.  

<br>

```python
print(temperatures_srt.loc[("Pakistan","Lahore"):("Russia","Moscow")])
```

                           date  avg_temp_c
    country  city                          
    Pakistan Lahore  2000-01-01      12.792
             Lahore  2000-02-01      14.339
             Lahore  2000-03-01      20.309
             Lahore  2000-04-01      29.072
             Lahore  2000-05-01      34.845
    ...                     ...         ...
    Russia   Moscow  2013-05-01      16.152
             Moscow  2013-06-01      18.718
             Moscow  2013-07-01      18.136
             Moscow  2013-08-01      17.485
             Moscow  2013-09-01         NaN
    
    [660 rows x 2 columns]
    
<br>

time series를 slice 해보기 위해 전에 배웠던 것을 사용해보자.  


```python
temperatures_bool = temperatures[(temperatures["date"] >= "2010-01-01") & (temperatures["date"] <= "2011-12-31")]
print(temperatures_bool)
```

                 date     city        country  avg_temp_c
    120    2010-01-01  Abidjan  Côte D'Ivoire      28.270
    121    2010-02-01  Abidjan  Côte D'Ivoire      29.262
    122    2010-03-01  Abidjan  Côte D'Ivoire      29.596
    123    2010-04-01  Abidjan  Côte D'Ivoire      29.068
    124    2010-05-01  Abidjan  Côte D'Ivoire      28.258
    ...           ...      ...            ...         ...
    16474  2011-08-01     Xian          China      23.069
    16475  2011-09-01     Xian          China      16.775
    16476  2011-10-01     Xian          China      12.587
    16477  2011-11-01     Xian          China       7.543
    16478  2011-12-01     Xian          China      -0.490
    
    [2400 rows x 4 columns]
    
<br>

위에서 배운 `loc`를 사용하면 쉽게 가능하다. 년도만 쓰면 연초에서 연말로 지정하는 것을 알 수 있다.  


```python
temperatures_ind = temperatures.set_index("date").sort_index()
print(temperatures_ind["2010":"2011"])
```

                      city    country  avg_temp_c
    date                                         
    2010-01-01  Faisalabad   Pakistan      11.810
    2010-01-01   Melbourne  Australia      20.016
    2010-01-01   Chongqing      China       7.921
    2010-01-01   São Paulo     Brazil      23.738
    2010-01-01   Guangzhou      China      14.136
    ...                ...        ...         ...
    2010-12-01     Jakarta  Indonesia      26.602
    2010-12-01       Gizeh      Egypt      16.530
    2010-12-01      Nagpur      India      19.120
    2010-12-01      Sydney  Australia      19.559
    2010-12-01    Salvador     Brazil      26.265
    
    [1200 rows x 3 columns]
    
<br>
<br>

### Working with pivot tables

위에서 date은 연,월,일로 구분 되어있는데 이를 각각 뽑아 data를 만들 수 있다. 방법은 `dataframe["column"].dt.component`의 방식이다. component에 month가 들어가면 월만, day가 들어가면 일만 뽑을 수 있다.  


```python
temperatures['date'] = pd.to_datetime(temperatures.date, format='%Y-%m-%d')
temperatures["year"]= temperatures["date"].dt.year
```
<br>

value가 `avg_temp_c`이고 index가 `country`,`city`이며 column이 `year`인 pivot table을 만들어 보도록 하자.  


```python
temp_by_country_city_vs_year = temperatures.pivot_table("avg_temp_c",index=["country","city"],columns="year")
print(temp_by_country_city_vs_year)
```

    year                                 2000       2001       2002       2003  \
    country       city                                                           
    Afghanistan   Kabul             15.822667  15.847917  15.714583  15.132583   
    Angola        Luanda            24.410333  24.427083  24.790917  24.867167   
    Australia     Melbourne         14.320083  14.180000  14.075833  13.985583   
                  Sydney            17.567417  17.854500  17.733833  17.592333   
    Bangladesh    Dhaka             25.905250  25.931250  26.095000  25.927417   
    ...                                   ...        ...        ...        ...   
    United States Chicago           11.089667  11.703083  11.532083  10.481583   
                  Los Angeles       16.643333  16.466250  16.430250  16.944667   
                  New York           9.969083  10.931000  11.252167   9.836000   
    Vietnam       Ho Chi Minh City  27.588917  27.831750  28.064750  27.827667   
    Zimbabwe      Harare            20.283667  20.861000  21.079333  20.889167   
    
    year                                 2004       2005       2006       2007  \
    country       city                                                           
    Afghanistan   Kabul             16.128417  14.847500  15.798500  15.518000   
    Angola        Luanda            24.216167  24.414583  24.138417  24.241583   
    Australia     Melbourne         13.742083  14.378500  13.991083  14.991833   
                  Sydney            17.869667  18.028083  17.749500  18.020833   
    Bangladesh    Dhaka             26.136083  26.193333  26.440417  25.951333   
    ...                                   ...        ...        ...        ...   
    United States Chicago           10.943417  11.583833  11.870500  11.448333   
                  Los Angeles       16.552833  16.431417  16.623083  16.699917   
                  New York          10.389500  10.681417  11.519250  10.627333   
    Vietnam       Ho Chi Minh City  27.686583  27.884000  28.044000  27.866667   
    Zimbabwe      Harare            20.307667  21.487417  20.699750  20.746250   
    
    year                                 2008       2009       2010       2011  \
    country       city                                                           
    Afghanistan   Kabul             15.479250  15.093333  15.676000  15.812167   
    Angola        Luanda            24.266333  24.325083  24.440250  24.150750   
    Australia     Melbourne         14.110583  14.647417  14.231667  14.190917   
                  Sydney            17.321083  18.175833  17.999000  17.713333   
    Bangladesh    Dhaka             26.004500  26.535583  26.648167  25.803250   
    ...                                   ...        ...        ...        ...   
    United States Chicago           10.242417  10.298333  11.815917  11.214250   
                  Los Angeles       17.014750  16.677000  15.887000  15.874833   
                  New York          10.641667  10.141833  11.357583  11.272250   
    Vietnam       Ho Chi Minh City  27.611417  27.853333  28.281750  27.675417   
    Zimbabwe      Harare            20.680500  20.523833  21.165833  20.781750   
    
    year                                 2012       2013  
    country       city                                    
    Afghanistan   Kabul             14.510333  16.206125  
    Angola        Luanda            24.240083  24.553875  
    Australia     Melbourne         14.268667  14.741500  
                  Sydney            17.474333  18.089750  
    Bangladesh    Dhaka             26.283583  26.587000  
    ...                                   ...        ...  
    United States Chicago           12.821250  11.586889  
                  Los Angeles       17.089583  18.120667  
                  New York          11.971500  12.163889  
    Vietnam       Ho Chi Minh City  28.248750  28.455000  
    Zimbabwe      Harare            20.523333  19.756500  
    
    [100 rows x 14 columns]
    


```python
temp_by_country_city_vs_year.loc[("Egypt","Cairo"):("India","Delhi"),2005:2010]
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
      <th>year</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
    </tr>
    <tr>
      <th>country</th>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">Egypt</th>
      <th>Cairo</th>
      <td>22.006500</td>
      <td>22.050000</td>
      <td>22.361000</td>
      <td>22.644500</td>
      <td>22.625000</td>
      <td>23.718250</td>
    </tr>
    <tr>
      <th>Gizeh</th>
      <td>22.006500</td>
      <td>22.050000</td>
      <td>22.361000</td>
      <td>22.644500</td>
      <td>22.625000</td>
      <td>23.718250</td>
    </tr>
    <tr>
      <th>Ethiopia</th>
      <th>Addis Abeba</th>
      <td>18.312833</td>
      <td>18.427083</td>
      <td>18.142583</td>
      <td>18.165000</td>
      <td>18.765333</td>
      <td>18.298250</td>
    </tr>
    <tr>
      <th>France</th>
      <th>Paris</th>
      <td>11.552917</td>
      <td>11.788500</td>
      <td>11.750833</td>
      <td>11.278250</td>
      <td>11.464083</td>
      <td>10.409833</td>
    </tr>
    <tr>
      <th>Germany</th>
      <th>Berlin</th>
      <td>9.919083</td>
      <td>10.545333</td>
      <td>10.883167</td>
      <td>10.657750</td>
      <td>10.062500</td>
      <td>8.606833</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">India</th>
      <th>Ahmadabad</th>
      <td>26.828083</td>
      <td>27.282833</td>
      <td>27.511167</td>
      <td>27.048500</td>
      <td>28.095833</td>
      <td>28.017833</td>
    </tr>
    <tr>
      <th>Bangalore</th>
      <td>25.476500</td>
      <td>25.418250</td>
      <td>25.464333</td>
      <td>25.352583</td>
      <td>25.725750</td>
      <td>25.705250</td>
    </tr>
    <tr>
      <th>Bombay</th>
      <td>27.035750</td>
      <td>27.381500</td>
      <td>27.634667</td>
      <td>27.177750</td>
      <td>27.844500</td>
      <td>27.765417</td>
    </tr>
    <tr>
      <th>Calcutta</th>
      <td>26.729167</td>
      <td>26.986250</td>
      <td>26.584583</td>
      <td>26.522333</td>
      <td>27.153250</td>
      <td>27.288833</td>
    </tr>
    <tr>
      <th>Delhi</th>
      <td>25.716083</td>
      <td>26.365917</td>
      <td>26.145667</td>
      <td>25.675000</td>
      <td>26.554250</td>
      <td>26.520250</td>
    </tr>
  </tbody>
</table>
</div>

<br>
<br>

##  Creating and Visualizing Df


```python
import pickle
avocados = pd.read_pickle('avoplott.pkl')
avocados_2016 = avocados[avocados["year"]==2016]
print(avocados.head())
```

             date          type  year  avg_price   size     nb_sold
    0  2015-12-27  conventional  2015       0.95  small  9626901.09
    1  2015-12-20  conventional  2015       0.98  small  8710021.76
    2  2015-12-13  conventional  2015       0.93  small  9855053.66
    3  2015-12-06  conventional  2015       0.89  small  9405464.36
    4  2015-11-29  conventional  2015       0.99  small  8094803.56
    
<br>

### Visualizing
각 사이즈마다 number sold의 총합을 구해보도록 하자.  


```python
nb_sold_by_size = avocados.groupby("size")["nb_sold"].sum()
print(nb_sold_by_size)
```

    size
    extra_large    1.561752e+08
    large          2.015012e+09
    small          2.054936e+09
    Name: nb_sold, dtype: float64
    


```python
nb_sold_by_size.plot(kind="bar")
plt.show()
```


    
![output_42_0](https://user-images.githubusercontent.com/96481582/151959521-1b477a42-8bce-4635-a30b-3a7b4c1239a7.png)
    

<br>

날짜별로 아보카도가 팔린 양의 합을 시각화 할수도 있다.  


```python
nb_sold_by_date = avocados.groupby("date")["nb_sold"].sum()
nb_sold_by_date.plot(kind="line")
plt.show()
```


    
![output_44_0](https://user-images.githubusercontent.com/96481582/151959512-e94a06d3-77fa-4778-b17b-aa2a359f140c.png)
    

<br>

scatter point는 numerical variable 끼리의 관계를 파악하는데 용이하다. `nb_sold`와 `avg_price`의 관계는 다음과 같다.  


```python
avocados.plot(x="nb_sold",y="avg_price",kind="scatter",title="Number of avocados sold vs. average price",s=2)
plt.show()
```


    
![output_46_0](https://user-images.githubusercontent.com/96481582/151959515-36357a90-4bb5-4a70-b0df-3da3a0dea17b.png)
    

<br>

conventional한 avocado 와 organic한 avocado의 평균 가격을 히스토그램을 통해 비교해보도록 하자.  


```python
avocados[avocados["type"]=="conventional"]["avg_price"].hist(alpha=0.5,bins = 20)
avocados[avocados["type"]=="organic"]["avg_price"].hist(alpha=0.5,bins = 20)
plt.legend(["conventional","organic"])
plt.show()
```


    
![output_48_0](https://user-images.githubusercontent.com/96481582/151959519-fdcfb4e0-301f-4324-8ea0-3bcad5e1ff07.png)
    

<br>
<br>

### Missing values

data는 완벽하지 않기 때문에, missing values가 있을 수 있다. pandas Df에서는 missing value를 `NaN` 이라고 표시된다. missing value인지 아닌지를 확인하는 방법은 `isna()`를 사용하는 것이다.  


```python
print(avocados_2016.isna())
```

          date   type   year  avg_price   size  nb_sold
    52   False  False  False      False  False    False
    53   False  False  False      False  False    False
    54   False  False  False      False  False    False
    55   False  False  False      False  False    False
    56   False  False  False      False  False    False
    ..     ...    ...    ...        ...    ...      ...
    944  False  False  False      False  False    False
    945  False  False  False      False  False    False
    946  False  False  False      False  False    False
    947  False  False  False      False  False    False
    948  False  False  False      False  False    False
    
    [312 rows x 6 columns]
    
<br>

`.any()`는 column에 대해 NaN이 있는지 없는지 알려준다.  


```python
print(avocados_2016.isna().any())
```

    date         False
    type         False
    year         False
    avg_price    False
    size         False
    nb_sold      False
    dtype: bool
    
<br>

흠..보니까 missing value가 없는 data긴 하네....
`.dropna()`는 missing value가 있는 row를 없애준다.  


```python
avocados_complete = avocados_2016.dropna()
print(avocados_complete.isna().any())
```

    date         False
    type         False
    year         False
    avg_price    False
    size         False
    nb_sold      False
    dtype: bool
    
<br>

그러나 이렇게 없애기만 하면, 많은 data 손실이 있을 수 있어 NaN을 다른 수로 바꾸는 방법도 있다.  


```python
avocados_filled = avocados_2016.fillna(0)
```
<br>
<br>

### Creating Df

Df를 만드는 방법은 크게 2가지가 있다. <span style="color:lightgreen">list of dictionaries</span> 나 <span style="color:salmon">dictionary of lists</span>를 통해서 말이다.  <span style="color:lightgreen">list of dictionaries</span>는 df가 row별로 만들어지며 <span style="color:salmon">dictionary of lists</span>는 data가 column 별로 만들어진다.  
<br>

__list of dictionaries__



```python
avocados_list = [
    {"date": "2019-11-03", "small_sold": 10376832, "large_sold": 7835071},
    {"date": "2019-11-10", "small_sold": 10717154, "large_sold": 8561348}
]
avocados_2019 = pd.DataFrame(avocados_list)
print(avocados_2019)
```

             date  small_sold  large_sold
    0  2019-11-03    10376832     7835071
    1  2019-11-10    10717154     8561348
    
<br>

__dictionary of list__


```python
avocados_dict = {
  "date": ["2019-11-17","2019-12-01"],
  "small_sold": [10859987,9291631],
  "large_sold": [	7674135,	6238096]
}

avocados_2019 = pd.DataFrame(avocados_dict)
print(avocados_2019)
```
