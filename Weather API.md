
TRENDS

1. Temperature gets warmer as one gets closer to the equator.  (Side note: There appears to be less habitated landmass in the extreme latitudes of the southern hemisphere as compared to the northern hemisphere.)
2. At latitudes close to the equator, there are higher humidity levels.
3. There does not appear to be a relationship between latitude and cloudiness.
4. Extremely high wind speeds were more likely to be found at extreme north and south latitudes.


```python
from citipy import citipy
import pandas as pd
import matplotlib.pyplot as plt
import random
import os
import requests
import json
import datetime
```


```python
#function for generating random cities
def randomCity(num_cities):
    counter = 0
    while counter < num_cities:
        lat = random.uniform(-90,90)
        lng = random.uniform(-180,180)
        lat = round(lat, 6)
        lng = round(lng, 6)

        find_city = citipy.nearest_city(lat, lng)
        city = find_city.city_name
        country = find_city.country_code
#         print(city)
#         print(country)
        dict = {"city":city, "country":country}
        return dict
        counter = counter + 1
# randomCity(2)



```


```python
city_list = []
country_list = []
city_df = pd.DataFrame({})
city_df["City"] = ""
city_df["Country"] = ""

while len(city_list) < 500:
    dict2 = randomCity(1)
    city = dict2["city"]
        
    if city not in city_list:
        city_list.append(dict2["city"])
        country_list.append(dict2["country"])

# print(city_list)
# print(country_list)
print(len(city_list))

city_df["City"] = city_list
city_df["Country"] = country_list
city_df.head()

```

    500





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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>chernyshevskiy</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hermanus</td>
      <td>za</td>
    </tr>
    <tr>
      <th>2</th>
      <td>saint-joseph</td>
      <td>re</td>
    </tr>
    <tr>
      <th>3</th>
      <td>mar del plata</td>
      <td>ar</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nabire</td>
      <td>id</td>
    </tr>
  </tbody>
</table>
</div>




```python
#city_df["Longitude"].isna()
```


```python
# Find Long/Lat of that city

# Google developer API key
gkey = "AIzaSyA_Clyz3478YAUnsESNHE5dyktvvMoa-vw"

#try:
for index, row in city_df.iterrows(): # city_df[city_df.Longitude.isna()]
# Target city
    target_city = row["City"]

# Build the endpoint URL
    target_url = "https://maps.googleapis.com/maps/api/geocode/json" \
            "?address=%s&key=%s" % (target_city, gkey)

# # Print the assembled URL
#     print(target_url)

# # Run a request to endpoint and convert result to json
    geo_data = requests.get(target_url).json()

# # Print the json (pretty printed)
#     print(json.dumps(geo_data, indent=4, sort_keys=True))
    if not geo_data["results"]:
        continue
# # Extract latitude and longitude
    lat = geo_data["results"][0]["geometry"]["location"]["lat"]
    lng = geo_data["results"][0]["geometry"]["location"]["lng"]

# # Print the latitude and longitude
#     print("%s: %s, %s" % (target_city, lat, lng))
# Put in Data Frame
    city_df.set_value(index, "Latitude", lat)
    city_df.set_value(index, "Longitude", lng)

#except IndexError:
    #pass

city_df
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
      <td>chernyshevskiy</td>
      <td>ru</td>
      <td>63.025038</td>
      <td>112.490249</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hermanus</td>
      <td>za</td>
      <td>-34.409200</td>
      <td>19.250444</td>
    </tr>
    <tr>
      <th>2</th>
      <td>saint-joseph</td>
      <td>re</td>
      <td>39.767458</td>
      <td>-94.846681</td>
    </tr>
    <tr>
      <th>3</th>
      <td>mar del plata</td>
      <td>ar</td>
      <td>-38.005477</td>
      <td>-57.542611</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nabire</td>
      <td>id</td>
      <td>-3.509546</td>
      <td>135.752098</td>
    </tr>
    <tr>
      <th>5</th>
      <td>sambava</td>
      <td>mg</td>
      <td>-14.271334</td>
      <td>50.167812</td>
    </tr>
    <tr>
      <th>6</th>
      <td>tome-acu</td>
      <td>br</td>
      <td>-2.672933</td>
      <td>-48.239353</td>
    </tr>
    <tr>
      <th>7</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td>-25.022531</td>
      <td>46.985369</td>
    </tr>
    <tr>
      <th>8</th>
      <td>kargasok</td>
      <td>ru</td>
      <td>59.053540</td>
      <td>80.877060</td>
    </tr>
    <tr>
      <th>9</th>
      <td>jamestown</td>
      <td>sh</td>
      <td>37.211638</td>
      <td>-76.775210</td>
    </tr>
    <tr>
      <th>10</th>
      <td>cidreira</td>
      <td>br</td>
      <td>-30.176854</td>
      <td>-50.208931</td>
    </tr>
    <tr>
      <th>11</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-33.586407</td>
      <td>26.885145</td>
    </tr>
    <tr>
      <th>12</th>
      <td>vaini</td>
      <td>to</td>
      <td>-21.193500</td>
      <td>-175.175153</td>
    </tr>
    <tr>
      <th>13</th>
      <td>geraldton</td>
      <td>au</td>
      <td>-28.777372</td>
      <td>114.614971</td>
    </tr>
    <tr>
      <th>14</th>
      <td>cloquet</td>
      <td>us</td>
      <td>46.721773</td>
      <td>-92.461182</td>
    </tr>
    <tr>
      <th>15</th>
      <td>kahului</td>
      <td>us</td>
      <td>20.889335</td>
      <td>-156.472947</td>
    </tr>
    <tr>
      <th>16</th>
      <td>castro</td>
      <td>cl</td>
      <td>34.498505</td>
      <td>-102.346388</td>
    </tr>
    <tr>
      <th>17</th>
      <td>busselton</td>
      <td>au</td>
      <td>-33.655493</td>
      <td>115.350019</td>
    </tr>
    <tr>
      <th>18</th>
      <td>bolungarvik</td>
      <td>is</td>
      <td>66.151772</td>
      <td>-23.261709</td>
    </tr>
    <tr>
      <th>19</th>
      <td>airai</td>
      <td>pw</td>
      <td>7.396612</td>
      <td>134.569022</td>
    </tr>
    <tr>
      <th>20</th>
      <td>nikolskoye</td>
      <td>ru</td>
      <td>55.198160</td>
      <td>166.001537</td>
    </tr>
    <tr>
      <th>21</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-23.119901</td>
      <td>-134.970265</td>
    </tr>
    <tr>
      <th>22</th>
      <td>sapa</td>
      <td>ph</td>
      <td>40.753554</td>
      <td>-111.888477</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.801912</td>
      <td>-68.302951</td>
    </tr>
    <tr>
      <th>24</th>
      <td>yebaishou</td>
      <td>cn</td>
      <td>41.405038</td>
      <td>119.636667</td>
    </tr>
    <tr>
      <th>25</th>
      <td>puerto madryn</td>
      <td>ar</td>
      <td>-42.769448</td>
      <td>-65.031717</td>
    </tr>
    <tr>
      <th>26</th>
      <td>andenes</td>
      <td>no</td>
      <td>69.316080</td>
      <td>16.120228</td>
    </tr>
    <tr>
      <th>27</th>
      <td>lata</td>
      <td>sb</td>
      <td>36.608590</td>
      <td>-95.979727</td>
    </tr>
    <tr>
      <th>28</th>
      <td>russell</td>
      <td>nz</td>
      <td>38.909472</td>
      <td>-98.748117</td>
    </tr>
    <tr>
      <th>29</th>
      <td>beloha</td>
      <td>mg</td>
      <td>-25.169449</td>
      <td>45.060656</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>470</th>
      <td>puerto pinasco</td>
      <td>py</td>
      <td>-22.646227</td>
      <td>-57.841006</td>
    </tr>
    <tr>
      <th>471</th>
      <td>skovorodino</td>
      <td>ru</td>
      <td>53.973257</td>
      <td>123.909222</td>
    </tr>
    <tr>
      <th>472</th>
      <td>hailey</td>
      <td>us</td>
      <td>43.519629</td>
      <td>-114.315325</td>
    </tr>
    <tr>
      <th>473</th>
      <td>mecca</td>
      <td>sa</td>
      <td>21.389082</td>
      <td>39.857912</td>
    </tr>
    <tr>
      <th>474</th>
      <td>eydhafushi</td>
      <td>mv</td>
      <td>5.102579</td>
      <td>73.068567</td>
    </tr>
    <tr>
      <th>475</th>
      <td>buchanan</td>
      <td>lr</td>
      <td>37.527357</td>
      <td>-79.679761</td>
    </tr>
    <tr>
      <th>476</th>
      <td>tselinnoye</td>
      <td>ru</td>
      <td>53.080378</td>
      <td>85.674715</td>
    </tr>
    <tr>
      <th>477</th>
      <td>hangal</td>
      <td>in</td>
      <td>14.771781</td>
      <td>75.125349</td>
    </tr>
    <tr>
      <th>478</th>
      <td>temaraia</td>
      <td>ki</td>
      <td>39.071430</td>
      <td>-77.133304</td>
    </tr>
    <tr>
      <th>479</th>
      <td>bathsheba</td>
      <td>bb</td>
      <td>13.210168</td>
      <td>-59.525354</td>
    </tr>
    <tr>
      <th>480</th>
      <td>emerald</td>
      <td>au</td>
      <td>-23.527291</td>
      <td>148.164573</td>
    </tr>
    <tr>
      <th>481</th>
      <td>naze</td>
      <td>jp</td>
      <td>51.866667</td>
      <td>1.283333</td>
    </tr>
    <tr>
      <th>482</th>
      <td>kuche</td>
      <td>cn</td>
      <td>41.717906</td>
      <td>82.962016</td>
    </tr>
    <tr>
      <th>483</th>
      <td>valparaiso</td>
      <td>cl</td>
      <td>41.473095</td>
      <td>-87.061141</td>
    </tr>
    <tr>
      <th>484</th>
      <td>rawson</td>
      <td>ar</td>
      <td>40.958942</td>
      <td>-83.784103</td>
    </tr>
    <tr>
      <th>485</th>
      <td>aksarka</td>
      <td>ru</td>
      <td>66.558310</td>
      <td>67.787762</td>
    </tr>
    <tr>
      <th>486</th>
      <td>hereford</td>
      <td>us</td>
      <td>34.815062</td>
      <td>-102.397704</td>
    </tr>
    <tr>
      <th>487</th>
      <td>palana</td>
      <td>ru</td>
      <td>59.084210</td>
      <td>159.954164</td>
    </tr>
    <tr>
      <th>488</th>
      <td>guarapari</td>
      <td>br</td>
      <td>-20.674120</td>
      <td>-40.499738</td>
    </tr>
    <tr>
      <th>489</th>
      <td>arman</td>
      <td>ru</td>
      <td>36.572491</td>
      <td>-96.703871</td>
    </tr>
    <tr>
      <th>490</th>
      <td>phan thiet</td>
      <td>vn</td>
      <td>10.980460</td>
      <td>108.261477</td>
    </tr>
    <tr>
      <th>491</th>
      <td>dunedin</td>
      <td>nz</td>
      <td>28.019740</td>
      <td>-82.771768</td>
    </tr>
    <tr>
      <th>492</th>
      <td>mezen</td>
      <td>ru</td>
      <td>65.861660</td>
      <td>44.229317</td>
    </tr>
    <tr>
      <th>493</th>
      <td>canberra</td>
      <td>au</td>
      <td>-35.280937</td>
      <td>149.130009</td>
    </tr>
    <tr>
      <th>494</th>
      <td>grand gaube</td>
      <td>mu</td>
      <td>-20.014212</td>
      <td>57.669408</td>
    </tr>
    <tr>
      <th>495</th>
      <td>yamada</td>
      <td>jp</td>
      <td>40.827312</td>
      <td>-74.103858</td>
    </tr>
    <tr>
      <th>496</th>
      <td>nizhneyansk</td>
      <td>ru</td>
      <td>71.450058</td>
      <td>136.112228</td>
    </tr>
    <tr>
      <th>497</th>
      <td>henties bay</td>
      <td>na</td>
      <td>-22.113496</td>
      <td>14.283204</td>
    </tr>
    <tr>
      <th>498</th>
      <td>bargal</td>
      <td>so</td>
      <td>11.285478</td>
      <td>51.076254</td>
    </tr>
    <tr>
      <th>499</th>
      <td>alice springs</td>
      <td>au</td>
      <td>-23.698042</td>
      <td>133.880747</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 4 columns</p>
</div>




```python
# Get Weather indicators for that city
api_key = "9e4061211f0bd062d8f853c78128227e"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "metric"

