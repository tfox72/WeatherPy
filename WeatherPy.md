
# WeatherPy

In this example, you'll be creating a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, you'll be utilizing a simple Python library, the OpenWeatherMap API, and a little common sense to create a representative model of weather across world cities.
Your objective is to build a series of scatter plots to showcase the following relationships:

* Temperature (F) vs. Latitude
* Humidity (%) vs. Latitude
* Cloudiness (%) vs. Latitude
* Wind Speed (mph) vs. Latitude

## Your final notebook must:

Randomly select at least 500 unique (non-repeat) cities based on latitude and longitude.
Perform a weather check on each of the cities using a series of successive API calls.
Include a print log of each city as it's being processed with the city number, city name, and requested URL.
Save both a CSV of all data retrieved and png images for each scatter plot.

### As final considerations:

* You must use the Matplotlib and Seaborn libraries.
* You must include a written description of three observable trends based on the data.
* You must use proper labeling of your plots, including aspects like: Plot Titles (with date of analysis) and Axes Labels.
* You must include an exported markdown version of your Notebook called  README.md in your GitHub repository.

See Example Solution for a reference on expected format.
Hints and Considerations

You may want to start this assignment by refreshing yourself on 4th grade geography, in particular, the geographic coordinate system.

Next, spend the requisite time necessary to study the OpenWeatherMap API. Based on your initial study, you should be able to answer basic questions about the API: Where do you request the API key? Which Weather API in particular will you need? What URL endpoints does it expect? What JSON structure does it respond with? Before you write a line of code, you should be aiming to have a crystal clear understanding of your intended outcome.
Though we've never worked with the citipy Python library, push yourself to decipher how it works, and why it might be relevant. Before you try to incorporate the library into your analysis, start by creating simple test cases outside your main script to confirm that you are using it correctly. Too often, when introduced to a new library, students get bogged down by the most minor of errors -- spending hours investigating their entire code -- when, in fact, a simple and focused test would have shown their basic utilization of the library was wrong from the start. Don't let this be you!

Part of our expectation in this challenge is that you will use critical thinking skills to understand how and why we're recommending the tools we are. What is Citipy for? Why would you use it in conjunction with the OpenWeatherMap API? How would you do so?

In building your script, pay attention to the cities you are using in your query pool. Are you getting coverage of the full gamut of latitudes and longitudes? Or are you simply choosing 500 cities concentrated in one region of the world? Even if you were a geographic genius, simply rattling 500 cities based on your human selection would create a biased dataset. Be thinking of how you should counter this. (Hint: Consider the full range of latitudes).
Lastly, remember -- this is a challenging activity. Push yourself! If you complete this task, then you can safely say that you've gained a strong mastery of the core foundations of data analytics and it will only go better from here. Good luck!


```python
import pandas as pd
import numpy as np
import seaborn as sea
import matplotlib.pyplot as plt
import requests as req
import API_Keys
import json
from citipy import citipy
```


```python
mykey = API_Keys.OWM_Key
```


```python
url = 'http://api.openweathermap.org/data/2.5/weather'

```


```python
city_df = pd.DataFrame()
```


```python
lat,lng = ["Latitude","Longitude"]
```


```python
for x in range(650):
    randlat = np.random.uniform(low=-90.000,high=90.000,size=1)
    randlon = np.random.uniform(low=-180.000,high=180.000,size=1)
    randomcity = pd.DataFrame([[randlat,randlon]],columns=[lat,lng]).astype(float)
    city_df = city_df.append(randomcity)
new_city_df = city_df.reset_index()

```


```python
city_df.head()

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
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-78.533106</td>
      <td>156.531638</td>
    </tr>
    <tr>
      <th>0</th>
      <td>-32.149709</td>
      <td>63.027527</td>
    </tr>
    <tr>
      <th>0</th>
      <td>22.590865</td>
      <td>60.044884</td>
    </tr>
    <tr>
      <th>0</th>
      <td>-87.166298</td>
      <td>134.993826</td>
    </tr>
    <tr>
      <th>0</th>
      <td>3.799089</td>
      <td>-172.185280</td>
    </tr>
  </tbody>
</table>
</div>




```python
mycities = []
counter = 0

while counter < 650:
    lat = new_city_df["Latitude"][counter]
    lng = new_city_df["Longitude"][counter]
    city = citipy.nearest_city(lat,lng)
    cityname = city.city_name
    country_code = city.country_code

    if cityname not in mycities:
        mycities.append([cityname,country_code,lat,lng])
    counter +=1
    
number_of_cities = len(mycities)
print(number_of_cities)
```

    650
    


```python
mycities = pd.DataFrame(mycities,columns=['City','Country','Latitude','Longitude'])
mycities
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-78.533106</td>
      <td>156.531638</td>
    </tr>
    <tr>
      <th>1</th>
      <td>souillac</td>
      <td>mu</td>
      <td>-32.149709</td>
      <td>63.027527</td>
    </tr>
    <tr>
      <th>2</th>
      <td>sur</td>
      <td>om</td>
      <td>22.590865</td>
      <td>60.044884</td>
    </tr>
    <tr>
      <th>3</th>
      <td>hobart</td>
      <td>au</td>
      <td>-87.166298</td>
      <td>134.993826</td>
    </tr>
    <tr>
      <th>4</th>
      <td>saleaula</td>
      <td>ws</td>
      <td>3.799089</td>
      <td>-172.185280</td>
    </tr>
    <tr>
      <th>5</th>
      <td>puerto princesa</td>
      <td>ph</td>
      <td>9.173430</td>
      <td>119.522781</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-65.228542</td>
      <td>157.003954</td>
    </tr>
    <tr>
      <th>7</th>
      <td>nadym</td>
      <td>ru</td>
      <td>64.754344</td>
      <td>72.614999</td>
    </tr>
    <tr>
      <th>8</th>
      <td>vaini</td>
      <td>to</td>
      <td>-65.135506</td>
      <td>-170.799764</td>
    </tr>
    <tr>
      <th>9</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-50.346107</td>
      <td>-126.241625</td>
    </tr>
    <tr>
      <th>10</th>
      <td>constitucion</td>
      <td>mx</td>
      <td>13.116350</td>
      <td>-128.192458</td>
    </tr>
    <tr>
      <th>11</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-3.358808</td>
      <td>-153.400505</td>
    </tr>
    <tr>
      <th>12</th>
      <td>khatanga</td>
      <td>ru</td>
      <td>72.535633</td>
      <td>98.230834</td>
    </tr>
    <tr>
      <th>13</th>
      <td>pevek</td>
      <td>ru</td>
      <td>85.395218</td>
      <td>166.791267</td>
    </tr>
    <tr>
      <th>14</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-85.703222</td>
      <td>52.718222</td>
    </tr>
    <tr>
      <th>15</th>
      <td>broome</td>
      <td>au</td>
      <td>-15.810206</td>
      <td>123.720580</td>
    </tr>
    <tr>
      <th>16</th>
      <td>yar-sale</td>
      <td>ru</td>
      <td>70.690356</td>
      <td>73.286975</td>
    </tr>
    <tr>
      <th>17</th>
      <td>kapaa</td>
      <td>us</td>
      <td>10.100514</td>
      <td>-168.218920</td>
    </tr>
    <tr>
      <th>18</th>
      <td>norman wells</td>
      <td>ca</td>
      <td>84.214985</td>
      <td>-117.068572</td>
    </tr>
    <tr>
      <th>19</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.894191</td>
      <td>17.067723</td>
    </tr>
    <tr>
      <th>20</th>
      <td>sukumo</td>
      <td>jp</td>
      <td>31.810758</td>
      <td>132.882614</td>
    </tr>
    <tr>
      <th>21</th>
      <td>chernyy yar</td>
      <td>ru</td>
      <td>47.928245</td>
      <td>45.867928</td>
    </tr>
    <tr>
      <th>22</th>
      <td>hobart</td>
      <td>au</td>
      <td>-59.910505</td>
      <td>149.883598</td>
    </tr>
    <tr>
      <th>23</th>
      <td>illoqqortoormiut</td>
      <td>gl</td>
      <td>86.261452</td>
      <td>-17.967623</td>
    </tr>
    <tr>
      <th>24</th>
      <td>port elizabeth</td>
      <td>za</td>
      <td>-55.108793</td>
      <td>29.292139</td>
    </tr>
    <tr>
      <th>25</th>
      <td>codrington</td>
      <td>ag</td>
      <td>25.214533</td>
      <td>-44.315424</td>
    </tr>
    <tr>
      <th>26</th>
      <td>hobart</td>
      <td>au</td>
      <td>-86.176501</td>
      <td>140.984077</td>
    </tr>
    <tr>
      <th>27</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-4.885917</td>
      <td>80.340187</td>
    </tr>
    <tr>
      <th>28</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-1.142985</td>
      <td>-153.437522</td>
    </tr>
    <tr>
      <th>29</th>
      <td>chapais</td>
      <td>ca</td>
      <td>49.248584</td>
      <td>-75.093825</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>620</th>
      <td>kapaa</td>
      <td>us</td>
      <td>14.605507</td>
      <td>-179.466261</td>
    </tr>
    <tr>
      <th>621</th>
      <td>riyadh</td>
      <td>sa</td>
      <td>25.712974</td>
      <td>48.006748</td>
    </tr>
    <tr>
      <th>622</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>14.276258</td>
      <td>-122.092956</td>
    </tr>
    <tr>
      <th>623</th>
      <td>komsomolskiy</td>
      <td>ru</td>
      <td>66.662030</td>
      <td>171.769525</td>
    </tr>
    <tr>
      <th>624</th>
      <td>pevek</td>
      <td>ru</td>
      <td>76.274421</td>
      <td>171.847570</td>
    </tr>
    <tr>
      <th>625</th>
      <td>biak</td>
      <td>id</td>
      <td>-1.174172</td>
      <td>138.416476</td>
    </tr>
    <tr>
      <th>626</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>71.982119</td>
      <td>146.057930</td>
    </tr>
    <tr>
      <th>627</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-47.733465</td>
      <td>-151.755503</td>
    </tr>
    <tr>
      <th>628</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td>-50.461454</td>
      <td>58.484173</td>
    </tr>
    <tr>
      <th>629</th>
      <td>mayo</td>
      <td>ca</td>
      <td>64.932732</td>
      <td>-138.099096</td>
    </tr>
    <tr>
      <th>630</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-80.898778</td>
      <td>-118.061485</td>
    </tr>
    <tr>
      <th>631</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-1.661261</td>
      <td>69.768811</td>
    </tr>
    <tr>
      <th>632</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-86.014614</td>
      <td>167.640742</td>
    </tr>
    <tr>
      <th>633</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-66.468737</td>
      <td>-57.381237</td>
    </tr>
    <tr>
      <th>634</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-88.881853</td>
      <td>-159.581487</td>
    </tr>
    <tr>
      <th>635</th>
      <td>port-cartier</td>
      <td>ca</td>
      <td>50.590148</td>
      <td>-67.114893</td>
    </tr>
    <tr>
      <th>636</th>
      <td>ribeira grande</td>
      <td>pt</td>
      <td>28.823514</td>
      <td>-40.313300</td>
    </tr>
    <tr>
      <th>637</th>
      <td>gzhatsk</td>
      <td>ru</td>
      <td>55.916195</td>
      <td>34.887285</td>
    </tr>
    <tr>
      <th>638</th>
      <td>beloha</td>
      <td>mg</td>
      <td>-26.585481</td>
      <td>43.429806</td>
    </tr>
    <tr>
      <th>639</th>
      <td>san angelo</td>
      <td>us</td>
      <td>31.481928</td>
      <td>-100.995152</td>
    </tr>
    <tr>
      <th>640</th>
      <td>belmonte</td>
      <td>br</td>
      <td>-16.741156</td>
      <td>-34.857529</td>
    </tr>
    <tr>
      <th>641</th>
      <td>arraial do cabo</td>
      <td>br</td>
      <td>-36.719489</td>
      <td>-23.192238</td>
    </tr>
    <tr>
      <th>642</th>
      <td>pevek</td>
      <td>ru</td>
      <td>89.900682</td>
      <td>170.126555</td>
    </tr>
    <tr>
      <th>643</th>
      <td>batemans bay</td>
      <td>au</td>
      <td>-39.326591</td>
      <td>151.752214</td>
    </tr>
    <tr>
      <th>644</th>
      <td>jamestown</td>
      <td>sh</td>
      <td>-17.618744</td>
      <td>-14.381594</td>
    </tr>
    <tr>
      <th>645</th>
      <td>yumen</td>
      <td>cn</td>
      <td>36.439571</td>
      <td>95.452783</td>
    </tr>
    <tr>
      <th>646</th>
      <td>barrow</td>
      <td>us</td>
      <td>73.292521</td>
      <td>-154.353817</td>
    </tr>
    <tr>
      <th>647</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-7.194384</td>
      <td>-127.710126</td>
    </tr>
    <tr>
      <th>648</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.422967</td>
      <td>18.908394</td>
    </tr>
    <tr>
      <th>649</th>
      <td>port lincoln</td>
      <td>au</td>
      <td>-37.057553</td>
      <td>135.209426</td>
    </tr>
  </tbody>
