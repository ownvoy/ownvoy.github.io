---

title: "Transforming and Aggregating df"
categories: datacamp
excerpt: Intermediate Python
toc: true
toc_sticky: true

date: 2022-01-27
---

## Transforming Data Frame

### Insepecing Data

처음 Data Frame을 받았을 떄 어떻게 생겨먹었는지 궁금할 수 있다. 다음 매소드를 통해 이를 확인 할 수 있다. 

아 근데 일단 데이터를 불러오도록 하겠다.  




```python
import pandas as pd
import numpy as np

homelessness = pd.read_csv('homelessness.csv', index_col = 0)
sales = pd.read_csv("sales_subset.csv", index_col = 0)
```
<br>

- `.head()`: 처음 5개의 row를 보여준다.  
- `.info()`: column, data types, missing value 숫자들에 대한 정보를 보여준다.  
- `.shape()`: row 와 column의 숫자를 알려준다.  
- `.describe()`: column에 대한 짤막한 정보를 알려준다.  
<br>

```python
print(homelessness.head())
```

                   region       state  individuals  family_members  state_pop
    0  East South Central     Alabama       2570.0           864.0    4887681
    1             Pacific      Alaska       1434.0           582.0     735139
    2            Mountain     Arizona       7259.0          2606.0    7158024
    3  West South Central    Arkansas       2280.0           432.0    3009733
    4             Pacific  California     109008.0         20964.0   39461588
    