weather_data = []

for index, row in city_df.iterrows():
    lat1 = row["Latitude"]
    lng1 = row["Longitude"]
    city1 = row["City"]
    weather_url = url + "appid=" + api_key + "&units=" + units + "&lat=%s&lon=%s" % (lat1, lng1)
#     print(weather_url)
    weather_data.append(requests.get(weather_url).json())
#     print(json.dumps(weather_data[0], indent=4, sort_keys=True))
    
#     if not weather_data["main"]:
#         continue
        
    temperature = [data.get("main").get("temp") for data in weather_data]
    
#     if not weather_data["main"]:
#         continue
        
    humidity = [data.get("main").get("humidity") for data in weather_data]
    
#     if not weather_data["clouds"]:
#         continue
    
    cloudiness = [data.get("clouds").get("all") for data in weather_data]
    
#     if not weather_data["wind"]:
#         continue
    
    wind_speed = [data.get("wind").get("speed") for data in weather_data]
    
    #Print Log
    print("Processing Record " + str(index) + ": " + city1)
    print("URL: " + weather_url)
    print("")
# print(temperature)
# print(humidity)
# print(cloudiness)
# print(wind_speed)
city_df["Temperature"] = temperature
city_df["Humidity"] = humidity
city_df["Cloudiness"] = cloudiness
city_df["Wind Speed"] = wind_speed
city_df
```

    Processing Record 0: chernyshevskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=63.0250384&lon=112.4902494
    
    Processing Record 1: hermanus
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.4092004&lon=19.2504436
    
    Processing Record 2: saint-joseph
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=39.7674578&lon=-94.84668099999999
    
    Processing Record 3: mar del plata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-38.0054771&lon=-57.5426106
    
    Processing Record 4: nabire
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.5095462&lon=135.7520985
    
    Processing Record 5: sambava
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-14.2713338&lon=50.1678121
    
    Processing Record 6: tome-acu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-2.6729327&lon=-48.2393528
    
    Processing Record 7: taolanaro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-25.0225309&lon=46.9853688
    
    Processing Record 8: kargasok
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=59.0535405&lon=80.8770602
    
    Processing Record 9: jamestown
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.2116383&lon=-76.7752102
    
    Processing Record 10: cidreira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-30.1768543&lon=-50.2089307
    
    Processing Record 11: port alfred
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.5864065&lon=26.8851448
    
    Processing Record 12: vaini
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-21.1935&lon=-175.1751534
    
    Processing Record 13: geraldton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-28.7773715&lon=114.6149715
    
    Processing Record 14: cloquet
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=46.7217735&lon=-92.46118249999999
    
    Processing Record 15: kahului
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=20.8893351&lon=-156.4729469
    
    Processing Record 16: castro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.4985047&lon=-102.3463875
    
    Processing Record 17: busselton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.6554927&lon=115.3500188
    
    Processing Record 18: bolungarvik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.1517719&lon=-23.261709
    
    Processing Record 19: airai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=7.396611799999999&lon=134.5690225
    
    Processing Record 20: nikolskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=55.1981604&lon=166.0015368
    
    Processing Record 21: rikitea
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-23.119901&lon=-134.9702654
    
    Processing Record 22: sapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.7535541&lon=-111.8884772
    
    Processing Record 23: ushuaia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-54.8019121&lon=-68.3029511
    
    Processing Record 24: yebaishou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.405038&lon=119.636667
    
    Processing Record 25: puerto madryn
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-42.7694476&lon=-65.0317175
    
    Processing Record 26: andenes
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.31607989999999&lon=16.1202285
    
    Processing Record 27: lata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.6085904&lon=-95.97972720000001
    
    Processing Record 28: russell
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.9094717&lon=-98.7481167
    
    Processing Record 29: beloha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-25.1694494&lon=45.0606557
    
    Processing Record 30: barentsburg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=78.0648475&lon=14.2334597
    
    Processing Record 31: ust-kuyga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.99674999999999&lon=135.5713649
    
    Processing Record 32: kaitangata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-46.28334&lon=169.8470967
    
    Processing Record 33: sept-iles
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.21329679999999&lon=-66.37579199999999
    
    Processing Record 34: demba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-5.4906936&lon=22.274168
    
    Processing Record 35: eureka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.8020712&lon=-124.1636729
    
    Processing Record 36: codrington
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.6425736&lon=-61.8204456
    
    Processing Record 37: batagay-alyta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=67.79280899999999&lon=130.4017789
    
    Processing Record 38: khatanga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.964027&lon=102.4406129
    
    Processing Record 39: avarua
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-21.2129007&lon=-159.7823059
    
    Processing Record 40: hobyo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.3515922&lon=48.5249751
    
    Processing Record 41: torbay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.4392329&lon=-3.5369899
    
    Processing Record 42: hithadhoo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.6060574&lon=73.0892245
    
    Processing Record 43: new norfolk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-42.7828332&lon=147.0593848
    
    Processing Record 44: bethel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.6098147&lon=-122.3585019
    
    Processing Record 45: fort nelson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=58.8050174&lon=-122.697236
    
    Processing Record 46: siuna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=13.7405282&lon=-84.7811621
    
    Processing Record 47: ponta do sol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=32.6854883&lon=-17.0864266
    
    Processing Record 48: provideniya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=64.43272&lon=-173.238567
    
    Processing Record 49: half moon bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.4635519&lon=-122.4285862
    
    Processing Record 50: bambous virieux
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.3438619&lon=57.7636821
    
    Processing Record 51: umm kaddadah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=13.60375&lon=26.68413
    
    Processing Record 52: genhe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.780344&lon=121.5203881
    
    Processing Record 53: sisimiut
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.9394722&lon=-53.6733857
    
    Processing Record 54: punta arenas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-53.1638329&lon=-70.9170683
    
    Processing Record 55: cape town
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.9248685&lon=18.4240553
    
    Processing Record 56: saint george
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.0965278&lon=-113.5684164
    
    Processing Record 57: sao filipe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.8951679&lon=-24.4945636
    
    Processing Record 58: marcona
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-15.3439659&lon=-75.0844757
    
    Processing Record 59: nanortalik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.1424955&lon=-45.2394951
    
    Processing Record 60: mergui
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.4492291&lon=98.62706279999999
    
    Processing Record 61: sao joao da barra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-21.6368446&lon=-41.0484428
    
    Processing Record 62: massaguet
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.4830445&lon=15.4390254
    
    Processing Record 63: cabedelo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.966667&lon=-34.833333
    
    Processing Record 64: marv dasht
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=29.87873799999999&lon=52.8205515
    
    Processing Record 65: leningradskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.12884210000001&lon=79.2387741
    
    Processing Record 66: tiksi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.6374819&lon=128.8644717
    
    Processing Record 67: talara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-4.581151999999999&lon=-81.26289609999999
    
    Processing Record 68: attawapiskat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=52.9258846&lon=-82.42889219999999
    
    Processing Record 69: general roca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-39.032803&lon=-67.5892227
    
    Processing Record 70: bluff
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-29.9218618&lon=31.0048384
    
    Processing Record 71: ahipara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-35.17133889999999&lon=173.1532718
    
    Processing Record 72: mataura
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-46.1934288&lon=168.8660155
    
    Processing Record 73: whitehorse
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.7211871&lon=-135.0568448
    
    Processing Record 74: tuktoyaktuk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.445358&lon=-133.034181
    
    Processing Record 75: port moresby
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-9.4780123&lon=147.1506542
    
    Processing Record 76: namibe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-15.1978317&lon=12.1575544
    
    Processing Record 77: puerto ayora
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.7432918&lon=-90.3156893
    
    Processing Record 78: pristol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=44.2258469&lon=22.7092072
    
    Processing Record 79: amderma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.751221&lon=61.6636961
    
    Processing Record 80: thunder bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=48.3808951&lon=-89.2476823
    
    Processing Record 81: bengkulu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.5778471&lon=102.3463875
    
    Processing Record 82: mahebourg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.4059505&lon=57.7034222
    
    Processing Record 83: esperance
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.8608027&lon=121.8896205
    
    Processing Record 84: pevek
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.692947&lon=170.399612
    
    Processing Record 85: ihosy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-22.4008873&lon=46.127902
    
    Processing Record 86: las cruces
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=32.3199396&lon=-106.7636538
    
    Processing Record 87: luderitz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-26.6420382&lon=15.1639082
    
    Processing Record 88: todi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.7819352&lon=12.4065686
    
    Processing Record 89: vestmanna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.16316999999999&lon=-7.165286099999999
    
    Processing Record 90: chokurdakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=70.622169&lon=147.916168
    
    Processing Record 91: jimenez
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=8.3227548&lon=123.7629465
    
    Processing Record 92: sao mateus
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-18.7190705&lon=-39.856734
    
    Processing Record 93: nusaybin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.0696439&lon=41.213997
    
    Processing Record 94: mrirt
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.16271400000001&lon=-5.5669786
    
    Processing Record 95: san andres
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.5450153&lon=-81.7075787
    
    Processing Record 96: victoria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=28.8052674&lon=-97.0035982
    
    Processing Record 97: kavieng
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-2.5781167&lon=150.8086082
    
    Processing Record 98: mansoa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.0813122&lon=-15.3189726
    
    Processing Record 99: ambilobe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-13.2026069&lon=49.0514094
    
    Processing Record 100: pasni
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=25.2509599&lon=63.4154233
    
    Processing Record 101: vaitupu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-7.476732699999999&lon=178.6747675
    
    Processing Record 102: dikson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=73.50489&lon=80.58091689999999
    
    Processing Record 103: walvis bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-22.9389587&lon=14.5247463
    
    Processing Record 104: loa janan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.7048801&lon=117.0323305
    
    Processing Record 105: qaanaaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=77.4670434&lon=-69.2284827
    
    Processing Record 106: gurgan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.8456427&lon=54.4393363
    
    Processing Record 107: albany
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.6525793&lon=-73.7562317
    
    Processing Record 108: touba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.8665572&lon=-15.8994956
    
    Processing Record 109: vila franca do campo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.7157269&lon=-25.4345063
    
    Processing Record 110: klaksvik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.2271922&lon=-6.5779064
    
    Processing Record 111: matara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.95492&lon=80.5549561
    
    Processing Record 112: takoradi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=4.9015794&lon=-1.7830973
    
    Processing Record 113: mirnyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.5313374&lon=113.9773882
    
    Processing Record 114: east london
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.0291582&lon=27.8545867
    
    Processing Record 115: taree
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-31.8894897&lon=152.4444453
    
    Processing Record 116: husavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.0449706&lon=-17.3383442
    
    Processing Record 117: barrow
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.29055559999999&lon=-156.788611
    
    Processing Record 118: xining
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.617134&lon=101.778223
    
    Processing Record 119: waverley
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.899693&lon=151.2554202
    
    Processing Record 120: kapaa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=22.0881391&lon=-159.3379818
    
    Processing Record 121: hohhot
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.842356&lon=111.749995
    
    Processing Record 122: manta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.9676533&lon=-80.7089101
    
    Processing Record 123: kulhudhuffushi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=6.6261633&lon=73.0670909
    
    Processing Record 124: hofn
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=64.24970259999999&lon=-15.2020077
    
    Processing Record 125: saleaula
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-13.4482906&lon=-172.3367114
    
    Processing Record 126: mandalgovi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=45.7631589&lon=106.2650297
    
    Processing Record 127: kamenskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.46661&lon=166.211243
    
    Processing Record 128: kachiry
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.0695445&lon=76.1038295
    
    Processing Record 129: saint-philippe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-21.3035757&lon=55.7356129
    
    Processing Record 130: huarmey
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-10.0664169&lon=-78.1506824
    
    Processing Record 131: portland
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=45.5230622&lon=-122.6764815
    
    Processing Record 132: kentau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=43.5161759&lon=68.5090258
    
    Processing Record 133: bereda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.8966354&lon=85.962385
    
    Processing Record 134: marzuq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=25.9182262&lon=13.9260001
    
    Processing Record 135: algiers
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.753768&lon=3.0587561
    
    Processing Record 136: dmitriyevskaya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=45.6592536&lon=40.7666934
    
    Processing Record 137: beian
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=63.6555366&lon=9.5695254
    
    Processing Record 138: kodiak
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=57.79000000000001&lon=-152.4072221
    
    Processing Record 139: bodden town
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.2816905&lon=-81.25738989999999
    
    Processing Record 140: fortuna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.5981867&lon=-124.1572756
    
    Processing Record 141: saskylakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.954308&lon=114.1198349
    
    Processing Record 142: toliary
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-23.3516191&lon=43.6854936
    
    Processing Record 143: fare
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.0193548&lon=-7.9304397
    
    Processing Record 144: san patricio
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=27.9999695&lon=-97.5247243
    
    Processing Record 145: oranjemund
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-28.5597111&lon=16.4346979
    
    Processing Record 146: les cayes
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=18.2042851&lon=-73.7536695
    
    Processing Record 147: tasiilaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.6134561&lon=-37.6335696
    
    Processing Record 148: lompoc
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.6391501&lon=-120.4579409
    
    Processing Record 149: atasu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=48.6789286&lon=71.6308323
    
    Processing Record 150: almaznyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.44681509999999&lon=114.3182712
    
    Processing Record 151: faanui
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-16.5004126&lon=-151.7414904
    
    Processing Record 152: carnarvon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-24.884&lon=113.661
    
    Processing Record 153: hilo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.7070942&lon=-155.0884869
    
    Processing Record 154: belushya guba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.54555599999999&lon=52.32027799999999
    
    Processing Record 155: kralendijk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.1443491&lon=-68.2655456
    
    Processing Record 156: mayo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.1509434&lon=-95.9921629
    
    Processing Record 157: saint-pierre
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=46.7758459&lon=-56.1806363
    
    Processing Record 158: illoqqortoormiut
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=70.48556909999999&lon=-21.9628757
    
    Processing Record 159: radford
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.13179239999999&lon=-80.5764477
    
    Processing Record 160: san rafael
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.9735346&lon=-122.5310874
    
    Processing Record 161: chagda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=58.75177799999999&lon=130.6163939
    
    Processing Record 162: huangchuan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=32.131522&lon=115.051908
    
    Processing Record 163: chuy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.421356&lon=-97.4950777
    
    Processing Record 164: iqaluit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=63.74669300000001&lon=-68.5169669
    
    Processing Record 165: hobart
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.5322592&lon=-87.2550353
    
    Processing Record 166: doha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=25.2854473&lon=51.53103979999999
    
    Processing Record 167: tuatapere
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-46.1357909&lon=167.6892231
    
    Processing Record 168: upernavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=72.7862882&lon=-56.1375527
    
    Processing Record 169: naifaru
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.4443719&lon=73.3657118
    
    Processing Record 170: yellowknife
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=62.4539717&lon=-114.3717887
    
    Processing Record 171: margate
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=51.38964600000001&lon=1.3868339
    
    Processing Record 172: pierre
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=44.3683156&lon=-100.3509665
    
    Processing Record 173: safaga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=26.7500171&lon=33.9359756
    
    Processing Record 174: hamadan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.7988575&lon=48.5150225
    
    Processing Record 175: chara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=13.4730731&lon=74.9734805
    
    Processing Record 176: labutta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=16.149328&lon=94.7562159
    
    Processing Record 177: burayevo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=55.8487604&lon=55.40569470000001
    
    Processing Record 178: kitimat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.049366&lon=-128.6283529
    
    Processing Record 179: butaritari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=3.1166667&lon=172.8
    
    Processing Record 180: tumannyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=68.878911&lon=35.6675264
    
    Processing Record 181: ivanovka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.74694059999999&lon=48.0332558
    
    Processing Record 182: port lincoln
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.7301943&lon=135.8504838
    
    Processing Record 183: vardo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=70.37063979999999&lon=31.1095471
    
    Processing Record 184: komsomolskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.2377642&lon=50.844555
    
    Processing Record 185: de-kastri
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=51.475113&lon=140.769104
    
    Processing Record 186: alofi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-19.0553711&lon=-169.9178709
    
    Processing Record 187: qaqortoq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.71898640000001&lon=-46.03534579999999
    
    Processing Record 188: talnakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.5&lon=88.39999999999999
    
    Processing Record 189: san cristobal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=16.7370359&lon=-92.6376186
    
    Processing Record 190: kirakira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.9162413&lon=-117.9937794
    
    Processing Record 191: grand-santi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=4.2507342&lon=-54.38225
    
    Processing Record 192: kuvandyk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=51.479495&lon=57.3641027
    
    Processing Record 193: corinto
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.4907414&lon=-87.1784334
    
    Processing Record 194: labuhan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.372081&lon=105.832298
    
    Processing Record 195: mount gambier
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-37.8284212&lon=140.7803529
    
    Processing Record 196: karamea
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-41.247636&lon=172.1114872
    
    Processing Record 197: thompson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.6454798&lon=-117.6413112
    
    Processing Record 198: constitucion
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.6022037&lon=-61.0471355
    
    Processing Record 199: ngukurr
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-14.7327677&lon=134.7323319
    
    Processing Record 200: eaglesfield
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.63982900000001&lon=-3.403552
    
    Processing Record 201: nakhon thai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.0476061&lon=-87.6813119
    
    Processing Record 202: christchurch
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-43.5320544&lon=172.6362254
    
    Processing Record 203: belmonte
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.5202145&lon=-122.2758008
    
    Processing Record 204: yumen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.29184300000001&lon=97.04567800000001
    
    Processing Record 205: bredasdorp
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.5385222&lon=20.0568771
    
    Processing Record 206: key west
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=24.5550593&lon=-81.7799871
    
    Processing Record 207: vila do maio
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=15.1385669&lon=-23.2066909
    
    Processing Record 208: harper
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.0810643&lon=-88.07289689999999
    
    Processing Record 209: santa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-9.0721136&lon=-78.58886960000001
    
    Processing Record 210: pisco
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-13.7134562&lon=-76.1841701
    
    Processing Record 211: mahibadhoo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=3.756783299999999&lon=72.9689398
    
    Processing Record 212: lovozero
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=68.0063722&lon=35.0259996
    
    Processing Record 213: guerrero negro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=27.9591758&lon=-114.056646
    
    Processing Record 214: nong khae
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.3839605&lon=100.8677256
    
    Processing Record 215: bardiyah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=31.7579946&lon=25.0792189
    
    Processing Record 216: callaway
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=30.1453538&lon=-85.5743227
    
    Processing Record 217: davidson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.4993031&lon=-80.8486846
    
    Processing Record 218: biu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=10.6116811&lon=12.1918637
    
    Processing Record 219: helena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=46.5883707&lon=-112.0245054
    
    Processing Record 220: coolum beach
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-26.5276917&lon=153.0736329
    
    Processing Record 221: kununurra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-15.7785409&lon=128.7416854
    
    Processing Record 222: buin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.7309374&lon=-70.7423218
    
    Processing Record 223: mumbwa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-15.0368764&lon=26.6081653
    
    Processing Record 224: gilroy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.0057816&lon=-121.5682751
    
    Processing Record 225: sovetskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.53926260000001&lon=28.6716201
    
    Processing Record 226: coos bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=43.3665007&lon=-124.2178903
    
    Processing Record 227: kigonsera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-10.794721&lon=35.069098
    
    Processing Record 228: georgetown
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.9076089&lon=-77.07225849999999
    
    Processing Record 229: tabou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=4.427919699999999&lon=-7.357057599999999
    
    Processing Record 230: petropavlovsk-kamchatskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.0409109&lon=158.677726
    
    Processing Record 231: pangnirtung
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.1465578&lon=-65.7012182
    
    Processing Record 232: dali
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=27.7659901&lon=-82.6314354
    
    Processing Record 233: srednekolymsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=67.43730699999999&lon=153.728674
    
    Processing Record 234: padang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.9470831999999999&lon=100.417181
    
    Processing Record 235: mbuji-mayi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.1306709&lon=23.5966577
    
    Processing Record 236: gengenbach
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=48.4098117&lon=8.009599699999999
    
    Processing Record 237: cambridge
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=52.205337&lon=0.121817
    
    Processing Record 238: flinders
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-38.474&lon=145.022
    
    Processing Record 239: tautira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-17.74932&lon=-149.160155
    
    Processing Record 240: santa cruz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.9741171&lon=-122.0307963
    
    Processing Record 241: owando
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.4829632&lon=15.8936224
    
    Processing Record 242: marquette
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=43.0382216&lon=-87.9297546
    
    Processing Record 243: santa vitoria do palmar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.5215632&lon=-53.3664818
    
    Processing Record 244: payo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=13.8935587&lon=124.2754988
    
    Processing Record 245: balkhash
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=46.2160876&lon=74.3775
    
    Processing Record 246: sobolevo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.300693&lon=155.956757
    
    Processing Record 247: amga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.8963675&lon=131.973712
    
    Processing Record 248: lodja
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.5245104&lon=23.5966577
    
    Processing Record 249: norman wells
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.28149379999999&lon=-126.8286524
    
    Processing Record 250: witrivier
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-25.3299288&lon=31.0163079
    
    Processing Record 251: petatlan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.5390397&lon=-101.2701934
    
    Processing Record 252: arlington
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.8816208&lon=-77.09098089999999
    
    Processing Record 253: frontera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=26.9377144&lon=-101.4446968
    
    Processing Record 254: paamiut
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=61.99411509999999&lon=-49.6665709
    
    Processing Record 255: longyearbyen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=78.22317220000001&lon=15.6267229
    
    Processing Record 256: ribeira grande
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.8210369&lon=-25.5148137
    
    Processing Record 257: fujin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.0577491&lon=-117.937401
    
    Processing Record 258: krasnomayskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.9955034&lon=82.52015540000001
    
    Processing Record 259: cabo san lucas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=22.8905327&lon=-109.9167371
    
    Processing Record 260: te anau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-45.4144515&lon=167.718053
    
    Processing Record 261: port elizabeth
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.7139247&lon=25.5207358
    
    Processing Record 262: azle
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=32.8951262&lon=-97.5458565
    
    Processing Record 263: narathiwat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=6.4254607&lon=101.8253143
    
    Processing Record 264: cururupu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-1.8303572&lon=-44.813111
    
    Processing Record 265: winneba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.3622295&lon=-0.6298922
    
    Processing Record 266: isangel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-19.54172&lon=169.2803499
    
    Processing Record 267: port macquarie
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-31.433333&lon=152.9
    
    Processing Record 268: severo-kurilsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.66808&lon=156.1138803
    
    Processing Record 269: baft
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=29.233038&lon=56.60089720000001
    
    Processing Record 270: dicabisagan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.0341572&lon=122.367428
    
    Processing Record 271: broken hill
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-31.9539135&lon=141.4539396
    
    Processing Record 272: hamilton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=39.3995008&lon=-84.5613355
    
    Processing Record 273: bethanien
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-26.5090889&lon=17.1514061
    
    Processing Record 274: kasongo-lunda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.4788114&lon=16.8119382
    
    Processing Record 275: elmira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.0897965&lon=-76.8077338
    
    Processing Record 276: maxixe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-23.8611302&lon=35.3411388
    
    Processing Record 277: great falls
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=47.4941836&lon=-111.2833449
    
    Processing Record 278: atuona
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-9.803279999999999&lon=-139.0425741
    
    Processing Record 279: nouakchott
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=18.0735299&lon=-15.9582372
    
    Processing Record 280: mehran
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.1176647&lon=46.1732912
    
    Processing Record 281: hambantota
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=6.1428829&lon=81.12123079999999
    
    Processing Record 282: ondorhaan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=47.3178096&lon=110.6535448
    
    Processing Record 283: vila
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-17.7332512&lon=168.3273245
    
    Processing Record 284: auki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-8.7678593&lon=160.6960306
    
    Processing Record 285: samarai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-10.6120235&lon=150.6640758
    
    Processing Record 286: kieta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.2204747&lon=155.6376301
    
    Processing Record 287: sao geraldo do araguaia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.204223199999999&lon=-48.7995122
    
    Processing Record 288: vilhena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-12.7414031&lon=-60.1304566
    
    Processing Record 289: katsuura
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.152283&lon=140.3209324
    
    Processing Record 290: shimoda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.6795334&lon=138.945316
    
    Processing Record 291: acarau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-2.8880562&lon=-40.11867669999999
    
    Processing Record 292: hasaki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.72969990000001&lon=-73.98881349999999
    
    Processing Record 293: meyungs
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=7.352715099999999&lon=134.4488579
    
    Processing Record 294: plettenberg bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.05748920000001&lon=23.3644925
    
    Processing Record 295: moyale
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=3.5399846&lon=39.0528407
    
    Processing Record 296: fethiye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.659246&lon=29.126347
    
    Processing Record 297: laguna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=29.4191226&lon=-100.0056207
    
    Processing Record 298: ajdabiya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=30.214647&lon=20.1402594
    
    Processing Record 299: oussouye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.4884485&lon=-16.543676
    
    Processing Record 300: shenjiamen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=31.8777376&lon=120.2891974
    
    Processing Record 301: tecoanapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=16.987876&lon=-99.25969049999999
    
    Processing Record 302: grindavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=63.8441808&lon=-22.4383818
    
    Processing Record 303: tezu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=27.9338514&lon=96.15800109999999
    
    Processing Record 304: puri
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.8133822&lon=85.8314655
    
    Processing Record 305: cherskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=68.742677&lon=161.3507839
    
    Processing Record 306: ketchikan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=55.34222219999999&lon=-131.6461112
    
    Processing Record 307: ca mau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=9.1526728&lon=105.1960795
    
    Processing Record 308: mendi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-6.1493907&lon=143.6474817
    
    Processing Record 309: pericos
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.0803044&lon=-106.6223302
    
    Processing Record 310: carlsbad
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.1580933&lon=-117.3505939
    
    Processing Record 311: redmond
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=47.6739881&lon=-122.121512
    
    Processing Record 312: alotau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-10.3157027&lon=150.4587795
    
    Processing Record 313: narsaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.9129622&lon=-46.0504831
    
    Processing Record 314: kargil
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.5538522&lon=76.1348944
    
    Processing Record 315: nandura
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=20.8307334&lon=76.4505243
    
    Processing Record 316: vitimskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=58.2269641&lon=113.248804
    
    Processing Record 317: umzimvubu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-30.7781755&lon=28.9528645
    
    Processing Record 318: dhidhdhoo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=6.888643999999999&lon=73.113569
    
    Processing Record 319: tignere
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=7.3730873&lon=12.6532004
    
    Processing Record 320: lowestoft
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=52.48113799999999&lon=1.753449
    
    Processing Record 321: sur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.0812107&lon=-118.3852167
    
    Processing Record 322: port-gentil
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.7351025999999999&lon=8.7591311
    
    Processing Record 323: boa vista
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=16.0950108&lon=-22.8078335
    
    Processing Record 324: voh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.9557855&lon=164.6839545
    
    Processing Record 325: artesia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=33.8658484&lon=-118.0831212
    
    Processing Record 326: pudozh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=61.80827619999999&lon=36.5298106
    
    Processing Record 327: bara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.1171432&lon=16.8718715
    
    Processing Record 328: mocambique
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-18.665695&lon=35.529562
    
    Processing Record 329: lucapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-8.4432485&lon=20.7301263
    
    Processing Record 330: semey
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.4233463&lon=80.250811
    
    Processing Record 331: jiayuguan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=39.772554&lon=98.289419
    
    Processing Record 332: holme
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.6208877&lon=9.6764394
    
    Processing Record 333: yei
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=4.095271299999999&lon=30.6775054
    
    Processing Record 334: tsihombe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-25.3168473&lon=45.48630929999999
    
    Processing Record 335: bani
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=18.2854798&lon=-70.3301173
    
    Processing Record 336: yulara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-25.2334936&lon=130.9848698
    
    Processing Record 337: kirkuk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.4655761&lon=44.38039209999999
    
    Processing Record 338: salinopolis
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-0.6228817999999999&lon=-47.3468841
    
    Processing Record 339: acari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-15.4384565&lon=-74.6180833
    
    Processing Record 340: colares
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.8066307&lon=-9.4426867
    
    Processing Record 341: clyde river
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=70.47636500000002&lon=-68.60126509999999
    
    Processing Record 342: grand river south east
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.2888094&lon=57.78141199999999
    
    Processing Record 343: itarema
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-2.9209642&lon=-39.91673979999999
    
    Processing Record 344: cam ranh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=11.9662117&lon=109.1915578
    
    Processing Record 345: bitung
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=1.4403744&lon=125.1216524
    
    Processing Record 346: livingston
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.7862871&lon=-74.3300842
    
    Processing Record 347: tsabong
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-26.0207794&lon=22.4112651
    
    Processing Record 348: banda aceh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.5482904&lon=95.3237559
    
    Processing Record 349: ilulissat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=69.2198118&lon=-51.0986031
    
    Processing Record 350: salalah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.0506675&lon=54.1065864
    
    Processing Record 351: tulagi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-9.1015826&lon=160.147069
    
    Processing Record 352: pitimbu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-7.3989338&lon=-34.8403408
    
    Processing Record 353: leopoldov
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=48.4465089&lon=17.7681976
    
    Processing Record 354: pyinmana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.7413519&lon=96.2004383
    
    Processing Record 355: ende
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-8.854053&lon=121.654198
    
    Processing Record 356: aranos
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-24.1377718&lon=19.1107114
    
    Processing Record 357: rocha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.4790141&lon=-54.3352997
    
    Processing Record 358: mackay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-21.1424956&lon=149.1821469
    
    Processing Record 359: upata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=8.0093062&lon=-62.40155360000001
    
    Processing Record 360: pegnitz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=49.751552&lon=11.5432054
    
    Processing Record 361: la tuque
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=47.4383405&lon=-72.7839311
    
    Processing Record 362: nenjiang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=49.185766&lon=125.221192
    
    Processing Record 363: novopavlovka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.8747944&lon=74.4848003
    
    Processing Record 364: kijang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=19.0815115&lon=84.0418536
    
    Processing Record 365: kadaya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=21.1000174&lon=70.33383719999999
    
    Processing Record 366: lebu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-37.6097143&lon=-73.6483294
    
    Processing Record 367: smirnykh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=49.7567238&lon=142.8481098
    
    Processing Record 368: cockburn town
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=21.4674584&lon=-71.13891009999999
    
    Processing Record 369: cabra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.3673946&lon=-6.3023348
    
    Processing Record 370: sinnamary
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.3746712&lon=-52.9545991
    
    Processing Record 371: marystown
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=47.1650754&lon=-55.1555299
    
    Processing Record 372: kendari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.9984597&lon=122.5129742
    
    Processing Record 373: achikulak
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=44.5479719&lon=44.8328236
    
    Processing Record 374: beringovskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=63.065438&lon=179.355123
    
    Processing Record 375: sitka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=57.0530556&lon=-135.33
    
    Processing Record 376: athabasca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.72140940000001&lon=-113.2861822
    
    Processing Record 377: panama city
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=8.9823792&lon=-79.51986959999999
    
    Processing Record 378: barbar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=26.2276456&lon=50.4773453
    
    Processing Record 379: asau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-13.5191366&lon=-172.6294361
    
    Processing Record 380: souillac
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=44.893626&lon=1.478613
    
    Processing Record 381: arraial do cabo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-22.9673373&lon=-42.0268099
    
    Processing Record 382: port hedland
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.3106621&lon=118.5878223
    
    Processing Record 383: ancud
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-41.8675003&lon=-73.8276965
    
    Processing Record 384: sembe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=1.6480399&lon=14.5742675
    
    Processing Record 385: newtownards
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.5913787&lon=-5.693170299999999
    
    Processing Record 386: port hardy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=50.7123867&lon=-127.460393
    
    Processing Record 387: shelburne
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=44.3806065&lon=-73.227626
    
    Processing Record 388: muli
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=22.6386101&lon=71.4581359
    
    Processing Record 389: pathein
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=16.7753609&lon=94.7381013
    
    Processing Record 390: senneterre
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=48.390597&lon=-77.2411674
    
    Processing Record 391: aykhal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.94171899999999&lon=111.4881131
    
    Processing Record 392: ambo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=8.972154699999999&lon=37.8618422
    
    Processing Record 393: brewster
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.3973163&lon=-73.6170721
    
    Processing Record 394: saint-louis
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.6270025&lon=-90.19940419999999
    
    Processing Record 395: belaya gora
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=68.549671&lon=146.232876
    
    Processing Record 396: lazaro cardenas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.9574888&lon=-102.1922321
    
    Processing Record 397: kaabong
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=3.5126215&lon=33.9750018
    
    Processing Record 398: lagoa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.1356631&lon=-8.451722799999999
    
    Processing Record 399: itamaraca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-7.748077599999999&lon=-34.8306327
    
    Processing Record 400: okhotsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=59.35845999999999&lon=143.2034909
    
    Processing Record 401: tokur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.1392062&lon=132.8938627
    
    Processing Record 402: buraydah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=26.3592309&lon=43.98181200000001
    
    Processing Record 403: ixtapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=17.6625661&lon=-101.58734
    
    Processing Record 404: nishihara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=26.2228445&lon=127.7588107
    
    Processing Record 405: guiratinga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-16.3760849&lon=-53.6501903
    
    Processing Record 406: bud
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=38.046766&lon=-84.461024
    
    Processing Record 407: aswan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=24.088938&lon=32.8998293
    
    Processing Record 408: ludvika
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.152358&lon=15.1916391
    
    Processing Record 409: neya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=58.28775649999999&lon=43.8806688
    
    Processing Record 410: chumikan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=54.708952&lon=135.322998
    
    Processing Record 411: ust-tsilma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.43333299999999&lon=52.15
    
    Processing Record 412: sonoita
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=31.6795337&lon=-110.6553594
    
    Processing Record 413: sentyabrskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.493056&lon=72.19638909999999
    
    Processing Record 414: bairiki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=1.3290526&lon=172.9790528
    
    Processing Record 415: lavrentiya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.58354899999999&lon=-171.0389249
    
    Processing Record 416: tres lagoas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.7881656&lon=-51.7030882
    
    Processing Record 417: san carlos de bariloche
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-41.1334722&lon=-71.3102778
    
    Processing Record 418: roma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.2191474&lon=-93.37085669999999
    
    Processing Record 419: ilinskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=55.6239201&lon=38.1058405
    
    Processing Record 420: iwaki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.0504195&lon=140.8876817
    
    Processing Record 421: vanavara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=60.3394104&lon=102.2965426
    
    Processing Record 422: coari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-4.094595700000001&lon=-63.14460319999999
    
    Processing Record 423: rungata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-1.3493599&lon=176.445007
    
    Processing Record 424: puerto escondido
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=15.8719795&lon=-97.0767365
    
    Processing Record 425: chapais
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=49.78221&lon=-74.85248
    
    Processing Record 426: banes
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=20.9570351&lon=-75.7197039
    
    Processing Record 427: richards bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-28.7807276&lon=32.0382856
    
    Processing Record 428: praia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.93305&lon=-23.5133267
    
    Processing Record 429: banjar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=31.6376776&lon=77.34407920000001
    
    Processing Record 430: narragansett
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.4500844&lon=-71.44950299999999
    
    Processing Record 431: hamada
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.55793509999999&lon=-87.7968763
    
    Processing Record 432: sydney
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.8688197&lon=151.2092955
    
    Processing Record 433: kishapu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.6124625&lon=33.8697326
    
    Processing Record 434: nouadhibou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=20.9425179&lon=-17.0362272
    
    Processing Record 435: jalu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=29.040621&lon=21.4991334
    
    Processing Record 436: ternate
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=0.7957999&lon=127.3613533
    
    Processing Record 437: bilibino
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=68.05963899999999&lon=166.443893
    
    Processing Record 438: beyneu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=45.3222362&lon=55.181848
    
    Processing Record 439: carutapera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-1.3464282&lon=-46.01818799999999
    
    Processing Record 440: colombo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=6.9270786&lon=79.861243
    
    Processing Record 441: bahia honda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=24.6698814&lon=-81.2546366
    
    Processing Record 442: warwick
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.7001009&lon=-71.4161671
    
    Processing Record 443: blonduos
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.66009249999999&lon=-20.2796586
    
    Processing Record 444: vicksburg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=32.3526456&lon=-90.877882
    
    Processing Record 445: jaypur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=23.0551658&lon=87.44801149999999
    
    Processing Record 446: general jose eduvigis diaz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-27.2013259&lon=-58.3688379
    
    Processing Record 447: broome
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=42.1348907&lon=-75.9088634
    
    Processing Record 448: saldanha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-33.0276981&lon=17.9176312
    
    Processing Record 449: namatanai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-3.6528325&lon=152.3917226
    
    Processing Record 450: anadyr
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=64.7336613&lon=177.4968265
    
    Processing Record 451: lujan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-34.5633312&lon=-59.1208805
    
    Processing Record 452: alta floresta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-9.8764576&lon=-56.08550899999999
    
    Processing Record 453: bourg-en-bresse
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=46.20516749999999&lon=5.2255007
    
    Processing Record 454: caravelas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-17.6793494&lon=-39.6105142
    
    Processing Record 455: nemuro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=43.3300759&lon=145.5827903
    
    Processing Record 456: yar-sale
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.86258699999999&lon=70.854469
    
    Processing Record 457: tallahassee
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=30.4382559&lon=-84.28073289999999
    
    Processing Record 458: bundaberg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-24.8669736&lon=152.3509714
    
    Processing Record 459: youghal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=51.9542704&lon=-7.8471707
    
    Processing Record 460: presidencia roque saenz pena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-26.8004428&lon=-60.4312354
    
    Processing Record 461: pousat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=12.3663211&lon=103.6362715
    
    Processing Record 462: kutum
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.2025174&lon=24.6638251
    
    Processing Record 463: caetite
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-14.0648657&lon=-42.48588669999999
    
    Processing Record 464: abu dhabi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=24.453884&lon=54.3773438
    
    Processing Record 465: las palmas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=28.1235459&lon=-15.4362574
    
    Processing Record 466: leshukonskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=64.89734659999999&lon=45.76517339999999
    
    Processing Record 467: bogorodskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=55.8209378&lon=37.7061587
    
    Processing Record 468: durham
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=35.9940329&lon=-78.898619
    
    Processing Record 469: fairbanks
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=64.8377778&lon=-147.7163888
    
    Processing Record 470: puerto pinasco
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-22.6462275&lon=-57.8410062
    
    Processing Record 471: skovorodino
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.97325739999999&lon=123.9092222
    
    Processing Record 472: hailey
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=43.51962880000001&lon=-114.3153245
    
    Processing Record 473: mecca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=21.3890824&lon=39.8579118
    
    Processing Record 474: eydhafushi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=5.1025787&lon=73.0685665
    
    Processing Record 475: buchanan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=37.5273566&lon=-79.67976139999999
    
    Processing Record 476: tselinnoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=53.0803781&lon=85.6747154
    
    Processing Record 477: hangal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=14.771781&lon=75.1253492
    
    Processing Record 478: temaraia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=39.07143&lon=-77.133304
    
    Processing Record 479: bathsheba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=13.2101681&lon=-59.5253545
    
    Processing Record 480: emerald
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-23.527291&lon=148.164573
    
    Processing Record 481: naze
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=51.8666667&lon=1.2833333
    
    Processing Record 482: kuche
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.717906&lon=82.96201599999999
    
    Processing Record 483: valparaiso
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=41.4730948&lon=-87.0611412
    
    Processing Record 484: rawson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.9589419&lon=-83.7841028
    
    Processing Record 485: aksarka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=66.5583101&lon=67.7877623
    
    Processing Record 486: hereford
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=34.8150622&lon=-102.3977036
    
    Processing Record 487: palana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=59.0842105&lon=159.9541642
    
    Processing Record 488: guarapari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.6741197&lon=-40.4997382
    
    Processing Record 489: arman
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=36.572491&lon=-96.70387099999999
    
    Processing Record 490: phan thiet
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=10.9804603&lon=108.2614775
    
    Processing Record 491: dunedin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=28.0197404&lon=-82.7717684
    
    Processing Record 492: mezen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=65.8616596&lon=44.2293172
    
    Processing Record 493: canberra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-35.2809368&lon=149.1300092
    
    Processing Record 494: grand gaube
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-20.014212&lon=57.6694082
    
    Processing Record 495: yamada
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=40.8273116&lon=-74.1038578
    
    Processing Record 496: nizhneyansk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=71.450058&lon=136.1122279
    
    Processing Record 497: henties bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-22.1134964&lon=14.2832038
    
    Processing Record 498: bargal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=11.2854783&lon=51.0762536
    
    Processing Record 499: alice springs
    URL: http://api.openweathermap.org/data/2.5/weather?appid=9e4061211f0bd062d8f853c78128227e&units=metric&lat=-23.698042&lon=133.8807471
    





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
      <th>Cloudiness</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>chernyshevskiy</td>
      <td>ru</td>
      <td>63.025038</td>
      <td>112.490249</td>
      <td>-33.87</td>
      <td>45</td>
      <td>0</td>
      <td>1.16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hermanus</td>
      <td>za</td>
      <td>-34.409200</td>
      <td>19.250444</td>
      <td>18.86</td>
      <td>65</td>
      <td>8</td>
      <td>4.01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>saint-joseph</td>
      <td>re</td>
      <td>39.767458</td>
      <td>-94.846681</td>
      <td>6.00</td>
      <td>45</td>
      <td>1</td>
      <td>7.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>mar del plata</td>
      <td>ar</td>
      <td>-38.005477</td>
      <td>-57.542611</td>
      <td>28.00</td>
      <td>48</td>
      <td>40</td>
      <td>3.10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nabire</td>
      <td>id</td>
      <td>-3.509546</td>
      <td>135.752098</td>
      <td>20.21</td>
      <td>98</td>
      <td>92</td>
      <td>0.86</td>
    </tr>
    <tr>
      <th>5</th>
      <td>sambava</td>
      <td>mg</td>
      <td>-14.271334</td>
      <td>50.167812</td>
      <td>28.14</td>
      <td>100</td>
      <td>8</td>
      <td>7.36</td>
    </tr>
    <tr>
      <th>6</th>
      <td>tome-acu</td>
      <td>br</td>
      <td>-2.672933</td>
      <td>-48.239353</td>
      <td>29.44</td>
      <td>72</td>
      <td>56</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>7</th>
      <td>taolanaro</td>
      <td>mg</td>
      <td>-25.022531</td>
      <td>46.985369</td>
      <td>28.00</td>
      <td>88</td>
      <td>40</td>
      <td>5.10</td>
    </tr>
    <tr>
      <th>8</th>
      <td>kargasok</td>
      <td>ru</td>
      <td>59.053540</td>
      <td>80.877060</td>
      <td>-29.07</td>
      <td>29</td>
      <td>0</td>
      <td>1.11</td>
    </tr>
    <tr>
      <th>9</th>
      <td>jamestown</td>
      <td>sh</td>
      <td>37.211638</td>
      <td>-76.775210</td>
      <td>12.38</td>
      <td>87</td>
      <td>75</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>10</th>
      <td>cidreira</td>
      <td>br</td>
      <td>-30.176854</td>
      <td>-50.208931</td>
      <td>27.56</td>
      <td>75</td>
      <td>0</td>
      <td>4.36</td>
    </tr>
    <tr>
      <th>11</th>
      <td>port alfred</td>
      <td>za</td>
      <td>-33.586407</td>
      <td>26.885145</td>
      <td>20.49</td>
      <td>92</td>
      <td>68</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>12</th>
      <td>vaini</td>
      <td>to</td>
      <td>-21.193500</td>
      <td>-175.175153</td>
      <td>23.00</td>
      <td>100</td>
      <td>90</td>
      <td>2.60</td>
    </tr>
    <tr>
      <th>13</th>
      <td>geraldton</td>
      <td>au</td>
      <td>-28.777372</td>
      <td>114.614971</td>
      <td>21.00</td>
      <td>88</td>
      <td>75</td>
      <td>7.20</td>
    </tr>
    <tr>
      <th>14</th>
      <td>cloquet</td>
      <td>us</td>
      <td>46.721773</td>
      <td>-92.461182</td>
      <td>-1.32</td>
      <td>80</td>
      <td>90</td>
      <td>3.10</td>
    </tr>
    <tr>
      <th>15</th>
      <td>kahului</td>
      <td>us</td>
      <td>20.889335</td>
      <td>-156.472947</td>
      <td>20.28</td>
      <td>82</td>
      <td>1</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>16</th>
      <td>castro</td>
      <td>cl</td>
      <td>34.498505</td>
      <td>-102.346388</td>
      <td>7.34</td>
      <td>30</td>
      <td>1</td>
      <td>3.60</td>
    </tr>
    <tr>
      <th>17</th>
      <td>busselton</td>
      <td>au</td>
      <td>-33.655493</td>
      <td>115.350019</td>
      <td>16.34</td>
      <td>100</td>
      <td>0</td>
      <td>7.16</td>
    </tr>
    <tr>
      <th>18</th>
      <td>bolungarvik</td>
      <td>is</td>
      <td>66.151772</td>
      <td>-23.261709</td>
      <td>0.43</td>
      <td>50</td>
      <td>0</td>
      <td>3.10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>airai</td>
      <td>pw</td>
      <td>7.396612</td>
      <td>134.569022</td>
      <td>27.00</td>
      <td>83</td>
      <td>75</td>
      <td>3.60</td>
    </tr>
    <tr>
      <th>20</th>
      <td>nikolskoye</td>
      <td>ru</td>
      <td>55.198160</td>
      <td>166.001537</td>
      <td>-2.14</td>
      <td>100</td>
      <td>48</td>
      <td>8.91</td>
    </tr>
    <tr>
      <th>21</th>
      <td>rikitea</td>
      <td>pf</td>
      <td>-23.119901</td>
      <td>-134.970265</td>
      <td>27.14</td>
      <td>99</td>
      <td>48</td>
      <td>8.21</td>
    </tr>
    <tr>
      <th>22</th>
      <td>sapa</td>
      <td>ph</td>
      <td>40.753554</td>
      <td>-111.888477</td>
      <td>4.19</td>
      <td>48</td>
      <td>75</td>
      <td>5.70</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ushuaia</td>
      <td>ar</td>
      <td>-54.801912</td>
      <td>-68.302951</td>
      <td>9.57</td>
      <td>81</td>
      <td>40</td>
      <td>9.30</td>
    </tr>
    <tr>
      <th>24</th>
      <td>yebaishou</td>
      <td>cn</td>
      <td>41.405038</td>
      <td>119.636667</td>
      <td>-6.67</td>
      <td>54</td>
      <td>44</td>
      <td>4.96</td>
    </tr>
    <tr>
      <th>25</th>
      <td>puerto madryn</td>
      <td>ar</td>
      <td>-42.769448</td>
      <td>-65.031717</td>
      <td>23.79</td>
      <td>53</td>
      <td>0</td>
      <td>3.51</td>
    </tr>
    <tr>
      <th>26</th>
      <td>andenes</td>
      <td>no</td>
      <td>69.316080</td>
      <td>16.120228</td>
      <td>0.00</td>
      <td>100</td>
      <td>75</td>
      <td>6.20</td>
    </tr>
    <tr>
      <th>27</th>
      <td>lata</td>
      <td>sb</td>
      <td>36.608590</td>
      <td>-95.979727</td>
      <td>9.94</td>
      <td>40</td>
      <td>1</td>
      <td>7.20</td>
    </tr>
    <tr>
      <th>28</th>
      <td>russell</td>
      <td>nz</td>
      <td>38.909472</td>
      <td>-98.748117</td>
      <td>3.66</td>
      <td>41</td>
      <td>1</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>29</th>
      <td>beloha</td>
      <td>mg</td>
      <td>-25.169449</td>
      <td>45.060656</td>
      <td>26.44</td>
      <td>80</td>
      <td>64</td>
      <td>3.06</td>
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
      <th>470</th>
      <td>puerto pinasco</td>
      <td>py</td>
      <td>-22.646227</td>
      <td>-57.841006</td>
      <td>30.39</td>
      <td>87</td>
      <td>20</td>
      <td>3.86</td>
    </tr>
    <tr>
      <th>471</th>
      <td>skovorodino</td>
      <td>ru</td>
      <td>53.973257</td>
      <td>123.909222</td>
      <td>-24.42</td>
      <td>36</td>
      <td>12</td>
      <td>1.16</td>
    </tr>
    <tr>
      <th>472</th>
      <td>hailey</td>
      <td>us</td>
      <td>43.519629</td>
      <td>-114.315325</td>
      <td>-5.00</td>
      <td>92</td>
      <td>90</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>473</th>
      <td>mecca</td>
      <td>sa</td>
      <td>21.389082</td>
      <td>39.857912</td>
      <td>27.00</td>
      <td>32</td>
      <td>0</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>474</th>
      <td>eydhafushi</td>
      <td>mv</td>
      <td>5.102579</td>
      <td>73.068567</td>
      <td>28.19</td>
      <td>99</td>
      <td>92</td>
      <td>5.71</td>
    </tr>
    <tr>
      <th>475</th>
      <td>buchanan</td>
      <td>lr</td>
      <td>37.527357</td>
      <td>-79.679761</td>
      <td>9.65</td>
      <td>87</td>
      <td>90</td>
      <td>1.26</td>
    </tr>
    <tr>
      <th>476</th>
      <td>tselinnoye</td>
      <td>ru</td>
      <td>53.080378</td>
      <td>85.674715</td>
      <td>-15.64</td>
      <td>79</td>
      <td>48</td>
      <td>6.86</td>
    </tr>
    <tr>
      <th>477</th>
      <td>hangal</td>
      <td>in</td>
      <td>14.771781</td>
      <td>75.125349</td>
      <td>19.49</td>
      <td>45</td>
      <td>0</td>
      <td>1.51</td>
    </tr>
    <tr>
      <th>478</th>
      <td>temaraia</td>
      <td>ki</td>
      <td>39.071430</td>
      <td>-77.133304</td>
      <td>10.58</td>
      <td>81</td>
      <td>90</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>479</th>
      <td>bathsheba</td>
      <td>bb</td>
      <td>13.210168</td>
      <td>-59.525354</td>
      <td>27.00</td>
      <td>78</td>
      <td>40</td>
      <td>6.70</td>
    </tr>
    <tr>
      <th>480</th>
      <td>emerald</td>
      <td>au</td>
      <td>-23.527291</td>
      <td>148.164573</td>
      <td>23.71</td>
      <td>89</td>
      <td>20</td>
      <td>2.41</td>
    </tr>
    <tr>
      <th>481</th>
      <td>naze</td>
      <td>jp</td>
      <td>51.866667</td>
      <td>1.283333</td>
      <td>-0.90</td>
      <td>64</td>
      <td>75</td>
      <td>6.70</td>
    </tr>
    <tr>
      <th>482</th>
      <td>kuche</td>
      <td>cn</td>
      <td>41.717906</td>
      <td>82.962016</td>
      <td>-4.47</td>
      <td>64</td>
      <td>48</td>
      <td>2.31</td>
    </tr>
    <tr>
      <th>483</th>
      <td>valparaiso</td>
      <td>cl</td>
      <td>41.473095</td>
      <td>-87.061141</td>
      <td>3.95</td>
      <td>87</td>
      <td>90</td>
      <td>3.60</td>
    </tr>
    <tr>
      <th>484</th>
      <td>rawson</td>
      <td>ar</td>
      <td>40.958942</td>
      <td>-83.784103</td>
      <td>5.20</td>
      <td>86</td>
      <td>90</td>
      <td>8.20</td>
    </tr>
    <tr>
      <th>485</th>
      <td>aksarka</td>
      <td>ru</td>
      <td>66.558310</td>
      <td>67.787762</td>
      <td>-14.87</td>
      <td>76</td>
      <td>64</td>
      <td>4.31</td>
    </tr>
    <tr>
      <th>486</th>
      <td>hereford</td>
      <td>us</td>
      <td>34.815062</td>
      <td>-102.397704</td>
      <td>7.00</td>
      <td>30</td>
      <td>1</td>
      <td>3.60</td>
    </tr>
    <tr>
      <th>487</th>
      <td>palana</td>
      <td>ru</td>
      <td>59.084210</td>
      <td>159.954164</td>
      <td>-8.37</td>
      <td>77</td>
      <td>68</td>
      <td>4.61</td>
    </tr>
    <tr>
      <th>488</th>
      <td>guarapari</td>
      <td>br</td>
      <td>-20.674120</td>
      <td>-40.499738</td>
      <td>34.00</td>
      <td>49</td>
      <td>75</td>
      <td>6.70</td>
    </tr>
    <tr>
      <th>489</th>
      <td>arman</td>
      <td>ru</td>
      <td>36.572491</td>
      <td>-96.703871</td>
      <td>10.19</td>
      <td>29</td>
      <td>1</td>
      <td>8.70</td>
    </tr>
    <tr>
      <th>490</th>
      <td>phan thiet</td>
      <td>vn</td>
      <td>10.980460</td>
      <td>108.261477</td>
      <td>24.61</td>
      <td>99</td>
      <td>12</td>
      <td>3.21</td>
    </tr>
    <tr>
      <th>491</th>
      <td>dunedin</td>
      <td>nz</td>
      <td>28.019740</td>
      <td>-82.771768</td>
      <td>26.88</td>
      <td>69</td>
      <td>40</td>
      <td>7.70</td>
    </tr>
    <tr>
      <th>492</th>
      <td>mezen</td>
      <td>ru</td>
      <td>65.861660</td>
      <td>44.229317</td>
      <td>-11.17</td>
      <td>70</td>
      <td>0</td>
      <td>2.51</td>
    </tr>
    <tr>
      <th>493</th>
      <td>canberra</td>
      <td>au</td>
      <td>-35.280937</td>
      <td>149.130009</td>
      <td>16.00</td>
      <td>82</td>
      <td>90</td>
      <td>2.10</td>
    </tr>
    <tr>
      <th>494</th>
      <td>grand gaube</td>
      <td>mu</td>
      <td>-20.014212</td>
      <td>57.669408</td>
      <td>26.00</td>
      <td>88</td>
      <td>40</td>
      <td>0.50</td>
    </tr>
    <tr>
      <th>495</th>
      <td>yamada</td>
      <td>jp</td>
      <td>40.827312</td>
      <td>-74.103858</td>
      <td>14.15</td>
      <td>51</td>
      <td>1</td>
      <td>2.60</td>
    </tr>
    <tr>
      <th>496</th>
      <td>nizhneyansk</td>
      <td>ru</td>
      <td>71.450058</td>
      <td>136.112228</td>
      <td>-24.39</td>
      <td>77</td>
      <td>76</td>
      <td>3.46</td>
    </tr>
    <tr>
      <th>497</th>
      <td>henties bay</td>
      <td>na</td>
      <td>-22.113496</td>
      <td>14.283204</td>
      <td>18.09</td>
      <td>92</td>
      <td>0</td>
      <td>3.61</td>
    </tr>
    <tr>
      <th>498</th>
      <td>bargal</td>
      <td>so</td>
      <td>11.285478</td>
      <td>51.076254</td>
      <td>19.71</td>
      <td>95</td>
      <td>64</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>499</th>
      <td>alice springs</td>
      <td>au</td>
      <td>-23.698042</td>
      <td>133.880747</td>
      <td>25.00</td>
      <td>22</td>
      <td>0</td>
      <td>3.60</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
date = datetime.datetime.now().strftime("%m-%d-%y")
```