</table>
<p>650 rows × 4 columns</p>
</div>




```python
weather= []
```


```python
params = {'appid': mykey,
          'q': '',
          'units': 'imperial'}


for index, row in mycities.iterrows():
    params['q'] = row.City + ',' + row.Country
    print("Processing record " + str(index + 1) + " of " + str(number_of_cities) + " | " + row.City)
    print(url + "?units=" + params['units'] + "&APPID=" + params['appid'] + "&q=" + row.City.replace(" ", "%20") + ',' + row.Country)
    response = req.get(url, params=params).json()
    weather.append(response)
```

    Processing record 1 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 2 of 650 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=souillac,mu
    Processing record 3 of 650 | sur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sur,om
    Processing record 4 of 650 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hobart,au
    Processing record 5 of 650 | saleaula
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saleaula,ws
    Processing record 6 of 650 | puerto princesa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20princesa,ph
    Processing record 7 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 8 of 650 | nadym
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nadym,ru
    Processing record 9 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 10 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 11 of 650 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=constitucion,mx
    Processing record 12 of 650 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=faanui,pf
    Processing record 13 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 14 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 15 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 16 of 650 | broome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=broome,au
    Processing record 17 of 650 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yar-sale,ru
    Processing record 18 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 19 of 650 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=norman%20wells,ca
    Processing record 20 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 21 of 650 | sukumo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sukumo,jp
    Processing record 22 of 650 | chernyy yar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chernyy%20yar,ru
    Processing record 23 of 650 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hobart,au
    Processing record 24 of 650 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=illoqqortoormiut,gl
    Processing record 25 of 650 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20elizabeth,za
    Processing record 26 of 650 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=codrington,ag
    Processing record 27 of 650 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hobart,au
    Processing record 28 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 29 of 650 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=faanui,pf
    Processing record 30 of 650 | chapais
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chapais,ca
    Processing record 31 of 650 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tuktoyaktuk,ca
    Processing record 32 of 650 | aberdeen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=aberdeen,us
    Processing record 33 of 650 | mongo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mongo,td
    Processing record 34 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 35 of 650 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yar-sale,ru
    Processing record 36 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 37 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 38 of 650 | wahiawa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=wahiawa,us
    Processing record 39 of 650 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=los%20llanos%20de%20aridane,es
    Processing record 40 of 650 | salalah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=salalah,om
    Processing record 41 of 650 | parakai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=parakai,nz
    Processing record 42 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 43 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 44 of 650 | alvaraes
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=alvaraes,br
    Processing record 45 of 650 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castro,cl
    Processing record 46 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 47 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 48 of 650 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-philippe,re
    Processing record 49 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 50 of 650 | san rafael
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20rafael,ar
    Processing record 51 of 650 | ust-kamchatsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ust-kamchatsk,ru
    Processing record 52 of 650 | saint-ambroise
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-ambroise,ca
    Processing record 53 of 650 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=norman%20wells,ca
    Processing record 54 of 650 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tiksi,ru
    Processing record 55 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 56 of 650 | mecca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mecca,sa
    Processing record 57 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 58 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 59 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 60 of 650 | revelstoke
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=revelstoke,ca
    Processing record 61 of 650 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bambous%20virieux,mu
    Processing record 62 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 63 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 64 of 650 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=longyearbyen,sj
    Processing record 65 of 650 | atar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atar,mr
    Processing record 66 of 650 | celestun
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=celestun,mx
    Processing record 67 of 650 | astipalaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=astipalaia,gr
    Processing record 68 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 69 of 650 | sitka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sitka,us
    Processing record 70 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 71 of 650 | roald
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=roald,no
    Processing record 72 of 650 | abashiri
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=abashiri,jp
    Processing record 73 of 650 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qaanaaq,gl
    Processing record 74 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 75 of 650 | aksarka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=aksarka,ru
    Processing record 76 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 77 of 650 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cidreira,br
    Processing record 78 of 650 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arraial%20do%20cabo,br
    Processing record 79 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 80 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 81 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 82 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 83 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 84 of 650 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hamilton,us
    Processing record 85 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 86 of 650 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=grand%20gaube,mu
    Processing record 87 of 650 | sisophon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sisophon,kh
    Processing record 88 of 650 | panaba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=panaba,mx
    Processing record 89 of 650 | darasun
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=darasun,ru
    Processing record 90 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 91 of 650 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taoudenni,ml
    Processing record 92 of 650 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=umzimvubu,za
    Processing record 93 of 650 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaitupu,wf
    Processing record 94 of 650 | tessalit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tessalit,ml
    Processing record 95 of 650 | bryan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bryan,us
    Processing record 96 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 97 of 650 | saleaula
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saleaula,ws
    Processing record 98 of 650 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chokurdakh,ru
    Processing record 99 of 650 | krasnyy yar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=krasnyy%20yar,ru
    Processing record 100 of 650 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cherskiy,ru
    Processing record 101 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 102 of 650 | mecca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mecca,sa
    Processing record 103 of 650 | hammerfest
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hammerfest,no
    Processing record 104 of 650 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=guerrero%20negro,mx
    Processing record 105 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 106 of 650 | airai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=airai,pw
    Processing record 107 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 108 of 650 | piacabucu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=piacabucu,br
    Processing record 109 of 650 | ust-barguzin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ust-barguzin,ru
    Processing record 110 of 650 | tarazona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tarazona,es
    Processing record 111 of 650 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tuktoyaktuk,ca
    Processing record 112 of 650 | bilma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bilma,ne
    Processing record 113 of 650 | palmares do sul
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=palmares%20do%20sul,br
    Processing record 114 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 115 of 650 | labuhan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=labuhan,id
    Processing record 116 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 117 of 650 | kupino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kupino,ru
    Processing record 118 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 119 of 650 | tarudant
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tarudant,ma
    Processing record 120 of 650 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lavrentiya,ru
    Processing record 121 of 650 | chernaya kholunitsa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chernaya%20kholunitsa,ru
    Processing record 122 of 650 | balkhash
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=balkhash,kz
    Processing record 123 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 124 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 125 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 126 of 650 | solnechnyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=solnechnyy,ru
    Processing record 127 of 650 | san andres
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20andres,co
    Processing record 128 of 650 | cintalapa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cintalapa,mx
    Processing record 129 of 650 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=isangel,vu
    Processing record 130 of 650 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hamilton,bm
    Processing record 131 of 650 | tahta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tahta,eg
    Processing record 132 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 133 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 134 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 135 of 650 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castro,cl
    Processing record 136 of 650 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ribeira%20grande,pt
    Processing record 137 of 650 | caravelas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=caravelas,br
    Processing record 138 of 650 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cidreira,br
    Processing record 139 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 140 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 141 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 142 of 650 | peterhead
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=peterhead,gb
    Processing record 143 of 650 | lagoa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lagoa,pt
    Processing record 144 of 650 | panzhihua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=panzhihua,cn
    Processing record 145 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 146 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 147 of 650 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=praia%20da%20vitoria,pt
    Processing record 148 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 149 of 650 | rottenmann
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rottenmann,at
    Processing record 150 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 151 of 650 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=codrington,ag
    Processing record 152 of 650 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=souillac,mu
    Processing record 153 of 650 | zaysan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=zaysan,kz
    Processing record 154 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 155 of 650 | hovd
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hovd,mn
    Processing record 156 of 650 | mayo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mayo,ca
    Processing record 157 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 158 of 650 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=geraldton,au
    Processing record 159 of 650 | manzhouli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=manzhouli,cn
    Processing record 160 of 650 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barrow,us
    Processing record 161 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 162 of 650 | sakakah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sakakah,sa
    Processing record 163 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 164 of 650 | kilindoni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kilindoni,tz
    Processing record 165 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 166 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 167 of 650 | broome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=broome,au
    Processing record 168 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 169 of 650 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castro,cl
    Processing record 170 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 171 of 650 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bandarbeyla,so
    Processing record 172 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 173 of 650 | airai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=airai,pw
    Processing record 174 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 175 of 650 | lewistown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lewistown,us
    Processing record 176 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 177 of 650 | hambantota
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hambantota,lk
    Processing record 178 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 179 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 180 of 650 | alappuzha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=alappuzha,in
    Processing record 181 of 650 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nikolskoye,ru
    Processing record 182 of 650 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=illoqqortoormiut,gl
    Processing record 183 of 650 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=okhotsk,ru
    Processing record 184 of 650 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=quatre%20cocos,mu
    Processing record 185 of 650 | louisbourg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=louisbourg,ca
    Processing record 186 of 650 | estelle
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=estelle,us
    Processing record 187 of 650 | joshimath
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=joshimath,in
    Processing record 188 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 189 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 190 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 191 of 650 | bolungarvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bolungarvik,is
    Processing record 192 of 650 | qinhuangdao
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qinhuangdao,cn
    Processing record 193 of 650 | dalbandin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=dalbandin,pk
    Processing record 194 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 195 of 650 | atambua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atambua,id
    Processing record 196 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 197 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 198 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 199 of 650 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ribeira%20grande,pt
    Processing record 200 of 650 | tahlequah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tahlequah,us
    Processing record 201 of 650 | dunedin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=dunedin,nz
    Processing record 202 of 650 | pangody
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pangody,ru
    Processing record 203 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 204 of 650 | charters towers
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=charters%20towers,au
    Processing record 205 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 206 of 650 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=georgetown,sh
    Processing record 207 of 650 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saskylakh,ru
    Processing record 208 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 209 of 650 | namibe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=namibe,ao
    Processing record 210 of 650 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=skalistyy,ru
    Processing record 211 of 650 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=belushya%20guba,ru
    Processing record 212 of 650 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=thompson,ca
    Processing record 213 of 650 | borgarnes
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=borgarnes,is
    Processing record 214 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 215 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 216 of 650 | husavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=husavik,is
    Processing record 217 of 650 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tuktoyaktuk,ca
    Processing record 218 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 219 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 220 of 650 | acapulco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=acapulco,mx
    Processing record 221 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 222 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 223 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 224 of 650 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tuktoyaktuk,ca
    Processing record 225 of 650 | manoel urbano
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=manoel%20urbano,br
    Processing record 226 of 650 | kholtoson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kholtoson,ru
    Processing record 227 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 228 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 229 of 650 | buala
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=buala,sb
    Processing record 230 of 650 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=faanui,pf
    Processing record 231 of 650 | jedrzejow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jedrzejow,pl
    Processing record 232 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 233 of 650 | nyurba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nyurba,ru
    Processing record 234 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 235 of 650 | biharamulo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=biharamulo,tz
    Processing record 236 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 237 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 238 of 650 | broken hill
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=broken%20hill,au
    Processing record 239 of 650 | bonthe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bonthe,sl
    Processing record 240 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 241 of 650 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qaanaaq,gl
    Processing record 242 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 243 of 650 | karia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=karia,gr
    Processing record 244 of 650 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sao%20filipe,cv
    Processing record 245 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 246 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 247 of 650 | robe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=robe,et
    Processing record 248 of 650 | alekseyevka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=alekseyevka,ru
    Processing record 249 of 650 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bengkulu,id
    Processing record 250 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 251 of 650 | blagoyevo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=blagoyevo,ru
    Processing record 252 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 253 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 254 of 650 | nortelandia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nortelandia,br
    Processing record 255 of 650 | samusu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=samusu,ws
    Processing record 256 of 650 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=beringovskiy,ru
    Processing record 257 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 258 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 259 of 650 | riberalta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=riberalta,bo
    Processing record 260 of 650 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=narsaq,gl
    Processing record 261 of 650 | never
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=never,ru
    Processing record 262 of 650 | sur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sur,om
    Processing record 263 of 650 | ekhabi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ekhabi,ru
    Processing record 264 of 650 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=coquimbo,cl
    Processing record 265 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 266 of 650 | khash
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khash,ir
    Processing record 267 of 650 | grootfontein
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=grootfontein,na
    Processing record 268 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 269 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 270 of 650 | miranda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=miranda,br
    Processing record 271 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 272 of 650 | jardim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jardim,br
    Processing record 273 of 650 | dali
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=dali,cn
    Processing record 274 of 650 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barentsburg,sj
    Processing record 275 of 650 | barcelona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barcelona,ve
    Processing record 276 of 650 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=geraldton,au
    Processing record 277 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 278 of 650 | plouzane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=plouzane,fr
    Processing record 279 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 280 of 650 | avera
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avera,pf
    Processing record 281 of 650 | san jose
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20jose,gt
    Processing record 282 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 283 of 650 | xining
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=xining,cn
    Processing record 284 of 650 | faya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=faya,td
    Processing record 285 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 286 of 650 | senneterre
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=senneterre,ca
    Processing record 287 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 288 of 650 | grafton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=grafton,au
    Processing record 289 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 290 of 650 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qaanaaq,gl
    Processing record 291 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 292 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 293 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 294 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 295 of 650 | karpathos
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=karpathos,gr
    Processing record 296 of 650 | longhua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=longhua,cn
    Processing record 297 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 298 of 650 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arraial%20do%20cabo,br
    Processing record 299 of 650 | samarai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=samarai,pg
    Processing record 300 of 650 | castletown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castletown,gb
    Processing record 301 of 650 | beloha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=beloha,mg
    Processing record 302 of 650 | perdika
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=perdika,gr
    Processing record 303 of 650 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-philippe,re
    Processing record 304 of 650 | sterling
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sterling,us
    Processing record 305 of 650 | rawson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rawson,ar
    Processing record 306 of 650 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saldanha,za
    Processing record 307 of 650 | likasi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=likasi,cd
    Processing record 308 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 309 of 650 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=georgetown,sh
    Processing record 310 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 311 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 312 of 650 | port hardy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20hardy,ca
    Processing record 313 of 650 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nikolskoye,ru
    Processing record 314 of 650 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sentyabrskiy,ru
    Processing record 315 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 316 of 650 | tingrela
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tingrela,ci
    Processing record 317 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 318 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 319 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 320 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 321 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 322 of 650 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=belushya%20guba,ru
    Processing record 323 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 324 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 325 of 650 | port hawkesbury
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20hawkesbury,ca
    Processing record 326 of 650 | lota
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lota,cl
    Processing record 327 of 650 | port hedland
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20hedland,au
    Processing record 328 of 650 | komsomolskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=komsomolskoye,ua
    Processing record 329 of 650 | trinidad
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=trinidad,bo
    Processing record 330 of 650 | airai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=airai,pw
    Processing record 331 of 650 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barrow,us
    Processing record 332 of 650 | watertown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=watertown,us
    Processing record 333 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 334 of 650 | puerto cabezas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20cabezas,ni
    Processing record 335 of 650 | honiara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=honiara,sb
    Processing record 336 of 650 | wajima
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=wajima,jp
    Processing record 337 of 650 | tazovskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tazovskiy,ru
    Processing record 338 of 650 | monrovia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=monrovia,lr
    Processing record 339 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 340 of 650 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=praia%20da%20vitoria,pt
    Processing record 341 of 650 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tasiilaq,gl
    Processing record 342 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 343 of 650 | arequipa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arequipa,pe
    Processing record 344 of 650 | omboue
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=omboue,ga
    Processing record 345 of 650 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bambous%20virieux,mu
    Processing record 346 of 650 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mar%20del%20plata,ar
    Processing record 347 of 650 | east london
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=east%20london,za
    Processing record 348 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 349 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 350 of 650 | novokayakent
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=novokayakent,ru
    Processing record 351 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 352 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 353 of 650 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=georgetown,sh
    Processing record 354 of 650 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tsihombe,mg
    Processing record 355 of 650 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=provideniya,ru
    Processing record 356 of 650 | warqla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=warqla,dz
    Processing record 357 of 650 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaitupu,wf
    Processing record 358 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 359 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 360 of 650 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=fairbanks,us
    Processing record 361 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 362 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 363 of 650 | azimur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=azimur,ma
    Processing record 364 of 650 | carauari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carauari,br
    Processing record 365 of 650 | poum
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=poum,nc
    Processing record 366 of 650 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=severo-kurilsk,ru
    Processing record 367 of 650 | tevriz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tevriz,ru
    Processing record 368 of 650 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bambous%20virieux,mu
    Processing record 369 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 370 of 650 | arman
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arman,ru
    Processing record 371 of 650 | chingola
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chingola,zm
    Processing record 372 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 373 of 650 | fevralsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=fevralsk,ru
    Processing record 374 of 650 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ilulissat,gl
    Processing record 375 of 650 | acapulco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=acapulco,mx
    Processing record 376 of 650 | saint george
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint%20george,bm
    Processing record 377 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 378 of 650 | bayji
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bayji,iq
    Processing record 379 of 650 | east london
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=east%20london,za
    Processing record 380 of 650 | port blair
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20blair,in
    Processing record 381 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 382 of 650 | necochea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=necochea,ar
    Processing record 383 of 650 | portree
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=portree,gb
    Processing record 384 of 650 | puerto suarez
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20suarez,bo
    Processing record 385 of 650 | seoul
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=seoul,kr
    Processing record 386 of 650 | tiarei
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tiarei,pf
    Processing record 387 of 650 | gat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=gat,ly
    Processing record 388 of 650 | dinguiraye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=dinguiraye,gn
    Processing record 389 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 390 of 650 | ternate
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ternate,id
    Processing record 391 of 650 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20cristobal,ec
    Processing record 392 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 393 of 650 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint%20anthony,ca
    Processing record 394 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 395 of 650 | tome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tome,cl
    Processing record 396 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 397 of 650 | ust-tsilma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ust-tsilma,ru
    Processing record 398 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 399 of 650 | ramshir
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ramshir,ir
    Processing record 400 of 650 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saldanha,za
    Processing record 401 of 650 | rudnogorsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rudnogorsk,ru
    Processing record 402 of 650 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vila%20franca%20do%20campo,pt
    Processing record 403 of 650 | chany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chany,ru
    Processing record 404 of 650 | bandar-e torkaman
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bandar-e%20torkaman,ir
    Processing record 405 of 650 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mys%20shmidta,ru
    Processing record 406 of 650 | minsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=minsk,by
    Processing record 407 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 408 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 409 of 650 | ekhabi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ekhabi,ru
    Processing record 410 of 650 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-philippe,re
    Processing record 411 of 650 | zheleznodorozhnyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=zheleznodorozhnyy,ru
    Processing record 412 of 650 | deniliquin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=deniliquin,au
    Processing record 413 of 650 | sajoszentpeter
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sajoszentpeter,hu
    Processing record 414 of 650 | krasnoslobodsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=krasnoslobodsk,ru
    Processing record 415 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 416 of 650 | samarai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=samarai,pg
    Processing record 417 of 650 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chokurdakh,ru
    Processing record 418 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 419 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 420 of 650 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castro,cl
    Processing record 421 of 650 | kungurtug
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kungurtug,ru
    Processing record 422 of 650 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=severo-kurilsk,ru
    Processing record 423 of 650 | singkawang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=singkawang,id
    Processing record 424 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 425 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 426 of 650 | salym
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=salym,ru
    Processing record 427 of 650 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20patricio,mx
    Processing record 428 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 429 of 650 | romitan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=romitan,uz
    Processing record 430 of 650 | tura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tura,ru
    Processing record 431 of 650 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arraial%20do%20cabo,br
    Processing record 432 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 433 of 650 | cape town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cape%20town,za
    Processing record 434 of 650 | mantua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mantua,cu
    Processing record 435 of 650 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hobart,au
    Processing record 436 of 650 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ostrovnoy,ru
    Processing record 437 of 650 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tsihombe,mg
    Processing record 438 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 439 of 650 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vila%20velha,br
    Processing record 440 of 650 | mehamn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mehamn,no
    Processing record 441 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 442 of 650 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=norman%20wells,ca
    Processing record 443 of 650 | fortuna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=fortuna,us
    Processing record 444 of 650 | pisco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pisco,pe
    Processing record 445 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 446 of 650 | muzhi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=muzhi,ru
    Processing record 447 of 650 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barentsburg,sj
    Processing record 448 of 650 | rungata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rungata,ki
    Processing record 449 of 650 | porto velho
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=porto%20velho,br
    Processing record 450 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 451 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 452 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 453 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 454 of 650 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cidreira,br
    Processing record 455 of 650 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=severo-kurilsk,ru
    Processing record 456 of 650 | louisbourg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=louisbourg,ca
    Processing record 457 of 650 | saurimo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saurimo,ao
    Processing record 458 of 650 | doha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=doha,kw
    Processing record 459 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 460 of 650 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sao%20joao%20da%20barra,br
    Processing record 461 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 462 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 463 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 464 of 650 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=provideniya,ru
    Processing record 465 of 650 | langxiang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=langxiang,cn
    Processing record 466 of 650 | salvador
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=salvador,br
    Processing record 467 of 650 | middle island
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=middle%20island,kn
    Processing record 468 of 650 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=richards%20bay,za
    Processing record 469 of 650 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kodiak,us
    Processing record 470 of 650 | east london
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=east%20london,za
    Processing record 471 of 650 | nguruka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nguruka,tz
    Processing record 472 of 650 | malakal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=malakal,sd
    Processing record 473 of 650 | piacabucu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=piacabucu,br
    Processing record 474 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 475 of 650 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hobart,au
    Processing record 476 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 477 of 650 | great falls
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=great%20falls,us
    Processing record 478 of 650 | macamic
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=macamic,ca
    Processing record 479 of 650 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hasaki,jp
    Processing record 480 of 650 | mrirt
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mrirt,ma
    Processing record 481 of 650 | nizhniy kuranakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nizhniy%20kuranakh,ru
    Processing record 482 of 650 | buala
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=buala,sb
    Processing record 483 of 650 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-francois,gp
    Processing record 484 of 650 | nome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nome,us
    Processing record 485 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 486 of 650 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=castro,cl
    Processing record 487 of 650 | voskresenskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=voskresenskoye,ru
    Processing record 488 of 650 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tasiilaq,gl
    Processing record 489 of 650 | izhma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=izhma,ru
    Processing record 490 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 491 of 650 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qaanaaq,gl
    Processing record 492 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 493 of 650 | shelburne
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=shelburne,ca
    Processing record 494 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 495 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 496 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 497 of 650 | chuy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chuy,uy
    Processing record 498 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 499 of 650 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hasaki,jp
    Processing record 500 of 650 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cabedelo,br
    Processing record 501 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 502 of 650 | nokaneng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nokaneng,bw
    Processing record 503 of 650 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sao%20filipe,cv
    Processing record 504 of 650 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mar%20del%20plata,ar
    Processing record 505 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 506 of 650 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nizhneyansk,ru
    Processing record 507 of 650 | nemuro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nemuro,jp
    Processing record 508 of 650 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20patricio,mx
    Processing record 509 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 510 of 650 | itarema
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=itarema,br
    Processing record 511 of 650 | craigieburn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=craigieburn,au
    Processing record 512 of 650 | sadiqabad
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sadiqabad,pk
    Processing record 513 of 650 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=carnarvon,au
    Processing record 514 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 515 of 650 | haian
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=haian,cn
    Processing record 516 of 650 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=komsomolskiy,ru
    Processing record 517 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 518 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 519 of 650 | dauphin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=dauphin,ca
    Processing record 520 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 521 of 650 | methoni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=methoni,gr
    Processing record 522 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 523 of 650 | saurimo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saurimo,ao
    Processing record 524 of 650 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=narsaq,gl
    Processing record 525 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 526 of 650 | chicama
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chicama,pe
    Processing record 527 of 650 | vao
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vao,nc
    Processing record 528 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 529 of 650 | losal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=losal,in
    Processing record 530 of 650 | paradwip
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=paradwip,in
    Processing record 531 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 532 of 650 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=rikitea,pf
    Processing record 533 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 534 of 650 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=provideniya,ru
    Processing record 535 of 650 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saskylakh,ru
    Processing record 536 of 650 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vaini,to
    Processing record 537 of 650 | guanhaes
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=guanhaes,br
    Processing record 538 of 650 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=esperance,au
    Processing record 539 of 650 | sawakin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sawakin,sd
    Processing record 540 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 541 of 650 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mys%20shmidta,ru
    Processing record 542 of 650 | burns lake
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=burns%20lake,ca
    Processing record 543 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 544 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 545 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 546 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 547 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 548 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 549 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 550 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 551 of 650 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yellowknife,ca
    Processing record 552 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 553 of 650 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=prince%20rupert,ca
    Processing record 554 of 650 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ambilobe,mg
    Processing record 555 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 556 of 650 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cherskiy,ru
    Processing record 557 of 650 | fasa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=fasa,ir
    Processing record 558 of 650 | olindina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=olindina,br
    Processing record 559 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 560 of 650 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=sao%20joao%20da%20barra,br
    Processing record 561 of 650 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hilo,us
    Processing record 562 of 650 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=new%20norfolk,au
    Processing record 563 of 650 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kaitangata,nz
    Processing record 564 of 650 | bushenyi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bushenyi,ug
    Processing record 565 of 650 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=clyde%20river,ca
    Processing record 566 of 650 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cabo%20san%20lucas,mx
    Processing record 567 of 650 | endicott
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=endicott,us
    Processing record 568 of 650 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=illoqqortoormiut,gl
    Processing record 569 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 570 of 650 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=torbay,ca
    Processing record 571 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 572 of 650 | novovarshavka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=novovarshavka,ru
    Processing record 573 of 650 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=butaritari,ki
    Processing record 574 of 650 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hermanus,za
    Processing record 575 of 650 | kuche
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kuche,cn
    Processing record 576 of 650 | vallenar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vallenar,cl
    Processing record 577 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 578 of 650 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lebu,cl
    Processing record 579 of 650 | noumea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=noumea,nc
    Processing record 580 of 650 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=batemans%20bay,au
    Processing record 581 of 650 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=victoria,sc
    Processing record 582 of 650 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=puerto%20ayora,ec
    Processing record 583 of 650 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nizhneyansk,ru
    Processing record 584 of 650 | vicente guerrero
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=vicente%20guerrero,mx
    Processing record 585 of 650 | zhezkazgan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=zhezkazgan,kz
    Processing record 586 of 650 | mount pleasant
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mount%20pleasant,us
    Processing record 587 of 650 | porterville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=porterville,us
    Processing record 588 of 650 | muros
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=muros,es
    Processing record 589 of 650 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=nanortalik,gl
    Processing record 590 of 650 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=guerrero%20negro,mx
    Processing record 591 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 592 of 650 | bajil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bajil,ye
    Processing record 593 of 650 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20alfred,za
    Processing record 594 of 650 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=busselton,au
    Processing record 595 of 650 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tsihombe,mg
    Processing record 596 of 650 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tiksi,ru
    Processing record 597 of 650 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=albany,au
    Processing record 598 of 650 | taksimo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taksimo,ru
    Processing record 599 of 650 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=saint-philippe,re
    Processing record 600 of 650 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=avarua,ck
    Processing record 601 of 650 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=qaanaaq,gl
    Processing record 602 of 650 | ballina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ballina,au
    Processing record 603 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 604 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 605 of 650 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=lebu,cl
    Processing record 606 of 650 | oranjestad
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=oranjestad,aw
    Processing record 607 of 650 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=leningradskiy,ru
    Processing record 608 of 650 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=tasiilaq,gl
    Processing record 609 of 650 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=khatanga,ru
    Processing record 610 of 650 | filadelfia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=filadelfia,py
    Processing record 611 of 650 | paamiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=paamiut,gl
    Processing record 612 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 613 of 650 | oparino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=oparino,ru
    Processing record 614 of 650 | itarema
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=itarema,br
    Processing record 615 of 650 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barentsburg,sj
    Processing record 616 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 617 of 650 | north bend
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=north%20bend,us
    Processing record 618 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 619 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 620 of 650 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chokurdakh,ru
    Processing record 621 of 650 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=kapaa,us
    Processing record 622 of 650 | riyadh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=riyadh,sa
    Processing record 623 of 650 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=cabo%20san%20lucas,mx
    Processing record 624 of 650 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=komsomolskiy,ru
    Processing record 625 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 626 of 650 | biak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=biak,id
    Processing record 627 of 650 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=chokurdakh,ru
    Processing record 628 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 629 of 650 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=taolanaro,mg
    Processing record 630 of 650 | mayo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mayo,ca
    Processing record 631 of 650 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=punta%20arenas,cl
    Processing record 632 of 650 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=hithadhoo,mv
    Processing record 633 of 650 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bluff,nz
    Processing record 634 of 650 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ushuaia,ar
    Processing record 635 of 650 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=mataura,pf
    Processing record 636 of 650 | port-cartier
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port-cartier,ca
    Processing record 637 of 650 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=ribeira%20grande,pt
    Processing record 638 of 650 | gzhatsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=gzhatsk,ru
    Processing record 639 of 650 | beloha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=beloha,mg
    Processing record 640 of 650 | san angelo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=san%20angelo,us
    Processing record 641 of 650 | belmonte
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=belmonte,br
    Processing record 642 of 650 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=arraial%20do%20cabo,br
    Processing record 643 of 650 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=pevek,ru
    Processing record 644 of 650 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=batemans%20bay,au
    Processing record 645 of 650 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=jamestown,sh
    Processing record 646 of 650 | yumen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=yumen,cn
    Processing record 647 of 650 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=barrow,us
    Processing record 648 of 650 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=atuona,pf
    Processing record 649 of 650 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=bredasdorp,za
    Processing record 650 of 650 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=ab5d135be77e22bb52e5cdd1e43a09e2&q=port%20lincoln,au
    