```python
print(homelessness.info())
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 51 entries, 0 to 50
    Data columns (total 5 columns):
     #   Column          Non-Null Count  Dtype  
    ---  ------          --------------  -----  
     0   region          51 non-null     object 
     1   state           51 non-null     object 
     2   individuals     51 non-null     float64
     3   family_members  51 non-null     float64
     4   state_pop       51 non-null     int64  
    dtypes: float64(2), int64(1), object(2)
    memory usage: 2.4+ KB
    None
    


```python
print(homelessness.shape)
```

    (51, 5)
    


```python
print(homelessness.describe())
```

             individuals  family_members     state_pop
    count      51.000000       51.000000  5.100000e+01
    mean     7225.784314     3504.882353  6.405637e+06
    std     15991.025083     7805.411811  7.327258e+06
    min       434.000000       75.000000  5.776010e+05
    25%      1446.500000      592.000000  1.777414e+06
    50%      3082.000000     1482.000000  4.461153e+06
    75%      6781.500000     3196.000000  7.340946e+06
    max    109008.000000    52070.000000  3.946159e+07
    
<br>

다음과 같은 매소드를 통해 부분들을 더 잘 이해할 수 있다.  
- `.value()`: 2D NumPy array의 value
- `.column()`: column name을 보여준다.(an index of column)  
- `.index()`: row name이나 row nummber을 보여준다. (an index of row)

<br>

```python
print(homelessness.values)
```

    [['East South Central' 'Alabama' 2570.0 864.0 4887681]
     ['Pacific' 'Alaska' 1434.0 582.0 735139]
     ['Mountain' 'Arizona' 7259.0 2606.0 7158024]
     ['West South Central' 'Arkansas' 2280.0 432.0 3009733]
     ['Pacific' 'California' 109008.0 20964.0 39461588]
     ['Mountain' 'Colorado' 7607.0 3250.0 5691287]
     ['New England' 'Connecticut' 2280.0 1696.0 3571520]
     ['South Atlantic' 'Delaware' 708.0 374.0 965479]
     ['South Atlantic' 'District of Columbia' 3770.0 3134.0 701547]
     ['South Atlantic' 'Florida' 21443.0 9587.0 21244317]
     ['South Atlantic' 'Georgia' 6943.0 2556.0 10511131]
     ['Pacific' 'Hawaii' 4131.0 2399.0 1420593]
     ['Mountain' 'Idaho' 1297.0 715.0 1750536]
     ['East North Central' 'Illinois' 6752.0 3891.0 12723071]
     ['East North Central' 'Indiana' 3776.0 1482.0 6695497]
     ['West North Central' 'Iowa' 1711.0 1038.0 3148618]
     ['West North Central' 'Kansas' 1443.0 773.0 2911359]
     ['East South Central' 'Kentucky' 2735.0 953.0 4461153]
     ['West South Central' 'Louisiana' 2540.0 519.0 4659690]
     ['New England' 'Maine' 1450.0 1066.0 1339057]
     ['South Atlantic' 'Maryland' 4914.0 2230.0 6035802]
     ['New England' 'Massachusetts' 6811.0 13257.0 6882635]
     ['East North Central' 'Michigan' 5209.0 3142.0 9984072]
     ['West North Central' 'Minnesota' 3993.0 3250.0 5606249]
     ['East South Central' 'Mississippi' 1024.0 328.0 2981020]
     ['West North Central' 'Missouri' 3776.0 2107.0 6121623]
     ['Mountain' 'Montana' 983.0 422.0 1060665]
     ['West North Central' 'Nebraska' 1745.0 676.0 1925614]
     ['Mountain' 'Nevada' 7058.0 486.0 3027341]
     ['New England' 'New Hampshire' 835.0 615.0 1353465]
     ['Mid-Atlantic' 'New Jersey' 6048.0 3350.0 8886025]
     ['Mountain' 'New Mexico' 1949.0 602.0 2092741]
     ['Mid-Atlantic' 'New York' 39827.0 52070.0 19530351]
     ['South Atlantic' 'North Carolina' 6451.0 2817.0 10381615]
     ['West North Central' 'North Dakota' 467.0 75.0 758080]
     ['East North Central' 'Ohio' 6929.0 3320.0 11676341]
     ['West South Central' 'Oklahoma' 2823.0 1048.0 3940235]
     ['Pacific' 'Oregon' 11139.0 3337.0 4181886]
     ['Mid-Atlantic' 'Pennsylvania' 8163.0 5349.0 12800922]
     ['New England' 'Rhode Island' 747.0 354.0 1058287]
     ['South Atlantic' 'South Carolina' 3082.0 851.0 5084156]
     ['West North Central' 'South Dakota' 836.0 323.0 878698]
     ['East South Central' 'Tennessee' 6139.0 1744.0 6771631]
     ['West South Central' 'Texas' 19199.0 6111.0 28628666]
     ['Mountain' 'Utah' 1904.0 972.0 3153550]
     ['New England' 'Vermont' 780.0 511.0 624358]
     ['South Atlantic' 'Virginia' 3928.0 2047.0 8501286]
     ['Pacific' 'Washington' 16424.0 5880.0 7523869]
     ['South Atlantic' 'West Virginia' 1021.0 222.0 1804291]
     ['East North Central' 'Wisconsin' 2740.0 2167.0 5807406]
     ['Mountain' 'Wyoming' 434.0 205.0 577601]]
    


```python
print(homelessness.columns)
```

    Index(['region', 'state', 'individuals', 'family_members', 'state_pop'], dtype='object')
    


```python
print(homelessness.index)
```

    Int64Index([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
                17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33,
                34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49,
                50],
               dtype='int64')
    
<br>
<br>

### sorting and subsetting

row의 순서를 바꾸고 싶다면, `.sort_values()`를 통해 바꿀 수 있다. 안에 collumn name을 넣으면 된다.  

예를 들어 `homelessness.sort_values('individuals')`를 하면 작은 수에서 큰 수로 정렬 된다.  


```python
homelessness_ind = homelessness.sort_values('individuals')
print(homelessness_ind.head())
```

                    region         state  individuals  family_members  state_pop
    50            Mountain       Wyoming        434.0           205.0     577601
    34  West North Central  North Dakota        467.0            75.0     758080
    7       South Atlantic      Delaware        708.0           374.0     965479
    39         New England  Rhode Island        747.0           354.0    1058287
    45         New England       Vermont        780.0           511.0     624358
    
<br>

큰 수 부터 하고 정렬하고 싶다면 `ascending = False`를 넣어 주면 된다.  


```python
homelessness_fam = homelessness.sort_values("family_members",ascending = False)
print(homelessness_fam.head())
```

                    region          state  individuals  family_members  state_pop
    32        Mid-Atlantic       New York      39827.0         52070.0   19530351
    4              Pacific     California     109008.0         20964.0   39461588
    21         New England  Massachusetts       6811.0         13257.0    6882635
    9       South Atlantic        Florida      21443.0          9587.0   21244317
    43  West South Central          Texas      19199.0          6111.0   28628666
    
<br>

column을 두개 선택해서 정렬 할 수 도 있다.  


```python
homelessness_reg_fam = homelessness.sort_values(["region","family_members"],ascending=[True,False])
print(homelessness_reg_fam.head())
```

                    region      state  individuals  family_members  state_pop
    13  East North Central   Illinois       6752.0          3891.0   12723071
    35  East North Central       Ohio       6929.0          3320.0   11676341
    22  East North Central   Michigan       5209.0          3142.0    9984072
    49  East North Central  Wisconsin       2740.0          2167.0    5807406
    14  East North Central    Indiana       3776.0          1482.0    6695497
    
<br>

subsetting column 를 하는 방법은 간단하다. `[]`를 이용하면 된다.  


```python
individuals = homelessness["individuals"]
print(individuals.head())
```

    0      2570.0
    1      1434.0
    2      7259.0
    3      2280.0
    4    109008.0
    Name: individuals, dtype: float64
    


```python
ind_state = homelessness[["individuals","state"]]
print(ind_state.head())
```

       individuals       state
    0       2570.0     Alabama
    1       1434.0      Alaska
    2       7259.0     Arizona
    3       2280.0    Arkansas
    4     109008.0  California
    
<br>

subsetting row 를 하는 방법은 많이 있지만 그중 가장 보편적인 것은 `True` 와 `False`를 통해 걸러 내는 것이다.  

region이 Mountain인 row를 선택해 보도록 하자.  


```python
mountain_reg = homelessness[homelessness["region"]=="Mountain"]
print(mountain_reg)
```

          region       state  individuals  family_members  state_pop
    2   Mountain     Arizona       7259.0          2606.0    7158024
    5   Mountain    Colorado       7607.0          3250.0    5691287
    12  Mountain       Idaho       1297.0           715.0    1750536
    26  Mountain     Montana        983.0           422.0    1060665
    28  Mountain      Nevada       7058.0           486.0    3027341
    31  Mountain  New Mexico       1949.0           602.0    2092741
    44  Mountain        Utah       1904.0           972.0    3153550
    50  Mountain     Wyoming        434.0           205.0     577601
    
<br>

family member가 1000 이하이고 region Pacific인 row를 찾아보자.  


```python
fam_lt_1k_pac= homelessness[(homelessness["family_members"]<1000) & (homelessness["region"]== "Pacific")]
print(fam_lt_1k_pac)
```

        region   state  individuals  family_members  state_pop
    1  Pacific  Alaska       1434.0           582.0     735139
    

조건이 두 개가 붙으면 `&` 통해 연결 할 수 있으며, `()`를 써주어야 한다.  
<br>

`or`를 통해 연결하고 싶으면 간단하게 `|`를 쓸 수도 있다. region 이 South Atlantic이거나 Mid Atlantic인 row를 찾아 보자.    


```python
south_mid_atlantic = homelessness[(homelessness["region"]=="South Atlantic")|(homelessness["region"]=="Mid-Atlantic")]
print(south_mid_atlantic)
```

                region                 state  individuals  family_members  \
    7   South Atlantic              Delaware        708.0           374.0   
    8   South Atlantic  District of Columbia       3770.0          3134.0   
    9   South Atlantic               Florida      21443.0          9587.0   
    10  South Atlantic               Georgia       6943.0          2556.0   
    20  South Atlantic              Maryland       4914.0          2230.0   
    30    Mid-Atlantic            New Jersey       6048.0          3350.0   
    32    Mid-Atlantic              New York      39827.0         52070.0   
    33  South Atlantic        North Carolina       6451.0          2817.0   
    38    Mid-Atlantic          Pennsylvania       8163.0          5349.0   
    40  South Atlantic        South Carolina       3082.0           851.0   
    46  South Atlantic              Virginia       3928.0          2047.0   
    48  South Atlantic         West Virginia       1021.0           222.0   
    
        state_pop  
    7      965479  
    8      701547  
    9    21244317  
    10   10511131  
    20    6035802  
    30    8886025  
    32   19530351  
    33   10381615  
    38   12800922  
    40    5084156  
    46    8501286  
    48    1804291  
    
<br>

이렇게 하면 너무 길어 질 수 있어서 `.isin()`를 통해도 가능하다.  


```python