```python

# Scatter Plot
plt.title("Temperature vs. Latitude on " + date)
plt.xlabel("Latitude")
plt.ylabel("Temperature")
plt.xlim((-80,80))

plt.scatter(city_df["Latitude"], city_df["Temperature"], marker="o", color="red")
plt.savefig("temp.png")
plt.show()

```


![png](output_8_0.png)



```python
plt.title("Temperature vs. Humidity on " + date)
plt.xlabel("Latitude")
plt.ylabel("Humidity")
plt.xlim((-80,80))

plt.scatter(city_df["Latitude"], city_df["Humidity"], marker="o", color="red")
plt.savefig("humidity.png")
plt.show()
```


![png](output_9_0.png)



```python
plt.title("Temperature vs. Cloudiness on " + date)
plt.xlabel("Latitude")
plt.ylabel("Cloudiness")
plt.xlim((-80,80))

plt.scatter(city_df["Latitude"], city_df["Cloudiness"], marker="o", color="red")
plt.savefig("cloudiness.png")
plt.show()
```


![png](output_10_0.png)



```python
plt.title("Temperature vs. Wind Speed")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed")
plt.xlim((-80,80))

plt.scatter(city_df["Latitude"], city_df["Wind Speed"], marker="o", color="red")
plt.savefig("wind.png")
plt.show()
```


![png](output_11_0.png)



```python
city_df.to_csv("output.csv")
```
