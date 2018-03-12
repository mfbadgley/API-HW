

```python
# Dependencies
import csv
import matplotlib.pyplot as plt
import requests
from citipy import citipy
import kdtree
import pandas as pd
import numpy as np
import os
from config import api_key

#Randomly select at least 500 unique (non-repeat) cities based on latitude and longitude.
#Perform a weather check on each of the cities using a series of successive API calls.
#Include a print log of each city as it's being processed with the city number, city name, and requested URL.
#Save both a CSV of all data retrieved and png images for each scatter plot.
```


```python
%%time
url = "http://api.openweathermap.org/data/2.5/weather?"
units="imperial"
latitudes =[]
longitudes=[]
clouds =[]
temps=[]
humidities=[]
windspeeds=[]
names=[]
nl='\n'
counter = 1
range_value = 600
for x in range(range_value):
    #generate random numbers in latitude range of -90to90, rounded to two decimals
    lats = np.random.uniform(low=-90, high=90, size=(1,)).round(2)
    #generate random numbers in longitude range of -180 to 180, rounded to two decimals
    lons = np.random.uniform(low=-180, high=180, size=(1,)).round(2)
    city = citipy.nearest_city(lats,lons)
    country_code =city.country_code

    # Build query URL, with exception handling...
   
    try:
        query_url = url + "appid=" + api_key +"&q=" + str(city.city_name) +","+ str(country_code) + "&units=" + units
        weather_response = requests.get(query_url)
        weather_json = weather_response.json()
        temperature = weather_json['main']["temp"]
        humidity = weather_json["main"]["humidity"]
        windspeed=weather_json['wind']['speed']
        name = weather_json['name']
        country = weather_json['sys']['country']
        latitude = weather_json['coord']['lat']
        longitude= weather_json['coord']['lon']
        cloudiness = weather_json['clouds']['all']
        print(f"Now retrieving weather for city # {counter}: {name}, {country}. Latitude of {latitude},{nl}Longitude of {longitude},  Where Humidity is: {humidity}, Temperature of: {temperature},{nl}and Winspeed of {windspeed}")
        print(query_url)
        print(lats, lons)
        print()
        latitudes.append(latitude)
        longitudes.append(longitude)
        temps.append(temperature)
        clouds.append(cloudiness)
        humidities.append(humidity)
        windspeeds.append(windspeed)
        names.append(name)
    except KeyError:
        continue
    
    counter +=1
    
   

```

    Now retrieving weather for city # 1: Nanzhou, CN. Latitude of 29.37,
    Longitude of 112.41,  Where Humidity is: 69, Temperature of: 70.88,
    and Winspeed of 7.16
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nanzhou,cn&units=imperial
    [ 29.22] [ 112.66]
    
    Now retrieving weather for city # 2: Leningradskiy, RU. Latitude of 69.38,
    Longitude of 178.42,  Where Humidity is: 92, Temperature of: -4.28,
    and Winspeed of 24.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=leningradskiy,ru&units=imperial
    [ 78.19] [ 176.54]
    
    Now retrieving weather for city # 3: Arraial do Cabo, BR. Latitude of -22.97,
    Longitude of -42.02,  Where Humidity is: 100, Temperature of: 74.88,
    and Winspeed of 1.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=arraial do cabo,br&units=imperial
    [-32.9] [-28.15]
    
    Now retrieving weather for city # 4: Vila Franca do Campo, PT. Latitude of 37.72,
    Longitude of -25.43,  Where Humidity is: 93, Temperature of: 60.8,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vila franca do campo,pt&units=imperial
    [ 38.52] [-23.29]
    
    Now retrieving weather for city # 5: San Patricio, MX. Latitude of 19.22,
    Longitude of -104.7,  Where Humidity is: 69, Temperature of: 78.8,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san patricio,mx&units=imperial
    [ 16.72] [-106.34]
    
    Now retrieving weather for city # 6: Chulman, RU. Latitude of 56.84,
    Longitude of 124.9,  Where Humidity is: 71, Temperature of: 12.6,
    and Winspeed of 3.58
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chulman,ru&units=imperial
    [ 56.91] [ 125.94]
    
    Now retrieving weather for city # 7: Cidreira, BR. Latitude of -30.17,
    Longitude of -50.22,  Where Humidity is: 100, Temperature of: 68.49,
    and Winspeed of 7.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cidreira,br&units=imperial
    [-36.3] [-41.61]
    
    Now retrieving weather for city # 8: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [ 6.16] [-124.08]
    
    Now retrieving weather for city # 9: Norman Wells, CA. Latitude of 65.28,
    Longitude of -126.83,  Where Humidity is: 45, Temperature of: 24.8,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=norman wells,ca&units=imperial
    [ 74.4] [-120.2]
    
    Now retrieving weather for city # 10: Saldanha, ZA. Latitude of -33.01,
    Longitude of 17.94,  Where Humidity is: 87, Temperature of: 59,
    and Winspeed of 1.12
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saldanha,za&units=imperial
    [-39.89] [ 1.65]
    
    Now retrieving weather for city # 11: Khatanga, RU. Latitude of 71.98,
    Longitude of 102.47,  Where Humidity is: 62, Temperature of: -21.15,
    and Winspeed of 8.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=khatanga,ru&units=imperial
    [ 69.68] [ 97.35]
    
    Now retrieving weather for city # 12: Mahebourg, MU. Latitude of -20.41,
    Longitude of 57.7,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mahebourg,mu&units=imperial
    [-54.2] [ 78.74]
    
    Now retrieving weather for city # 13: Calabozo, VE. Latitude of 8.92,
    Longitude of -67.43,  Where Humidity is: 44, Temperature of: 85.23,
    and Winspeed of 16.4
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=calabozo,ve&units=imperial
    [ 9.13] [-68.02]
    
    Now retrieving weather for city # 14: Haines Junction, CA. Latitude of 60.75,
    Longitude of -137.51,  Where Humidity is: 77, Temperature of: 27.86,
    and Winspeed of 8.79
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=haines junction,ca&units=imperial
    [ 58.77] [-142.09]
    
    Now retrieving weather for city # 15: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-71.96] [-109.79]
    
    Now retrieving weather for city # 16: Ribeira Grande, PT. Latitude of 38.52,
    Longitude of -28.7,  Where Humidity is: 99, Temperature of: 62.19,
    and Winspeed of 26.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ribeira grande,pt&units=imperial
    [ 34.94] [-31.61]
    
    Now retrieving weather for city # 17: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-49.38] [ 168.24]
    
    Now retrieving weather for city # 18: Hithadhoo, MV. Latitude of -0.6,
    Longitude of 73.08,  Where Humidity is: 100, Temperature of: 81.54,
    and Winspeed of 7.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hithadhoo,mv&units=imperial
    [-20.71] [ 84.06]
    
    Now retrieving weather for city # 19: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-64.91] [-2.12]
    
    Now retrieving weather for city # 20: Chuy, UY. Latitude of -33.69,
    Longitude of -53.46,  Where Humidity is: 86, Temperature of: 63.23,
    and Winspeed of 16.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chuy,uy&units=imperial
    [-60.75] [-23.58]
    
    Now retrieving weather for city # 21: Mehamn, NO. Latitude of 71.03,
    Longitude of 27.85,  Where Humidity is: 88, Temperature of: 19.53,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mehamn,no&units=imperial
    [ 89.33] [ 30.67]
    
    Now retrieving weather for city # 22: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-40.04] [ 26.15]
    
    Now retrieving weather for city # 23: Cidreira, BR. Latitude of -30.17,
    Longitude of -50.22,  Where Humidity is: 100, Temperature of: 68.49,
    and Winspeed of 7.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cidreira,br&units=imperial
    [-59.76] [-20.1]
    
    Now retrieving weather for city # 24: Barrow, US. Latitude of 39.51,
    Longitude of -90.4,  Where Humidity is: 55, Temperature of: 36.3,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barrow,us&units=imperial
    [ 67.59] [-156.82]
    
    Now retrieving weather for city # 25: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-23.84] [-141.46]
    
    Now retrieving weather for city # 26: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-58.57] [-79.86]
    
    Now retrieving weather for city # 27: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-54.67] [-96.52]
    
    Now retrieving weather for city # 28: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-40.12] [-110.84]
    
    Now retrieving weather for city # 29: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-34.68] [ 113.94]
    
    Now retrieving weather for city # 30: Iqaluit, CA. Latitude of 63.75,
    Longitude of -68.52,  Where Humidity is: 78, Temperature of: 10.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=iqaluit,ca&units=imperial
    [ 64.43] [-70.57]
    
    Now retrieving weather for city # 31: Ternate, ID. Latitude of 0.8,
    Longitude of 127.4,  Where Humidity is: 97, Temperature of: 85.1,
    and Winspeed of 4.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ternate,id&units=imperial
    [ 2.21] [ 128.07]
    
    Now retrieving weather for city # 32: Sao Felix do Xingu, BR. Latitude of -6.64,
    Longitude of -51.99,  Where Humidity is: 97, Temperature of: 74.75,
    and Winspeed of 2.08
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sao felix do xingu,br&units=imperial
    [-6.57] [-53.94]
    
    Now retrieving weather for city # 33: Miedzyrzecz, PL. Latitude of 52.44,
    Longitude of 15.58,  Where Humidity is: 100, Temperature of: 33.8,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=miedzyrzecz,pl&units=imperial
    [ 52.52] [ 15.55]
    
    Now retrieving weather for city # 34: Barra Patuca, HN. Latitude of 15.8,
    Longitude of -84.28,  Where Humidity is: 93, Temperature of: 79.38,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barra patuca,hn&units=imperial
    [ 17.16] [-82.69]
    
    Now retrieving weather for city # 35: East London, ZA. Latitude of -33.02,
    Longitude of 27.91,  Where Humidity is: 100, Temperature of: 68.94,
    and Winspeed of 11.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=east london,za&units=imperial
    [-76.95] [ 55.13]
    
    Now retrieving weather for city # 36: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-43.12] [-115.09]
    
    Now retrieving weather for city # 37: Caravelas, BR. Latitude of -17.73,
    Longitude of -39.27,  Where Humidity is: 96, Temperature of: 82.76,
    and Winspeed of 20.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=caravelas,br&units=imperial
    [-23.07] [-25.41]
    
    Now retrieving weather for city # 38: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 76.66] [-80.42]
    
    Now retrieving weather for city # 39: Sitka, US. Latitude of 37.17,
    Longitude of -99.65,  Where Humidity is: 46, Temperature of: 40.1,
    and Winspeed of 7.38
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sitka,us&units=imperial
    [ 47.57] [-142.57]
    
    Now retrieving weather for city # 40: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-20.99] [-114.86]
    
    Now retrieving weather for city # 41: Vila Franca do Campo, PT. Latitude of 37.72,
    Longitude of -25.43,  Where Humidity is: 93, Temperature of: 60.8,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vila franca do campo,pt&units=imperial
    [ 42.77] [-19.03]
    
    Now retrieving weather for city # 42: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-67.76] [-74.5]
    
    Now retrieving weather for city # 43: Avarua, CK. Latitude of -21.21,
    Longitude of -159.78,  Where Humidity is: 70, Temperature of: 84.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=avarua,ck&units=imperial
    [-16.2] [-157.82]
    
    Now retrieving weather for city # 44: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-78.61] [-106.02]
    
    Now retrieving weather for city # 45: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-40.46] [-18.94]
    
    Now retrieving weather for city # 46: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-77.58] [-99.19]
    
    Now retrieving weather for city # 47: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-51.75] [-141.45]
    
    Now retrieving weather for city # 48: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-26.44] [-134.93]
    
    Now retrieving weather for city # 49: Lebu, CL. Latitude of -37.62,
    Longitude of -73.65,  Where Humidity is: 100, Temperature of: 60.66,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lebu,cl&units=imperial
    [-33.82] [-93.45]
    
    Now retrieving weather for city # 50: Hilo, US. Latitude of 19.71,
    Longitude of -155.08,  Where Humidity is: 53, Temperature of: 67.15,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hilo,us&units=imperial
    [ 32.2] [-143.26]
    
    Now retrieving weather for city # 51: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [-6.09] [-135.34]
    
    Now retrieving weather for city # 52: Georgetown, SH. Latitude of -7.93,
    Longitude of -14.42,  Where Humidity is: 100, Temperature of: 78.26,
    and Winspeed of 11.92
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=georgetown,sh&units=imperial
    [-3.46] [-19.82]
    
    Now retrieving weather for city # 53: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-64.33] [-175.89]
    
    Now retrieving weather for city # 54: Tual, ID. Latitude of -5.67,
    Longitude of 132.75,  Where Humidity is: 100, Temperature of: 79.56,
    and Winspeed of 10.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tual,id&units=imperial
    [-5.04] [ 132.87]
    
    Now retrieving weather for city # 55: Bathsheba, BB. Latitude of 13.22,
    Longitude of -59.52,  Where Humidity is: 78, Temperature of: 78.8,
    and Winspeed of 19.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bathsheba,bb&units=imperial
    [ 14.94] [-52.66]
    
    Now retrieving weather for city # 56: Husavik, IS. Latitude of 66.04,
    Longitude of -17.34,  Where Humidity is: 86, Temperature of: 30.2,
    and Winspeed of 1.12
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=husavik,is&units=imperial
    [ 82.24] [-5.55]
    
    Now retrieving weather for city # 57: Yinchuan, CN. Latitude of 38.48,
    Longitude of 106.21,  Where Humidity is: 32, Temperature of: 60.98,
    and Winspeed of 4.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yinchuan,cn&units=imperial
    [ 38.25] [ 108.02]
    
    Now retrieving weather for city # 58: Guerrero Negro, MX. Latitude of 27.97,
    Longitude of -114.04,  Where Humidity is: 81, Temperature of: 63.23,
    and Winspeed of 10.85
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=guerrero negro,mx&units=imperial
    [ 12.74] [-130.25]
    
    Now retrieving weather for city # 59: Predivinsk, RU. Latitude of 57.07,
    Longitude of 93.44,  Where Humidity is: 66, Temperature of: -0.72,
    and Winspeed of 3.53
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=predivinsk,ru&units=imperial
    [ 56.89] [ 93.82]
    
    Now retrieving weather for city # 60: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-75.87] [-73.7]
    
    Now retrieving weather for city # 61: Yellowknife, CA. Latitude of 62.45,
    Longitude of -114.38,  Where Humidity is: 57, Temperature of: 23,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yellowknife,ca&units=imperial
    [ 70.25] [-103.94]
    
    Now retrieving weather for city # 62: Oriximina, BR. Latitude of -1.77,
    Longitude of -55.87,  Where Humidity is: 79, Temperature of: 79.88,
    and Winspeed of 4.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=oriximina,br&units=imperial
    [-0.42] [-56.81]
    
    Now retrieving weather for city # 63: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-55.18] [ 95.96]
    
    Now retrieving weather for city # 64: Salta, AR. Latitude of -24.79,
    Longitude of -65.41,  Where Humidity is: 82, Temperature of: 68,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=salta,ar&units=imperial
    [-24.56] [-65.29]
    
    Now retrieving weather for city # 65: Talnakh, RU. Latitude of 69.49,
    Longitude of 88.39,  Where Humidity is: 85, Temperature of: -27.41,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=talnakh,ru&units=imperial
    [ 76.51] [ 92.35]
    
    Now retrieving weather for city # 66: Albany, AU. Latitude of -35.02,
    Longitude of 117.88,  Where Humidity is: 67, Temperature of: 71.82,
    and Winspeed of 14.88
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=albany,au&units=imperial
    [-83.67] [ 93.53]
    
    Now retrieving weather for city # 67: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-84.61] [-111.22]
    
    Now retrieving weather for city # 68: Puerto Leguizamo, CO. Latitude of -0.19,
    Longitude of -74.78,  Where Humidity is: 70, Temperature of: 81.5,
    and Winspeed of 3.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto leguizamo,co&units=imperial
    [-0.31] [-73.54]
    
    Now retrieving weather for city # 69: Geraldton, AU. Latitude of -28.77,
    Longitude of 114.6,  Where Humidity is: 35, Temperature of: 89.6,
    and Winspeed of 25.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=geraldton,au&units=imperial
    [-29.04] [ 115.32]
    
    Now retrieving weather for city # 70: Wuan, CN. Latitude of 31.68,
    Longitude of 112,  Where Humidity is: 80, Temperature of: 62.96,
    and Winspeed of 4.14
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=wuan,cn&units=imperial
    [ 37.36] [ 113.6]
    
    Now retrieving weather for city # 71: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-58.98] [ 141.92]
    
    Now retrieving weather for city # 72: Fevik, NO. Latitude of 58.38,
    Longitude of 8.68,  Where Humidity is: 92, Temperature of: 30.2,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=fevik,no&units=imperial
    [ 57.96] [ 8.93]
    
    Now retrieving weather for city # 73: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-40.13] [ 95.88]
    
    Now retrieving weather for city # 74: San Cristobal, EC. Latitude of -1.02,
    Longitude of -79.44,  Where Humidity is: 94, Temperature of: 61.25,
    and Winspeed of 1.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san cristobal,ec&units=imperial
    [-3.24] [-89.68]
    
    Now retrieving weather for city # 75: Zhigansk, RU. Latitude of 66.77,
    Longitude of 123.37,  Where Humidity is: 72, Temperature of: -6.12,
    and Winspeed of 4.14
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=zhigansk,ru&units=imperial
    [ 74.01] [ 121.83]
    
    Now retrieving weather for city # 76: Trinidad, BO. Latitude of -14.83,
    Longitude of -64.9,  Where Humidity is: 92, Temperature of: 76.95,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=trinidad,bo&units=imperial
    [-14.02] [-65.17]
    
    Now retrieving weather for city # 77: Arraial do Cabo, BR. Latitude of -22.97,
    Longitude of -42.02,  Where Humidity is: 100, Temperature of: 74.88,
    and Winspeed of 1.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=arraial do cabo,br&units=imperial
    [-38.51] [-27.63]
    
    Now retrieving weather for city # 78: Kaitangata, NZ. Latitude of -46.28,
    Longitude of 169.85,  Where Humidity is: 63, Temperature of: 70.92,
    and Winspeed of 4.76
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kaitangata,nz&units=imperial
    [-71.95] [ 179.8]
    
    Now retrieving weather for city # 79: Chokurdakh, RU. Latitude of 70.62,
    Longitude of 147.9,  Where Humidity is: 42, Temperature of: -26.78,
    and Winspeed of 3.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chokurdakh,ru&units=imperial
    [ 80.67] [ 152.84]
    
    Now retrieving weather for city # 80: Tha Mai, TH. Latitude of 12.62,
    Longitude of 102.01,  Where Humidity is: 94, Temperature of: 75.2,
    and Winspeed of 3.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tha mai,th&units=imperial
    [ 12.86] [ 102.04]
    
    Now retrieving weather for city # 81: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-39.06] [ 97.61]
    
    Now retrieving weather for city # 82: Onda, ES. Latitude of 39.96,
    Longitude of -0.26,  Where Humidity is: 58, Temperature of: 55.4,
    and Winspeed of 23.04
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=onda,es&units=imperial
    [ 40.29] [-0.41]
    
    Now retrieving weather for city # 83: Bredasdorp, ZA. Latitude of -34.53,
    Longitude of 20.04,  Where Humidity is: 77, Temperature of: 66.2,
    and Winspeed of 7.16
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bredasdorp,za&units=imperial
    [-88.81] [ 23.18]
    
    Now retrieving weather for city # 84: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-55.24] [ 11.07]
    
    Now retrieving weather for city # 85: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-47.61] [-129.69]
    
    Now retrieving weather for city # 86: Lebu, CL. Latitude of -37.62,
    Longitude of -73.65,  Where Humidity is: 100, Temperature of: 60.66,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lebu,cl&units=imperial
    [-35.8] [-76.59]
    
    Now retrieving weather for city # 87: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-53.] [ 154.73]
    
    Now retrieving weather for city # 88: Chumikan, RU. Latitude of 54.72,
    Longitude of 135.31,  Where Humidity is: 62, Temperature of: 19.58,
    and Winspeed of 3.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chumikan,ru&units=imperial
    [ 56.45] [ 136.14]
    
    Now retrieving weather for city # 89: San Ramon, BO. Latitude of -13.27,
    Longitude of -64.62,  Where Humidity is: 82, Temperature of: 79.02,
    and Winspeed of 7.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san ramon,bo&units=imperial
    [-13.39] [-65.12]
    
    Now retrieving weather for city # 90: Butaritari, KI. Latitude of 3.07,
    Longitude of 172.79,  Where Humidity is: 100, Temperature of: 82.85,
    and Winspeed of 14.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=butaritari,ki&units=imperial
    [ 25.59] [ 173.49]
    
    Now retrieving weather for city # 91: Yellowknife, CA. Latitude of 62.45,
    Longitude of -114.38,  Where Humidity is: 57, Temperature of: 23,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yellowknife,ca&units=imperial
    [ 67.78] [-114.8]
    
    Now retrieving weather for city # 92: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-67.67] [ 156.64]
    
    Now retrieving weather for city # 93: Arraial do Cabo, BR. Latitude of -22.97,
    Longitude of -42.02,  Where Humidity is: 100, Temperature of: 74.88,
    and Winspeed of 1.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=arraial do cabo,br&units=imperial
    [-31.59] [-37.76]
    
    Now retrieving weather for city # 94: Luderitz, NA. Latitude of -26.65,
    Longitude of 15.16,  Where Humidity is: 93, Temperature of: 62.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=luderitz,na&units=imperial
    [-29.6] [ 6.37]
    
    Now retrieving weather for city # 95: Tchibanga, GA. Latitude of -2.92,
    Longitude of 11,  Where Humidity is: 99, Temperature of: 72.27,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tchibanga,ga&units=imperial
    [-3.07] [ 11.1]
    
    Now retrieving weather for city # 96: Tommot, RU. Latitude of 58.97,
    Longitude of 126.27,  Where Humidity is: 65, Temperature of: 13.41,
    and Winspeed of 3.31
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tommot,ru&units=imperial
    [ 57.35] [ 127.92]
    
    Now retrieving weather for city # 97: Coihaique, CL. Latitude of -45.58,
    Longitude of -72.07,  Where Humidity is: 81, Temperature of: 48.99,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=coihaique,cl&units=imperial
    [-48.12] [-74.82]
    
    Now retrieving weather for city # 98: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-64.51] [ 143.37]
    
    Now retrieving weather for city # 99: Port Alfred, ZA. Latitude of -33.59,
    Longitude of 26.89,  Where Humidity is: 100, Temperature of: 64.58,
    and Winspeed of 5.66
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port alfred,za&units=imperial
    [-76.75] [ 46.73]
    
    Now retrieving weather for city # 100: Balgazyn, RU. Latitude of 51,
    Longitude of 95.2,  Where Humidity is: 90, Temperature of: 25.56,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=balgazyn,ru&units=imperial
    [ 51.03] [ 95.21]
    
    Now retrieving weather for city # 101: Starozhilovo, RU. Latitude of 54.23,
    Longitude of 39.91,  Where Humidity is: 67, Temperature of: 2.52,
    and Winspeed of 8.39
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=starozhilovo,ru&units=imperial
    [ 54.29] [ 39.73]
    
    Now retrieving weather for city # 102: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-29.35] [-141.67]
    
    Now retrieving weather for city # 103: Saint-Leu, RE. Latitude of -21.15,
    Longitude of 55.28,  Where Humidity is: 73, Temperature of: 78.03,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-leu,re&units=imperial
    [-21.36] [ 53.41]
    
    Now retrieving weather for city # 104: Sassandra, CI. Latitude of 4.95,
    Longitude of -6.09,  Where Humidity is: 98, Temperature of: 80.19,
    and Winspeed of 7.94
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sassandra,ci&units=imperial
    [ 3.95] [-5.37]
    
    Now retrieving weather for city # 105: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-69.35] [ 87.91]
    
    Now retrieving weather for city # 106: Bay Roberts, CA. Latitude of 47.58,
    Longitude of -53.28,  Where Humidity is: 100, Temperature of: 30.2,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bay roberts,ca&units=imperial
    [ 44.51] [-52.68]
    
    Now retrieving weather for city # 107: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-31.61] [-171.15]
    
    Now retrieving weather for city # 108: Pisco, PE. Latitude of -13.71,
    Longitude of -76.2,  Where Humidity is: 78, Temperature of: 71.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pisco,pe&units=imperial
    [-24.84] [-96.44]
    
    Now retrieving weather for city # 109: Saskylakh, RU. Latitude of 71.97,
    Longitude of 114.09,  Where Humidity is: 39, Temperature of: -25.7,
    and Winspeed of 5.44
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saskylakh,ru&units=imperial
    [ 83.15] [ 112.79]
    
    Now retrieving weather for city # 110: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-2.9] [-113.36]
    
    Now retrieving weather for city # 111: Saint-Paul, RE. Latitude of -21.01,
    Longitude of 55.27,  Where Humidity is: 65, Temperature of: 77.85,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-paul,re&units=imperial
    [-21.07] [ 52.13]
    
    Now retrieving weather for city # 112: Tinde, TZ. Latitude of -3.87,
    Longitude of 33.2,  Where Humidity is: 92, Temperature of: 59.63,
    and Winspeed of 3.09
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tinde,tz&units=imperial
    [-3.62] [ 33.07]
    
    Now retrieving weather for city # 113: Thompson, CA. Latitude of 55.74,
    Longitude of -97.86,  Where Humidity is: 53, Temperature of: 26.6,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=thompson,ca&units=imperial
    [ 71.23] [-89.63]
    
    Now retrieving weather for city # 114: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-65.52] [-109.88]
    
    Now retrieving weather for city # 115: Cidreira, BR. Latitude of -30.17,
    Longitude of -50.22,  Where Humidity is: 100, Temperature of: 68.49,
    and Winspeed of 7.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cidreira,br&units=imperial
    [-48.74] [-29.38]
    
    Now retrieving weather for city # 116: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-22.81] [-1.85]
    
    Now retrieving weather for city # 117: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-63.95] [ 146.46]
    
    Now retrieving weather for city # 118: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-49.11] [ 13.65]
    
    Now retrieving weather for city # 119: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-14.96] [-9.82]
    
    Now retrieving weather for city # 120: Iqaluit, CA. Latitude of 63.75,
    Longitude of -68.52,  Where Humidity is: 78, Temperature of: 10.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=iqaluit,ca&units=imperial
    [ 59.87] [-75.87]
    
    Now retrieving weather for city # 121: Sault Sainte Marie, CA. Latitude of 46.49,
    Longitude of -84.36,  Where Humidity is: 68, Temperature of: 24.21,
    and Winspeed of 1.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sault sainte marie,ca&units=imperial
    [ 46.82] [-84.78]
    
    Now retrieving weather for city # 122: Esperance, AU. Latitude of -33.86,
    Longitude of 121.89,  Where Humidity is: 82, Temperature of: 71.33,
    and Winspeed of 10.47
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=esperance,au&units=imperial
    [-39.19] [ 124.82]
    
    Now retrieving weather for city # 123: Rio Gallegos, AR. Latitude of -51.62,
    Longitude of -69.22,  Where Humidity is: 69, Temperature of: 60.8,
    and Winspeed of 17.22
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rio gallegos,ar&units=imperial
    [-50.87] [-70.19]
    
    Now retrieving weather for city # 124: Yellowknife, CA. Latitude of 62.45,
    Longitude of -114.38,  Where Humidity is: 57, Temperature of: 23,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yellowknife,ca&units=imperial
    [ 74.13] [-102.05]
    
    Now retrieving weather for city # 125: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-77.79] [ 84.63]
    
    Now retrieving weather for city # 126: Sao Filipe, CV. Latitude of 14.9,
    Longitude of -24.5,  Where Humidity is: 94, Temperature of: 71.19,
    and Winspeed of 19.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sao filipe,cv&units=imperial
    [ 12.57] [-36.53]
    
    Now retrieving weather for city # 127: Fergus Falls, US. Latitude of 46.28,
    Longitude of -96.08,  Where Humidity is: 86, Temperature of: 25.83,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=fergus falls,us&units=imperial
    [ 45.9] [-96.21]
    
    Now retrieving weather for city # 128: Esperance, AU. Latitude of -33.86,
    Longitude of 121.89,  Where Humidity is: 82, Temperature of: 71.33,
    and Winspeed of 10.47
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=esperance,au&units=imperial
    [-32.36] [ 122.62]
    
    Now retrieving weather for city # 129: Richards Bay, ZA. Latitude of -28.77,
    Longitude of 32.06,  Where Humidity is: 100, Temperature of: 69.08,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=richards bay,za&units=imperial
    [-29.83] [ 34.48]
    
    Now retrieving weather for city # 130: Khatanga, RU. Latitude of 71.98,
    Longitude of 102.47,  Where Humidity is: 62, Temperature of: -21.15,
    and Winspeed of 8.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=khatanga,ru&units=imperial
    [ 68.48] [ 101.54]
    
    Now retrieving weather for city # 131: Avarua, CK. Latitude of -21.21,
    Longitude of -159.78,  Where Humidity is: 70, Temperature of: 84.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=avarua,ck&units=imperial
    [-24.07] [-157.68]
    
    Now retrieving weather for city # 132: Sao Joao da Barra, BR. Latitude of -21.64,
    Longitude of -41.05,  Where Humidity is: 100, Temperature of: 78.3,
    and Winspeed of 9.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sao joao da barra,br&units=imperial
    [-23.73] [-37.61]
    
    Now retrieving weather for city # 133: Dmitriyevka, RU. Latitude of 54.05,
    Longitude of 40.44,  Where Humidity is: 67, Temperature of: 2.52,
    and Winspeed of 8.39
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dmitriyevka,ru&units=imperial
    [ 52.61] [ 40.87]
    
    Now retrieving weather for city # 134: Tasiilaq, GL. Latitude of 65.61,
    Longitude of -37.64,  Where Humidity is: 67, Temperature of: 21.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tasiilaq,gl&units=imperial
    [ 66.17] [-35.28]
    
    Now retrieving weather for city # 135: Fayaoue, NC. Latitude of -20.65,
    Longitude of 166.53,  Where Humidity is: 100, Temperature of: 79.38,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=fayaoue,nc&units=imperial
    [-19.59] [ 165.79]
    
    Now retrieving weather for city # 136: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-56.44] [ 31.5]
    
    Now retrieving weather for city # 137: Benguela, AO. Latitude of -12.58,
    Longitude of 13.4,  Where Humidity is: 100, Temperature of: 71.28,
    and Winspeed of 4.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=benguela,ao&units=imperial
    [-11.44] [ 9.63]
    
    Now retrieving weather for city # 138: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-80.1] [-135.52]
    
    Now retrieving weather for city # 139: Orasje, BA. Latitude of 45.04,
    Longitude of 18.69,  Where Humidity is: 71, Temperature of: 55.4,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=orasje,ba&units=imperial
    [ 45.] [ 18.72]
    
    Now retrieving weather for city # 140: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-61.79] [ 9.61]
    
    Now retrieving weather for city # 141: Sao Filipe, CV. Latitude of 14.9,
    Longitude of -24.5,  Where Humidity is: 94, Temperature of: 71.19,
    and Winspeed of 19.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sao filipe,cv&units=imperial
    [ 5.65] [-24.5]
    
    Now retrieving weather for city # 142: Lavrentiya, RU. Latitude of 65.58,
    Longitude of -170.99,  Where Humidity is: 100, Temperature of: 3.06,
    and Winspeed of 13.15
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lavrentiya,ru&units=imperial
    [ 76.98] [-169.84]
    
    Now retrieving weather for city # 143: Grindavik, IS. Latitude of 63.84,
    Longitude of -22.43,  Where Humidity is: 100, Temperature of: 30.94,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=grindavik,is&units=imperial
    [ 59.41] [-22.53]
    
    Now retrieving weather for city # 144: Pochutla, MX. Latitude of 15.74,
    Longitude of -96.47,  Where Humidity is: 100, Temperature of: 79.54,
    and Winspeed of 17.22
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pochutla,mx&units=imperial
    [ 8.47] [-98.]
    
    Now retrieving weather for city # 145: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-35.1] [ 102.71]
    
    Now retrieving weather for city # 146: Northam, AU. Latitude of -31.65,
    Longitude of 116.67,  Where Humidity is: 50, Temperature of: 77,
    and Winspeed of 16.11
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=northam,au&units=imperial
    [-30.47] [ 117.09]
    
    Now retrieving weather for city # 147: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-73.74] [ 30.3]
    
    Now retrieving weather for city # 148: Thompson, CA. Latitude of 55.74,
    Longitude of -97.86,  Where Humidity is: 53, Temperature of: 26.6,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=thompson,ca&units=imperial
    [ 63.35] [-93.1]
    
    Now retrieving weather for city # 149: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [-12.08] [-123.12]
    
    Now retrieving weather for city # 150: Dicabisagan, PH. Latitude of 17.08,
    Longitude of 122.42,  Where Humidity is: 100, Temperature of: 73.98,
    and Winspeed of 9.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dicabisagan,ph&units=imperial
    [ 17.58] [ 123.68]
    
    Now retrieving weather for city # 151: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-49.23] [-109.68]
    
    Now retrieving weather for city # 152: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-55.78] [-57.37]
    
    Now retrieving weather for city # 153: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-54.49] [ 30.44]
    
    Now retrieving weather for city # 154: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-43.65] [ 89.24]
    
    Now retrieving weather for city # 155: Honiara, SB. Latitude of -9.43,
    Longitude of 159.96,  Where Humidity is: 83, Temperature of: 84.2,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=honiara,sb&units=imperial
    [-11.73] [ 158.8]
    
    Now retrieving weather for city # 156: Bagdarin, RU. Latitude of 54.44,
    Longitude of 113.59,  Where Humidity is: 40, Temperature of: 18.23,
    and Winspeed of 4.76
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bagdarin,ru&units=imperial
    [ 54.99] [ 112.46]
    
    Now retrieving weather for city # 157: Pangnirtung, CA. Latitude of 66.15,
    Longitude of -65.72,  Where Humidity is: 70, Temperature of: 1.62,
    and Winspeed of 0.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pangnirtung,ca&units=imperial
    [ 68.58] [-64.36]
    
    Now retrieving weather for city # 158: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-51.68] [-133.93]
    
    Now retrieving weather for city # 159: Tautira, PF. Latitude of -17.73,
    Longitude of -149.15,  Where Humidity is: 69, Temperature of: 82.4,
    and Winspeed of 11.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tautira,pf&units=imperial
    [-15.59] [-146.24]
    
    Now retrieving weather for city # 160: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-88.09] [ 155.42]
    
    Now retrieving weather for city # 161: Maniitsoq, GL. Latitude of 65.42,
    Longitude of -52.9,  Where Humidity is: 76, Temperature of: 15.12,
    and Winspeed of 4.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=maniitsoq,gl&units=imperial
    [ 60.34] [-57.27]
    
    Now retrieving weather for city # 162: Swinoujscie, PL. Latitude of 53.91,
    Longitude of 14.25,  Where Humidity is: 87, Temperature of: 44.6,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=swinoujscie,pl&units=imperial
    [ 53.87] [ 14.24]
    
    Now retrieving weather for city # 163: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-81.58] [-130.48]
    
    Now retrieving weather for city # 164: Pedasi, PA. Latitude of 7.53,
    Longitude of -80.03,  Where Humidity is: 92, Temperature of: 77.76,
    and Winspeed of 6.26
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pedasi,pa&units=imperial
    [ 6.79] [-79.64]
    
    Now retrieving weather for city # 165: Balakhta, RU. Latitude of 55.38,
    Longitude of 91.62,  Where Humidity is: 67, Temperature of: -1.58,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=balakhta,ru&units=imperial
    [ 55.49] [ 91.28]
    
    Now retrieving weather for city # 166: Dunedin, NZ. Latitude of -45.87,
    Longitude of 170.5,  Where Humidity is: 67, Temperature of: 68.4,
    and Winspeed of 5.32
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dunedin,nz&units=imperial
    [-54.15] [ 176.75]
    
    Now retrieving weather for city # 167: Lompoc, US. Latitude of 34.64,
    Longitude of -120.46,  Where Humidity is: 68, Temperature of: 64.4,
    and Winspeed of 10.18
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lompoc,us&units=imperial
    [ 26.01] [-129.43]
    
    Now retrieving weather for city # 168: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 82.49] [-88.13]
    
    Now retrieving weather for city # 169: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-61.65] [ 162.48]
    
    Now retrieving weather for city # 170: Hasaki, JP. Latitude of 35.73,
    Longitude of 140.83,  Where Humidity is: 37, Temperature of: 48.09,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hasaki,jp&units=imperial
    [ 33.4] [ 143.4]
    
    Now retrieving weather for city # 171: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-82.55] [ 163.26]
    
    Now retrieving weather for city # 172: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [-12.82] [-133.73]
    
    Now retrieving weather for city # 173: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-36.59] [-168.68]
    
    Now retrieving weather for city # 174: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-83.47] [-77.41]
    
    Now retrieving weather for city # 175: Mar del Plata, AR. Latitude of -46.43,
    Longitude of -67.52,  Where Humidity is: 56, Temperature of: 60.84,
    and Winspeed of 16.96
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mar del plata,ar&units=imperial
    [-49.13] [-51.16]
    
    Now retrieving weather for city # 176: Kavieng, PG. Latitude of -2.57,
    Longitude of 150.8,  Where Humidity is: 98, Temperature of: 86,
    and Winspeed of 11.48
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kavieng,pg&units=imperial
    [ 16.31] [ 153.25]
    
    Now retrieving weather for city # 177: Castro, CL. Latitude of -42.48,
    Longitude of -73.76,  Where Humidity is: 98, Temperature of: 56.34,
    and Winspeed of 12.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=castro,cl&units=imperial
    [-42.37] [-81.11]
    
    Now retrieving weather for city # 178: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-70.48] [-44.69]
    
    Now retrieving weather for city # 179: Lebu, CL. Latitude of -37.62,
    Longitude of -73.65,  Where Humidity is: 100, Temperature of: 60.66,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lebu,cl&units=imperial
    [-35.82] [-91.23]
    
    Now retrieving weather for city # 180: Luoyang, CN. Latitude of 34.66,
    Longitude of 112.42,  Where Humidity is: 78, Temperature of: 61.88,
    and Winspeed of 4.14
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=luoyang,cn&units=imperial
    [ 34.55] [ 112.35]
    
    Now retrieving weather for city # 181: Port Lincoln, AU. Latitude of -34.72,
    Longitude of 135.86,  Where Humidity is: 80, Temperature of: 68.81,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port lincoln,au&units=imperial
    [-43.76] [ 132.82]
    
    Now retrieving weather for city # 182: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 88.69] [-73.96]
    
    Now retrieving weather for city # 183: Luderitz, NA. Latitude of -26.65,
    Longitude of 15.16,  Where Humidity is: 93, Temperature of: 62.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=luderitz,na&units=imperial
    [-30.98] [ 5.32]
    
    Now retrieving weather for city # 184: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-63.89] [-97.59]
    
    Now retrieving weather for city # 185: Pevek, RU. Latitude of 69.7,
    Longitude of 170.27,  Where Humidity is: 100, Temperature of: -9.54,
    and Winspeed of 7.11
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pevek,ru&units=imperial
    [ 84.34] [ 166.14]
    
    Now retrieving weather for city # 186: Geraldton, AU. Latitude of -28.77,
    Longitude of 114.6,  Where Humidity is: 35, Temperature of: 89.6,
    and Winspeed of 25.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=geraldton,au&units=imperial
    [-27.77] [ 114.05]
    
    Now retrieving weather for city # 187: Mar del Plata, AR. Latitude of -46.43,
    Longitude of -67.52,  Where Humidity is: 56, Temperature of: 60.84,
    and Winspeed of 16.96
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mar del plata,ar&units=imperial
    [-57.] [-33.61]
    
    Now retrieving weather for city # 188: Gospic, HR. Latitude of 44.55,
    Longitude of 15.37,  Where Humidity is: 93, Temperature of: 53.6,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=gospic,hr&units=imperial
    [ 44.72] [ 15.35]
    
    Now retrieving weather for city # 189: Tura, RU. Latitude of 64.27,
    Longitude of 100.22,  Where Humidity is: 68, Temperature of: -10.35,
    and Winspeed of 2.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tura,ru&units=imperial
    [ 64.78] [ 104.34]
    
    Now retrieving weather for city # 190: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-54.71] [-60.7]
    
    Now retrieving weather for city # 191: Lardos, GR. Latitude of 36.09,
    Longitude of 28.02,  Where Humidity is: 93, Temperature of: 53.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lardos,gr&units=imperial
    [ 35.76] [ 28.31]
    
    Now retrieving weather for city # 192: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [ 5.78] [-131.86]
    
    Now retrieving weather for city # 193: Polson, US. Latitude of 47.69,
    Longitude of -114.16,  Where Humidity is: 75, Temperature of: 29.21,
    and Winspeed of 1.79
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=polson,us&units=imperial
    [ 47.69] [-113.95]
    
    Now retrieving weather for city # 194: Camocim, BR. Latitude of -2.9,
    Longitude of -40.84,  Where Humidity is: 92, Temperature of: 77.9,
    and Winspeed of 7.9
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=camocim,br&units=imperial
    [-1.37] [-41.2]
    
    Now retrieving weather for city # 195: Blackfoot, US. Latitude of 43.19,
    Longitude of -112.35,  Where Humidity is: 75, Temperature of: 40.03,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=blackfoot,us&units=imperial
    [ 43.65] [-112.77]
    
    Now retrieving weather for city # 196: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-56.7] [-130.37]
    
    Now retrieving weather for city # 197: Cidreira, BR. Latitude of -30.17,
    Longitude of -50.22,  Where Humidity is: 100, Temperature of: 68.49,
    and Winspeed of 7.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cidreira,br&units=imperial
    [-39.82] [-41.19]
    
    Now retrieving weather for city # 198: Pangnirtung, CA. Latitude of 66.15,
    Longitude of -65.72,  Where Humidity is: 70, Temperature of: 1.62,
    and Winspeed of 0.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pangnirtung,ca&units=imperial
    [ 62.24] [-62.74]
    
    Now retrieving weather for city # 199: Bonavista, CA. Latitude of 48.65,
    Longitude of -53.11,  Where Humidity is: 94, Temperature of: 28.85,
    and Winspeed of 6.6
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bonavista,ca&units=imperial
    [ 53.27] [-48.1]
    
    Now retrieving weather for city # 200: Fairhaven, US. Latitude of 41.64,
    Longitude of -70.9,  Where Humidity is: 74, Temperature of: 39.04,
    and Winspeed of 8.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=fairhaven,us&units=imperial
    [ 41.49] [-70.87]
    
    Now retrieving weather for city # 201: Bibbiena, IT. Latitude of 43.7,
    Longitude of 11.82,  Where Humidity is: 93, Temperature of: 51.8,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bibbiena,it&units=imperial
    [ 43.97] [ 11.85]
    
    Now retrieving weather for city # 202: Hilo, US. Latitude of 19.71,
    Longitude of -155.08,  Where Humidity is: 53, Temperature of: 67.15,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hilo,us&units=imperial
    [ 8.46] [-156.7]
    
    Now retrieving weather for city # 203: Bathsheba, BB. Latitude of 13.22,
    Longitude of -59.52,  Where Humidity is: 78, Temperature of: 78.8,
    and Winspeed of 19.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bathsheba,bb&units=imperial
    [ 16.96] [-56.2]
    
    Now retrieving weather for city # 204: Tynda, RU. Latitude of 55.15,
    Longitude of 124.74,  Where Humidity is: 64, Temperature of: 13.5,
    and Winspeed of 3.53
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tynda,ru&units=imperial
    [ 55.13] [ 124.85]
    
    Now retrieving weather for city # 205: Barrow, US. Latitude of 39.51,
    Longitude of -90.4,  Where Humidity is: 55, Temperature of: 36.3,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barrow,us&units=imperial
    [ 74.93] [-160.94]
    
    Now retrieving weather for city # 206: New Norfolk, AU. Latitude of -42.78,
    Longitude of 147.06,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=new norfolk,au&units=imperial
    [-58.61] [ 129.98]
    
    Now retrieving weather for city # 207: Kirkuk, IQ. Latitude of 35.47,
    Longitude of 44.4,  Where Humidity is: 50, Temperature of: 44.6,
    and Winspeed of 6.89
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kirkuk,iq&units=imperial
    [ 35.64] [ 45.09]
    
    Now retrieving weather for city # 208: Orthez, FR. Latitude of 43.49,
    Longitude of -0.77,  Where Humidity is: 62, Temperature of: 52.72,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=orthez,fr&units=imperial
    [ 43.43] [-0.67]
    
    Now retrieving weather for city # 209: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-13.6] [ 1.62]
    
    Now retrieving weather for city # 210: Muisne, EC. Latitude of 0.61,
    Longitude of -80.02,  Where Humidity is: 87, Temperature of: 73.13,
    and Winspeed of 4.03
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=muisne,ec&units=imperial
    [ 3.25] [-83.79]
    
    Now retrieving weather for city # 211: Bathsheba, BB. Latitude of 13.22,
    Longitude of -59.52,  Where Humidity is: 78, Temperature of: 78.8,
    and Winspeed of 19.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bathsheba,bb&units=imperial
    [ 19.65] [-48.62]
    
    Now retrieving weather for city # 212: Lebu, CL. Latitude of -37.62,
    Longitude of -73.65,  Where Humidity is: 100, Temperature of: 60.66,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lebu,cl&units=imperial
    [-35.9] [-100.7]
    
    Now retrieving weather for city # 213: Bitung, ID. Latitude of 1.44,
    Longitude of 125.19,  Where Humidity is: 70, Temperature of: 86,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bitung,id&units=imperial
    [ 2.45] [ 125.34]
    
    Now retrieving weather for city # 214: Nishihara, JP. Latitude of 35.74,
    Longitude of 139.53,  Where Humidity is: 29, Temperature of: 51.35,
    and Winspeed of 11.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nishihara,jp&units=imperial
    [ 22.11] [ 130.55]
    
    Now retrieving weather for city # 215: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-87.1] [-28.93]
    
    Now retrieving weather for city # 216: Komsomolskiy, RU. Latitude of 67.55,
    Longitude of 63.78,  Where Humidity is: 79, Temperature of: 6.53,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=komsomolskiy,ru&units=imperial
    [ 72.06] [ 173.77]
    
    Now retrieving weather for city # 217: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-66.68] [-101.52]
    
    Now retrieving weather for city # 218: Carndonagh, IE. Latitude of 55.25,
    Longitude of -7.27,  Where Humidity is: 95, Temperature of: 42.84,
    and Winspeed of 5.66
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=carndonagh,ie&units=imperial
    [ 56.27] [-7.78]
    
    Now retrieving weather for city # 219: Beloha, MG. Latitude of -25.17,
    Longitude of 45.06,  Where Humidity is: 72, Temperature of: 70.56,
    and Winspeed of 10.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=beloha,mg&units=imperial
    [-30.69] [ 41.88]
    
    Now retrieving weather for city # 220: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-81.61] [-46.59]
    
    Now retrieving weather for city # 221: Boshnyakovo, RU. Latitude of 49.65,
    Longitude of 142.17,  Where Humidity is: 78, Temperature of: 19.71,
    and Winspeed of 7.34
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=boshnyakovo,ru&units=imperial
    [ 50.16] [ 141.77]
    
    Now retrieving weather for city # 222: Butaritari, KI. Latitude of 3.07,
    Longitude of 172.79,  Where Humidity is: 100, Temperature of: 82.85,
    and Winspeed of 14.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=butaritari,ki&units=imperial
    [ 17.7] [ 173.15]
    
    Now retrieving weather for city # 223: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-47.93] [ 86.98]
    
    Now retrieving weather for city # 224: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-84.5] [ 34.06]
    
    Now retrieving weather for city # 225: Shimoda, JP. Latitude of 34.7,
    Longitude of 138.93,  Where Humidity is: 53, Temperature of: 51.8,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=shimoda,jp&units=imperial
    [ 28.42] [ 139.88]
    
    Now retrieving weather for city # 226: Khilok, RU. Latitude of 51.36,
    Longitude of 110.46,  Where Humidity is: 74, Temperature of: 31.14,
    and Winspeed of 3.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=khilok,ru&units=imperial
    [ 50.16] [ 110.79]
    
    Now retrieving weather for city # 227: Cananea, MX. Latitude of 30.98,
    Longitude of -110.3,  Where Humidity is: 48, Temperature of: 60.8,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cananea,mx&units=imperial
    [ 30.96] [-110.44]
    
    Now retrieving weather for city # 228: Narsaq, GL. Latitude of 60.91,
    Longitude of -46.05,  Where Humidity is: 31, Temperature of: 35.6,
    and Winspeed of 19.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=narsaq,gl&units=imperial
    [ 76.9] [-62.21]
    
    Now retrieving weather for city # 229: Thompson, CA. Latitude of 55.74,
    Longitude of -97.86,  Where Humidity is: 53, Temperature of: 26.6,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=thompson,ca&units=imperial
    [ 65.21] [-94.89]
    
    Now retrieving weather for city # 230: Tabou, CI. Latitude of 4.42,
    Longitude of -7.36,  Where Humidity is: 100, Temperature of: 80.73,
    and Winspeed of 9.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tabou,ci&units=imperial
    [ 0.53] [-5.23]
    
    Now retrieving weather for city # 231: East London, ZA. Latitude of -33.02,
    Longitude of 27.91,  Where Humidity is: 100, Temperature of: 68.94,
    and Winspeed of 11.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=east london,za&units=imperial
    [-83.69] [ 60.06]
    
    Now retrieving weather for city # 232: Allapalli, IN. Latitude of 19.43,
    Longitude of 80.06,  Where Humidity is: 58, Temperature of: 80.33,
    and Winspeed of 9.84
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=allapalli,in&units=imperial
    [ 19.09] [ 80.25]
    
    Now retrieving weather for city # 233: Nara, ML. Latitude of 15.17,
    Longitude of -7.29,  Where Humidity is: 21, Temperature of: 77.94,
    and Winspeed of 11.48
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nara,ml&units=imperial
    [ 16.01] [-7.33]
    
    Now retrieving weather for city # 234: Deogarh, IN. Latitude of 21.53,
    Longitude of 84.72,  Where Humidity is: 54, Temperature of: 78.53,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=deogarh,in&units=imperial
    [ 21.69] [ 84.69]
    
    Now retrieving weather for city # 235: Dikson, RU. Latitude of 73.51,
    Longitude of 80.55,  Where Humidity is: 100, Temperature of: -9.05,
    and Winspeed of 20.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dikson,ru&units=imperial
    [ 84.07] [ 89.78]
    
    Now retrieving weather for city # 236: Hami, CN. Latitude of 42.84,
    Longitude of 93.51,  Where Humidity is: 48, Temperature of: 54.41,
    and Winspeed of 4.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hami,cn&units=imperial
    [ 43.09] [ 91.46]
    
    Now retrieving weather for city # 237: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-25.16] [-103.79]
    
    Now retrieving weather for city # 238: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 86.61] [-87.71]
    
    Now retrieving weather for city # 239: Sorsk, RU. Latitude of 54,
    Longitude of 90.25,  Where Humidity is: 82, Temperature of: 3.78,
    and Winspeed of 3.31
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sorsk,ru&units=imperial
    [ 53.7] [ 90.63]
    
    Now retrieving weather for city # 240: Vyartsilya, RU. Latitude of 62.18,
    Longitude of 30.69,  Where Humidity is: 92, Temperature of: 24.93,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vyartsilya,ru&units=imperial
    [ 62.25] [ 30.64]
    
    Now retrieving weather for city # 241: Bethel, US. Latitude of 60.79,
    Longitude of -161.76,  Where Humidity is: 72, Temperature of: 12.2,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bethel,us&units=imperial
    [ 45.91] [-171.2]
    
    Now retrieving weather for city # 242: Geraldton, AU. Latitude of -28.77,
    Longitude of 114.6,  Where Humidity is: 35, Temperature of: 89.6,
    and Winspeed of 25.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=geraldton,au&units=imperial
    [-32.39] [ 98.47]
    
    Now retrieving weather for city # 243: Hithadhoo, MV. Latitude of -0.6,
    Longitude of 73.08,  Where Humidity is: 100, Temperature of: 81.54,
    and Winspeed of 7.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hithadhoo,mv&units=imperial
    [-18.61] [ 80.41]
    
    Now retrieving weather for city # 244: Lavrentiya, RU. Latitude of 65.58,
    Longitude of -170.99,  Where Humidity is: 100, Temperature of: 3.06,
    and Winspeed of 13.15
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lavrentiya,ru&units=imperial
    [ 78.57] [-169.16]
    
    Now retrieving weather for city # 245: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-34.66] [-133.67]
    
    Now retrieving weather for city # 246: Maryville, US. Latitude of 40.35,
    Longitude of -94.87,  Where Humidity is: 100, Temperature of: 34.27,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=maryville,us&units=imperial
    [ 40.57] [-94.83]
    
    Now retrieving weather for city # 247: Kapaa, US. Latitude of 22.08,
    Longitude of -159.32,  Where Humidity is: 64, Temperature of: 71.6,
    and Winspeed of 18.34
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kapaa,us&units=imperial
    [ 36.25] [-168.35]
    
    Now retrieving weather for city # 248: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-79.57] [-99.94]
    
    Now retrieving weather for city # 249: Murgab, TM. Latitude of 37.5,
    Longitude of 61.97,  Where Humidity is: 71, Temperature of: 51.8,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=murgab,tm&units=imperial
    [ 37.38] [ 61.86]
    
    Now retrieving weather for city # 250: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [ 1.76] [-97.88]
    
    Now retrieving weather for city # 251: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-64.67] [-38.04]
    
    Now retrieving weather for city # 252: Touros, BR. Latitude of -5.2,
    Longitude of -35.46,  Where Humidity is: 89, Temperature of: 79.92,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=touros,br&units=imperial
    [ 1.14] [-29.78]
    
    Now retrieving weather for city # 253: Broken Hill, AU. Latitude of -31.97,
    Longitude of 141.45,  Where Humidity is: 38, Temperature of: 83.97,
    and Winspeed of 16.84
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=broken hill,au&units=imperial
    [-31.63] [ 139.64]
    
    Now retrieving weather for city # 254: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-42.01] [ 103.07]
    
    Now retrieving weather for city # 255: Luderitz, NA. Latitude of -26.65,
    Longitude of 15.16,  Where Humidity is: 93, Temperature of: 62.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=luderitz,na&units=imperial
    [-33.99] [ 6.22]
    
    Now retrieving weather for city # 256: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-89.79] [-179.24]
    
    Now retrieving weather for city # 257: Merauke, ID. Latitude of -8.49,
    Longitude of 140.4,  Where Humidity is: 100, Temperature of: 80.6,
    and Winspeed of 10.58
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=merauke,id&units=imperial
    [-7.64] [ 139.64]
    
    Now retrieving weather for city # 258: Alta Floresta, BR. Latitude of -9.87,
    Longitude of -56.08,  Where Humidity is: 96, Temperature of: 75.15,
    and Winspeed of 1.86
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=alta floresta,br&units=imperial
    [-7.28] [-54.86]
    
    Now retrieving weather for city # 259: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-55.72] [-140.37]
    
    Now retrieving weather for city # 260: Chuy, UY. Latitude of -33.69,
    Longitude of -53.46,  Where Humidity is: 86, Temperature of: 63.23,
    and Winspeed of 16.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chuy,uy&units=imperial
    [-50.82] [-36.06]
    
    Now retrieving weather for city # 261: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-79.8] [-35.24]
    
    Now retrieving weather for city # 262: Talnakh, RU. Latitude of 69.49,
    Longitude of 88.39,  Where Humidity is: 85, Temperature of: -27.41,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=talnakh,ru&units=imperial
    [ 72.44] [ 93.39]
    
    Now retrieving weather for city # 263: Yellowknife, CA. Latitude of 62.45,
    Longitude of -114.38,  Where Humidity is: 57, Temperature of: 23,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yellowknife,ca&units=imperial
    [ 67.39] [-115.71]
    
    Now retrieving weather for city # 264: Mersing, MY. Latitude of 2.43,
    Longitude of 103.84,  Where Humidity is: 94, Temperature of: 79.88,
    and Winspeed of 11.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mersing,my&units=imperial
    [ 3.05] [ 106.07]
    
    Now retrieving weather for city # 265: Coquimbo, CL. Latitude of -29.95,
    Longitude of -71.34,  Where Humidity is: 82, Temperature of: 59,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=coquimbo,cl&units=imperial
    [-28.09] [-88.75]
    
    Now retrieving weather for city # 266: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-80.84] [-127.13]
    
    Now retrieving weather for city # 267: Castro, CL. Latitude of -42.48,
    Longitude of -73.76,  Where Humidity is: 98, Temperature of: 56.34,
    and Winspeed of 12.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=castro,cl&units=imperial
    [-52.67] [-108.82]
    
    Now retrieving weather for city # 268: Sorland, NO. Latitude of 67.67,
    Longitude of 12.69,  Where Humidity is: 100, Temperature of: 23,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sorland,no&units=imperial
    [ 72.51] [ 5.09]
    
    Now retrieving weather for city # 269: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [ 4.19] [-91.65]
    
    Now retrieving weather for city # 270: Longyearbyen, SJ. Latitude of 78.22,
    Longitude of 15.64,  Where Humidity is: 71, Temperature of: 5,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=longyearbyen,sj&units=imperial
    [ 75.07] [ 18.93]
    
    Now retrieving weather for city # 271: Kwakoa, TZ. Latitude of -3.77,
    Longitude of 37.72,  Where Humidity is: 99, Temperature of: 65.52,
    and Winspeed of 3.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kwakoa,tz&units=imperial
    [-3.76] [ 37.68]
    
    Now retrieving weather for city # 272: North Bend, US. Latitude of 43.41,
    Longitude of -124.22,  Where Humidity is: 82, Temperature of: 55.24,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=north bend,us&units=imperial
    [ 43.69] [-130.67]
    
    Now retrieving weather for city # 273: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-44.28] [ 10.18]
    
    Now retrieving weather for city # 274: Tuktoyaktuk, CA. Latitude of 69.44,
    Longitude of -133.03,  Where Humidity is: 76, Temperature of: -0.77,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tuktoyaktuk,ca&units=imperial
    [ 83.97] [-126.07]
    
    Now retrieving weather for city # 275: Naze, JP. Latitude of 28.37,
    Longitude of 129.48,  Where Humidity is: 55, Temperature of: 64.4,
    and Winspeed of 1.12
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=naze,jp&units=imperial
    [ 29.68] [ 127.4]
    
    Now retrieving weather for city # 276: Fallon, US. Latitude of 46.84,
    Longitude of -105.12,  Where Humidity is: 65, Temperature of: 20.03,
    and Winspeed of 7.94
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=fallon,us&units=imperial
    [ 38.09] [-117.65]
    
    Now retrieving weather for city # 277: Tiznit, MA. Latitude of 29.7,
    Longitude of -9.73,  Where Humidity is: 95, Temperature of: 55.76,
    and Winspeed of 3.42
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tiznit,ma&units=imperial
    [ 26.78] [-9.29]
    
    Now retrieving weather for city # 278: Castro, CL. Latitude of -42.48,
    Longitude of -73.76,  Where Humidity is: 98, Temperature of: 56.34,
    and Winspeed of 12.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=castro,cl&units=imperial
    [-48.5] [-103.94]
    
    Now retrieving weather for city # 279: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-7.3] [-92.2]
    
    Now retrieving weather for city # 280: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-64.04] [ 30.76]
    
    Now retrieving weather for city # 281: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-76.12] [-120.25]
    
    Now retrieving weather for city # 282: Kirakira, SB. Latitude of -10.46,
    Longitude of 161.92,  Where Humidity is: 100, Temperature of: 79.83,
    and Winspeed of 22.26
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kirakira,sb&units=imperial
    [-14.67] [ 161.87]
    
    Now retrieving weather for city # 283: San Patricio, MX. Latitude of 19.22,
    Longitude of -104.7,  Where Humidity is: 69, Temperature of: 78.8,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san patricio,mx&units=imperial
    [ 12.39] [-110.2]
    
    Now retrieving weather for city # 284: Upernavik, GL. Latitude of 72.79,
    Longitude of -56.15,  Where Humidity is: 92, Temperature of: 3.6,
    and Winspeed of 1.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=upernavik,gl&units=imperial
    [ 83.06] [-54.28]
    
    Now retrieving weather for city # 285: Touros, BR. Latitude of -5.2,
    Longitude of -35.46,  Where Humidity is: 89, Temperature of: 79.92,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=touros,br&units=imperial
    [ 1.23] [-25.89]
    
    Now retrieving weather for city # 286: Bambous Virieux, MU. Latitude of -20.34,
    Longitude of 57.76,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bambous virieux,mu&units=imperial
    [-31.74] [ 83.28]
    
    Now retrieving weather for city # 287: Cayenne, GF. Latitude of 4.94,
    Longitude of -52.33,  Where Humidity is: 78, Temperature of: 78.8,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cayenne,gf&units=imperial
    [ 11.63] [-42.27]
    
    Now retrieving weather for city # 288: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-57.63] [-133.46]
    
    Now retrieving weather for city # 289: Vanavara, RU. Latitude of 60.35,
    Longitude of 102.28,  Where Humidity is: 63, Temperature of: 0,
    and Winspeed of 3.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vanavara,ru&units=imperial
    [ 60.91] [ 102.69]
    
    Now retrieving weather for city # 290: Hamilton, BM. Latitude of 32.3,
    Longitude of -64.78,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hamilton,bm&units=imperial
    [ 32.24] [-69.46]
    
    Now retrieving weather for city # 291: Constitucion, CL. Latitude of -35.33,
    Longitude of -72.42,  Where Humidity is: 91, Temperature of: 58.05,
    and Winspeed of 13.71
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=constitucion,cl&units=imperial
    [-33.86] [-76.81]
    
    Now retrieving weather for city # 292: Takoradi, GH. Latitude of 4.89,
    Longitude of -1.75,  Where Humidity is: 100, Temperature of: 80.6,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=takoradi,gh&units=imperial
    [ 3.84] [-1.54]
    
    Now retrieving weather for city # 293: Saskylakh, RU. Latitude of 71.97,
    Longitude of 114.09,  Where Humidity is: 39, Temperature of: -25.7,
    and Winspeed of 5.44
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saskylakh,ru&units=imperial
    [ 78.46] [ 112.48]
    
    Now retrieving weather for city # 294: Valdivia, CL. Latitude of -39.81,
    Longitude of -73.25,  Where Humidity is: 93, Temperature of: 53.6,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=valdivia,cl&units=imperial
    [-39.6] [-74.36]
    
    Now retrieving weather for city # 295: Nikolskoye, RU. Latitude of 59.7,
    Longitude of 30.79,  Where Humidity is: 100, Temperature of: 24.8,
    and Winspeed of 4.47
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nikolskoye,ru&units=imperial
    [ 47.04] [ 172.6]
    
    Now retrieving weather for city # 296: Hilo, US. Latitude of 19.71,
    Longitude of -155.08,  Where Humidity is: 53, Temperature of: 67.15,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hilo,us&units=imperial
    [ 25.22] [-142.93]
    
    Now retrieving weather for city # 297: Adrar, DZ. Latitude of 27.87,
    Longitude of -0.29,  Where Humidity is: 37, Temperature of: 64.4,
    and Winspeed of 2.86
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=adrar,dz&units=imperial
    [ 27.3] [-3.79]
    
    Now retrieving weather for city # 298: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-25.72] [-124.31]
    
    Now retrieving weather for city # 299: Port Hardy, CA. Latitude of 50.7,
    Longitude of -127.42,  Where Humidity is: 50, Temperature of: 55.4,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port hardy,ca&units=imperial
    [ 45.92] [-136.66]
    
    Now retrieving weather for city # 300: Sioux Lookout, CA. Latitude of 50.1,
    Longitude of -91.92,  Where Humidity is: 50, Temperature of: 33.8,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sioux lookout,ca&units=imperial
    [ 53.32] [-93.15]
    
    Now retrieving weather for city # 301: Saskylakh, RU. Latitude of 71.97,
    Longitude of 114.09,  Where Humidity is: 39, Temperature of: -25.7,
    and Winspeed of 5.44
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saskylakh,ru&units=imperial
    [ 72.15] [ 119.29]
    
    Now retrieving weather for city # 302: Redkino, RU. Latitude of 56.65,
    Longitude of 36.29,  Where Humidity is: 89, Temperature of: 21.78,
    and Winspeed of 9.95
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=redkino,ru&units=imperial
    [ 56.74] [ 36.25]
    
    Now retrieving weather for city # 303: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-44.46] [ 97.17]
    
    Now retrieving weather for city # 304: Anadyr, RU. Latitude of 64.73,
    Longitude of 177.51,  Where Humidity is: 76, Temperature of: -7.61,
    and Winspeed of 20.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=anadyr,ru&units=imperial
    [ 61.84] [ 175.14]
    
    Now retrieving weather for city # 305: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-43.72] [ 84.66]
    
    Now retrieving weather for city # 306: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-14.02] [-99.84]
    
    Now retrieving weather for city # 307: Yerbogachen, RU. Latitude of 61.28,
    Longitude of 108.01,  Where Humidity is: 63, Temperature of: 3.2,
    and Winspeed of 3.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yerbogachen,ru&units=imperial
    [ 59.61] [ 107.54]
    
    Now retrieving weather for city # 308: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-88.57] [-37.98]
    
    Now retrieving weather for city # 309: Labuhan, ID. Latitude of -2.54,
    Longitude of 115.51,  Where Humidity is: 77, Temperature of: 84.15,
    and Winspeed of 3.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=labuhan,id&units=imperial
    [-20.28] [ 94.61]
    
    Now retrieving weather for city # 310: Hilo, US. Latitude of 19.71,
    Longitude of -155.08,  Where Humidity is: 53, Temperature of: 67.15,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hilo,us&units=imperial
    [ 21.74] [-147.57]
    
    Now retrieving weather for city # 311: Takoradi, GH. Latitude of 4.89,
    Longitude of -1.75,  Where Humidity is: 100, Temperature of: 80.6,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=takoradi,gh&units=imperial
    [ 1.17] [-0.65]
    
    Now retrieving weather for city # 312: Mar del Plata, AR. Latitude of -46.43,
    Longitude of -67.52,  Where Humidity is: 56, Temperature of: 60.84,
    and Winspeed of 16.96
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mar del plata,ar&units=imperial
    [-70.42] [-21.71]
    
    Now retrieving weather for city # 313: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-56.34] [ 6.11]
    
    Now retrieving weather for city # 314: Kapaa, US. Latitude of 22.08,
    Longitude of -159.32,  Where Humidity is: 64, Temperature of: 71.6,
    and Winspeed of 18.34
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kapaa,us&units=imperial
    [ 26.21] [-157.04]
    
    Now retrieving weather for city # 315: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 76.8] [-87.35]
    
    Now retrieving weather for city # 316: Southbridge, NZ. Latitude of -43.81,
    Longitude of 172.25,  Where Humidity is: 60, Temperature of: 71.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=southbridge,nz&units=imperial
    [-44.24] [ 172.41]
    
    Now retrieving weather for city # 317: Arraial do Cabo, BR. Latitude of -22.97,
    Longitude of -42.02,  Where Humidity is: 100, Temperature of: 74.88,
    and Winspeed of 1.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=arraial do cabo,br&units=imperial
    [-37.44] [-35.63]
    
    Now retrieving weather for city # 318: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-67.34] [-135.01]
    
    Now retrieving weather for city # 319: Barra Patuca, HN. Latitude of 15.8,
    Longitude of -84.28,  Where Humidity is: 93, Temperature of: 79.38,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barra patuca,hn&units=imperial
    [ 17.65] [-84.1]
    
    Now retrieving weather for city # 320: Awjilah, LY. Latitude of 29.14,
    Longitude of 21.3,  Where Humidity is: 60, Temperature of: 51.66,
    and Winspeed of 2.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=awjilah,ly&units=imperial
    [ 29.85] [ 21.63]
    
    Now retrieving weather for city # 321: Ilulissat, GL. Latitude of 69.22,
    Longitude of -51.1,  Where Humidity is: 71, Temperature of: 8.6,
    and Winspeed of 2.42
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ilulissat,gl&units=imperial
    [ 84.02] [-44.22]
    
    Now retrieving weather for city # 322: Dunedin, NZ. Latitude of -45.87,
    Longitude of 170.5,  Where Humidity is: 67, Temperature of: 68.4,
    and Winspeed of 5.32
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dunedin,nz&units=imperial
    [-56.13] [ 178.]
    
    Now retrieving weather for city # 323: Kaitangata, NZ. Latitude of -46.28,
    Longitude of 169.85,  Where Humidity is: 63, Temperature of: 70.92,
    and Winspeed of 4.76
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kaitangata,nz&units=imperial
    [-60.54] [ 178.02]
    
    Now retrieving weather for city # 324: Ostrovnoy, RU. Latitude of 68.05,
    Longitude of 39.51,  Where Humidity is: 86, Temperature of: 16.29,
    and Winspeed of 9.84
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ostrovnoy,ru&units=imperial
    [ 71.3] [ 42.04]
    
    Now retrieving weather for city # 325: Kattivakkam, IN. Latitude of 13.22,
    Longitude of 80.32,  Where Humidity is: 88, Temperature of: 78.8,
    and Winspeed of 3.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kattivakkam,in&units=imperial
    [ 13.52] [ 82.74]
    
    Now retrieving weather for city # 326: San Patricio, MX. Latitude of 19.22,
    Longitude of -104.7,  Where Humidity is: 69, Temperature of: 78.8,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san patricio,mx&units=imperial
    [ 12.94] [-112.95]
    
    Now retrieving weather for city # 327: Albany, AU. Latitude of -35.02,
    Longitude of 117.88,  Where Humidity is: 67, Temperature of: 71.82,
    and Winspeed of 14.88
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=albany,au&units=imperial
    [-83.39] [ 105.85]
    
    Now retrieving weather for city # 328: Dordrecht, ZA. Latitude of -31.37,
    Longitude of 27.05,  Where Humidity is: 94, Temperature of: 44.78,
    and Winspeed of 2.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dordrecht,za&units=imperial
    [-31.36] [ 27.39]
    
    Now retrieving weather for city # 329: Mar del Plata, AR. Latitude of -46.43,
    Longitude of -67.52,  Where Humidity is: 56, Temperature of: 60.84,
    and Winspeed of 16.96
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mar del plata,ar&units=imperial
    [-67.79] [-27.06]
    
    Now retrieving weather for city # 330: Saint-Philippe, RE. Latitude of -21.36,
    Longitude of 55.77,  Where Humidity is: 73, Temperature of: 78.1,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-philippe,re&units=imperial
    [-65.05] [ 77.54]
    
    Now retrieving weather for city # 331: Sitka, US. Latitude of 37.17,
    Longitude of -99.65,  Where Humidity is: 46, Temperature of: 40.1,
    and Winspeed of 7.38
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sitka,us&units=imperial
    [ 56.84] [-135.94]
    
    Now retrieving weather for city # 332: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-78.79] [ 161.06]
    
    Now retrieving weather for city # 333: Castro, CL. Latitude of -42.48,
    Longitude of -73.76,  Where Humidity is: 98, Temperature of: 56.34,
    and Winspeed of 12.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=castro,cl&units=imperial
    [-48.69] [-99.16]
    
    Now retrieving weather for city # 334: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-5.07] [-98.09]
    
    Now retrieving weather for city # 335: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-74.69] [-9.75]
    
    Now retrieving weather for city # 336: Beira, MZ. Latitude of -19.84,
    Longitude of 34.84,  Where Humidity is: 87, Temperature of: 75.96,
    and Winspeed of 3.53
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=beira,mz&units=imperial
    [-21.12] [ 36.75]
    
    Now retrieving weather for city # 337: Upernavik, GL. Latitude of 72.79,
    Longitude of -56.15,  Where Humidity is: 92, Temperature of: 3.6,
    and Winspeed of 1.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=upernavik,gl&units=imperial
    [ 85.11] [-53.52]
    
    Now retrieving weather for city # 338: Bandundu, CD. Latitude of -3.32,
    Longitude of 17.38,  Where Humidity is: 94, Temperature of: 74.66,
    and Winspeed of 5.32
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bandundu,cd&units=imperial
    [-4.15] [ 17.74]
    
    Now retrieving weather for city # 339: New Norfolk, AU. Latitude of -42.78,
    Longitude of 147.06,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=new norfolk,au&units=imperial
    [-83.27] [ 124.63]
    
    Now retrieving weather for city # 340: Belfast, US. Latitude of 44.43,
    Longitude of -69.01,  Where Humidity is: 86, Temperature of: 31.3,
    and Winspeed of 6.55
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=belfast,us&units=imperial
    [ 44.67] [-69.25]
    
    Now retrieving weather for city # 341: Barrow, US. Latitude of 39.51,
    Longitude of -90.4,  Where Humidity is: 55, Temperature of: 36.3,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barrow,us&units=imperial
    [ 82.54] [-145.27]
    
    Now retrieving weather for city # 342: Sioux Lookout, CA. Latitude of 50.1,
    Longitude of -91.92,  Where Humidity is: 50, Temperature of: 33.8,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sioux lookout,ca&units=imperial
    [ 54.71] [-90.24]
    
    Now retrieving weather for city # 343: Kavieng, PG. Latitude of -2.57,
    Longitude of 150.8,  Where Humidity is: 98, Temperature of: 86,
    and Winspeed of 11.48
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kavieng,pg&units=imperial
    [ 2.01] [ 151.06]
    
    Now retrieving weather for city # 344: Albany, AU. Latitude of -35.02,
    Longitude of 117.88,  Where Humidity is: 67, Temperature of: 71.82,
    and Winspeed of 14.88
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=albany,au&units=imperial
    [-54.23] [ 123.56]
    
    Now retrieving weather for city # 345: Esperance, AU. Latitude of -33.86,
    Longitude of 121.89,  Where Humidity is: 82, Temperature of: 71.33,
    and Winspeed of 10.47
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=esperance,au&units=imperial
    [-47.43] [ 125.43]
    
    Now retrieving weather for city # 346: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-85.61] [ 75.48]
    
    Now retrieving weather for city # 347: Saint-Augustin, CA. Latitude of 45.63,
    Longitude of -73.98,  Where Humidity is: 78, Temperature of: 32,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-augustin,ca&units=imperial
    [ 58.87] [-61.75]
    
    Now retrieving weather for city # 348: Port Alfred, ZA. Latitude of -33.59,
    Longitude of 26.89,  Where Humidity is: 100, Temperature of: 64.58,
    and Winspeed of 5.66
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port alfred,za&units=imperial
    [-61.22] [ 41.84]
    
    Now retrieving weather for city # 349: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-21.41] [-127.27]
    
    Now retrieving weather for city # 350: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-40.64] [-16.97]
    
    Now retrieving weather for city # 351: Talnakh, RU. Latitude of 69.49,
    Longitude of 88.39,  Where Humidity is: 85, Temperature of: -27.41,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=talnakh,ru&units=imperial
    [ 77.16] [ 88.43]
    
    Now retrieving weather for city # 352: Pangkalanbuun, ID. Latitude of -2.68,
    Longitude of 111.62,  Where Humidity is: 100, Temperature of: 79.56,
    and Winspeed of 4.25
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pangkalanbuun,id&units=imperial
    [-2.63] [ 111.81]
    
    Now retrieving weather for city # 353: Kruisfontein, ZA. Latitude of -34,
    Longitude of 24.73,  Where Humidity is: 83, Temperature of: 58.23,
    and Winspeed of 3.31
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kruisfontein,za&units=imperial
    [-84.29] [ 28.09]
    
    Now retrieving weather for city # 354: Namsos, NO. Latitude of 64.47,
    Longitude of 11.51,  Where Humidity is: 88, Temperature of: 9.41,
    and Winspeed of 3.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=namsos,no&units=imperial
    [ 64.47] [ 11.44]
    
    Now retrieving weather for city # 355: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-78.14] [-83.45]
    
    Now retrieving weather for city # 356: Port Lincoln, AU. Latitude of -34.72,
    Longitude of 135.86,  Where Humidity is: 80, Temperature of: 68.81,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port lincoln,au&units=imperial
    [-40.53] [ 134.09]
    
    Now retrieving weather for city # 357: Ashdod, IL. Latitude of 31.8,
    Longitude of 34.65,  Where Humidity is: 82, Temperature of: 55.4,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ashdod,il&units=imperial
    [ 31.82] [ 34.64]
    
    Now retrieving weather for city # 358: Thompson, CA. Latitude of 55.74,
    Longitude of -97.86,  Where Humidity is: 53, Temperature of: 26.6,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=thompson,ca&units=imperial
    [ 71.26] [-98.55]
    
    Now retrieving weather for city # 359: De-Kastri, RU. Latitude of 51.48,
    Longitude of 140.77,  Where Humidity is: 95, Temperature of: 22.41,
    and Winspeed of 7.94
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=de-kastri,ru&units=imperial
    [ 50.99] [ 140.71]
    
    Now retrieving weather for city # 360: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-59.98] [-11.55]
    
    Now retrieving weather for city # 361: Dikson, RU. Latitude of 73.51,
    Longitude of 80.55,  Where Humidity is: 100, Temperature of: -9.05,
    and Winspeed of 20.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dikson,ru&units=imperial
    [ 88.08] [ 88.79]
    
    Now retrieving weather for city # 362: Pingyin, CN. Latitude of 36.28,
    Longitude of 116.45,  Where Humidity is: 62, Temperature of: 63.36,
    and Winspeed of 12.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pingyin,cn&units=imperial
    [ 36.47] [ 116.33]
    
    Now retrieving weather for city # 363: Mehamn, NO. Latitude of 71.03,
    Longitude of 27.85,  Where Humidity is: 88, Temperature of: 19.53,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mehamn,no&units=imperial
    [ 80.02] [ 28.16]
    
    Now retrieving weather for city # 364: Butaritari, KI. Latitude of 3.07,
    Longitude of 172.79,  Where Humidity is: 100, Temperature of: 82.85,
    and Winspeed of 14.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=butaritari,ki&units=imperial
    [ 28.17] [ 173.56]
    
    Now retrieving weather for city # 365: Tanout, NE. Latitude of 14.97,
    Longitude of 8.88,  Where Humidity is: 19, Temperature of: 64.53,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tanout,ne&units=imperial
    [ 16.22] [ 9.79]
    
    Now retrieving weather for city # 366: Port Alfred, ZA. Latitude of -33.59,
    Longitude of 26.89,  Where Humidity is: 100, Temperature of: 64.58,
    and Winspeed of 5.66
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port alfred,za&units=imperial
    [-86.36] [ 56.84]
    
    Now retrieving weather for city # 367: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-20.97] [ 0.44]
    
    Now retrieving weather for city # 368: Kyzyl-Suu, KG. Latitude of 42.34,
    Longitude of 78,  Where Humidity is: 100, Temperature of: 35.28,
    and Winspeed of 4.32
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kyzyl-suu,kg&units=imperial
    [ 41.65] [ 77.19]
    
    Now retrieving weather for city # 369: Saint-Philippe, RE. Latitude of -21.36,
    Longitude of 55.77,  Where Humidity is: 73, Temperature of: 78.1,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-philippe,re&units=imperial
    [-56.98] [ 67.38]
    
    Now retrieving weather for city # 370: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-34.09] [-130.57]
    
    Now retrieving weather for city # 371: Jinka, ET. Latitude of 5.79,
    Longitude of 36.57,  Where Humidity is: 59, Temperature of: 63.99,
    and Winspeed of 1.9
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jinka,et&units=imperial
    [ 4.55] [ 36.34]
    
    Now retrieving weather for city # 372: Haines Junction, CA. Latitude of 60.75,
    Longitude of -137.51,  Where Humidity is: 77, Temperature of: 27.86,
    and Winspeed of 8.79
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=haines junction,ca&units=imperial
    [ 59.74] [-140.62]
    
    Now retrieving weather for city # 373: Tazovskiy, RU. Latitude of 67.47,
    Longitude of 78.7,  Where Humidity is: 64, Temperature of: -21.6,
    and Winspeed of 12.97
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tazovskiy,ru&units=imperial
    [ 68.83] [ 79.79]
    
    Now retrieving weather for city # 374: Chuy, UY. Latitude of -33.69,
    Longitude of -53.46,  Where Humidity is: 86, Temperature of: 63.23,
    and Winspeed of 16.67
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chuy,uy&units=imperial
    [-39.94] [-46.35]
    
    Now retrieving weather for city # 375: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-35.41] [ 100.67]
    
    Now retrieving weather for city # 376: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-51.46] [-177.91]
    
    Now retrieving weather for city # 377: Takoradi, GH. Latitude of 4.89,
    Longitude of -1.75,  Where Humidity is: 100, Temperature of: 80.6,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=takoradi,gh&units=imperial
    [-0.96] [-0.07]
    
    Now retrieving weather for city # 378: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-69.24] [ 157.03]
    
    Now retrieving weather for city # 379: Nome, US. Latitude of 30.04,
    Longitude of -94.42,  Where Humidity is: 82, Temperature of: 63.48,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nome,us&units=imperial
    [ 59.67] [-169.23]
    
    Now retrieving weather for city # 380: Puerto Ayora, EC. Latitude of -0.74,
    Longitude of -90.35,  Where Humidity is: 97, Temperature of: 79.43,
    and Winspeed of 7.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto ayora,ec&units=imperial
    [-5.72] [-112.46]
    
    Now retrieving weather for city # 381: Jumla, NP. Latitude of 29.28,
    Longitude of 82.18,  Where Humidity is: 75, Temperature of: 29.43,
    and Winspeed of 2.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jumla,np&units=imperial
    [ 34.74] [ 83.66]
    
    Now retrieving weather for city # 382: Esperance, AU. Latitude of -33.86,
    Longitude of 121.89,  Where Humidity is: 82, Temperature of: 71.33,
    and Winspeed of 10.47
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=esperance,au&units=imperial
    [-35.63] [ 123.34]
    
    Now retrieving weather for city # 383: Sri Aman, MY. Latitude of 1.24,
    Longitude of 111.46,  Where Humidity is: 88, Temperature of: 80.6,
    and Winspeed of 1.12
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sri aman,my&units=imperial
    [ 1.2] [ 112.23]
    
    Now retrieving weather for city # 384: Upernavik, GL. Latitude of 72.79,
    Longitude of -56.15,  Where Humidity is: 92, Temperature of: 3.6,
    and Winspeed of 1.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=upernavik,gl&units=imperial
    [ 82.25] [-48.68]
    
    Now retrieving weather for city # 385: Lebu, CL. Latitude of -37.62,
    Longitude of -73.65,  Where Humidity is: 100, Temperature of: 60.66,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lebu,cl&units=imperial
    [-37.65] [-98.15]
    
    Now retrieving weather for city # 386: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-88.36] [-7.16]
    
    Now retrieving weather for city # 387: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-85.49] [ 162.49]
    
    Now retrieving weather for city # 388: Bambous Virieux, MU. Latitude of -20.34,
    Longitude of 57.76,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bambous virieux,mu&units=imperial
    [-28.84] [ 83.11]
    
    Now retrieving weather for city # 389: Barrow, US. Latitude of 39.51,
    Longitude of -90.4,  Where Humidity is: 55, Temperature of: 36.3,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barrow,us&units=imperial
    [ 72.8] [-157.63]
    
    Now retrieving weather for city # 390: Bambui, BR. Latitude of -20.01,
    Longitude of -45.98,  Where Humidity is: 91, Temperature of: 64.44,
    and Winspeed of 0.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bambui,br&units=imperial
    [-20.35] [-46.14]
    
    Now retrieving weather for city # 391: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-89.26] [ 171.87]
    
    Now retrieving weather for city # 392: Havre-Saint-Pierre, CA. Latitude of 50.23,
    Longitude of -63.6,  Where Humidity is: 80, Temperature of: 30.2,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=havre-saint-pierre,ca&units=imperial
    [ 55.71] [-63.97]
    
    Now retrieving weather for city # 393: New Norfolk, AU. Latitude of -42.78,
    Longitude of 147.06,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=new norfolk,au&units=imperial
    [-51.05] [ 136.59]
    
    Now retrieving weather for city # 394: Vysokopillya, UA. Latitude of 47.49,
    Longitude of 33.53,  Where Humidity is: 88, Temperature of: 33.8,
    and Winspeed of 6.71
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vysokopillya,ua&units=imperial
    [ 47.47] [ 33.37]
    
    Now retrieving weather for city # 395: Upernavik, GL. Latitude of 72.79,
    Longitude of -56.15,  Where Humidity is: 92, Temperature of: 3.6,
    and Winspeed of 1.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=upernavik,gl&units=imperial
    [ 80.78] [-49.59]
    
    Now retrieving weather for city # 396: Misratah, LY. Latitude of 32.38,
    Longitude of 15.09,  Where Humidity is: 62, Temperature of: 64.4,
    and Winspeed of 8.95
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=misratah,ly&units=imperial
    [ 31.72] [ 15.73]
    
    Now retrieving weather for city # 397: Komsomolskiy, RU. Latitude of 67.55,
    Longitude of 63.78,  Where Humidity is: 79, Temperature of: 6.53,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=komsomolskiy,ru&units=imperial
    [ 68.03] [ 63.15]
    
    Now retrieving weather for city # 398: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-41.78] [-10.48]
    
    Now retrieving weather for city # 399: Hermanus, ZA. Latitude of -34.42,
    Longitude of 19.24,  Where Humidity is: 94, Temperature of: 51.21,
    and Winspeed of 2.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hermanus,za&units=imperial
    [-88.03] [ 2.21]
    
    Now retrieving weather for city # 400: Busselton, AU. Latitude of -33.64,
    Longitude of 115.35,  Where Humidity is: 82, Temperature of: 75.51,
    and Winspeed of 15.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=busselton,au&units=imperial
    [-35.04] [ 112.59]
    
    Now retrieving weather for city # 401: Tuktoyaktuk, CA. Latitude of 69.44,
    Longitude of -133.03,  Where Humidity is: 76, Temperature of: -0.77,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tuktoyaktuk,ca&units=imperial
    [ 70.49] [-129.08]
    
    Now retrieving weather for city # 402: Bambous Virieux, MU. Latitude of -20.34,
    Longitude of 57.76,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bambous virieux,mu&units=imperial
    [-27.33] [ 67.97]
    
    Now retrieving weather for city # 403: Irbit, RU. Latitude of 57.67,
    Longitude of 63.06,  Where Humidity is: 57, Temperature of: -0.86,
    and Winspeed of 2.8
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=irbit,ru&units=imperial
    [ 57.89] [ 63.35]
    
    Now retrieving weather for city # 404: Vardo, NO. Latitude of 70.37,
    Longitude of 31.11,  Where Humidity is: 100, Temperature of: 29.43,
    and Winspeed of 6.6
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vardo,no&units=imperial
    [ 80.94] [ 36.05]
    
    Now retrieving weather for city # 405: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-66.99] [-71.06]
    
    Now retrieving weather for city # 406: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-75.43] [-82.86]
    
    Now retrieving weather for city # 407: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-69.3] [-38.16]
    
    Now retrieving weather for city # 408: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-49.31] [ 9.01]
    
    Now retrieving weather for city # 409: Beringovskiy, RU. Latitude of 63.05,
    Longitude of 179.32,  Where Humidity is: 95, Temperature of: 16.74,
    and Winspeed of 26.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=beringovskiy,ru&units=imperial
    [ 60.32] [ 176.08]
    
    Now retrieving weather for city # 410: Khatanga, RU. Latitude of 71.98,
    Longitude of 102.47,  Where Humidity is: 62, Temperature of: -21.15,
    and Winspeed of 8.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=khatanga,ru&units=imperial
    [ 84.49] [ 107.6]
    
    Now retrieving weather for city # 411: Tsaratanana, MG. Latitude of -16.8,
    Longitude of 47.65,  Where Humidity is: 88, Temperature of: 67.41,
    and Winspeed of 6.11
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tsaratanana,mg&units=imperial
    [-16.23] [ 47.32]
    
    Now retrieving weather for city # 412: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [-9.38] [-129.93]
    
    Now retrieving weather for city # 413: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-69.66] [ 168.54]
    
    Now retrieving weather for city # 414: Abonnema, NG. Latitude of 4.71,
    Longitude of 6.79,  Where Humidity is: 95, Temperature of: 73.4,
    and Winspeed of 4.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=abonnema,ng&units=imperial
    [ 2.48] [ 6.31]
    
    Now retrieving weather for city # 415: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-85.4] [ 158.48]
    
    Now retrieving weather for city # 416: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-86.92] [-123.33]
    
    Now retrieving weather for city # 417: Atar, MR. Latitude of 20.52,
    Longitude of -13.05,  Where Humidity is: 33, Temperature of: 67.46,
    and Winspeed of 7.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atar,mr&units=imperial
    [ 20.15] [-14.64]
    
    Now retrieving weather for city # 418: Saldanha, ZA. Latitude of -33.01,
    Longitude of 17.94,  Where Humidity is: 87, Temperature of: 59,
    and Winspeed of 1.12
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saldanha,za&units=imperial
    [-37.65] [ 6.02]
    
    Now retrieving weather for city # 419: Emerald, AU. Latitude of -23.53,
    Longitude of 148.16,  Where Humidity is: 42, Temperature of: 85.73,
    and Winspeed of 14.09
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=emerald,au&units=imperial
    [-25.34] [ 146.75]
    
    Now retrieving weather for city # 420: Avarua, CK. Latitude of -21.21,
    Longitude of -159.78,  Where Humidity is: 70, Temperature of: 84.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=avarua,ck&units=imperial
    [-13.99] [-162.95]
    
    Now retrieving weather for city # 421: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-34.57] [-176.83]
    
    Now retrieving weather for city # 422: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-22.2] [-179.83]
    
    Now retrieving weather for city # 423: Mehamn, NO. Latitude of 71.03,
    Longitude of 27.85,  Where Humidity is: 88, Temperature of: 19.53,
    and Winspeed of 3.02
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mehamn,no&units=imperial
    [ 70.12] [ 27.97]
    
    Now retrieving weather for city # 424: Barrow, US. Latitude of 39.51,
    Longitude of -90.4,  Where Humidity is: 55, Temperature of: 36.3,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=barrow,us&units=imperial
    [ 81.15] [-159.16]
    
    Now retrieving weather for city # 425: Rikitea, PF. Latitude of -23.12,
    Longitude of -134.97,  Where Humidity is: 100, Temperature of: 80.28,
    and Winspeed of 18.23
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rikitea,pf&units=imperial
    [-31.9] [-114.62]
    
    Now retrieving weather for city # 426: Mahebourg, MU. Latitude of -20.41,
    Longitude of 57.7,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mahebourg,mu&units=imperial
    [-44.81] [ 78.4]
    
    Now retrieving weather for city # 427: Faanui, PF. Latitude of -16.48,
    Longitude of -151.75,  Where Humidity is: 100, Temperature of: 80.91,
    and Winspeed of 12.19
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=faanui,pf&units=imperial
    [-0.5] [-154.26]
    
    Now retrieving weather for city # 428: Kulhudhuffushi, MV. Latitude of 6.62,
    Longitude of 73.07,  Where Humidity is: 99, Temperature of: 84.24,
    and Winspeed of 8.28
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kulhudhuffushi,mv&units=imperial
    [ 6.92] [ 65.39]
    
    Now retrieving weather for city # 429: Beringovskiy, RU. Latitude of 63.05,
    Longitude of 179.32,  Where Humidity is: 95, Temperature of: 16.74,
    and Winspeed of 26.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=beringovskiy,ru&units=imperial
    [ 53.27] [ 177.55]
    
    Now retrieving weather for city # 430: Kloulklubed, PW. Latitude of 7.04,
    Longitude of 134.26,  Where Humidity is: 59, Temperature of: 87.75,
    and Winspeed of 8.05
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kloulklubed,pw&units=imperial
    [ 7.25] [ 131.05]
    
    Now retrieving weather for city # 431: Iqaluit, CA. Latitude of 63.75,
    Longitude of -68.52,  Where Humidity is: 78, Temperature of: 10.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=iqaluit,ca&units=imperial
    [ 64.75] [-72.9]
    
    Now retrieving weather for city # 432: Adrian, US. Latitude of 41.9,
    Longitude of -84.04,  Where Humidity is: 74, Temperature of: 29.66,
    and Winspeed of 5.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=adrian,us&units=imperial
    [ 41.71] [-84.4]
    
    Now retrieving weather for city # 433: Hudson Bay, CA. Latitude of 52.86,
    Longitude of -102.4,  Where Humidity is: 73, Temperature of: 18.45,
    and Winspeed of 2.57
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hudson bay,ca&units=imperial
    [ 53.5] [-102.5]
    
    Now retrieving weather for city # 434: Bogdanovich, RU. Latitude of 56.77,
    Longitude of 62.05,  Where Humidity is: 70, Temperature of: 4.41,
    and Winspeed of 2.64
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bogdanovich,ru&units=imperial
    [ 56.63] [ 62.16]
    
    Now retrieving weather for city # 435: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-69.18] [ 33.31]
    
    Now retrieving weather for city # 436: Tasiilaq, GL. Latitude of 65.61,
    Longitude of -37.64,  Where Humidity is: 67, Temperature of: 21.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tasiilaq,gl&units=imperial
    [ 70.] [-33.53]
    
    Now retrieving weather for city # 437: Butaritari, KI. Latitude of 3.07,
    Longitude of 172.79,  Where Humidity is: 100, Temperature of: 82.85,
    and Winspeed of 14.65
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=butaritari,ki&units=imperial
    [ 8.49] [ 172.88]
    
    Now retrieving weather for city # 438: Myronivka, UA. Latitude of 49.66,
    Longitude of 30.98,  Where Humidity is: 98, Temperature of: 31.41,
    and Winspeed of 11.3
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=myronivka,ua&units=imperial
    [ 49.58] [ 31.1]
    
    Now retrieving weather for city # 439: Portland, AU. Latitude of -33.35,
    Longitude of 149.98,  Where Humidity is: 60, Temperature of: 73.58,
    and Winspeed of 1.63
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=portland,au&units=imperial
    [-48.77] [ 134.54]
    
    Now retrieving weather for city # 440: Dehloran, IR. Latitude of 32.69,
    Longitude of 47.27,  Where Humidity is: 65, Temperature of: 48.15,
    and Winspeed of 2.08
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dehloran,ir&units=imperial
    [ 32.98] [ 46.97]
    
    Now retrieving weather for city # 441: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-75.74] [-80.41]
    
    Now retrieving weather for city # 442: Cap Malheureux, MU. Latitude of -19.98,
    Longitude of 57.61,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cap malheureux,mu&units=imperial
    [-15.19] [ 57.31]
    
    Now retrieving weather for city # 443: Souillac, MU. Latitude of -20.52,
    Longitude of 57.52,  Where Humidity is: 78, Temperature of: 80.6,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=souillac,mu&units=imperial
    [-57.59] [ 79.29]
    
    Now retrieving weather for city # 444: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-33.75] [-13.19]
    
    Now retrieving weather for city # 445: Ilulissat, GL. Latitude of 69.22,
    Longitude of -51.1,  Where Humidity is: 71, Temperature of: 8.6,
    and Winspeed of 2.42
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ilulissat,gl&units=imperial
    [ 87.26] [-40.95]
    
    Now retrieving weather for city # 446: Dikson, RU. Latitude of 73.51,
    Longitude of 80.55,  Where Humidity is: 100, Temperature of: -9.05,
    and Winspeed of 20.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=dikson,ru&units=imperial
    [ 74.93] [ 79.88]
    
    Now retrieving weather for city # 447: Ponta do Sol, CV. Latitude of 17.2,
    Longitude of -25.09,  Where Humidity is: 100, Temperature of: 69.57,
    and Winspeed of 19.08
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ponta do sol,cv&units=imperial
    [ 25.27] [-32.6]
    
    Now retrieving weather for city # 448: Ekhabi, RU. Latitude of 53.51,
    Longitude of 142.97,  Where Humidity is: 88, Temperature of: 10.31,
    and Winspeed of 3.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ekhabi,ru&units=imperial
    [ 54.24] [ 146.28]
    
    Now retrieving weather for city # 449: Sur, OM. Latitude of 22.57,
    Longitude of 59.53,  Where Humidity is: 89, Temperature of: 75.11,
    and Winspeed of 7.94
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sur,om&units=imperial
    [ 15.4] [ 62.19]
    
    Now retrieving weather for city # 450: Pangnirtung, CA. Latitude of 66.15,
    Longitude of -65.72,  Where Humidity is: 70, Temperature of: 1.62,
    and Winspeed of 0.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pangnirtung,ca&units=imperial
    [ 59.8] [-61.23]
    
    Now retrieving weather for city # 451: Henties Bay, NA. Latitude of -22.12,
    Longitude of 14.28,  Where Humidity is: 100, Temperature of: 60.3,
    and Winspeed of 5.37
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=henties bay,na&units=imperial
    [-23.44] [ 4.88]
    
    Now retrieving weather for city # 452: Waipawa, NZ. Latitude of -39.94,
    Longitude of 176.59,  Where Humidity is: 98, Temperature of: 58.01,
    and Winspeed of 6.26
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=waipawa,nz&units=imperial
    [-46.] [ 178.7]
    
    Now retrieving weather for city # 453: Pisco, PE. Latitude of -13.71,
    Longitude of -76.2,  Where Humidity is: 78, Temperature of: 71.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pisco,pe&units=imperial
    [-22.] [-89.37]
    
    Now retrieving weather for city # 454: Shenjiamen, CN. Latitude of 29.96,
    Longitude of 122.3,  Where Humidity is: 84, Temperature of: 55.31,
    and Winspeed of 14.16
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=shenjiamen,cn&units=imperial
    [ 29.33] [ 122.95]
    
    Now retrieving weather for city # 455: Alofi, NU. Latitude of -19.06,
    Longitude of -169.92,  Where Humidity is: 70, Temperature of: 86,
    and Winspeed of 11.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=alofi,nu&units=imperial
    [-26.12] [-167.4]
    
    Now retrieving weather for city # 456: Rio Grande, BR. Latitude of -32.03,
    Longitude of -52.1,  Where Humidity is: 79, Temperature of: 71.96,
    and Winspeed of 19.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=rio grande,br&units=imperial
    [-40.35] [-42.8]
    
    Now retrieving weather for city # 457: Vaini, TO. Latitude of -21.2,
    Longitude of -175.2,  Where Humidity is: 66, Temperature of: 87.8,
    and Winspeed of 10.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=vaini,to&units=imperial
    [-24.55] [-177.94]
    
    Now retrieving weather for city # 458: Jamestown, SH. Latitude of -15.94,
    Longitude of -5.72,  Where Humidity is: 100, Temperature of: 73.26,
    and Winspeed of 10.69
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=jamestown,sh&units=imperial
    [-33.77] [-19.68]
    
    Now retrieving weather for city # 459: Tautira, PF. Latitude of -17.73,
    Longitude of -149.15,  Where Humidity is: 69, Temperature of: 82.4,
    and Winspeed of 11.41
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tautira,pf&units=imperial
    [-15.63] [-142.79]
    
    Now retrieving weather for city # 460: Flinders, AU. Latitude of -34.58,
    Longitude of 150.85,  Where Humidity is: 64, Temperature of: 73.4,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=flinders,au&units=imperial
    [-36.97] [ 128.96]
    
    Now retrieving weather for city # 461: Ottawa, US. Latitude of 38.62,
    Longitude of -95.27,  Where Humidity is: 66, Temperature of: 39.2,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ottawa,us&units=imperial
    [ 41.27] [-88.93]
    
    Now retrieving weather for city # 462: Wajima, JP. Latitude of 37.4,
    Longitude of 136.9,  Where Humidity is: 55, Temperature of: 39.2,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=wajima,jp&units=imperial
    [ 38.61] [ 134.35]
    
    Now retrieving weather for city # 463: Qaanaaq, GL. Latitude of 77.48,
    Longitude of -69.36,  Where Humidity is: 100, Temperature of: -11.03,
    and Winspeed of 2.35
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=qaanaaq,gl&units=imperial
    [ 75.65] [-79.25]
    
    Now retrieving weather for city # 464: Porto Novo, CV. Latitude of 17.02,
    Longitude of -25.06,  Where Humidity is: 77, Temperature of: 68,
    and Winspeed of 32.21
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=porto novo,cv&units=imperial
    [ 13.87] [-34.9]
    
    Now retrieving weather for city # 465: Tuktoyaktuk, CA. Latitude of 69.44,
    Longitude of -133.03,  Where Humidity is: 76, Temperature of: -0.77,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tuktoyaktuk,ca&units=imperial
    [ 81.3] [-127.56]
    
    Now retrieving weather for city # 466: Albany, AU. Latitude of -35.02,
    Longitude of 117.88,  Where Humidity is: 67, Temperature of: 71.82,
    and Winspeed of 14.88
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=albany,au&units=imperial
    [-42.16] [ 121.36]
    
    Now retrieving weather for city # 467: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-67.09] [-83.97]
    
    Now retrieving weather for city # 468: Havelock, US. Latitude of 34.88,
    Longitude of -76.9,  Where Humidity is: 76, Temperature of: 46.85,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=havelock,us&units=imperial
    [ 32.65] [-72.15]
    
    Now retrieving weather for city # 469: Atemar, RU. Latitude of 54.18,
    Longitude of 45.4,  Where Humidity is: 58, Temperature of: -6.35,
    and Winspeed of 4.81
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atemar,ru&units=imperial
    [ 54.36] [ 45.58]
    
    Now retrieving weather for city # 470: Comodoro Rivadavia, AR. Latitude of -45.87,
    Longitude of -67.48,  Where Humidity is: 37, Temperature of: 66.2,
    and Winspeed of 17.22
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=comodoro rivadavia,ar&units=imperial
    [-45.27] [-66.85]
    
    Now retrieving weather for city # 471: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-77.7] [-40.71]
    
    Now retrieving weather for city # 472: Tual, ID. Latitude of -5.67,
    Longitude of 132.75,  Where Humidity is: 100, Temperature of: 79.56,
    and Winspeed of 10.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tual,id&units=imperial
    [-4.61] [ 130.91]
    
    Now retrieving weather for city # 473: Mantua, CU. Latitude of 22.29,
    Longitude of -84.28,  Where Humidity is: 93, Temperature of: 74.03,
    and Winspeed of 4.14
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=mantua,cu&units=imperial
    [ 24.98] [-86.01]
    
    Now retrieving weather for city # 474: Pitimbu, BR. Latitude of -7.47,
    Longitude of -34.81,  Where Humidity is: 83, Temperature of: 78.8,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=pitimbu,br&units=imperial
    [-7.05] [-28.03]
    
    Now retrieving weather for city # 475: Nome, US. Latitude of 30.04,
    Longitude of -94.42,  Where Humidity is: 82, Temperature of: 63.48,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=nome,us&units=imperial
    [ 63.82] [-163.5]
    
    Now retrieving weather for city # 476: Saint-Philippe, RE. Latitude of -21.36,
    Longitude of 55.77,  Where Humidity is: 73, Temperature of: 78.1,
    and Winspeed of 5.82
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=saint-philippe,re&units=imperial
    [-70.67] [ 72.97]
    
    Now retrieving weather for city # 477: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-54.7] [ 144.87]
    
    Now retrieving weather for city # 478: Baykit, RU. Latitude of 61.68,
    Longitude of 96.39,  Where Humidity is: 73, Temperature of: -3.78,
    and Winspeed of 3.91
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=baykit,ru&units=imperial
    [ 61.84] [ 95.7]
    
    Now retrieving weather for city # 479: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-46.12] [ 2.43]
    
    Now retrieving weather for city # 480: Ribeira Grande, PT. Latitude of 38.52,
    Longitude of -28.7,  Where Humidity is: 99, Temperature of: 62.19,
    and Winspeed of 26.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ribeira grande,pt&units=imperial
    [ 40.05] [-38.92]
    
    Now retrieving weather for city # 481: Avarua, CK. Latitude of -21.21,
    Longitude of -159.78,  Where Humidity is: 70, Temperature of: 84.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=avarua,ck&units=imperial
    [-42.72] [-159.01]
    
    Now retrieving weather for city # 482: Ushuaia, AR. Latitude of -54.81,
    Longitude of -68.31,  Where Humidity is: 70, Temperature of: 48.2,
    and Winspeed of 15.61
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ushuaia,ar&units=imperial
    [-64.1] [-61.31]
    
    Now retrieving weather for city # 483: Tasiilaq, GL. Latitude of 65.61,
    Longitude of -37.64,  Where Humidity is: 67, Temperature of: 21.2,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tasiilaq,gl&units=imperial
    [ 59.83] [-31.05]
    
    Now retrieving weather for city # 484: Tiksi, RU. Latitude of 71.64,
    Longitude of 128.87,  Where Humidity is: 69, Temperature of: -23.58,
    and Winspeed of 3.76
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tiksi,ru&units=imperial
    [ 85.85] [ 121.77]
    
    Now retrieving weather for city # 485: Yellowknife, CA. Latitude of 62.45,
    Longitude of -114.38,  Where Humidity is: 57, Temperature of: 23,
    and Winspeed of 3.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yellowknife,ca&units=imperial
    [ 61.4] [-112.34]
    
    Now retrieving weather for city # 486: Comodoro Rivadavia, AR. Latitude of -45.87,
    Longitude of -67.48,  Where Humidity is: 37, Temperature of: 66.2,
    and Winspeed of 17.22
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=comodoro rivadavia,ar&units=imperial
    [-49.31] [-63.05]
    
    Now retrieving weather for city # 487: Muros, ES. Latitude of 42.77,
    Longitude of -9.06,  Where Humidity is: 93, Temperature of: 46.4,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=muros,es&units=imperial
    [ 43.74] [-14.59]
    
    Now retrieving weather for city # 488: Slavsk, RU. Latitude of 55.05,
    Longitude of 21.68,  Where Humidity is: 81, Temperature of: 34.7,
    and Winspeed of 9.4
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=slavsk,ru&units=imperial
    [ 54.87] [ 21.73]
    
    Now retrieving weather for city # 489: Bethel, US. Latitude of 60.79,
    Longitude of -161.76,  Where Humidity is: 72, Temperature of: 12.2,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bethel,us&units=imperial
    [ 51.65] [-162.11]
    
    Now retrieving weather for city # 490: Hobart, AU. Latitude of -42.88,
    Longitude of 147.33,  Where Humidity is: 48, Temperature of: 64.4,
    and Winspeed of 14.99
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hobart,au&units=imperial
    [-78.2] [ 137.83]
    
    Now retrieving weather for city # 491: Kamenka, RU. Latitude of 53.19,
    Longitude of 44.05,  Where Humidity is: 69, Temperature of: -9.72,
    and Winspeed of 3.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kamenka,ru&units=imperial
    [ 70.73] [ 44.11]
    
    Now retrieving weather for city # 492: Arraial do Cabo, BR. Latitude of -22.97,
    Longitude of -42.02,  Where Humidity is: 100, Temperature of: 74.88,
    and Winspeed of 1.74
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=arraial do cabo,br&units=imperial
    [-54.28] [-20.47]
    
    Now retrieving weather for city # 493: Punta Arenas, CL. Latitude of -53.16,
    Longitude of -70.91,  Where Humidity is: 76, Temperature of: 48.2,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=punta arenas,cl&units=imperial
    [-86.26] [-98.43]
    
    Now retrieving weather for city # 494: Atuona, PF. Latitude of -9.8,
    Longitude of -139.03,  Where Humidity is: 100, Temperature of: 81.68,
    and Winspeed of 17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=atuona,pf&units=imperial
    [-4.27] [-127.51]
    
    Now retrieving weather for city # 495: Ostrovnoy, RU. Latitude of 68.05,
    Longitude of 39.51,  Where Humidity is: 86, Temperature of: 16.29,
    and Winspeed of 9.84
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ostrovnoy,ru&units=imperial
    [ 74.23] [ 39.32]
    
    Now retrieving weather for city # 496: Port Elizabeth, ZA. Latitude of -33.92,
    Longitude of 25.57,  Where Humidity is: 82, Temperature of: 64.4,
    and Winspeed of 4.7
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=port elizabeth,za&units=imperial
    [-74.09] [ 29.43]
    
    Now retrieving weather for city # 497: Narsaq, GL. Latitude of 60.91,
    Longitude of -46.05,  Where Humidity is: 31, Temperature of: 35.6,
    and Winspeed of 19.46
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=narsaq,gl&units=imperial
    [ 78.77] [-64.2]
    
    Now retrieving weather for city # 498: Caravelas, BR. Latitude of -17.73,
    Longitude of -39.27,  Where Humidity is: 96, Temperature of: 82.76,
    and Winspeed of 20.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=caravelas,br&units=imperial
    [-23.52] [-27.73]
    
    Now retrieving weather for city # 499: Luderitz, NA. Latitude of -26.65,
    Longitude of 15.16,  Where Humidity is: 93, Temperature of: 62.6,
    and Winspeed of 2.24
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=luderitz,na&units=imperial
    [-29.43] [ 8.58]
    
    Now retrieving weather for city # 500: Lorengau, PG. Latitude of -2.02,
    Longitude of 147.27,  Where Humidity is: 100, Temperature of: 81.5,
    and Winspeed of 11.14
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lorengau,pg&units=imperial
    [-3.2] [ 146.65]
    
    Now retrieving weather for city # 501: Tuktoyaktuk, CA. Latitude of 69.44,
    Longitude of -133.03,  Where Humidity is: 76, Temperature of: -0.77,
    and Winspeed of 12.75
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tuktoyaktuk,ca&units=imperial
    [ 74.06] [-128.21]
    
    Now retrieving weather for city # 502: Ilulissat, GL. Latitude of 69.22,
    Longitude of -51.1,  Where Humidity is: 71, Temperature of: 8.6,
    and Winspeed of 2.42
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=ilulissat,gl&units=imperial
    [ 77.12] [-46.69]
    
    Now retrieving weather for city # 503: Kavieng, PG. Latitude of -2.57,
    Longitude of 150.8,  Where Humidity is: 98, Temperature of: 86,
    and Winspeed of 11.48
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kavieng,pg&units=imperial
    [ 1.5] [ 153.33]
    
    Now retrieving weather for city # 504: Leningradskiy, RU. Latitude of 69.38,
    Longitude of 178.42,  Where Humidity is: 92, Temperature of: -4.28,
    and Winspeed of 24.45
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=leningradskiy,ru&units=imperial
    [ 88.62] [ 178.8]
    
    Now retrieving weather for city # 505: Sobolevo, RU. Latitude of 54.43,
    Longitude of 31.9,  Where Humidity is: 93, Temperature of: 29.61,
    and Winspeed of 16.4
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=sobolevo,ru&units=imperial
    [ 53.44] [ 155.15]
    
    Now retrieving weather for city # 506: Puerto Madryn, AR. Latitude of -42.77,
    Longitude of -65.04,  Where Humidity is: 78, Temperature of: 64.8,
    and Winspeed of 19.86
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=puerto madryn,ar&units=imperial
    [-42.15] [-65.26]
    
    Now retrieving weather for city # 507: Batagay, RU. Latitude of 67.65,
    Longitude of 134.64,  Where Humidity is: 72, Temperature of: -16.7,
    and Winspeed of 4.36
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=batagay,ru&units=imperial
    [ 66.02] [ 135.1]
    
    Now retrieving weather for city # 508: Bluff, NZ. Latitude of -46.6,
    Longitude of 168.33,  Where Humidity is: 86, Temperature of: 64.62,
    and Winspeed of 16.51
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bluff,nz&units=imperial
    [-87.5] [ 165.33]
    
    Now retrieving weather for city # 509: East London, ZA. Latitude of -33.02,
    Longitude of 27.91,  Where Humidity is: 100, Temperature of: 68.94,
    and Winspeed of 11.59
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=east london,za&units=imperial
    [-71.83] [ 51.55]
    
    Now retrieving weather for city # 510: San Carlos de Bariloche, AR. Latitude of -41.13,
    Longitude of -71.31,  Where Humidity is: 66, Temperature of: 53.6,
    and Winspeed of 23.04
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san carlos de bariloche,ar&units=imperial
    [-42.32] [-70.47]
    
    Now retrieving weather for city # 511: San Patricio, MX. Latitude of 19.22,
    Longitude of -104.7,  Where Humidity is: 69, Temperature of: 78.8,
    and Winspeed of 9.17
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=san patricio,mx&units=imperial
    [ 0.84] [-113.21]
    
    Now retrieving weather for city # 512: Bethel, US. Latitude of 60.79,
    Longitude of -161.76,  Where Humidity is: 72, Temperature of: 12.2,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=bethel,us&units=imperial
    [ 55.55] [-158.7]
    
    Now retrieving weather for city # 513: Yabrud, SY. Latitude of 33.97,
    Longitude of 36.66,  Where Humidity is: 87, Temperature of: 48.2,
    and Winspeed of 13.87
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=yabrud,sy&units=imperial
    [ 33.25] [ 37.41]
    
    Now retrieving weather for city # 514: Hilo, US. Latitude of 19.71,
    Longitude of -155.08,  Where Humidity is: 53, Temperature of: 67.15,
    and Winspeed of 10.29
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=hilo,us&units=imperial
    [ 16.61] [-135.28]
    
    Now retrieving weather for city # 515: Chokurdakh, RU. Latitude of 70.62,
    Longitude of 147.9,  Where Humidity is: 42, Temperature of: -26.78,
    and Winspeed of 3.13
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=chokurdakh,ru&units=imperial
    [ 88.09] [ 145.81]
    
    Now retrieving weather for city # 516: Flin Flon, CA. Latitude of 54.77,
    Longitude of -101.88,  Where Humidity is: 67, Temperature of: 23.45,
    and Winspeed of 6.89
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=flin flon,ca&units=imperial
    [ 63.15] [-102.13]
    
    Now retrieving weather for city # 517: Lufilufi, WS. Latitude of -13.87,
    Longitude of -171.6,  Where Humidity is: 66, Temperature of: 89.6,
    and Winspeed of 16.11
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=lufilufi,ws&units=imperial
    [-10.53] [-169.79]
    
    Now retrieving weather for city # 518: Cape Town, ZA. Latitude of -33.93,
    Longitude of 18.42,  Where Humidity is: 88, Temperature of: 62.6,
    and Winspeed of 6.93
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=cape town,za&units=imperial
    [-58.75] [-15.9]
    
    Now retrieving weather for city # 519: Kapaa, US. Latitude of 22.08,
    Longitude of -159.32,  Where Humidity is: 64, Temperature of: 71.6,
    and Winspeed of 18.34
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=kapaa,us&units=imperial
    [ 21.11] [-159.74]
    
    Now retrieving weather for city # 520: Tommot, RU. Latitude of 58.97,
    Longitude of 126.27,  Where Humidity is: 65, Temperature of: 13.41,
    and Winspeed of 3.31
    http://api.openweathermap.org/data/2.5/weather?appid=73b5758678e83312834aad0521ba41d5&q=tommot,ru&units=imperial
    [ 58.49] [ 127.01]
    
    CPU times: user 4.61 s, sys: 2.61 s, total: 7.22 s
    Wall time: 2min 46s