canu = ["California", "Arizona", "Nevada", "Utah"]
mojave_homelessness = homelessness[homelessness["state"].isin(canu)]

print(mojave_homelessness)
```

          region       state  individuals  family_members  state_pop
    2   Mountain     Arizona       7259.0          2606.0    7158024
    4    Pacific  California     109008.0         20964.0   39461588
    28  Mountain      Nevada       7058.0           486.0    3027341
    44  Mountain        Utah       1904.0           972.0    3153550
    
<br>

새로운 column을 추가하는 것은 간단하다. `[]`안에 column name을 써주고 값을 넣어주면 된다.  


```python
homelessness["total"]= homelessness["individuals"]+homelessness["family_members"]
homelessness["p_individuals"] = homelessness["individuals"]/homelessness["total"]
print(homelessness.head())
```

                   region       state  individuals  family_members  state_pop  \
    0  East South Central     Alabama       2570.0           864.0    4887681   
    1             Pacific      Alaska       1434.0           582.0     735139   
    2            Mountain     Arizona       7259.0          2606.0    7158024   
    3  West South Central    Arkansas       2280.0           432.0    3009733   
    4             Pacific  California     109008.0         20964.0   39461588   
    
          total  p_individuals  
    0    3434.0       0.748398  
    1    2016.0       0.711310  
    2    9865.0       0.735834  
    3    2712.0       0.840708  
    4  129972.0       0.838704  
    
<br>
<br>

## Aggregaiting Dataframe

### Summary statistics

summary statistics은 mean, median, minimum, maximum, and standard deviation 같은 것을 의미한다. 이를 봐 보도록 하자.  


```python
print(sales["weekly_sales"].mean())
```

    23843.950148505668
    


```python
print(sales["weekly_sales"].median())
```

    12049.064999999999
    
<br>

`.max()` , `.min()`를 통해 최대치와 최소치도 알 수 있다.  


```python
print(sales["date"].max())
```

    2012-10-26
    


```python
print(sales["date"].min())
```

    2010-02-05
    
<br>

`.agg()`를 통해 Data Frame에 custom function을 적용할 수도 있다.  

iqr 함수를 만들어 적용해 보도록 하자.  


```python
def iqr(column):
    return column.quantile(0.75) - column.quantile(0.25)

