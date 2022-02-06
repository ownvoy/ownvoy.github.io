## Plotting Time Series
```python
import pandas as pd
import matplotlib.pyplot as plt

climate_change = pd.read_csv("climate_change.csv",parse_dates=["date"])
climate_change_ind = pd.read_csv("climate_change.csv",parse_dates=["date"] , index_col = "date")

```
<br>

### Introduction to Matplotlib
일단 `Figure` object와 `Axes` object를 만들어 보도록 할 것이다. `Figure` object는 페이지에서 너가 볼 모든 것을 갖고 있으며, `Axes`는 데이터를 가지고 있는 페이지의 일부이다.  

`subplots()`를 통해 이를 설정할 수 있다.  


```python
fig, ax = plt.subplots()
plt.show()
```


    
![output_2_0](https://user-images.githubusercontent.com/96481582/152695980-6a795763-e062-44d1-8c34-5f95cd9553c6.png)
    
<br>

단지 빈 도화지인 것을 알 수 있다.  
이런 식으로 그래프를 만들 수 있다.  


```python
fig, ax = plt.subplots()
ax.plot(climate_change["date"],climate_change["co2"])
ax.plot(climate_change["date"],climate_change["relative_temp"])
plt.show()
```


    
![output_5_0](https://user-images.githubusercontent.com/96481582/152695982-7e97c2fe-5e02-404e-86a1-620fc24a9e61.png)
    

<br>

데이터가 다음과 같이 생겼을 수도 있다.  


```python
print(climate_change_ind.head())
```

                   co2  relative_temp
    date                             
    1958-03-06  315.71           0.10
    1958-04-06  317.45           0.01
    1958-05-06  317.50           0.08
    1958-06-06     NaN          -0.05
    1958-07-06  315.86           0.06
    
<br>

이렇게 인덱스를 x로 뽑고 싶다면 다음과 같이 가능하다.  


```python
fig, ax = plt.subplots()
ax.plot(climate_change_ind.index, climate_change['relative_temp'])
plt.show()
```


    
![output_9_0](https://user-images.githubusercontent.com/96481582/152695963-f5c3558c-1cc3-4eb0-ac79-379b5e51ca9d.png)
    

<br>

### Customizing

`set_xlabel`과 `set_ylabel`를 통해 x축 y축 이름을 지어줄 수 도 있다.  


```python
fig, ax = plt.subplots()
ax.plot(climate_change_ind.index, climate_change_ind['relative_temp'])

ax.set_xlabel('Time')
ax.set_ylabel('Relative temperature (Celsius)')

plt.show()
```


    
![output_11_0](https://user-images.githubusercontent.com/96481582/152695964-5ed36db8-b5e1-4cc9-bbff-fd0efb181183.png)
    

<br>

70년대만 따로 보고 싶을 수도 있다.  


```python
fig, ax= plt.subplots()
seventies = climate_change_ind["1970-01-01":"1979-12-31"]

ax.plot(seventies.index, seventies["co2"])
plt.show()
```


    
![output_13_0](https://user-images.githubusercontent.com/96481582/152695965-9bf3fa62-138d-4e03-aacc-458d7c58c445.png)
    

<br>

color, marker, linestyle을 지정할 수도 있다.  


```python
fig, ax= plt.subplots()
seventies = climate_change_ind["1970-01-01":"1979-12-31"]

ax.plot(seventies.index, seventies["co2"],color ='b',marker='o', linestyle='--')
plt.show()
```


    
![output_15_0](https://user-images.githubusercontent.com/96481582/152695966-d009c371-4fc2-499d-a7f8-0f48eecb1e33.png)
    



```python
fig, ax= plt.subplots()
seventies = climate_change_ind["1970-01-01":"1979-12-31"]

ax.plot(seventies.index, seventies["co2"],color ='red',marker='v', linestyle='--')
plt.show()
```


    
![output_16_0](https://user-images.githubusercontent.com/96481582/152695967-91d6664a-7df1-4ecd-9ce7-2f6d797a1520.png)
    

<br>

여러 가지의 그래프를 표현 하고 싶을 수도 있다. 그럴 때는 subplot에 row와 column의 개수를 지정해주면 된다. 그래프 또한 이를 이용하여 만들 수 있다.  


```python
fig, ax= plt.subplots(2,2)
sixties = climate_change_ind["1960-01-01":"1969-12-31"]
seventies = climate_change_ind["1970-01-01":"1979-12-31"]
eighties = climate_change_ind["1980-01-01":"1989-12-31"]
nineties = climate_change_ind["1990-01-01":"1999-12-31"]

ax[0, 0].plot(sixties.index, sixties["co2"])
ax[0, 1].plot(seventies.index, seventies["co2"])
ax[1, 0].plot(eighties.index, eighties["co2"])
ax[1, 1].plot(nineties.index, nineties["co2"])

plt.show()
```


    
![output_18_0](https://user-images.githubusercontent.com/96481582/152695969-7206e438-216d-49c0-a4f3-e419215c5a5d.png)
    

<br>

### Plotting two variables
두 개의 그래프를 한 곳에 그리고 싶을 수도 있다.  


```python
fig, ax= plt.subplots()
seventies = climate_change_ind["1970-01-01":"1979-12-31"]

ax.plot(seventies.index, seventies["co2"])
ax.plot(seventies.index, seventies["relative_temp"])
plt.show()
```


    
![output_20_0](https://user-images.githubusercontent.com/96481582/152695970-e391d599-a7a4-46d2-92a9-e0277e6fc4e3.png)
    

<br>

이렇게 되면 보기가 불편한데 서로의 scale이 다르기 때문이다.  
따라서 새로운 y axis를 만들고 싶은 것이다. 그럴떄는 `twinx()`를 써주면 된다.  


```python
fig, ax= plt.subplots()
ax.plot(climate_change.index,climate_change["co2"] , color='blue')

ax2 = ax.twinx()
ax2.plot(climate_change.index,climate_change["relative_temp"] , color='red')

plt.show()
```


    
![output_22_0](https://user-images.githubusercontent.com/96481582/152695973-d83589a2-631b-441a-ac2e-01411dcb6672.png)
    

<br>

라벨의 이름과 색, 틱의 색 또한 지정할 수 있다.  


```python
fig, ax= plt.subplots()
ax.plot(climate_change.index,climate_change["co2"] , color='blue')

ax.set_xlabel('Time')
ax.set_ylabel('CO2', color='blue')
ax.tick_params('y', colors='blue')

ax2 = ax.twinx()
ax2.plot(climate_change.index,climate_change["relative_temp"] , color='red')

ax2.set_xlabel('Time')
ax2.set_ylabel('Relative temperatures', color='red')
ax2.tick_params('y', colors='red')

plt.show()
```


    
![output_24_0](https://user-images.githubusercontent.com/96481582/152695974-4029dcc2-b66f-4a7e-9601-7c0eece08fdb.png)
    
<br>

이러면 너무 길고 반복하니까 함수를 만들 수도 있다.  


```python
def plot_timeseries(axes, x, y, color, xlabel, ylabel):

  axes.plot(x, y, color=color)

  axes.set_xlabel(xlabel)

  axes.set_ylabel(ylabel, color= color)

  axes.tick_params('y', colors=color)
```


```python
fig, ax = plt.subplots()
plot_timeseries(ax, climate_change.index, climate_change["co2"], "blue", "Time (years)", "CO2 levels")

ax2 = ax.twinx()
plot_timeseries(ax2, climate_change.index, climate_change["relative_temp"], "red", "Time (years)", "Relative temperature (Celsius)")

plt.show()
```


    
![output_27_0](https://user-images.githubusercontent.com/96481582/152695976-9da6c2db-73d2-4ae6-a984-f63c635563f0.png)
    
<br>

### Annotating

이제 annotate하는 방법을 볼 것이다. pandas의 `Timestamp`를 통해 x를 찾고 ,y를 지정해주면 된다.  


```python
fig, ax = plt.subplots()

ax.plot(climate_change_ind.index, climate_change["relative_temp"])
ax.annotate('>1 degree', xy=(pd.Timestamp('2015-10-06'), 1))

plt.show()
```


    
![output_29_0](https://user-images.githubusercontent.com/96481582/152695977-6f8686b0-3355-477a-828a-83f0b3acb778.png)
    

<br>

다음과 같이 annotate할 때 화살표를 그릴 수도 있다.  


```python
fig, ax = plt.subplots()
plot_timeseries(ax, climate_change_ind.index, climate_change['co2'], 'blue', 'Time (years)' , 'CO2 levels')

ax2 = ax.twinx()
plot_timeseries(ax2,climate_change_ind.index, climate_change['relative_temp'] , 'red', "Time (years)",'Relative temp (Celsius)')

ax2.annotate(">1 degree", xy=(pd.Timestamp('2015-10-06 00:00:00'), 1), xytext=(pd.Timestamp('2008-10-06'), -0.2), arrowprops={"arrowstyle":"->","color":"gray"})

plt.show()
```


    
![output_31_0](https://user-images.githubusercontent.com/96481582/152695979-4e6ef536-2f4b-4465-ad09-8be15cc4f275.png)
    

