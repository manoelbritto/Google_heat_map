# Project: Canada Immigration
This Project analyzed data from immigration to Canada, in this case, show us the top 3 countries that people come from. Moreover, which province they choose to leave when they land in Canada. 

# Data Set
I downloaded many data set from Open Canada (https://open.canada.ca/data/en/dataset). And they are related to:
 > Temporary Foreign Worker Program work permit holders with a valid permit on December 31st by top 50 countries of citizenship

 > Temporary Foreign Worker Program work permit holders with a valid permit on December 31st by destination¹, 2008 to 2017

 > Temporary Foreign Worker Program work permit holders who became permanent residents by program and landing year, 2008 to 2017
 
 > Canada - Permanent residents by source country, 2008 – 2017

# Prerequisites 
You need to have Python installed with Jupyter notebook in your machine and these libraries: Panda, matplotlib, requests, JSON. 

Afterward, you have to install gmaps to be accessed by jupyter notebook, and this the installation procedure https://jupyter-gmaps.readthedocs.io/en/latest/install.html

Also, I created a separated file called config.py which should be added a google API key, if you don’t have, then follow the instructions here: https://developers.google.com/places/web-service/get-api-key

# Coding tips
After your machine is ready, and you have your own API key. Then, to make this google heat map works, you can use this code which is going to extract from google geocode the latitude and longitude from each Province’s name:
```
import requests
import json
###### Google API Key
from config import gkey
from pprint import pprint
base_url = "https://maps.googleapis.com/maps/api/geocode/json"
location=[]
for city in df_canada_destination["Province"]:
    param = {"address": city, "key": gkey}
    response = requests.get(base_url, params=param).json()

# Extract lat/lng
    lat = response["results"][0]["geometry"]["location"]["lat"]
    lng = response["results"][0]["geometry"]["location"]["lng"]
    print(f"{city}: {lat}, {lng}")
    location.append([lat, lng])
```
Now you have latitude and longitude in a list (location),  then you can pass it through gmaps and call the heat map.
```
import gmaps
# Configure gmaps with API key
gmaps.configure(api_key=gkey)
len(location)

fig = gmaps.figure()

heat_layer = gmaps.heatmap_layer(location, weights=max_df_canada, 
                                 dissipating=False, max_intensity=30,
                                 point_radius = 1)

# Adjust heat_layer setting to help with heatmap dissipating on zoom
heat_layer.dissipating = False
heat_layer.max_intensity = 30
heat_layer.point_radius = 5

fig.add_layer(heat_layer)

fig
```
# Data Analysis
The first step of this analysis was to import all CSV files and create data frames for them. Then, I plotted some results, for instance:

Temporary Worker of Top 3 countries:

![GitHub Logo](/Screenshots/TemporaryWorkerTop3countries.png)

Number of immigrants moving to Canada:

![GitHub Logo](/Screenshots/NumberofimmigrantsmovingtoCanada.png)

Top 3 countries that migrate to Canada

![GitHub Logo](/Screenshots/Top3countriesthatmigratetoCanada.png)

Number of immigrants received by Province

![GitHub Logo](/Screenshots/NumberofimmigrantsreceivedbyProvince.png)

Based on the result above, I could generate the heat map based on the province location (latitude and longitude), and the number of people:

![GitHub Logo](/Screenshots/map.png)


## Features

1.	Python
2.	Libraries: 
a.	Panda
b.	Matplotlib
c. gmaps
3.	IDE - jupyter notebook
 