print(sales["temperature_c"].agg(iqr))
```

    16.583333333333336
    
<br>

여러 row에도 적용할 수 있다.  


```python
print(sales[["temperature_c", "fuel_price_usd_per_l", "unemployment"]].agg(iqr))
```

    temperature_c           16.583333
    fuel_price_usd_per_l     0.073176
    unemployment             0.565000
    dtype: float64
    
<br>

함수 2개를 적용할 수 도 있다.  


```python
print(sales[["temperature_c", "fuel_price_usd_per_l", "unemployment"]].agg([iqr,np.median]))
```

            temperature_c  fuel_price_usd_per_l  unemployment
    iqr         16.583333              0.073176         0.565
    median      16.966667              0.743381         8.099
    
<br>

pandas 에는 cumulative statistics도 있다. `.cumsum()`은 row의 합들이 쌓여가며, `.cummax()`는 row에서 큰 값들이 적힌다.  


```python

sales_1_1 = sales.sort_values("date")

sales_1_1["cum_weekly_sales"] = sales_1_1["weekly_sales"].cumsum()

sales_1_1["cum_max_sales"]=sales_1_1["weekly_sales"].cummax()

print(sales_1_1[["date", "weekly_sales", "cum_weekly_sales", "cum_max_sales"]])
```

                 date  weekly_sales  cum_weekly_sales  cum_max_sales
    0      2010-02-05      24924.50      2.492450e+04       24924.50
    6437   2010-02-05      38597.52      6.352202e+04       38597.52
    1249   2010-02-05       3840.21      6.736223e+04       38597.52
    6449   2010-02-05      17590.59      8.495282e+04       38597.52
    6461   2010-02-05       4929.87      8.988269e+04       38597.52
    ...           ...           ...               ...            ...
    3592   2012-10-05        440.00      2.568932e+08      293966.05
    8108   2012-10-05        660.00      2.568938e+08      293966.05
    10773  2012-10-05        915.00      2.568947e+08      293966.05
    6257   2012-10-12          3.00      2.568947e+08      293966.05
    3384   2012-10-26        -21.63      2.568947e+08      293966.05
    
    [10774 rows x 4 columns]
    
<br>

### Counting

지금까지 numeric variable을 다루는 방법을 배웠다면, 이제는 categorical variable을 다루는 방법을 공부할 것이다.

`sales`의 중복된 값을 제거해보도록 하자.  


```python
store_types = sales.drop_duplicates(["store","type"])
print(store_types.head())
```

          store type  department        date  weekly_sales  is_holiday  \
    0         1    A           1  2010-02-05      24924.50       False   
    901       2    A           1  2010-02-05      35034.06       False   
    1798      4    A           1  2010-02-05      38724.42       False   
    2699      6    A           1  2010-02-05      25619.00       False   
    3593     10    B           1  2010-02-05      40212.84       False   
    
          temperature_c  fuel_price_usd_per_l  unemployment  
    0          5.727778              0.679451         8.106  
    901        4.550000              0.679451         8.324  
    1798       6.533333              0.686319         8.623  
    2699       4.683333              0.679451         7.259  
    3593      12.411111              0.782478         9.765  
    


```python
store_depts = sales.drop_duplicates(["store","department"])
print(store_depts.head())
```

        store type  department        date  weekly_sales  is_holiday  \
    0       1    A           1  2010-02-05      24924.50       False   
    12      1    A           2  2010-02-05      50605.27       False   
    24      1    A           3  2010-02-05      13740.12       False   
    36      1    A           4  2010-02-05      39954.04       False   
    48      1    A           5  2010-02-05      32229.38       False   
    
        temperature_c  fuel_price_usd_per_l  unemployment  
    0        5.727778              0.679451         8.106  
    12       5.727778              0.679451         8.106  
    24       5.727778              0.679451         8.106  
    36       5.727778              0.679451         8.106  
    48       5.727778              0.679451         8.106  
    
<br>

`is_holiday`가 `True`인 row 중에서 `date`가 중복인 row를 drop 해보자.  


```python
holiday_dates = sales[sales["is_holiday"]== True].drop_duplicates("date")
print(holiday_dates["date"])
```

    498     2010-09-10
    691     2011-11-25
    2315    2010-02-12
    6735    2012-09-07
    6810    2010-12-31
    6815    2012-02-10
    6820    2011-09-09
    Name: date, dtype: object
    
<br>

`.value_counts`를 통해 categorical value를 셀 수 있다.  


```python
store_counts = store_types["type"].value_counts()
print(store_counts)
```

    A    11
    B     1
    Name: type, dtype: int64
    
<br>

`normalize`는 count를 비율로 바꿔 준다.  


```python
store_props = store_types["type"].value_counts(normalize=True)
print(store_props)