```python
# Create new df empty column names for the weather
mycities["Temperature"]= ""
mycities["Humidity"]=""
mycities["Cloudliness"]= ""
mycities["Wind_Speed"]=""

count_rows= 0
```


```python
params = {"q":mycities['City']+","+mycities['Country'],"units":"IMPERIAL","mode":"json","appid":mykey}
```


```python
for index, row in mycities.iterrows():
    
    params2 = {"q":mycities.loc[index]['City']+","+mycities.loc[index]['Country'],"units":"IMPERIAL","mode":"json","appid":mykey}
    
    print("Processing record "+ (mycities.loc[index]['City'])+" "+"City "+str(index+1)+" of "+str(len(mycities)))
    count_rows +=1
    print(url)
    

    testcity = req.get(url,params=params2).json()
    try:
        city_temp= testcity["main"]["temp"]
        city_humidity = testcity["main"]["humidity"]
        city_cloudliness = testcity["clouds"]["all"]
        city_windspeed = testcity["wind"]["speed"]
    
        mycities.set_value(index,"Temperature",city_temp)
        mycities.set_value(index,"Humidity",city_humidity)
        mycities.set_value(index,"Cloudliness", city_cloudliness)
        mycities.set_value(index,"Wind_Speed",city_windspeed)
    except:
        print("Not found in city data, Skipping this City")
        continue
    
print([city_temp,city_humidity,city_cloudliness,city_windspeed])
mycities
```

    Processing record bluff City 1 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record souillac City 2 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sur City 3 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hobart City 4 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saleaula City 5 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record puerto princesa City 6 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 7 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nadym City 8 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 9 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 10 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record constitucion City 11 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record faanui City 12 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 13 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 14 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port alfred City 15 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record broome City 16 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yar-sale City 17 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 18 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record norman wells City 19 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bredasdorp City 20 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sukumo City 21 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chernyy yar City 22 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hobart City 23 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record illoqqortoormiut City 24 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record port elizabeth City 25 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record codrington City 26 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record hobart City 27 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 28 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record faanui City 29 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chapais City 30 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tuktoyaktuk City 31 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record aberdeen City 32 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mongo City 33 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port alfred City 34 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yar-sale City 35 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 36 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 37 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record wahiawa City 38 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record los llanos de aridane City 39 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record salalah City 40 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record parakai City 41 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 42 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 43 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record alvaraes City 44 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castro City 45 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 46 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 47 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saint-philippe City 48 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 49 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san rafael City 50 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ust-kamchatsk City 51 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record saint-ambroise City 52 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record norman wells City 53 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tiksi City 54 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 55 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mecca City 56 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 57 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto ayora City 58 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 59 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record revelstoke City 60 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bambous virieux City 61 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 62 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 63 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record longyearbyen City 64 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atar City 65 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record celestun City 66 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record astipalaia City 67 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record punta arenas City 68 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sitka City 69 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 70 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record roald City 71 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record abashiri City 72 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record qaanaaq City 73 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 74 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record aksarka City 75 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 76 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cidreira City 77 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record arraial do cabo City 78 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port alfred City 79 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 80 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 81 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 82 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 83 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hamilton City 84 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 85 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record grand gaube City 86 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sisophon City 87 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record panaba City 88 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record darasun City 89 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 90 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taoudenni City 91 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record umzimvubu City 92 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record vaitupu City 93 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record tessalit City 94 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bryan City 95 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 96 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saleaula City 97 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record chokurdakh City 98 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record krasnyy yar City 99 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cherskiy City 100 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 101 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mecca City 102 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hammerfest City 103 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record guerrero negro City 104 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 105 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record airai City 106 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record atuona City 107 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record piacabucu City 108 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ust-barguzin City 109 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tarazona City 110 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tuktoyaktuk City 111 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bilma City 112 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record palmares do sul City 113 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 114 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record labuhan City 115 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 116 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kupino City 117 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 118 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record tarudant City 119 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record lavrentiya City 120 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chernaya kholunitsa City 121 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record balkhash City 122 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record bredasdorp City 123 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 124 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 125 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record solnechnyy City 126 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san andres City 127 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cintalapa City 128 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record isangel City 129 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hamilton City 130 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tahta City 131 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 132 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record kapaa City 133 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 134 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castro City 135 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ribeira grande City 136 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record caravelas City 137 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cidreira City 138 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 139 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 140 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 141 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record peterhead City 142 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record lagoa City 143 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record panzhihua City 144 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 145 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 146 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record praia da vitoria City 147 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bredasdorp City 148 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rottenmann City 149 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 150 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record codrington City 151 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record souillac City 152 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record zaysan City 153 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 154 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hovd City 155 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mayo City 156 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 157 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record geraldton City 158 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record manzhouli City 159 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record barrow City 160 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 161 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sakakah City 162 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record kodiak City 163 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kilindoni City 164 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 165 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 166 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record broome City 167 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 168 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castro City 169 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 170 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bandarbeyla City 171 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 172 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record airai City 173 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record ushuaia City 174 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record lewistown City 175 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 176 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record hambantota City 177 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 178 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 179 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record alappuzha City 180 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record nikolskoye City 181 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record illoqqortoormiut City 182 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record okhotsk City 183 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record quatre cocos City 184 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record louisbourg City 185 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record estelle City 186 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record joshimath City 187 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 188 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 189 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 190 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bolungarvik City 191 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record qinhuangdao City 192 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record dalbandin City 193 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 194 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atambua City 195 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 196 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 197 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto ayora City 198 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ribeira grande City 199 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tahlequah City 200 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record dunedin City 201 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pangody City 202 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 203 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record charters towers City 204 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 205 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record georgetown City 206 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saskylakh City 207 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 208 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record namibe City 209 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record skalistyy City 210 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record belushya guba City 211 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record thompson City 212 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record borgarnes City 213 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 214 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 215 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record husavik City 216 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tuktoyaktuk City 217 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 218 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 219 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record acapulco City 220 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record avarua City 221 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 222 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 223 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tuktoyaktuk City 224 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record manoel urbano City 225 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kholtoson City 226 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 227 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record bredasdorp City 228 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record buala City 229 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record faanui City 230 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jedrzejow City 231 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 232 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nyurba City 233 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 234 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record biharamulo City 235 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 236 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 237 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record broken hill City 238 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bonthe City 239 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 240 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record qaanaaq City 241 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 242 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record karia City 243 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record sao filipe City 244 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 245 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 246 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record robe City 247 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record alekseyevka City 248 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bengkulu City 249 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 250 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record blagoyevo City 251 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 252 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 253 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nortelandia City 254 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record samusu City 255 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record beringovskiy City 256 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 257 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 258 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record riberalta City 259 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record narsaq City 260 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record never City 261 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sur City 262 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ekhabi City 263 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record coquimbo City 264 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 265 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khash City 266 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record grootfontein City 267 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 268 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto ayora City 269 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record miranda City 270 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 271 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record jardim City 272 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record dali City 273 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record barentsburg City 274 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record barcelona City 275 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record geraldton City 276 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 277 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record plouzane City 278 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 279 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avera City 280 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record san jose City 281 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 282 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record xining City 283 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record faya City 284 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record puerto ayora City 285 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record senneterre City 286 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 287 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record grafton City 288 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 289 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record qaanaaq City 290 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 291 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 292 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 293 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 294 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record karpathos City 295 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record longhua City 296 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 297 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record arraial do cabo City 298 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record samarai City 299 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castletown City 300 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record beloha City 301 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record perdika City 302 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saint-philippe City 303 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sterling City 304 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rawson City 305 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saldanha City 306 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record likasi City 307 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 308 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record georgetown City 309 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 310 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record new norfolk City 311 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port hardy City 312 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nikolskoye City 313 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record sentyabrskiy City 314 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record jamestown City 315 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tingrela City 316 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record puerto ayora City 317 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 318 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 319 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 320 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 321 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record belushya guba City 322 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record ushuaia City 323 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 324 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port hawkesbury City 325 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record lota City 326 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port hedland City 327 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record komsomolskoye City 328 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record trinidad City 329 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record airai City 330 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record barrow City 331 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record watertown City 332 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 333 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto cabezas City 334 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record honiara City 335 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record wajima City 336 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tazovskiy City 337 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record monrovia City 338 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 339 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record praia da vitoria City 340 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tasiilaq City 341 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 342 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record arequipa City 343 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record omboue City 344 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bambous virieux City 345 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mar del plata City 346 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record east london City 347 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 348 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record hermanus City 349 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record novokayakent City 350 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 351 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 352 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record georgetown City 353 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tsihombe City 354 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record provideniya City 355 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record warqla City 356 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record vaitupu City 357 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record ushuaia City 358 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 359 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record fairbanks City 360 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 361 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 362 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record azimur City 363 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record carauari City 364 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record poum City 365 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record severo-kurilsk City 366 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tevriz City 367 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bambous virieux City 368 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 369 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record arman City 370 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chingola City 371 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 372 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record fevralsk City 373 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record ilulissat City 374 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record acapulco City 375 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record saint george City 376 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 377 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bayji City 378 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record east london City 379 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port blair City 380 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bredasdorp City 381 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record necochea City 382 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record portree City 383 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto suarez City 384 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record seoul City 385 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tiarei City 386 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record gat City 387 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record dinguiraye City 388 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 389 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ternate City 390 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san cristobal City 391 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 392 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saint anthony City 393 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record taolanaro City 394 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record tome City 395 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 396 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ust-tsilma City 397 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 398 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ramshir City 399 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saldanha City 400 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rudnogorsk City 401 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vila franca do campo City 402 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chany City 403 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bandar-e torkaman City 404 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mys shmidta City 405 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record minsk City 406 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yellowknife City 407 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port alfred City 408 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ekhabi City 409 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record saint-philippe City 410 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record zheleznodorozhnyy City 411 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record deniliquin City 412 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sajoszentpeter City 413 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record krasnoslobodsk City 414 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 415 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record samarai City 416 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chokurdakh City 417 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 418 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 419 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castro City 420 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kungurtug City 421 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record severo-kurilsk City 422 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record singkawang City 423 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 424 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 425 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record salym City 426 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san patricio City 427 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 428 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record romitan City 429 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record tura City 430 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record arraial do cabo City 431 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 432 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cape town City 433 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mantua City 434 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hobart City 435 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ostrovnoy City 436 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tsihombe City 437 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record punta arenas City 438 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vila velha City 439 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mehamn City 440 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 441 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record norman wells City 442 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record fortuna City 443 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pisco City 444 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 445 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record muzhi City 446 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record barentsburg City 447 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record rungata City 448 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record porto velho City 449 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 450 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 451 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 452 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 453 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cidreira City 454 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record severo-kurilsk City 455 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record louisbourg City 456 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record saurimo City 457 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record doha City 458 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record punta arenas City 459 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sao joao da barra City 460 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 461 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 462 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 463 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record provideniya City 464 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record langxiang City 465 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record salvador City 466 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record middle island City 467 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record richards bay City 468 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kodiak City 469 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record east london City 470 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nguruka City 471 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record malakal City 472 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record piacabucu City 473 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 474 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hobart City 475 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 476 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record great falls City 477 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record macamic City 478 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hasaki City 479 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mrirt City 480 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record nizhniy kuranakh City 481 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record buala City 482 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saint-francois City 483 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nome City 484 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto ayora City 485 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record castro City 486 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record voskresenskoye City 487 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tasiilaq City 488 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record izhma City 489 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 490 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record qaanaaq City 491 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 492 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record shelburne City 493 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 494 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 495 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 496 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record chuy City 497 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 498 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hasaki City 499 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cabedelo City 500 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 501 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nokaneng City 502 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sao filipe City 503 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mar del plata City 504 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 505 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nizhneyansk City 506 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record nemuro City 507 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san patricio City 508 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 509 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record itarema City 510 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record craigieburn City 511 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sadiqabad City 512 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record carnarvon City 513 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 514 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record haian City 515 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record komsomolskiy City 516 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 517 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 518 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record dauphin City 519 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 520 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record methoni City 521 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 522 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saurimo City 523 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record narsaq City 524 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taolanaro City 525 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record chicama City 526 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vao City 527 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 528 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record losal City 529 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record paradwip City 530 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record port alfred City 531 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record rikitea City 532 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 533 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record provideniya City 534 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saskylakh City 535 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record vaini City 536 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record guanhaes City 537 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record esperance City 538 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sawakin City 539 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 540 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mys shmidta City 541 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record burns lake City 542 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 543 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 544 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 545 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 546 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 547 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 548 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mataura City 549 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mataura City 550 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record yellowknife City 551 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 552 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record prince rupert City 553 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ambilobe City 554 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 555 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cherskiy City 556 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record fasa City 557 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record olindina City 558 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 559 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record sao joao da barra City 560 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hilo City 561 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record new norfolk City 562 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kaitangata City 563 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record bushenyi City 564 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record clyde river City 565 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cabo san lucas City 566 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record endicott City 567 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record illoqqortoormiut City 568 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record taolanaro City 569 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record torbay City 570 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 571 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record novovarshavka City 572 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record butaritari City 573 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hermanus City 574 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kuche City 575 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record vallenar City 576 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 577 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record lebu City 578 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record noumea City 579 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record batemans bay City 580 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record victoria City 581 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record puerto ayora City 582 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nizhneyansk City 583 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record vicente guerrero City 584 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record zhezkazgan City 585 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mount pleasant City 586 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record porterville City 587 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record muros City 588 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record nanortalik City 589 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record guerrero negro City 590 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 591 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bajil City 592 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port alfred City 593 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record busselton City 594 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tsihombe City 595 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record tiksi City 596 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record albany City 597 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record taksimo City 598 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record saint-philippe City 599 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record avarua City 600 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record qaanaaq City 601 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ballina City 602 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 603 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 604 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record lebu City 605 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record oranjestad City 606 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record leningradskiy City 607 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record tasiilaq City 608 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record khatanga City 609 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record filadelfia City 610 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record paamiut City 611 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 612 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record oparino City 613 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record itarema City 614 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record barentsburg City 615 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record punta arenas City 616 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record north bend City 617 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 618 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 619 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record chokurdakh City 620 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record kapaa City 621 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record riyadh City 622 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record cabo san lucas City 623 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record komsomolskiy City 624 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 625 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record biak City 626 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record chokurdakh City 627 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 628 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record taolanaro City 629 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record mayo City 630 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record punta arenas City 631 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record hithadhoo City 632 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bluff City 633 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ushuaia City 634 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record mataura City 635 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record port-cartier City 636 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record ribeira grande City 637 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record gzhatsk City 638 of 650
    http://api.openweathermap.org/data/2.5/weather
    Not found in city data, Skipping this City
    Processing record beloha City 639 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record san angelo City 640 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record belmonte City 641 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record arraial do cabo City 642 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record pevek City 643 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record batemans bay City 644 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record jamestown City 645 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record yumen City 646 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record barrow City 647 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record atuona City 648 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record bredasdorp City 649 of 650
    http://api.openweathermap.org/data/2.5/weather
    Processing record port lincoln City 650 of 650
    http://api.openweathermap.org/data/2.5/weather
    [61.74, 91, 68, 9.42]
    




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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudliness</th>
      <th>Wind_Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-78.533106</td>
      <td>156.531638</td>
      <td>67.54</td>
      <td>79</td>
      <td>48</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>1</th>
      <td>souillac</td>
      <td>mu</td>
      <td>-32.149709</td>
      <td>63.027527</td>
      <td>75.64</td>
      <td>83</td>
      <td>48</td>
      <td>11.21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>sur</td>
      <td>om</td>
      <td>22.590865</td>
      <td>60.044884</td>
      <td>72.94</td>
      <td>100</td>
      <td>20</td>
      <td>15.79</td>
    </tr>
    <tr>
      <th>3</th>
      <td>hobart</td>
      <td>au</td>
      <td>-87.166298</td>
      <td>134.993826</td>
      <td>59</td>
      <td>47</td>
      <td>75</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>saleaula</td>
      <td>ws</td>
      <td>3.799089</td>
      <td>-172.185280</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>puerto princesa</td>
      <td>ph</td>
      <td>9.173430</td>
      <td>119.522781</td>
      <td>80.6</td>
      <td>83</td>
      <td>75</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-65.228542</td>
      <td>157.003954</td>
      <td>67.54</td>
      <td>79</td>
      <td>48</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>7</th>
      <td>nadym</td>
      <td>ru</td>
      <td>64.754344</td>
      <td>72.614999</td>
      <td>23.85</td>
      <td>90</td>
      <td>44</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>8</th>
      <td>vaini</td>
      <td>to</td>
      <td>-65.135506</td>
      <td>-170.799764</td>
      <td>84.2</td>
      <td>79</td>
      <td>75</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>9</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-50.346107</td>
      <td>-126.241625</td>
      <td>76.86</td>
      <td>100</td>
      <td>92</td>
      <td>16.35</td>
    </tr>
    <tr>
      <th>10</th>
      <td>constitucion</td>
      <td>mx</td>
      <td>13.116350</td>
      <td>-128.192458</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>11</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-3.358808</td>
      <td>-153.400505</td>
      <td>82.71</td>
      <td>98</td>
      <td>0</td>
      <td>12.44</td>
    </tr>
    <tr>
      <th>12</th>
      <td>khatanga</td>
      <td>ru</td>
      <td>72.535633</td>
      <td>98.230834</td>
      <td>-19.45</td>
      <td>28</td>
      <td>0</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>13</th>
      <td>pevek</td>
      <td>ru</td>
      <td>85.395218</td>
      <td>166.791267</td>
      <td>21.91</td>
      <td>88</td>
      <td>88</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>14</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-85.703222</td>
      <td>52.718222</td>
      <td>63.9</td>
      <td>99</td>
      <td>56</td>
      <td>2.71</td>
    </tr>
    <tr>
      <th>15</th>
      <td>broome</td>
      <td>au</td>
      <td>-15.810206</td>
      <td>123.720580</td>
      <td>89.6</td>
      <td>62</td>
      <td>0</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>16</th>
      <td>yar-sale</td>
      <td>ru</td>
      <td>70.690356</td>
      <td>73.286975</td>
      <td>25.87</td>
      <td>89</td>
      <td>76</td>
      <td>16.8</td>
    </tr>
    <tr>
      <th>17</th>
      <td>kapaa</td>
      <td>us</td>
      <td>10.100514</td>
      <td>-168.218920</td>
      <td>76.37</td>
      <td>69</td>
      <td>40</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>18</th>
      <td>norman wells</td>
      <td>ca</td>
      <td>84.214985</td>
      <td>-117.068572</td>
      <td>-9.41</td>
      <td>76</td>
      <td>20</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>19</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.894191</td>
      <td>17.067723</td>
      <td>62.6</td>
      <td>67</td>
      <td>20</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>sukumo</td>
      <td>jp</td>
      <td>31.810758</td>
      <td>132.882614</td>
      <td>53.19</td>
      <td>97</td>
      <td>100</td>
      <td>7.96</td>
    </tr>
    <tr>
      <th>21</th>
      <td>chernyy yar</td>
      <td>ru</td>
      <td>47.928245</td>
      <td>45.867928</td>
      <td>38.61</td>
      <td>96</td>
      <td>92</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>22</th>
      <td>hobart</td>
      <td>au</td>
      <td>-59.910505</td>
      <td>149.883598</td>
      <td>59</td>
      <td>47</td>
      <td>75</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>23</th>
      <td>illoqqortoormiut</td>
      <td>gl</td>
      <td>86.261452</td>
      <td>-17.967623</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>24</th>
      <td>port elizabeth</td>
      <td>za</td>
      <td>-55.108793</td>
      <td>29.292139</td>
      <td>57.2</td>
      <td>93</td>
      <td>0</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th>25</th>
      <td>codrington</td>
      <td>ag</td>
      <td>25.214533</td>
      <td>-44.315424</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>26</th>
      <td>hobart</td>
      <td>au</td>
      <td>-86.176501</td>
      <td>140.984077</td>
      <td>59</td>
      <td>47</td>
      <td>75</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>27</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-4.885917</td>
      <td>80.340187</td>
      <td>79.56</td>
      <td>100</td>
      <td>80</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>28</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-1.142985</td>
      <td>-153.437522</td>
      <td>82.71</td>
      <td>98</td>
      <td>0</td>
      <td>12.44</td>
    </tr>
    <tr>
      <th>29</th>
      <td>chapais</td>
      <td>ca</td>
      <td>49.248584</td>
      <td>-75.093825</td>
      <td>28.4</td>
      <td>92</td>
      <td>90</td>
      <td>5.82</td>
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
      <td>...</td>
    </tr>
    <tr>
      <th>620</th>
      <td>kapaa</td>
      <td>us</td>
      <td>14.605507</td>
      <td>-179.466261</td>
      <td>76.37</td>
      <td>69</td>
      <td>40</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>621</th>
      <td>riyadh</td>
      <td>sa</td>
      <td>25.712974</td>
      <td>48.006748</td>
      <td>48.2</td>
      <td>83</td>
      <td>0</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th>622</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>14.276258</td>
      <td>-122.092956</td>
      <td>77</td>
      <td>69</td>
      <td>20</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>623</th>
      <td>komsomolskiy</td>
      <td>ru</td>
      <td>66.662030</td>
      <td>171.769525</td>
      <td>46.08</td>
      <td>90</td>
      <td>8</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>624</th>
      <td>pevek</td>
      <td>ru</td>
      <td>76.274421</td>
      <td>171.847570</td>
      <td>21.91</td>
      <td>88</td>
      <td>88</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>625</th>
      <td>biak</td>
      <td>id</td>
      <td>-1.174172</td>
      <td>138.416476</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>626</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>71.982119</td>
      <td>146.057930</td>
      <td>5.13</td>
      <td>77</td>
      <td>68</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>627</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-47.733465</td>
      <td>-151.755503</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>628</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td>-50.461454</td>
      <td>58.484173</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>629</th>
      <td>mayo</td>
      <td>ca</td>
      <td>64.932732</td>
      <td>-138.099096</td>
      <td>-4.01</td>
      <td>83</td>
      <td>75</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>630</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-80.898778</td>
      <td>-118.061485</td>
      <td>57.2</td>
      <td>58</td>
      <td>0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>631</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-1.661261</td>
      <td>69.768811</td>
      <td>79.56</td>
      <td>100</td>
      <td>80</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>632</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-86.014614</td>
      <td>167.640742</td>
      <td>67.54</td>
      <td>79</td>
      <td>48</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>633</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-66.468737</td>
      <td>-57.381237</td>
      <td>60.8</td>
      <td>44</td>
      <td>0</td>
      <td>13.22</td>
    </tr>
    <tr>
      <th>634</th>
      <td>mataura</td>
      <td>pf</td>
      <td>-88.881853</td>
      <td>-159.581487</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>635</th>
      <td>port-cartier</td>
      <td>ca</td>
      <td>50.590148</td>
      <td>-67.114893</td>
      <td>17.6</td>
      <td>85</td>
      <td>90</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>636</th>
      <td>ribeira grande</td>
      <td>pt</td>
      <td>28.823514</td>
      <td>-40.313300</td>
      <td>66.96</td>
      <td>97</td>
      <td>80</td>
      <td>17.47</td>
    </tr>
    <tr>
      <th>637</th>
      <td>gzhatsk</td>
      <td>ru</td>
      <td>55.916195</td>
      <td>34.887285</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>638</th>
      <td>beloha</td>
      <td>mg</td>
      <td>-26.585481</td>
      <td>43.429806</td>
      <td>70.56</td>
      <td>92</td>
      <td>0</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>639</th>
      <td>san angelo</td>
      <td>us</td>
      <td>31.481928</td>
      <td>-100.995152</td>
      <td>69.8</td>
      <td>60</td>
      <td>1</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>640</th>
      <td>belmonte</td>
      <td>br</td>
      <td>-16.741156</td>
      <td>-34.857529</td>
      <td>77</td>
      <td>94</td>
      <td>40</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>641</th>
      <td>arraial do cabo</td>
      <td>br</td>
      <td>-36.719489</td>
      <td>-23.192238</td>
      <td>75.2</td>
      <td>88</td>
      <td>75</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>642</th>
      <td>pevek</td>
      <td>ru</td>
      <td>89.900682</td>
      <td>170.126555</td>
      <td>21.91</td>
      <td>88</td>
      <td>88</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>643</th>
      <td>batemans bay</td>
      <td>au</td>
      <td>-39.326591</td>
      <td>151.752214</td>
      <td>59.62</td>
      <td>89</td>
      <td>64</td>
      <td>5.95</td>
    </tr>
    <tr>
      <th>644</th>
      <td>jamestown</td>
      <td>sh</td>
      <td>-17.618744</td>
      <td>-14.381594</td>
      <td>68.58</td>
      <td>100</td>
      <td>92</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>645</th>
      <td>yumen</td>
      <td>cn</td>
      <td>36.439571</td>
      <td>95.452783</td>
      <td>6.52</td>
      <td>74</td>
      <td>20</td>
      <td>4.72</td>
    </tr>
    <tr>
      <th>646</th>
      <td>barrow</td>
      <td>us</td>
      <td>73.292521</td>
      <td>-154.353817</td>
      <td>3.2</td>
      <td>84</td>
      <td>90</td>
      <td>20.8</td>
    </tr>
    <tr>
      <th>647</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-7.194384</td>
      <td>-127.710126</td>
      <td>81.22</td>
      <td>99</td>
      <td>0</td>
      <td>19.04</td>
    </tr>
    <tr>
      <th>648</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.422967</td>
      <td>18.908394</td>
      <td>62.6</td>
      <td>67</td>
      <td>20</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>649</th>
      <td>port lincoln</td>
      <td>au</td>
      <td>-37.057553</td>
      <td>135.209426</td>
      <td>61.74</td>
      <td>91</td>
      <td>68</td>
      <td>9.42</td>
    </tr>
  </tbody>