```python
weather_dict = {
    "name": names,
    "lat": latitudes,
    "lon": longitudes,
    "temp": temps,
    "clouds": clouds,
    "windspeeds":windspeeds,
    "humidity": humidities
}

```


```python
weather_data = pd.DataFrame(weather_dict)
weather_data.head(10)
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
      <th>clouds</th>
      <th>humidity</th>
      <th>lat</th>
      <th>lon</th>
      <th>name</th>
      <th>temp</th>
      <th>windspeeds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
      <td>69</td>
      <td>29.37</td>
      <td>112.41</td>
      <td>Nanzhou</td>
      <td>70.88</td>
      <td>7.16</td>
    </tr>
    <tr>
      <th>1</th>
      <td>48</td>
      <td>92</td>
      <td>69.38</td>
      <td>178.42</td>
      <td>Leningradskiy</td>
      <td>-4.28</td>
      <td>24.45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>100</td>
      <td>-22.97</td>
      <td>-42.02</td>
      <td>Arraial do Cabo</td>
      <td>74.88</td>
      <td>1.74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>75</td>
      <td>93</td>
      <td>37.72</td>
      <td>-25.43</td>
      <td>Vila Franca do Campo</td>
      <td>60.80</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>69</td>
      <td>19.22</td>
      <td>-104.70</td>
      <td>San Patricio</td>
      <td>78.80</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>5</th>
      <td>80</td>
      <td>71</td>
      <td>56.84</td>
      <td>124.90</td>
      <td>Chulman</td>
      <td>12.60</td>
      <td>3.58</td>
    </tr>
    <tr>
      <th>6</th>
      <td>88</td>
      <td>100</td>
      <td>-30.17</td>
      <td>-50.22</td>
      <td>Cidreira</td>
      <td>68.49</td>
      <td>7.61</td>
    </tr>
    <tr>
      <th>7</th>
      <td>92</td>
      <td>100</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>Atuona</td>
      <td>81.68</td>
      <td>17.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>75</td>
      <td>45</td>
      <td>65.28</td>
      <td>-126.83</td>
      <td>Norman Wells</td>
      <td>24.80</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>9</th>
      <td>80</td>
      <td>87</td>
      <td>-33.01</td>
      <td>17.94</td>
      <td>Saldanha</td>
      <td>59.00</td>
      <td>1.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.scatter(weather_data['lat'], weather_data['humidity'], marker="o", facecolors="red", edgecolors="black")
# Set the upper and lower limits of our y axis
plt.ylim(0,125)

# Set the upper and lower limits of our x axis
plt.xlim(-90,90)

# Create a title, x label, and y label for our chart
plt.title("Humidity vs Latitudes")
plt.xlabel("Latitude")
plt.ylabel("Humidity")

# Save an image of the chart and print to screen
plt.savefig("Humidity_Latitudes.png")
plt.show()
```