```

    A    0.916667
    B    0.083333
    Name: type, dtype: float64
    
<br>

`sort = True`를 하면 count가 큰 거부터 정렬된다.  


```python
dept_counts_sorted = store_depts["department"].value_counts(sort=True)
print(dept_counts_sorted)
```

    1     12
    55    12
    72    12
    71    12
    67    12
          ..
    37    10
    48     8
    50     6
    39     4
    43     2
    Name: department, Length: 80, dtype: int64
    
<br>

### Grouped summary statistics
data 를 group 지어서 보고 싶을 수도 있을 것이다. 다음은 타입별로 weekly_sales의 합이다.  


```python

sales_all = sales["weekly_sales"].sum()

sales_A = sales[sales["type"] == "A"]["weekly_sales"].sum()
sales_B = sales[sales["type"] == "B"]["weekly_sales"].sum()
sales_C = sales[sales["type"] == "C"]["weekly_sales"].sum()

sales_propn_by_type = [sales_A, sales_B, sales_C] / sales_all

print(sales_propn_by_type)
```

    [0.9097747 0.0902253 0.       ]
    
<br>

이런식으로 sales_A, sales_B 하는 방식은 약간 피곤하다. 이를 간단하게 하는 것으로는 `.groupby()`가 있다.  


```python
sales_by_type = sales.groupby("type")["weekly_sales"].sum()