</table>
<p>650 rows × 8 columns</p>
</div>




```python
# save the Series as a csv file
mycities.to_csv("mycities_weather.csv",encoding="utf-8",index=False)
```


```python
weather_df = pd.read_csv("mycities_weather.csv")
```


```python
weather_df.dtypes
```




    City            object
    Country         object
    Latitude       float64
    Longitude      float64
    Temperature    float64
    Humidity       float64
    Cloudliness    float64
    Wind_Speed     float64
    dtype: object




```python
#convert to DataFrame and drop the cities that were skipped due to no data
cities_remove = weather_df.dropna(how='any')
cities_remove.count()
cities_remove
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudliness</th>
      <th>Wind_Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-78.533106</td>
      <td>156.531638</td>
      <td>67.54</td>
      <td>79.0</td>
      <td>48.0</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>1</th>
      <td>souillac</td>
      <td>mu</td>
      <td>-32.149709</td>
      <td>63.027527</td>
      <td>75.64</td>
      <td>83.0</td>
      <td>48.0</td>
      <td>11.21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>sur</td>
      <td>om</td>
      <td>22.590865</td>
      <td>60.044884</td>
      <td>72.94</td>
      <td>100.0</td>
      <td>20.0</td>
      <td>15.79</td>
    </tr>
    <tr>
      <th>3</th>
      <td>hobart</td>
      <td>au</td>
      <td>-87.166298</td>
      <td>134.993826</td>
      <td>59.00</td>
      <td>47.0</td>
      <td>75.0</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>5</th>
      <td>puerto princesa</td>
      <td>ph</td>
      <td>9.173430</td>
      <td>119.522781</td>
      <td>80.60</td>
      <td>83.0</td>
      <td>75.0</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-65.228542</td>
      <td>157.003954</td>
      <td>67.54</td>
      <td>79.0</td>
      <td>48.0</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>7</th>
      <td>nadym</td>
      <td>ru</td>
      <td>64.754344</td>
      <td>72.614999</td>
      <td>23.85</td>
      <td>90.0</td>
      <td>44.0</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>8</th>
      <td>vaini</td>
      <td>to</td>
      <td>-65.135506</td>
      <td>-170.799764</td>
      <td>84.20</td>
      <td>79.0</td>
      <td>75.0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>9</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-50.346107</td>
      <td>-126.241625</td>
      <td>76.86</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>16.35</td>
    </tr>
    <tr>
      <th>11</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-3.358808</td>
      <td>-153.400505</td>
      <td>82.71</td>
      <td>98.0</td>
      <td>0.0</td>
      <td>12.44</td>
    </tr>
    <tr>
      <th>12</th>
      <td>khatanga</td>
      <td>ru</td>
      <td>72.535633</td>
      <td>98.230834</td>
      <td>-19.45</td>
      <td>28.0</td>
      <td>0.0</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>13</th>
      <td>pevek</td>
      <td>ru</td>
      <td>85.395218</td>
      <td>166.791267</td>
      <td>21.91</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>14</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-85.703222</td>
      <td>52.718222</td>
      <td>63.90</td>
      <td>99.0</td>
      <td>56.0</td>
      <td>2.71</td>
    </tr>
    <tr>
      <th>15</th>
      <td>broome</td>
      <td>au</td>
      <td>-15.810206</td>
      <td>123.720580</td>
      <td>89.60</td>
      <td>62.0</td>
      <td>0.0</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>16</th>
      <td>yar-sale</td>
      <td>ru</td>
      <td>70.690356</td>
      <td>73.286975</td>
      <td>25.87</td>
      <td>89.0</td>
      <td>76.0</td>
      <td>16.80</td>
    </tr>
    <tr>
      <th>17</th>
      <td>kapaa</td>
      <td>us</td>
      <td>10.100514</td>
      <td>-168.218920</td>
      <td>76.37</td>
      <td>69.0</td>
      <td>40.0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>18</th>
      <td>norman wells</td>
      <td>ca</td>
      <td>84.214985</td>
      <td>-117.068572</td>
      <td>-9.41</td>
      <td>76.0</td>
      <td>20.0</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>19</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.894191</td>
      <td>17.067723</td>
      <td>62.60</td>
      <td>67.0</td>
      <td>20.0</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>sukumo</td>
      <td>jp</td>
      <td>31.810758</td>
      <td>132.882614</td>
      <td>53.19</td>
      <td>97.0</td>
      <td>100.0</td>
      <td>7.96</td>
    </tr>
    <tr>
      <th>21</th>
      <td>chernyy yar</td>
      <td>ru</td>
      <td>47.928245</td>
      <td>45.867928</td>
      <td>38.61</td>
      <td>96.0</td>
      <td>92.0</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>22</th>
      <td>hobart</td>
      <td>au</td>
      <td>-59.910505</td>
      <td>149.883598</td>
      <td>59.00</td>
      <td>47.0</td>
      <td>75.0</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>24</th>
      <td>port elizabeth</td>
      <td>za</td>
      <td>-55.108793</td>
      <td>29.292139</td>
      <td>57.20</td>
      <td>93.0</td>
      <td>0.0</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>26</th>
      <td>hobart</td>
      <td>au</td>
      <td>-86.176501</td>
      <td>140.984077</td>
      <td>59.00</td>
      <td>47.0</td>
      <td>75.0</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>27</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-4.885917</td>
      <td>80.340187</td>
      <td>79.56</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>28</th>
      <td>faanui</td>
      <td>pf</td>
      <td>-1.142985</td>
      <td>-153.437522</td>
      <td>82.71</td>
      <td>98.0</td>
      <td>0.0</td>
      <td>12.44</td>
    </tr>
    <tr>
      <th>29</th>
      <td>chapais</td>
      <td>ca</td>
      <td>49.248584</td>
      <td>-75.093825</td>
      <td>28.40</td>
      <td>92.0</td>
      <td>90.0</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>30</th>
      <td>tuktoyaktuk</td>
      <td>ca</td>
      <td>81.230022</td>
      <td>-139.298499</td>
      <td>-5.81</td>
      <td>83.0</td>
      <td>20.0</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>31</th>
      <td>aberdeen</td>
      <td>us</td>
      <td>45.560654</td>
      <td>-99.181872</td>
      <td>37.40</td>
      <td>69.0</td>
      <td>1.0</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>32</th>
      <td>mongo</td>
      <td>td</td>
      <td>12.579391</td>
      <td>18.399558</td>
      <td>61.24</td>
      <td>63.0</td>
      <td>8.0</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>33</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-69.879169</td>
      <td>43.891967</td>
      <td>63.90</td>
      <td>99.0</td>
      <td>56.0</td>
      <td>2.71</td>
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
      <td>...</td>
    </tr>
    <tr>
      <th>612</th>
      <td>oparino</td>
      <td>ru</td>
      <td>59.695470</td>
      <td>48.248524</td>
      <td>29.74</td>
      <td>90.0</td>
      <td>76.0</td>
      <td>5.39</td>
    </tr>
    <tr>
      <th>615</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-52.176099</td>
      <td>-79.787868</td>
      <td>57.20</td>
      <td>58.0</td>
      <td>0.0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>616</th>
      <td>north bend</td>
      <td>us</td>
      <td>43.438691</td>
      <td>-133.720216</td>
      <td>45.61</td>
      <td>93.0</td>
      <td>75.0</td>
      <td>5.17</td>
    </tr>
    <tr>
      <th>617</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-69.808270</td>
      <td>-40.360155</td>
      <td>60.80</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.22</td>
    </tr>
    <tr>
      <th>619</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>74.908094</td>
      <td>151.693901</td>
      <td>5.13</td>
      <td>77.0</td>
      <td>68.0</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>620</th>
      <td>kapaa</td>
      <td>us</td>
      <td>14.605507</td>
      <td>-179.466261</td>
      <td>76.37</td>
      <td>69.0</td>
      <td>40.0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>621</th>
      <td>riyadh</td>
      <td>sa</td>
      <td>25.712974</td>
      <td>48.006748</td>
      <td>48.20</td>
      <td>83.0</td>
      <td>0.0</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>622</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>14.276258</td>
      <td>-122.092956</td>
      <td>77.00</td>
      <td>69.0</td>
      <td>20.0</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>623</th>
      <td>komsomolskiy</td>
      <td>ru</td>
      <td>66.662030</td>
      <td>171.769525</td>
      <td>46.08</td>
      <td>90.0</td>
      <td>8.0</td>
      <td>14.34</td>
    </tr>
    <tr>
      <th>624</th>
      <td>pevek</td>
      <td>ru</td>
      <td>76.274421</td>
      <td>171.847570</td>
      <td>21.91</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>626</th>
      <td>chokurdakh</td>
      <td>ru</td>
      <td>71.982119</td>
      <td>146.057930</td>
      <td>5.13</td>
      <td>77.0</td>
      <td>68.0</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>629</th>
      <td>mayo</td>
      <td>ca</td>
      <td>64.932732</td>
      <td>-138.099096</td>
      <td>-4.01</td>
      <td>83.0</td>
      <td>75.0</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>630</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-80.898778</td>
      <td>-118.061485</td>
      <td>57.20</td>
      <td>58.0</td>
      <td>0.0</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>631</th>
      <td>hithadhoo</td>
      <td>mv</td>
      <td>-1.661261</td>
      <td>69.768811</td>
      <td>79.56</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>632</th>
      <td>bluff</td>
      <td>nz</td>
      <td>-86.014614</td>
      <td>167.640742</td>
      <td>67.54</td>
      <td>79.0</td>
      <td>48.0</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>633</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-66.468737</td>
      <td>-57.381237</td>
      <td>60.80</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.22</td>
    </tr>
    <tr>
      <th>635</th>
      <td>port-cartier</td>
      <td>ca</td>
      <td>50.590148</td>
      <td>-67.114893</td>
      <td>17.60</td>
      <td>85.0</td>
      <td>90.0</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>636</th>
      <td>ribeira grande</td>
      <td>pt</td>
      <td>28.823514</td>
      <td>-40.313300</td>
      <td>66.96</td>
      <td>97.0</td>
      <td>80.0</td>
      <td>17.47</td>
    </tr>
    <tr>
      <th>638</th>
      <td>beloha</td>
      <td>mg</td>
      <td>-26.585481</td>
      <td>43.429806</td>
      <td>70.56</td>
      <td>92.0</td>
      <td>0.0</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>639</th>
      <td>san angelo</td>
      <td>us</td>
      <td>31.481928</td>
      <td>-100.995152</td>
      <td>69.80</td>
      <td>60.0</td>
      <td>1.0</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>640</th>
      <td>belmonte</td>
      <td>br</td>
      <td>-16.741156</td>
      <td>-34.857529</td>
      <td>77.00</td>
      <td>94.0</td>
      <td>40.0</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>641</th>
      <td>arraial do cabo</td>
      <td>br</td>
      <td>-36.719489</td>
      <td>-23.192238</td>
      <td>75.20</td>
      <td>88.0</td>
      <td>75.0</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>642</th>
      <td>pevek</td>
      <td>ru</td>
      <td>89.900682</td>
      <td>170.126555</td>
      <td>21.91</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>643</th>
      <td>batemans bay</td>
      <td>au</td>
      <td>-39.326591</td>
      <td>151.752214</td>
      <td>59.62</td>
      <td>89.0</td>
      <td>64.0</td>
      <td>5.95</td>
    </tr>
    <tr>
      <th>644</th>
      <td>jamestown</td>
      <td>sh</td>
      <td>-17.618744</td>
      <td>-14.381594</td>
      <td>68.58</td>
      <td>100.0</td>
      <td>92.0</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>645</th>
      <td>yumen</td>
      <td>cn</td>
      <td>36.439571</td>
      <td>95.452783</td>
      <td>6.52</td>
      <td>74.0</td>
      <td>20.0</td>
      <td>4.72</td>
    </tr>
    <tr>
      <th>646</th>
      <td>barrow</td>
      <td>us</td>
      <td>73.292521</td>
      <td>-154.353817</td>
      <td>3.20</td>
      <td>84.0</td>
      <td>90.0</td>
      <td>20.80</td>
    </tr>
    <tr>
      <th>647</th>
      <td>atuona</td>
      <td>pf</td>
      <td>-7.194384</td>
      <td>-127.710126</td>
      <td>81.22</td>
      <td>99.0</td>
      <td>0.0</td>
      <td>19.04</td>
    </tr>
    <tr>
      <th>648</th>
      <td>bredasdorp</td>
      <td>za</td>
      <td>-85.422967</td>
      <td>18.908394</td>
      <td>62.60</td>
      <td>67.0</td>
      <td>20.0</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>649</th>
      <td>port lincoln</td>
      <td>au</td>
      <td>-37.057553</td>
      <td>135.209426</td>
      <td>61.74</td>
      <td>91.0</td>
      <td>68.0</td>
      <td>9.42</td>
    </tr>
  </tbody>
</table>
<p>547 rows × 8 columns</p>
</div>



## Temperature (F) vs. Latitude Plot dated 12/03/2017


```python
#Temperature Plot
plt.scatter(cities_remove["Latitude"],cities_remove["Temperature"],edgecolors="black",linewidths=1.5,marker="o")