![png](output_4_0.png)



```python
plt.scatter(weather_data['lat'], weather_data['temp'], marker="o", facecolors="red", edgecolors="black")
# Set the upper and lower limits of our y axis
plt.ylim(-50,125)

# Set the upper and lower limits of our x axis
plt.xlim(-90,90)

# Create a title, x label, and y label for our chart
plt.title("Temps vs Latitudes")
plt.xlabel("Latitude")
plt.ylabel("Temps")

# Save an image of the chart and print to screen
plt.savefig("Temps_Latitudes.png")
plt.show()
```


![png](output_5_0.png)



```python
plt.scatter(weather_data['lat'], weather_data['clouds'], marker="o", facecolors="red", edgecolors="black")
# Set the upper and lower limits of our y axis
plt.ylim(-10,100)

# Set the upper and lower limits of our x axis
plt.xlim(-90,90)

# Create a title, x label, and y label for our chart
plt.title("Cloudiness vs Latitudes")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness")

# Save an image of the chart and print to screen
plt.savefig("Cloudiness_Latitudes.png")
plt.show()
```


![png](output_6_0.png)



```python
plt.scatter(weather_data['lat'], weather_data['windspeeds'], marker="o", facecolors="red", edgecolors="black")
# Set the upper and lower limits of our y axis
plt.ylim(0,20)

# Set the upper and lower limits of our x axis
plt.xlim(0,90)

# Create a title, x label, and y label for our chart
plt.title("Windspeeds vs Latitudes")
plt.xlabel("Latitude")
plt.ylabel("Windspeeds")

# Save an image of the chart and print to screen
plt.savefig("Windspeeds_Latitudes.png")
plt.show()
```


![png](output_7_0.png)



```python
import cartopy.crs as ccrs
```


```python

fig, ax = plt.subplots(figsize=(20,10))
ax= plt.axes(projection=ccrs.PlateCarree())
ax.stock_img()
plt.scatter(weather_data['lon'], weather_data['lat'], color='blue', marker='o',
            transform=ccrs.Geodetic(),)
plt.title("Distribution of 500+ Random Selected Cities")
plt.savefig('randomcitiesworldmap.png')
plt.show()
```


![png](output_9_0.png)



```python
weather_data.to_csv("WeatherpyData.csv", index=False)
```


```python
#observable trends:
#temperature had a 'frown' pattern ..highest temps, as you would expect
#near the equator..as you move away from the equator, to higher latitudes esp
#temps go down. Lower latitudes are cooler but we are now coming off the 
#southern hemisphere summer, and northern hemisphere winter, so south of equator is
#not as cold as north right now..
#Clouidness, Humidity and Winspeed appear to have no relationship
#with latitudes. Cloudiness has alot of 0 readings, but that is prob just
#because there are many places at any given time with no clouds
```