sales_propn_by_type = sales_by_type/sum(sales_by_type)
print(sales_propn_by_type)
```

    type
    A    0.909775
    B    0.090225
    Name: weekly_sales, dtype: float64
    
<br>

여러가지 함수를 적용하고 싶다면 `.agg()`를 사용하면 된다.  


```python
sales_stats = sales.groupby("type")["weekly_sales"].agg([np.min,np.max,np.mean,np.median])
print(sales_stats)
```

            amin       amax          mean    median
    type                                           
    A    -1098.0  293966.05  23674.667242  11943.92
    B     -798.0  232558.51  25696.678370  13336.08
    
<br>

type에 대해서 두가지 variable을 aggregate할 수도 있다.  


```python
unemp_fuel_stats = sales.groupby("type")["unemployment","fuel_price_usd_per_l"].agg([np.min,np.max,np.mean,np.median])
print(unemp_fuel_stats)
```

         unemployment                         fuel_price_usd_per_l            \
                 amin   amax      mean median                 amin      amax   
    type                                                                       
    A           3.879  8.992  7.972611  8.067             0.664129  1.107410   
    B           7.170  9.765  9.279323  9.199             0.760023  1.107674   
    
                              
              mean    median  
    type                      
    A     0.744619  0.735455  
    B     0.805858  0.803348  
    

    C:\Users\wj527\AppData\Local\Temp/ipykernel_42672/196932519.py:1: FutureWarning: Indexing with multiple keys (implicitly converted to a tuple of keys) will be deprecated, use a list instead.
      unemp_fuel_stats = sales.groupby("type")["unemployment","fuel_price_usd_per_l"].agg([np.min,np.max,np.mean,np.median])
    
### Pivot tables

pivot table은 grouped summary statistics을 계산하는 또 다른 방법이다. 위와 같은 작업을 `.pivot_table()`를 통해 똑같이 할 수 있다. 

<span style="color:orange">"values"</span> argument 은 summarize하고 싶은 column 이며, <span style="color:orange">"index"</span> argument는 group by 하고 싶은 column이다. pivot_table은 <span style="color:orange">default로 mean</span>을 value로 취한다.  


```python
mean_sales_by_type = sales.pivot_table(values="weekly_sales",index="type")
print(mean_sales_by_type)
```

          weekly_sales
    type              
    A     23674.667242
    B     25696.678370
    
<br>

mean 과 median을 둘 다 pivot 하려면 `aggfunc`을 사용하면 된다.  


```python

import numpy as np

mean_med_sales_by_type = sales.pivot_table(values="weekly_sales",index= "type",aggfunc=[np.mean, np.median])