#Plot labels

plt.title("Temperature (F) vs. Latitude (12/03/17)")
plt.ylabel("Temperature (F)")
plt.xlabel("Latitude")
plt.grid=True
plt.xlim([-95,95])
plt.ylim([-115,155])

#Save Plot
plt.savefig("Temperature (F) vs. Latitude.png")

plt.show()
```


![png](output_20_0.png)


## Humidity (%) vs. Latitude Plot dated 12/03/2017


```python
#Humidity Plot
plt.scatter(cities_remove["Latitude"],cities_remove["Humidity"],edgecolors="black",linewidths=1.5,marker="o")

#Plot labels

plt.title("Humidity (%) vs. Latitude (12/03/2017)")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid=True
plt.xlim([-95,95])
plt.ylim([-5,105])

#Save Plot
plt.savefig("Humidity (%) vs. Latitude.png")

plt.show()
```


![png](output_22_0.png)


## Cloudliness (%) vs. Latitude Plot dated (12/03/2017)


```python
#Cloudiness Plot
plt.scatter(cities_remove["Latitude"],cities_remove["Cloudliness"],edgecolors="black",linewidths=1.5,marker="o")

#Plot labels

plt.title("Cloudiness (%) vs. Latitude (12/03/2017)")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid=True
plt.xlim([-95,95])
plt.ylim([-5,105])

#Save Plot
plt.savefig("Cloudiness (%) vs. Latitude.png")

plt.show()
```


![png](output_24_0.png)


## Wind Speed (mph) vs. Latitude Plot dated 12/03/2017


```python
#Wind Speed Plot
plt.scatter(cities_remove["Latitude"],cities_remove["Wind_Speed"],edgecolors="black",linewidths=1.5,marker="o")

#Plot labels

plt.title("Wind Speed (mph) vs. Latitude (12/03/17)")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid=True
plt.xlim([-95,95])
plt.ylim([-5,50])

#Save Plot
plt.savefig("Wind Speed (mph) vs. Latitude.png")

plt.show()
```


![png](output_26_0.png)


### Findings

* Latitude seems to have the most impact on Temperature
* Latitude seems to have little impact on Cloudliness and Wind Speed
* Latitude seems to have a minor effect on Humidity


```python

```
