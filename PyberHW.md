
# Pyber Ride Sharing

# Analysis
    1 - As expected urban areas drive the demand for ride sharing services
    2 - While demand is highest in urban areas suburban areas have a high share of total fares
    3 - Rural demand outpaces the amount of drivers in rural areas


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import scipy

file = "city_data.csv"
city_df = pd.read_csv(file)
file2 = "ride_data.csv"
ride_df = pd.read_csv(file2)
city_df.head()

```


```python
file_df = pd.merge(ride_df, city_df, on="city")
file_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
pyber = file_df[["city"]]
pyber = pyber.groupby("city").count()
pyber["Avg Fare"] = file_df.groupby(["city"])["fare"].mean()
pyber["Rides per city"] = file_df.groupby(["city"])["ride_id"].count()
pyber["Total Fare"] = file_df.groupby(["city"])["fare"].sum()
#pyber["Drivers per city"] = file_df.groupby(["city"])["driver_count"].mean()
pyber.head()
pyber["city"] = pyber.index
#pyber.head()

combined = pd.merge(pyber, city_df, on="city")
combined.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg Fare</th>
      <th>Rides per city</th>
      <th>Total Fare</th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>23.928710</td>
      <td>31</td>
      <td>741.79</td>
      <td>Alvarezhaven</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.609615</td>
      <td>26</td>
      <td>535.85</td>
      <td>Alyssaberg</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37.315556</td>
      <td>9</td>
      <td>335.84</td>
      <td>Anitamouth</td>
      <td>16</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.625000</td>
      <td>22</td>
      <td>519.75</td>
      <td>Antoniomouth</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21.981579</td>
      <td>19</td>
      <td>417.65</td>
      <td>Aprilchester</td>
      <td>49</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
urban = combined.loc[(combined["type"] == "Urban")]
suburban = combined.loc[(combined["type"] == "Suburban")]
rural = combined.loc[(combined["type"] == "Rural")]
rural.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg Fare</th>
      <th>Rides per city</th>
      <th>Total Fare</th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>17</th>
      <td>33.660909</td>
      <td>11</td>
      <td>370.27</td>
      <td>East Leslie</td>
      <td>9</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>18</th>
      <td>39.053000</td>
      <td>10</td>
      <td>390.53</td>
      <td>East Stephen</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>19</th>
      <td>33.244286</td>
      <td>7</td>
      <td>232.71</td>
      <td>East Troybury</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>21</th>
      <td>30.043750</td>
      <td>8</td>
      <td>240.35</td>
      <td>Erikport</td>
      <td>3</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>25</th>
      <td>32.002222</td>
      <td>9</td>
      <td>288.02</td>
      <td>Hernandezshire</td>
      <td>10</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>



# Bubble Plot of Ride Sharing Data


```python
fig = plt.figure(dpi=100)

plt.scatter(urban["Rides per city"], urban["Avg Fare"], linewidths = 1, alpha= 1, label = "Urban", color = 'lightcoral', edgecolor='lightcoral', s=urban["driver_count"], edgecolors="lightcoral")

plt.scatter(suburban["Rides per city"], suburban["Avg Fare"], linewidths = 1, alpha=1, label = "Suburban", color = 'lightskyblue', edgecolor='darkblue', s=suburban["driver_count"], edgecolors="darkblue")

plt.scatter(rural["Rides per city"], rural["Avg Fare"], linewidths = .5, alpha=1, label = "Rural", color = 'gold', edgecolor='black', s=rural["driver_count"], edgecolors="black")

plt.ylim(0, 55)
plt.xlim(0, 40)
plt.title("Pyber Ride Sharing Data")
plt.xlabel("Rides Per City")
plt.ylabel("Avg Fare")

plt.text(0, -15, "*Circle size correlates with driver count per city.")
plt.legend(fancybox = True,loc='upper right', title = 'City Types*', frameon=True, borderpad = 1, shadow=True)


plt.grid(which='major', color='white', linestyle='--')
plt.style.use('ggplot')

plt.show()
print(plt.style.available)
```


![png](output_7_0.png)


    ['bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark-palette', 'seaborn-dark', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'seaborn', 'Solarize_Light2', '_classic_test']
    


```python
file_df = pd.merge(ride_df, city_df, on="city")
file_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>



# Total Rides by City


```python
ride_file = file_df.groupby(["type"])
type_count = ride_file["ride_id"].count()

explode = (0, 0, .15)
colors = ['gold', 'lightskyblue', 'lightcoral']

type_count = type_count.plot(kind="pie", explode=explode, colors=colors, autopct='%1.1f%%', shadow=True, startangle=175, title=("% of Total Rides by City Type"))

type_count.set_ylabel(" ")
plt.axis("equal")
plt.show()
```


![png](output_10_0.png)


# Total Fares by City


```python
sum_file = file_df.groupby(["type"])
sum_type = sum_file["fare"].sum()

explode = (0, 0, .15)
colors = ['gold', 'lightskyblue', 'lightcoral']

sum_type = sum_type.plot(kind="pie", explode=explode, colors=colors, autopct='%1.1f%%', shadow=False, startangle=100, title=("% of Total Fares by City Type"))

sum_type.set_ylabel(" ")
plt.axis("equal")
plt.show()
```


![png](output_12_0.png)


# Total Drivers by City


```python
driver_file = file_df.groupby(["type"])
driver_count = driver_file["driver_count"].sum()

explode = (0, 0, .15)
colors = ['gold', 'lightskyblue', 'lightcoral']

driver_count = driver_count.plot(kind="pie", explode=explode, colors=colors, autopct='%1.1f%%', shadow=True, startangle=25, title=("% of Total Fares by City Type"))

driver_count.set_ylabel(" ")
plt.axis("equal")
plt.show()
```


![png](output_14_0.png)