print(mean_med_sales_by_type)
```

                  mean       median
          weekly_sales weekly_sales
    type                           
    A     23674.667242     11943.92
    B     25696.678370     13336.08
    
<br>

column을 설정함으로써 two variable을 group by 할 수 도 있다.  


```python

mean_sales_by_type_holiday = sales.pivot_table(values="weekly_sales",index="type",columns="is_holiday")

print(mean_sales_by_type_holiday)
```

    is_holiday         False       True
    type                               
    A           23768.583523  590.04525
    B           25751.980533  810.70500
    


```python
print(sales.pivot_table(values="weekly_sales",index="type",columns = "department"))
```

    department            1              2             3             4   \
    type                                                                  
    A           30961.725379   67600.158788  17160.002955  44285.399091   
    B           44050.626667  112958.526667  30580.655000  51219.654167   
    
    department            5             6             7             8   \
    type                                                                 
    A           34821.011364   7136.292652  38454.336818  48583.475303   
    B           63236.875000  10717.297500  52909.653333  90733.753333   
    
    department            9             10  ...            90            91  \
    type                                    ...                               
    A           30120.449924  30930.456364  ...  85776.905909  70423.165227   
    B           66679.301667  48595.126667  ...  14780.210000  13199.602500   
    
    department             92            93            94             95  \
    type                                                                   
    A           139722.204773  53413.633939  60081.155303  123933.787121   
    B            50859.278333   1466.274167    161.445833   77082.102500   
    
    department            96            97            98          99  
    type                                                              
    A           21367.042857  28471.266970  12875.423182  379.123659  
    B            9528.538333   5828.873333    217.428333         NaN  
    
    [2 rows x 80 columns]
    
<br>

missingi value로 `NaN`이 있는 것을 확인할 수 있다. `fill_value=0`을 통해 이를 0으로 바꿀 수 있다.  


```python
print(sales.pivot_table(values="weekly_sales",index="type",columns = "department",fill_value=0))
```

    department            1              2             3             4   \
    type                                                                  
    A           30961.725379   67600.158788  17160.002955  44285.399091   
    B           44050.626667  112958.526667  30580.655000  51219.654167   
    
    department            5             6             7             8   \
    type                                                                 
    A           34821.011364   7136.292652  38454.336818  48583.475303   
    B           63236.875000  10717.297500  52909.653333  90733.753333   
    
    department            9             10  ...            90            91  \
    type                                    ...                               
    A           30120.449924  30930.456364  ...  85776.905909  70423.165227   
    B           66679.301667  48595.126667  ...  14780.210000  13199.602500   
    
    department             92            93            94             95  \
    type                                                                   
    A           139722.204773  53413.633939  60081.155303  123933.787121   
    B            50859.278333   1466.274167    161.445833   77082.102500   
    
    department            96            97            98          99  
    type                                                              
    A           21367.042857  28471.266970  12875.423182  379.123659  
    B            9528.538333   5828.873333    217.428333    0.000000  
    
    [2 rows x 80 columns]
    
<br>

~~`margin= True`를 설정하면 row와 column의 합을 제시한다.(합도 아니고 mean도 아닌데?)~~  


```python
print(sales.pivot_table(values="weekly_sales", index="department", columns="type",fill_value=0, margins= True))
```

    type                   A              B           All
    department                                           
    1           30961.725379   44050.626667  32052.467153
    2           67600.158788  112958.526667  71380.022778
    3           17160.002955   30580.655000  18278.390625
    4           44285.399091   51219.654167  44863.253681
    5           34821.011364   63236.875000  37189.000000
    ...                  ...            ...           ...
    96          21367.042857    9528.538333  20337.607681
    97          28471.266970    5828.873333  26584.400833
    98          12875.423182     217.428333  11820.590278
    99            379.123659       0.000000    379.123659
    All         23674.667242   25696.678370  23843.950149
    
    [81 rows x 3 columns]
    
