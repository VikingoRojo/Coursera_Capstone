Segmenting and Clustering Neighborhoods in Toronto

Section 1:  

*Downloading and Scraping Data from Wikipedia*


```python
#Import pandas

import pandas as pd
import numpy as np
!pip install lxml
import urllib.request
```

    Requirement already satisfied: lxml in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (4.6.2)



```python
url = "https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M"

tables = pd.read_html(url)
tables[0].head()

#df = pd.read_csv(filename, names = headers)
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
      <th>Postal Code</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M2A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
  </tbody>
</table>
</div>



Adding Header Names


```python
headers = ["PostalCode","Borough","Neighbourhood"]
tables[0].columns = headers
tables[0].head(10)
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M2A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M8A</td>
      <td>Not assigned</td>
      <td>Not assigned</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M9A</td>
      <td>Etobicoke</td>
      <td>Islington Avenue, Humber Valley Village</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
    </tr>
  </tbody>
</table>
</div>



Replacing "Not assigned" with NaN


```python
df=tables[0].replace('Not assigned',np.NaN)
```


```python

df.head(5)
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1A</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M2A</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
  </tbody>
</table>
</div>



Dropping any boroughs that are not assigned and viewing the first five rows


```python
df1=df.dropna(subset=["Borough"], axis=0)
df1.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
    </tr>
  </tbody>
</table>
</div>



Viewing the last five rows


```python
df1.tail()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>160</th>
      <td>M8X</td>
      <td>Etobicoke</td>
      <td>The Kingsway, Montgomery Road, Old Mill North</td>
    </tr>
    <tr>
      <th>165</th>
      <td>M4Y</td>
      <td>Downtown Toronto</td>
      <td>Church and Wellesley</td>
    </tr>
    <tr>
      <th>168</th>
      <td>M7Y</td>
      <td>East Toronto</td>
      <td>Business reply mail Processing Centre, South C...</td>
    </tr>
    <tr>
      <th>169</th>
      <td>M8Y</td>
      <td>Etobicoke</td>
      <td>Old Mill South, King's Mill Park, Sunnylea, Hu...</td>
    </tr>
    <tr>
      <th>178</th>
      <td>M8Z</td>
      <td>Etobicoke</td>
      <td>Mimico NW, The Queensway West, South of Bloor,...</td>
    </tr>
  </tbody>
</table>
</div>



Getting the information about the dataframe


```python
df1.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 103 entries, 2 to 178
    Data columns (total 3 columns):
     #   Column         Non-Null Count  Dtype 
    ---  ------         --------------  ----- 
     0   PostalCode     103 non-null    object
     1   Borough        103 non-null    object
     2   Neighbourhood  103 non-null    object
    dtypes: object(3)
    memory usage: 3.2+ KB


Getting the data types


```python
df1.dtypes
```




    PostalCode       object
    Borough          object
    Neighbourhood    object
    dtype: object



Getting the dataframe shape


```python
df1.shape
```




    (103, 3)






Section 2: 

*Adding GeoLocation Data*


```python
url = "http://cocl.us/Geospatial_data"

df2 = pd.read_csv(url)
df2.head()

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
      <th>Postal Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
  </tbody>
</table>
</div>



Changing Header Names (Postal Code to PostalCode)


```python
headers = ["PostalCode","Latitude","Longitude"]
df2.columns = headers
df2.head(10)
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
      <th>PostalCode</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M1B</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M1C</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M1E</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M1G</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M1H</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M1J</td>
      <td>43.744734</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M1K</td>
      <td>43.727929</td>
      <td>-79.262029</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M1L</td>
      <td>43.711112</td>
      <td>-79.284577</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M1M</td>
      <td>43.716316</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M1N</td>
      <td>43.692657</td>
      <td>-79.264848</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2.dtypes
```




    PostalCode     object
    Latitude      float64
    Longitude     float64
    dtype: object




```python
df2.shape
```




    (103, 3)



Merging Dataframes


```python
# merge data frame "df" and "dummy_variable_1" 
#df_latlong = pd.concat([df1, df2], axis=1)

# drop original column "fuel-type" from "df"
#df_latlong.drop("PostalCode", axis = 1, inplace=True)


toronto_df = pd.merge(left=df1, right=df2, on='PostalCode')
toronto_df.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_df.head(15)
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
    <tr>
      <th>5</th>
      <td>M9A</td>
      <td>Etobicoke</td>
      <td>Islington Avenue, Humber Valley Village</td>
      <td>43.667856</td>
      <td>-79.532242</td>
    </tr>
    <tr>
      <th>6</th>
      <td>M1B</td>
      <td>Scarborough</td>
      <td>Malvern, Rouge</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>7</th>
      <td>M3B</td>
      <td>North York</td>
      <td>Don Mills</td>
      <td>43.745906</td>
      <td>-79.352188</td>
    </tr>
    <tr>
      <th>8</th>
      <td>M4B</td>
      <td>East York</td>
      <td>Parkview Hill, Woodbine Gardens</td>
      <td>43.706397</td>
      <td>-79.309937</td>
    </tr>
    <tr>
      <th>9</th>
      <td>M5B</td>
      <td>Downtown Toronto</td>
      <td>Garden District, Ryerson</td>
      <td>43.657162</td>
      <td>-79.378937</td>
    </tr>
    <tr>
      <th>10</th>
      <td>M6B</td>
      <td>North York</td>
      <td>Glencairn</td>
      <td>43.709577</td>
      <td>-79.445073</td>
    </tr>
    <tr>
      <th>11</th>
      <td>M9B</td>
      <td>Etobicoke</td>
      <td>West Deane Park, Princess Gardens, Martin Grov...</td>
      <td>43.650943</td>
      <td>-79.554724</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M1C</td>
      <td>Scarborough</td>
      <td>Rouge Hill, Port Union, Highland Creek</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>13</th>
      <td>M3C</td>
      <td>North York</td>
      <td>Don Mills</td>
      <td>43.725900</td>
      <td>-79.340923</td>
    </tr>
    <tr>
      <th>14</th>
      <td>M4C</td>
      <td>East York</td>
      <td>Woodbine Heights</td>
      <td>43.695344</td>
      <td>-79.318389</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_df.shape
```




    (103, 5)



Section 3:  

*Clustering Neighbourhoods*

Import Libraries


```python
import requests # library to handle requests
import random # library for random number generation


!pip install geopy
from geopy.geocoders import Nominatim # module to convert an address into latitude and longitude values

# libraries for displaying images
from IPython.display import Image 
from IPython.core.display import HTML 
    
# tranforming json file into a pandas dataframe library
from pandas.io.json import json_normalize


! pip install folium==0.5.0
import folium # plotting library

print('Folium installed')
print('Libraries imported.')


```

    Requirement already satisfied: geopy in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (2.1.0)
    Requirement already satisfied: geographiclib<2,>=1.49 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from geopy) (1.50)
    Requirement already satisfied: folium==0.5.0 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (0.5.0)
    Requirement already satisfied: requests in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from folium==0.5.0) (2.25.0)
    Requirement already satisfied: six in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from folium==0.5.0) (1.15.0)
    Requirement already satisfied: branca in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from folium==0.5.0) (0.4.1)
    Requirement already satisfied: jinja2 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from folium==0.5.0) (2.11.2)
    Requirement already satisfied: chardet<4,>=3.0.2 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from requests->folium==0.5.0) (3.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from requests->folium==0.5.0) (1.25.11)
    Requirement already satisfied: certifi>=2017.4.17 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from requests->folium==0.5.0) (2020.12.5)
    Requirement already satisfied: idna<3,>=2.5 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from requests->folium==0.5.0) (2.10)
    Requirement already satisfied: MarkupSafe>=0.23 in /home/jupyterlab/conda/envs/python/lib/python3.6/site-packages (from jinja2->folium==0.5.0) (1.1.1)
    Folium installed
    Libraries imported.



```python

pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

import json # library to handle JSON files

!conda install -c conda-forge geopy --yes # uncomment this line if you haven't completed the Foursquare API lab
from geopy.geocoders import Nominatim # convert an address into latitude and longitude values

import requests # library to handle requests
from pandas.io.json import json_normalize # tranform JSON file into a pandas dataframe

# Matplotlib and associated plotting modules
import matplotlib.cm as cm
import matplotlib.colors as colors

# import k-means from clustering stage
from sklearn.cluster import KMeans

import matplotlib.pyplot as plt # plotting library
# backend for rendering plots within the browser
%matplotlib inline 

from sklearn.datasets.samples_generator import make_blobs
print('Libraries imported.')
```

    Collecting package metadata (current_repodata.json): done
    Solving environment: done
    
    # All requested packages already installed.
    
    Libraries imported.



```python
print('The dataframe has {} boroughs and {} neighborhoods.'.format(
        len(toronto_df['Borough'].unique()),
        toronto_df.shape[0]
    )
)
```

    The dataframe has 10 boroughs and 103 neighborhoods.



```python
address = 'Toronto, ON'

geolocator = Nominatim(user_agent="TOR_explorer")
location = geolocator.geocode(address)
latitude = location.latitude
longitude = location.longitude
print('The geograpical coordinate of Toronto are {}, {}.'.format(latitude, longitude))
```

    The geograpical coordinate of Toronto are 43.6534817, -79.3839347.


Creating Map of Toronto and Surrounding Area based on Lats and Longs


```python
# create map of Toronto using latitude and longitude values
map_toronto = folium.Map(location=[latitude, longitude], zoom_start=10)

# add markers to map
for lat, lng, borough, neighbourhood in zip(toronto_df['Latitude'], toronto_df['Longitude'], toronto_df['Borough'], toronto_df['Neighbourhood']):
    label = '{}, {}'.format(neighbourhood, borough)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=label,
        color='blue',
        fill=True,
        fill_color='#3186cc',
        fill_opacity=0.7,
        parse_html=False).add_to(map_toronto)  
    
map_toronto
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3IiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNycsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUzNDgxNywtNzkuMzgzOTM0N10sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMCwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfN2E1NDM0ZGU5YmUwNDI3NTk2YWYwY2FiMzIzZjk5Y2YgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFmZWNjYWI3MTYwZjQ2NDFhZmZkMjEwMTUwNzQwNjI2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUzMjU4NiwtNzkuMzI5NjU2NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMjljODIxNDE4YWI0YWFkODUwMTMwMWQ0YzExNDg0MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lZDZjZTFiNDRhNWQ0M2Y2OTIwNmEzN2Y1ZWZmMmY5NCA9ICQoJzxkaXYgaWQ9Imh0bWxfZWQ2Y2UxYjQ0YTVkNDNmNjkyMDZhMzdmNWVmZjJmOTQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhcmt3b29kcywgTm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDI5YzgyMTQxOGFiNGFhZDg1MDEzMDFkNGMxMTQ4NDMuc2V0Q29udGVudChodG1sX2VkNmNlMWI0NGE1ZDQzZjY5MjA2YTM3ZjVlZmYyZjk0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFmZWNjYWI3MTYwZjQ2NDFhZmZkMjEwMTUwNzQwNjI2LmJpbmRQb3B1cChwb3B1cF9kMjljODIxNDE4YWI0YWFkODUwMTMwMWQ0YzExNDg0Myk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wMTZmNmJlYzE3MTM0YTUwODU2ZWY2OTA0ZDlkZjliYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNTg4MjI5OTk5OTk5NSwtNzkuMzE1NTcxNTk5OTk5OThdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMGNkOGViNWE1MTMyNDU5YTk1N2IyOGIxOGUxOWJhMWEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjJmYzA0ODA3NGY4NGQ1MzkxOWM5OTU1YjZiNzg2Y2IgPSAkKCc8ZGl2IGlkPSJodG1sX2IyZmMwNDgwNzRmODRkNTM5MTljOTk1NWI2Yjc4NmNiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5WaWN0b3JpYSBWaWxsYWdlLCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wY2Q4ZWI1YTUxMzI0NTlhOTU3YjI4YjE4ZTE5YmExYS5zZXRDb250ZW50KGh0bWxfYjJmYzA0ODA3NGY4NGQ1MzkxOWM5OTU1YjZiNzg2Y2IpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDE2ZjZiZWMxNzEzNGE1MDg1NmVmNjkwNGQ5ZGY5YmIuYmluZFBvcHVwKHBvcHVwXzBjZDhlYjVhNTEzMjQ1OWE5NTdiMjhiMThlMTliYTFhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzA2ZjE3YTQ4NzBmMDQ2NjQ4YTBjYjE1ZmQ1NDg4YzMyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU0MjU5OSwtNzkuMzYwNjM1OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83MTM5M2U3N2ViNzc0ODNkOTIwOTA3NDM4MTUzYzc4YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mYzA5NGIyMDBlMDY0OWRkOTZiMTcxZDM3YjA3ZTMwZSA9ICQoJzxkaXYgaWQ9Imh0bWxfZmMwOTRiMjAwZTA2NDlkZDk2YjE3MWQzN2IwN2UzMGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJlZ2VudCBQYXJrLCBIYXJib3VyZnJvbnQsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzcxMzkzZTc3ZWI3NzQ4M2Q5MjA5MDc0MzgxNTNjNzhiLnNldENvbnRlbnQoaHRtbF9mYzA5NGIyMDBlMDY0OWRkOTZiMTcxZDM3YjA3ZTMwZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wNmYxN2E0ODcwZjA0NjY0OGEwY2IxNWZkNTQ4OGMzMi5iaW5kUG9wdXAocG9wdXBfNzEzOTNlNzdlYjc3NDgzZDkyMDkwNzQzODE1M2M3OGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDI3MzExNmE2MGU3NDQ1N2JlNjJhMDA1ODZkM2Q0MjUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTg1MTc5OTk5OTk5OTYsLTc5LjQ2NDc2MzI5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NlZTdjOWQ5N2I3NDRlYTRiZWQ4MjNmYzI0MjA1ZjgxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2RmYjg4NTE0YTRiYjQyY2ZiYWNmNWM3YjM1ODc4ZDc1ID0gJCgnPGRpdiBpZD0iaHRtbF9kZmI4ODUxNGE0YmI0MmNmYmFjZjVjN2IzNTg3OGQ3NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TGF3cmVuY2UgTWFub3IsIExhd3JlbmNlIEhlaWdodHMsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NlZTdjOWQ5N2I3NDRlYTRiZWQ4MjNmYzI0MjA1ZjgxLnNldENvbnRlbnQoaHRtbF9kZmI4ODUxNGE0YmI0MmNmYmFjZjVjN2IzNTg3OGQ3NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kMjczMTE2YTYwZTc0NDU3YmU2MmEwMDU4NmQzZDQyNS5iaW5kUG9wdXAocG9wdXBfY2VlN2M5ZDk3Yjc0NGVhNGJlZDgyM2ZjMjQyMDVmODEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGI1MTMwYTczYjE2NDE5MDk3OTBiOWFiNWYxZWNjMWYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjIzMDE1LC03OS4zODk0OTM4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FmNjdkNzcwNDM4NDQ4ZDRiNzQ0YWUzZWYwYmM4M2ZiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU0NjAyNmE2MjlkNzQ0NGNiNTkwY2IxNzRhZTJlZmZjID0gJCgnPGRpdiBpZD0iaHRtbF81NDYwMjZhNjI5ZDc0NDRjYjU5MGNiMTc0YWUyZWZmYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UXVlZW4mIzM5O3MgUGFyaywgT250YXJpbyBQcm92aW5jaWFsIEdvdmVybm1lbnQsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2FmNjdkNzcwNDM4NDQ4ZDRiNzQ0YWUzZWYwYmM4M2ZiLnNldENvbnRlbnQoaHRtbF81NDYwMjZhNjI5ZDc0NDRjYjU5MGNiMTc0YWUyZWZmYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wYjUxMzBhNzNiMTY0MTkwOTc5MGI5YWI1ZjFlY2MxZi5iaW5kUG9wdXAocG9wdXBfYWY2N2Q3NzA0Mzg0NDhkNGI3NDRhZTNlZjBiYzgzZmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjZhNmYwOWU2YWMzNDI5ZThhOTQzMmVmMDdlODU4MjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njc4NTU2LC03OS41MzIyNDI0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kYjc4MDVlNTFhYjg0OWE3ODEzNTE3ODU5OTg5NTE3NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81NjE2MzY3NTdjMTM0ZDBkYjc5ZWE4YzY4MzJmNTA4ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNTYxNjM2NzU3YzEzNGQwZGI3OWVhOGM2ODMyZjUwOGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPklzbGluZ3RvbiBBdmVudWUsIEh1bWJlciBWYWxsZXkgVmlsbGFnZSwgRXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kYjc4MDVlNTFhYjg0OWE3ODEzNTE3ODU5OTg5NTE3Ny5zZXRDb250ZW50KGh0bWxfNTYxNjM2NzU3YzEzNGQwZGI3OWVhOGM2ODMyZjUwOGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYjZhNmYwOWU2YWMzNDI5ZThhOTQzMmVmMDdlODU4MjYuYmluZFBvcHVwKHBvcHVwX2RiNzgwNWU1MWFiODQ5YTc4MTM1MTc4NTk5ODk1MTc3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzlmMDY0YjJkN2E2NzQ3YjViYzZhZTczMTVkOWNmNWI1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuODA2Njg2Mjk5OTk5OTk2LC03OS4xOTQzNTM0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZTYxODBlMWViOGE0NGZkYmY0ZTc5NmFmOGRlY2YwNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kYTc4NmY5YWI3YWM0OTMxYTBjYmQxZjZhY2QxNzE5ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfZGE3ODZmOWFiN2FjNDkzMWEwY2JkMWY2YWNkMTcxOWUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1hbHZlcm4sIFJvdWdlLCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWU2MTgwZTFlYjhhNDRmZGJmNGU3OTZhZjhkZWNmMDQuc2V0Q29udGVudChodG1sX2RhNzg2ZjlhYjdhYzQ5MzFhMGNiZDFmNmFjZDE3MTllKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzlmMDY0YjJkN2E2NzQ3YjViYzZhZTczMTVkOWNmNWI1LmJpbmRQb3B1cChwb3B1cF9lZTYxODBlMWViOGE0NGZkYmY0ZTc5NmFmOGRlY2YwNCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80MGQ3ODUwZGRjYTM0MTYzODAwMmJjNzM1OTJhMTgxYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc0NTkwNTc5OTk5OTk5NiwtNzkuMzUyMTg4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ZjYWJkNTEzMmQ4ODQ1MWM4NjFiOGZkYWM4YjRlOTBiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzg2NzkyZTVhODEzZTRkOWU5NzFjNjY3N2RhNjMyMmI4ID0gJCgnPGRpdiBpZD0iaHRtbF84Njc5MmU1YTgxM2U0ZDllOTcxYzY2NzdkYTYzMjJiOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG9uIE1pbGxzLCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mY2FiZDUxMzJkODg0NTFjODYxYjhmZGFjOGI0ZTkwYi5zZXRDb250ZW50KGh0bWxfODY3OTJlNWE4MTNlNGQ5ZTk3MWM2Njc3ZGE2MzIyYjgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDBkNzg1MGRkY2EzNDE2MzgwMDJiYzczNTkyYTE4MWMuYmluZFBvcHVwKHBvcHVwX2ZjYWJkNTEzMmQ4ODQ1MWM4NjFiOGZkYWM4YjRlOTBiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU4YmYzMmVjZjYwNDQxYmU5NTk2Y2YxNGQ3YjM2YzA1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA2Mzk3MiwtNzkuMzA5OTM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JkYjY5ZDhiYWY0NDRhZTY4YThhN2Y1ODU3YmI5Y2I1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzM3YmIyOWY1MzlhMDQyOTA5MDNjZWMwZmVlZjkyZDQwID0gJCgnPGRpdiBpZD0iaHRtbF8zN2JiMjlmNTM5YTA0MjkwOTAzY2VjMGZlZWY5MmQ0MCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UGFya3ZpZXcgSGlsbCwgV29vZGJpbmUgR2FyZGVucywgRWFzdCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iZGI2OWQ4YmFmNDQ0YWU2OGE4YTdmNTg1N2JiOWNiNS5zZXRDb250ZW50KGh0bWxfMzdiYjI5ZjUzOWEwNDI5MDkwM2NlYzBmZWVmOTJkNDApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNThiZjMyZWNmNjA0NDFiZTk1OTZjZjE0ZDdiMzZjMDUuYmluZFBvcHVwKHBvcHVwX2JkYjY5ZDhiYWY0NDRhZTY4YThhN2Y1ODU3YmI5Y2I1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg3NGJlZWE2OGIxNzRlZjc4MDVmNmE3ODQ3MWYzMTg4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3MTYxOCwtNzkuMzc4OTM3MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2RmYjBkYzgwNTRhNDQ3NmI4NTZmZTRjNGRlN2M5ZDYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjMxYWVmZDQ2MWNlNDQxNTk4NTYyMmM2MWI4OTdmNjggPSAkKCc8ZGl2IGlkPSJodG1sX2YzMWFlZmQ0NjFjZTQ0MTU5ODU2MjJjNjFiODk3ZjY4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5HYXJkZW4gRGlzdHJpY3QsIFJ5ZXJzb24sIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NkZmIwZGM4MDU0YTQ0NzZiODU2ZmU0YzRkZTdjOWQ2LnNldENvbnRlbnQoaHRtbF9mMzFhZWZkNDYxY2U0NDE1OTg1NjIyYzYxYjg5N2Y2OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84NzRiZWVhNjhiMTc0ZWY3ODA1ZjZhNzg0NzFmMzE4OC5iaW5kUG9wdXAocG9wdXBfY2RmYjBkYzgwNTRhNDQ3NmI4NTZmZTRjNGRlN2M5ZDYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGFhNDM3Y2RlNmMxNGE2MmE0NGM2ZjVhNGE1N2Q5YjAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDk1NzcsLTc5LjQ0NTA3MjU5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzBmYzgxOWZiOWJhZTQ0YWM4Nzk2YmFhNTUzMjI3OWJlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MxNzc2ZGMwZDkwNzRmNjRiZmE2NzRmMDRhNjBkMDI2ID0gJCgnPGRpdiBpZD0iaHRtbF9jMTc3NmRjMGQ5MDc0ZjY0YmZhNjc0ZjA0YTYwZDAyNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R2xlbmNhaXJuLCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wZmM4MTlmYjliYWU0NGFjODc5NmJhYTU1MzIyNzliZS5zZXRDb250ZW50KGh0bWxfYzE3NzZkYzBkOTA3NGY2NGJmYTY3NGYwNGE2MGQwMjYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGFhNDM3Y2RlNmMxNGE2MmE0NGM2ZjVhNGE1N2Q5YjAuYmluZFBvcHVwKHBvcHVwXzBmYzgxOWZiOWJhZTQ0YWM4Nzk2YmFhNTUzMjI3OWJlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzYxN2Q4NTE1N2Y0YTRmYmFhNTA1NDM1Mzc4ZWJhNTE3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUwOTQzMiwtNzkuNTU0NzI0NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOTIzNmYxZjdhYTNjNDlhMjgzMjBmZGJkODkwMWM3M2MgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTgwZjA2NWVmMjY3NDM5ZTg1MTliNDJlY2JkYzQ0NDUgPSAkKCc8ZGl2IGlkPSJodG1sXzk4MGYwNjVlZjI2NzQzOWU4NTE5YjQyZWNiZGM0NDQ1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0IERlYW5lIFBhcmssIFByaW5jZXNzIEdhcmRlbnMsIE1hcnRpbiBHcm92ZSwgSXNsaW5ndG9uLCBDbG92ZXJkYWxlLCBFdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzkyMzZmMWY3YWEzYzQ5YTI4MzIwZmRiZDg5MDFjNzNjLnNldENvbnRlbnQoaHRtbF85ODBmMDY1ZWYyNjc0MzllODUxOWI0MmVjYmRjNDQ0NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82MTdkODUxNTdmNGE0ZmJhYTUwNTQzNTM3OGViYTUxNy5iaW5kUG9wdXAocG9wdXBfOTIzNmYxZjdhYTNjNDlhMjgzMjBmZGJkODkwMWM3M2MpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmEzMGIwYmUyZDJlNGQxOGJkZDk0Y2IxMmY4MjNhNGMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43ODQ1MzUxLC03OS4xNjA0OTcwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jMTQwNTc1YzZhY2U0NDgzOTUwZmE5OWVmMzQyMmNiNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wYmI4OGZlODI1NmU0NzQ1YTU0NmIyODk4OGY1NjIyMyA9ICQoJzxkaXYgaWQ9Imh0bWxfMGJiODhmZTgyNTZlNDc0NWE1NDZiMjg5ODhmNTYyMjMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJvdWdlIEhpbGwsIFBvcnQgVW5pb24sIEhpZ2hsYW5kIENyZWVrLCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYzE0MDU3NWM2YWNlNDQ4Mzk1MGZhOTllZjM0MjJjYjcuc2V0Q29udGVudChodG1sXzBiYjg4ZmU4MjU2ZTQ3NDVhNTQ2YjI4OTg4ZjU2MjIzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJhMzBiMGJlMmQyZTRkMThiZGQ5NGNiMTJmODIzYTRjLmJpbmRQb3B1cChwb3B1cF9jMTQwNTc1YzZhY2U0NDgzOTUwZmE5OWVmMzQyMmNiNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lYWY1N2VmOGUxZmY0MjI5ODBkNDM0NjM4YTlkZjc2YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNTg5OTcwMDAwMDAxLC03OS4zNDA5MjNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDVjMGNjMjVmYTdhNGEzNjgyNGZiNzc0OGM1YjQ3MGQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzZhZTM0MWMwZTExNGU3ZWIwNzcwZTQ1MTYwM2I5NzMgPSAkKCc8ZGl2IGlkPSJodG1sXzM2YWUzNDFjMGUxMTRlN2ViMDc3MGU0NTE2MDNiOTczIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb24gTWlsbHMsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ1YzBjYzI1ZmE3YTRhMzY4MjRmYjc3NDhjNWI0NzBkLnNldENvbnRlbnQoaHRtbF8zNmFlMzQxYzBlMTE0ZTdlYjA3NzBlNDUxNjAzYjk3Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lYWY1N2VmOGUxZmY0MjI5ODBkNDM0NjM4YTlkZjc2YS5iaW5kUG9wdXAocG9wdXBfNDVjMGNjMjVmYTdhNGEzNjgyNGZiNzc0OGM1YjQ3MGQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmRmM2Q5MDY4N2IzNDkwMTk3Njc1MTFhODljYThkYzMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTUzNDM5MDAwMDAwMDUsLTc5LjMxODM4ODddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2Y4MzI4MTkxZDZjNDJhZjhmYTFiY2M5NzkwZDZhOGEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMmJkYjVkODhjYmI1NDE3YmE0ZWNmMjIzMThlYzlmYWIgPSAkKCc8ZGl2IGlkPSJodG1sXzJiZGI1ZDg4Y2JiNTQxN2JhNGVjZjIyMzE4ZWM5ZmFiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Xb29kYmluZSBIZWlnaHRzLCBFYXN0IFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NmODMyODE5MWQ2YzQyYWY4ZmExYmNjOTc5MGQ2YThhLnNldENvbnRlbnQoaHRtbF8yYmRiNWQ4OGNiYjU0MTdiYTRlY2YyMjMxOGVjOWZhYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iZGYzZDkwNjg3YjM0OTAxOTc2NzUxMWE4OWNhOGRjMy5iaW5kUG9wdXAocG9wdXBfY2Y4MzI4MTkxZDZjNDJhZjhmYTFiY2M5NzkwZDZhOGEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfM2VjMzZiYWNmMDhkNGEwYmEwMWFhZGNiNjZlODJkNDAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTE0OTM5LC03OS4zNzU0MTc5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2UxYjIzNjNmZDhkYTRiZTY5MDNiOTU2NjJkMDBjZGFjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU1MWIzYWI4MGI1ZTQ1MGM4MWMwZWM0MDMwODc5YWM0ID0gJCgnPGRpdiBpZD0iaHRtbF81NTFiM2FiODBiNWU0NTBjODFjMGVjNDAzMDg3OWFjNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QuIEphbWVzIFRvd24sIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2UxYjIzNjNmZDhkYTRiZTY5MDNiOTU2NjJkMDBjZGFjLnNldENvbnRlbnQoaHRtbF81NTFiM2FiODBiNWU0NTBjODFjMGVjNDAzMDg3OWFjNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zZWMzNmJhY2YwOGQ0YTBiYTAxYWFkY2I2NmU4MmQ0MC5iaW5kUG9wdXAocG9wdXBfZTFiMjM2M2ZkOGRhNGJlNjkwM2I5NTY2MmQwMGNkYWMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzIzN2NhYzQ1ZjU3NDg0MWE2YzhkZjRhMzhlNjYzZGIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTM3ODEzLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kODg5NTczMjRkOTk0Yzc1YjQ4ZDIwZmVkN2NkMjM2NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMzQwMDZmMTFkYzI0ZGM5OTdkYzZjYzdiOGJiNmQ0ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfYTM0MDA2ZjExZGMyNGRjOTk3ZGM2Y2M3YjhiYjZkNGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkh1bWV3b29kLUNlZGFydmFsZSwgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDg4OTU3MzI0ZDk5NGM3NWI0OGQyMGZlZDdjZDIzNjcuc2V0Q29udGVudChodG1sX2EzNDAwNmYxMWRjMjRkYzk5N2RjNmNjN2I4YmI2ZDRmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzcyMzdjYWM0NWY1NzQ4NDFhNmM4ZGY0YTM4ZTY2M2RiLmJpbmRQb3B1cChwb3B1cF9kODg5NTczMjRkOTk0Yzc1YjQ4ZDIwZmVkN2NkMjM2Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wODI0MjFlNmY5NWQ0YmNmYWYyNDFhMGFhMGZjZDgxZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0MzUxNTIsLTc5LjU3NzIwMDc5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2MzNGE2NDIwY2MxNDQ3OGJhMzQ0YzA5ZmIxYzAzMGNjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzBjZTczNTYxNjNkZjQ0MDE4MmZjNGFjMjRkMGMzNzdkID0gJCgnPGRpdiBpZD0iaHRtbF8wY2U3MzU2MTYzZGY0NDAxODJmYzRhYzI0ZDBjMzc3ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RXJpbmdhdGUsIEJsb29yZGFsZSBHYXJkZW5zLCBPbGQgQnVybmhhbXRob3JwZSwgTWFya2xhbmQgV29vZCwgRXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jMzRhNjQyMGNjMTQ0NzhiYTM0NGMwOWZiMWMwMzBjYy5zZXRDb250ZW50KGh0bWxfMGNlNzM1NjE2M2RmNDQwMTgyZmM0YWMyNGQwYzM3N2QpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDgyNDIxZTZmOTVkNGJjZmFmMjQxYTBhYTBmY2Q4MWYuYmluZFBvcHVwKHBvcHVwX2MzNGE2NDIwY2MxNDQ3OGJhMzQ0YzA5ZmIxYzAzMGNjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzlkYTNkOWNkOGIyOTQ4MzQ4MTUzYTZmNjBjMWRlZjQ5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzYzNTcyNiwtNzkuMTg4NzExNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zYzVkMTAyZTlkZTA0MzI4ODNkYWY2NjJjNGU3ODRhMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84YzM5NWRkMWU5MTg0MTYwYjRjM2U1MTM2ODNmZmQyNyA9ICQoJzxkaXYgaWQ9Imh0bWxfOGMzOTVkZDFlOTE4NDE2MGI0YzNlNTEzNjgzZmZkMjciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkd1aWxkd29vZCwgTW9ybmluZ3NpZGUsIFdlc3QgSGlsbCwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzNjNWQxMDJlOWRlMDQzMjg4M2RhZjY2MmM0ZTc4NGEyLnNldENvbnRlbnQoaHRtbF84YzM5NWRkMWU5MTg0MTYwYjRjM2U1MTM2ODNmZmQyNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ZGEzZDljZDhiMjk0ODM0ODE1M2E2ZjYwYzFkZWY0OS5iaW5kUG9wdXAocG9wdXBfM2M1ZDEwMmU5ZGUwNDMyODgzZGFmNjYyYzRlNzg0YTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzFjZTA2NDM4NzUxNGZkOTg3MmE1MzczZDRlODdmMWMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NzYzNTczOTk5OTk5OSwtNzkuMjkzMDMxMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hMzJmYjQ0YjYzNTA0YzA2ODM4NTg5M2ZhYzZhYTIwMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80YjAyYWYxMGZjYzk0YjkwYmZjMDBmZTMzN2E4MTg2ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNGIwMmFmMTBmY2M5NGI5MGJmYzAwZmUzMzdhODE4NmUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBCZWFjaGVzLCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2EzMmZiNDRiNjM1MDRjMDY4Mzg1ODkzZmFjNmFhMjAwLnNldENvbnRlbnQoaHRtbF80YjAyYWYxMGZjYzk0YjkwYmZjMDBmZTMzN2E4MTg2ZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zMWNlMDY0Mzg3NTE0ZmQ5ODcyYTUzNzNkNGU4N2YxYy5iaW5kUG9wdXAocG9wdXBfYTMyZmI0NGI2MzUwNGMwNjgzODU4OTNmYWM2YWEyMDApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjRhNTZkMThhZWJlNDU0Yzk0MzFiYjM4NDk5ODU0NmYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDQ3NzA3OTk5OTk5OTYsLTc5LjM3MzMwNjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjhmMTE3NzM2MWEzNGJlNTliYTY1OTY3MWUwNGZkYTEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzg2ODBlOGVkNjc2NGVmYmExMWFkYWU1MWE5NmJhODEgPSAkKCc8ZGl2IGlkPSJodG1sXzc4NjgwZThlZDY3NjRlZmJhMTFhZGFlNTFhOTZiYTgxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CZXJjenkgUGFyaywgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjhmMTE3NzM2MWEzNGJlNTliYTY1OTY3MWUwNGZkYTEuc2V0Q29udGVudChodG1sXzc4NjgwZThlZDY3NjRlZmJhMTFhZGFlNTFhOTZiYTgxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y0YTU2ZDE4YWViZTQ1NGM5NDMxYmIzODQ5OTg1NDZmLmJpbmRQb3B1cChwb3B1cF9mOGYxMTc3MzYxYTM0YmU1OWJhNjU5NjcxZTA0ZmRhMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mMWM2YmRiM2UzNjY0YzQ3OGVkODEyNjljYzNlN2Q0MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4OTAyNTYsLTc5LjQ1MzUxMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wYzM1YmNiY2VkMTk0NzM5YTE2YTBiNTY5NWM1YmI1ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MDNiZmFlMzRlYTA0OGQ0ODRkYmI1ODFiMjJiNmU2ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNzAzYmZhZTM0ZWEwNDhkNDg0ZGJiNTgxYjIyYjZlNmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNhbGVkb25pYS1GYWlyYmFua3MsIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzBjMzViY2JjZWQxOTQ3MzlhMTZhMGI1Njk1YzViYjVmLnNldENvbnRlbnQoaHRtbF83MDNiZmFlMzRlYTA0OGQ0ODRkYmI1ODFiMjJiNmU2ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mMWM2YmRiM2UzNjY0YzQ3OGVkODEyNjljYzNlN2Q0My5iaW5kUG9wdXAocG9wdXBfMGMzNWJjYmNlZDE5NDczOWExNmEwYjU2OTVjNWJiNWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzEzMjU5YmQ5Y2NlNDAzZmJhNjg2YzEzNDBiODFmZjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NzA5OTIxLC03OS4yMTY5MTc0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMDVhNGM5YTRhYTk0NTNjYTk2ZjIyZjFiOGUwY2EzNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ZmI5OGRjYTM4MDA0NmQwOGUwMDE3NjQwYjQ1ZWI5MyA9ICQoJzxkaXYgaWQ9Imh0bWxfN2ZiOThkY2EzODAwNDZkMDhlMDAxNzY0MGI0NWViOTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldvYnVybiwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2QwNWE0YzlhNGFhOTQ1M2NhOTZmMjJmMWI4ZTBjYTM1LnNldENvbnRlbnQoaHRtbF83ZmI5OGRjYTM4MDA0NmQwOGUwMDE3NjQwYjQ1ZWI5Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zMTMyNTliZDljY2U0MDNmYmE2ODZjMTM0MGI4MWZmNC5iaW5kUG9wdXAocG9wdXBfZDA1YTRjOWE0YWE5NDUzY2E5NmYyMmYxYjhlMGNhMzUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODA2YTljOTM4YjJkNDZiNDlhYWMyY2U3ZGU5YTdhZjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDkwNjA0LC03OS4zNjM0NTE3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NjNDBkZmQ2MjBhMjQyYjk4MTRhYWIyODQ4MWUwYzgzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgzZWU5OGFmMzFmODRiNDY4MGM5MWRjOWNlZjY2YTgzID0gJCgnPGRpdiBpZD0iaHRtbF84M2VlOThhZjMxZjg0YjQ2ODBjOTFkYzljZWY2NmE4MyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TGVhc2lkZSwgRWFzdCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jYzQwZGZkNjIwYTI0MmI5ODE0YWFiMjg0ODFlMGM4My5zZXRDb250ZW50KGh0bWxfODNlZTk4YWYzMWY4NGI0NjgwYzkxZGM5Y2VmNjZhODMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODA2YTljOTM4YjJkNDZiNDlhYWMyY2U3ZGU5YTdhZjQuYmluZFBvcHVwKHBvcHVwX2NjNDBkZmQ2MjBhMjQyYjk4MTRhYWIyODQ4MWUwYzgzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q4MGY1MDJjZTFiMzQzODJiMGM2Zjk5YjUxYzE1Y2Q3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3OTUyNCwtNzkuMzg3MzgyNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81MTgwZWJkYmJhMDU0NDEyODUzMWZmMDRkZDM1MTUxZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85ODY2ODNmMmRlZDc0NmVjOTAzZmJkZGM2MWE4MjQxMiA9ICQoJzxkaXYgaWQ9Imh0bWxfOTg2NjgzZjJkZWQ3NDZlYzkwM2ZiZGRjNjFhODI0MTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgQmF5IFN0cmVldCwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTE4MGViZGJiYTA1NDQxMjg1MzFmZjA0ZGQzNTE1MWQuc2V0Q29udGVudChodG1sXzk4NjY4M2YyZGVkNzQ2ZWM5MDNmYmRkYzYxYTgyNDEyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Q4MGY1MDJjZTFiMzQzODJiMGM2Zjk5YjUxYzE1Y2Q3LmJpbmRQb3B1cChwb3B1cF81MTgwZWJkYmJhMDU0NDEyODUzMWZmMDRkZDM1MTUxZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83MWZhN2ZjYzNjNDQ0MzY3OWQxMmEwMmFkMmFhNWM2MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2OTU0MiwtNzkuNDIyNTYzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NGI2Njc5Y2VhZmE0OWYzYjNlNTViOTQ5MDI3YzAzMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NGFjZmNiNTNjYzY0N2ZjYjBhMTc2ZmZjYmM2MmQ4ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNzRhY2ZjYjUzY2M2NDdmY2IwYTE3NmZmY2JjNjJkOGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNocmlzdGllLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80NGI2Njc5Y2VhZmE0OWYzYjNlNTViOTQ5MDI3YzAzMy5zZXRDb250ZW50KGh0bWxfNzRhY2ZjYjUzY2M2NDdmY2IwYTE3NmZmY2JjNjJkOGUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzFmYTdmY2MzYzQ0NDM2NzlkMTJhMDJhZDJhYTVjNjMuYmluZFBvcHVwKHBvcHVwXzQ0YjY2NzljZWFmYTQ5ZjNiM2U1NWI5NDkwMjdjMDMzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFiMDBjOWNlM2EwYzQ5NTA4NDI0NTU1ZDFkODM1NzVmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzczMTM2LC03OS4yMzk0NzYwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wMjEyN2I3MDQwZjE0NzFlODZiYzY1YmFhODA4NzlmMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85ZWIwYTg3ODQ1Y2I0MjAyYjQ2YWE1ZjNlNGY0OTQ4OSA9ICQoJzxkaXYgaWQ9Imh0bWxfOWViMGE4Nzg0NWNiNDIwMmI0NmFhNWYzZTRmNDk0ODkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlZGFyYnJhZSwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzAyMTI3YjcwNDBmMTQ3MWU4NmJjNjViYWE4MDg3OWYxLnNldENvbnRlbnQoaHRtbF85ZWIwYTg3ODQ1Y2I0MjAyYjQ2YWE1ZjNlNGY0OTQ4OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xYjAwYzljZTNhMGM0OTUwODQyNDU1NWQxZDgzNTc1Zi5iaW5kUG9wdXAocG9wdXBfMDIxMjdiNzA0MGYxNDcxZTg2YmM2NWJhYTgwODc5ZjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWQ2NGYwZjQ4NGY4NDYwYjhhOWQwYjA3NzdhMDMzYzAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MDM3NjIyLC03OS4zNjM0NTE3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVmOGE5MmFlY2Y0ODRjYmVhNTM2YTQxMTAwY2Y1MjhmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JhZTgyNTE2MDBhNjRiYWE5OTUyN2M1NGNlNGQxNTkxID0gJCgnPGRpdiBpZD0iaHRtbF9iYWU4MjUxNjAwYTY0YmFhOTk1MjdjNTRjZTRkMTU5MSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGlsbGNyZXN0IFZpbGxhZ2UsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVmOGE5MmFlY2Y0ODRjYmVhNTM2YTQxMTAwY2Y1MjhmLnNldENvbnRlbnQoaHRtbF9iYWU4MjUxNjAwYTY0YmFhOTk1MjdjNTRjZTRkMTU5MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZDY0ZjBmNDg0Zjg0NjBiOGE5ZDBiMDc3N2EwMzNjMC5iaW5kUG9wdXAocG9wdXBfNWY4YTkyYWVjZjQ4NGNiZWE1MzZhNDExMDBjZjUyOGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGE0ZTQ1M2FmZmFiNDhiOGFiMWRhNTc1ZmYyYTY1MmQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTQzMjgzLC03OS40NDIyNTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2M5MTAzMTNmZTEyZTQ1MjJhMTFhYzJiNWY0NGQ3NTY3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QyYzQ0ZDQzMjZkZDQxMGRiMjBkZjMyN2I5OTUwYTA3ID0gJCgnPGRpdiBpZD0iaHRtbF9kMmM0NGQ0MzI2ZGQ0MTBkYjIwZGYzMjdiOTk1MGEwNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmF0aHVyc3QgTWFub3IsIFdpbHNvbiBIZWlnaHRzLCBEb3duc3ZpZXcgTm9ydGgsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M5MTAzMTNmZTEyZTQ1MjJhMTFhYzJiNWY0NGQ3NTY3LnNldENvbnRlbnQoaHRtbF9kMmM0NGQ0MzI2ZGQ0MTBkYjIwZGYzMjdiOTk1MGEwNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84YTRlNDUzYWZmYWI0OGI4YWIxZGE1NzVmZjJhNjUyZC5iaW5kUG9wdXAocG9wdXBfYzkxMDMxM2ZlMTJlNDUyMmExMWFjMmI1ZjQ0ZDc1NjcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZWUxODRmYzllOTI0NDA4YTgwYjEzZjZjNTI2YWM1YjkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDUzNjg5LC03OS4zNDkzNzE5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80YzVjNWJhNDQxY2E0ZmJhODEzNzcxMDBiMWFjNjBiNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iYTQwNzY0NWYzMDY0MDM5ODAyNjkxYWMxNmQzZjgxNyA9ICQoJzxkaXYgaWQ9Imh0bWxfYmE0MDc2NDVmMzA2NDAzOTgwMjY5MWFjMTZkM2Y4MTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRob3JuY2xpZmZlIFBhcmssIEVhc3QgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNGM1YzViYTQ0MWNhNGZiYTgxMzc3MTAwYjFhYzYwYjYuc2V0Q29udGVudChodG1sX2JhNDA3NjQ1ZjMwNjQwMzk4MDI2OTFhYzE2ZDNmODE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2VlMTg0ZmM5ZTkyNDQwOGE4MGIxM2Y2YzUyNmFjNWI5LmJpbmRQb3B1cChwb3B1cF80YzVjNWJhNDQxY2E0ZmJhODEzNzcxMDBiMWFjNjBiNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85OWIwZTQ1YmEwOTM0YjQ0YjQ2OWQ3MGRjMzUwNWNlZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MDU3MTIwMDAwMDAxLC03OS4zODQ1Njc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzczYjc4ZjUzZWRlYTQ5ODFhYjUzMDc3NzNjNWJlODVjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JiNGU2Y2JjZTlhYjQwYTdiMjljNmU1NTkzYmYwMTM0ID0gJCgnPGRpdiBpZD0iaHRtbF9iYjRlNmNiY2U5YWI0MGE3YjI5YzZlNTU5M2JmMDEzNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UmljaG1vbmQsIEFkZWxhaWRlLCBLaW5nLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83M2I3OGY1M2VkZWE0OTgxYWI1MzA3NzczYzViZTg1Yy5zZXRDb250ZW50KGh0bWxfYmI0ZTZjYmNlOWFiNDBhN2IyOWM2ZTU1OTNiZjAxMzQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTliMGU0NWJhMDkzNGI0NGI0NjlkNzBkYzM1MDVjZWQuYmluZFBvcHVwKHBvcHVwXzczYjc4ZjUzZWRlYTQ5ODFhYjUzMDc3NzNjNWJlODVjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU1MjkwMWU5ODQ4YTQ1MTk4OGZmMTNkN2FkZGZhMzNlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY5MDA1MTAwMDAwMDEsLTc5LjQ0MjI1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDFiN2RmMWMxODQ5NDViNGI5YmNhNjY0ODRlOWM2YWQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjU0MDlhZmJjZWZhNDA2ZmI4ZjdkODg0NjkwYTBhNDcgPSAkKCc8ZGl2IGlkPSJodG1sXzI1NDA5YWZiY2VmYTQwNmZiOGY3ZDg4NDY5MGEwYTQ3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5EdWZmZXJpbiwgRG92ZXJjb3VydCBWaWxsYWdlLCBXZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzAxYjdkZjFjMTg0OTQ1YjRiOWJjYTY2NDg0ZTljNmFkLnNldENvbnRlbnQoaHRtbF8yNTQwOWFmYmNlZmE0MDZmYjhmN2Q4ODQ2OTBhMGE0Nyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81NTI5MDFlOTg0OGE0NTE5ODhmZjEzZDdhZGRmYTMzZS5iaW5kUG9wdXAocG9wdXBfMDFiN2RmMWMxODQ5NDViNGI5YmNhNjY0ODRlOWM2YWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzBiNjdlMGE4MGZlNDQxMzljZmZhZjljMWIzYzE3ZTkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NDQ3MzQyLC03OS4yMzk0NzYwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85NTlkYzZiYmYwY2E0MWE5OWU2YjIyNjYzNmRlZDg1OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yNmE3NDg0ZTNkNzU0YTI3YTI2ZmE2ZmM2N2RiZjg3MyA9ICQoJzxkaXYgaWQ9Imh0bWxfMjZhNzQ4NGUzZDc1NGEyN2EyNmZhNmZjNjdkYmY4NzMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoIFZpbGxhZ2UsIFNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85NTlkYzZiYmYwY2E0MWE5OWU2YjIyNjYzNmRlZDg1OS5zZXRDb250ZW50KGh0bWxfMjZhNzQ4NGUzZDc1NGEyN2EyNmZhNmZjNjdkYmY4NzMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzBiNjdlMGE4MGZlNDQxMzljZmZhZjljMWIzYzE3ZTkuYmluZFBvcHVwKHBvcHVwXzk1OWRjNmJiZjBjYTQxYTk5ZTZiMjI2NjM2ZGVkODU5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2M0M2JhMzhiMzk3NjRkMDRhMjIyMTY5ZmUzZWQ3NWVhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzc4NTE3NSwtNzkuMzQ2NTU1N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kNmIzY2ZhM2QzMWU0NGEwYjM1NWFkMWZmNDI4ZGQyYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lNjFlZDlkZTJhNzI0OWYxODhjMDdkMjNjN2JmN2JhNiA9ICQoJzxkaXYgaWQ9Imh0bWxfZTYxZWQ5ZGUyYTcyNDlmMTg4YzA3ZDIzYzdiZjdiYTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZhaXJ2aWV3LCBIZW5yeSBGYXJtLCBPcmlvbGUsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q2YjNjZmEzZDMxZTQ0YTBiMzU1YWQxZmY0MjhkZDJjLnNldENvbnRlbnQoaHRtbF9lNjFlZDlkZTJhNzI0OWYxODhjMDdkMjNjN2JmN2JhNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jNDNiYTM4YjM5NzY0ZDA0YTIyMjE2OWZlM2VkNzVlYS5iaW5kUG9wdXAocG9wdXBfZDZiM2NmYTNkMzFlNDRhMGIzNTVhZDFmZjQyOGRkMmMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWNlYmNiNzk1YTM1NGRhZWI1OGYxZDhhZWQ0YWMyYjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43Njc5ODAzLC03OS40ODcyNjE5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wZGE2YWM5NmQzMjM0MjIxOGM3MTkwZDdkN2E1NzljZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hNDlhZWYzNjlkMzg0NzJlYjdjMjIyNmY3OWI4ZDkzYSA9ICQoJzxkaXYgaWQ9Imh0bWxfYTQ5YWVmMzY5ZDM4NDcyZWI3YzIyMjZmNzliOGQ5M2EiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRod29vZCBQYXJrLCBZb3JrIFVuaXZlcnNpdHksIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzBkYTZhYzk2ZDMyMzQyMjE4YzcxOTBkN2Q3YTU3OWNmLnNldENvbnRlbnQoaHRtbF9hNDlhZWYzNjlkMzg0NzJlYjdjMjIyNmY3OWI4ZDkzYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hY2ViY2I3OTVhMzU0ZGFlYjU4ZjFkOGFlZDRhYzJiNC5iaW5kUG9wdXAocG9wdXBfMGRhNmFjOTZkMzIzNDIyMThjNzE5MGQ3ZDdhNTc5Y2YpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjdjYWE4Y2E5YmYxNDk1Y2FhMDkyZjdlMmU1ZTcxYzUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODUzNDcsLTc5LjMzODEwNjVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTRjMDEyYTFkZjI2NGJlNzk4OTc0NDU5NGI3ZjZmZDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWQzMjYyY2NhNWIwNGI3OWI2NGMyMmNkODEzZDE5MzQgPSAkKCc8ZGl2IGlkPSJodG1sXzFkMzI2MmNjYTViMDRiNzliNjRjMjJjZDgxM2QxOTM0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG8sIEJyb2FkdmlldyBOb3J0aCAoT2xkIEVhc3QgWW9yayksIEVhc3QgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTRjMDEyYTFkZjI2NGJlNzk4OTc0NDU5NGI3ZjZmZDguc2V0Q29udGVudChodG1sXzFkMzI2MmNjYTViMDRiNzliNjRjMjJjZDgxM2QxOTM0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y3Y2FhOGNhOWJmMTQ5NWNhYTA5MmY3ZTJlNWU3MWM1LmJpbmRQb3B1cChwb3B1cF81NGMwMTJhMWRmMjY0YmU3OTg5NzQ0NTk0YjdmNmZkOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85ZmU0ZTQ2ZGMyNjQ0Y2NiYmZiMWFlMWRiMDIwYWEwMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0MDgxNTcsLTc5LjM4MTc1MjI5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzhlYWRkYzU1MWNkYjQ1YTM4NWMyMmFmOGUwN2QzOWZhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzNmMDZmZTBjYzcwNDRkZWU5ZTBjMDQxODg3Y2I4MTQ1ID0gJCgnPGRpdiBpZD0iaHRtbF8zZjA2ZmUwY2M3MDQ0ZGVlOWUwYzA0MTg4N2NiODE0NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGFyYm91cmZyb250IEVhc3QsIFVuaW9uIFN0YXRpb24sIFRvcm9udG8gSXNsYW5kcywgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGVhZGRjNTUxY2RiNDVhMzg1YzIyYWY4ZTA3ZDM5ZmEuc2V0Q29udGVudChodG1sXzNmMDZmZTBjYzcwNDRkZWU5ZTBjMDQxODg3Y2I4MTQ1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzlmZTRlNDZkYzI2NDRjY2JiZmIxYWUxZGIwMjBhYTAxLmJpbmRQb3B1cChwb3B1cF84ZWFkZGM1NTFjZGI0NWEzODVjMjJhZjhlMDdkMzlmYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wMDk0MDFkYjQ4YWY0OTYxYjI2N2Q5ZDczNzIwOGNjNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NzkyNjcwMDAwMDAwNiwtNzkuNDE5NzQ5N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jY2QyZTExNmU5ZDE0NGJhYTA0MTMyZjdkMGFkMTNhZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kMjg4ZjE0OTAwNjQ0ZjQ2OGY3YjYzNjdkMjA5NzBkZSA9ICQoJzxkaXYgaWQ9Imh0bWxfZDI4OGYxNDkwMDY0NGY0NjhmN2I2MzY3ZDIwOTcwZGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxpdHRsZSBQb3J0dWdhbCwgVHJpbml0eSwgV2VzdCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jY2QyZTExNmU5ZDE0NGJhYTA0MTMyZjdkMGFkMTNhZi5zZXRDb250ZW50KGh0bWxfZDI4OGYxNDkwMDY0NGY0NjhmN2I2MzY3ZDIwOTcwZGUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDA5NDAxZGI0OGFmNDk2MWIyNjdkOWQ3MzcyMDhjYzQuYmluZFBvcHVwKHBvcHVwX2NjZDJlMTE2ZTlkMTQ0YmFhMDQxMzJmN2QwYWQxM2FmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2YyNWZmMWViODJmOTRjZThhY2FhYjBiOTBhNTA5ZWExID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzI3OTI5MiwtNzkuMjYyMDI5NDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmFmYzIyMTU3NGRjNDk5YjkyMzJjNmUwNzMwYzIwMTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjVmMzYwYTY4YjdhNDBmZTgyNTNmMzA0M2ExOTAxM2UgPSAkKCc8ZGl2IGlkPSJodG1sX2Y1ZjM2MGE2OGI3YTQwZmU4MjUzZjMwNDNhMTkwMTNlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5LZW5uZWR5IFBhcmssIElvbnZpZXcsIEVhc3QgQmlyY2htb3VudCBQYXJrLCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmFmYzIyMTU3NGRjNDk5YjkyMzJjNmUwNzMwYzIwMTYuc2V0Q29udGVudChodG1sX2Y1ZjM2MGE2OGI3YTQwZmU4MjUzZjMwNDNhMTkwMTNlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2YyNWZmMWViODJmOTRjZThhY2FhYjBiOTBhNTA5ZWExLmJpbmRQb3B1cChwb3B1cF8yYWZjMjIxNTc0ZGM0OTliOTIzMmM2ZTA3MzBjMjAxNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iNmYxMzg5ZWUyYzk0NDIzODc2ODE2MjBmZWQ3NGY0OCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4Njk0NzMsLTc5LjM4NTk3NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81OTUyODk2MzBhNjA0NTVkODRmZjg3NDkzMTQ2ZjRmNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80YTM3MWI3ZTcyYjQ0YTE4YWFmNmQxOWEyOTM2NTUxMyA9ICQoJzxkaXYgaWQ9Imh0bWxfNGEzNzFiN2U3MmI0NGExOGFhZjZkMTlhMjkzNjU1MTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJheXZpZXcgVmlsbGFnZSwgTm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTk1Mjg5NjMwYTYwNDU1ZDg0ZmY4NzQ5MzE0NmY0ZjYuc2V0Q29udGVudChodG1sXzRhMzcxYjdlNzJiNDRhMThhYWY2ZDE5YTI5MzY1NTEzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2I2ZjEzODllZTJjOTQ0MjM4NzY4MTYyMGZlZDc0ZjQ4LmJpbmRQb3B1cChwb3B1cF81OTUyODk2MzBhNjA0NTVkODRmZjg3NDkzMTQ2ZjRmNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83ZmM5YmI2YjhmYWE0MzY1YmFjZjM4ODZkYzFhMmQ2ZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjczNzQ3MzIwMDAwMDAwNCwtNzkuNDY0NzYzMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZGIzMWNlZTZjNDAxNDZlMjk4MTU1MjhkZGE2M2EyM2IgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTYyZjRkNGIzMTk0NGYyOGJjNzQ5M2RhOTk3MDNiNGIgPSAkKCc8ZGl2IGlkPSJodG1sXzk2MmY0ZDRiMzE5NDRmMjhiYzc0OTNkYTk5NzAzYjRiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3duc3ZpZXcsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2RiMzFjZWU2YzQwMTQ2ZTI5ODE1NTI4ZGRhNjNhMjNiLnNldENvbnRlbnQoaHRtbF85NjJmNGQ0YjMxOTQ0ZjI4YmM3NDkzZGE5OTcwM2I0Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83ZmM5YmI2YjhmYWE0MzY1YmFjZjM4ODZkYzFhMmQ2Zi5iaW5kUG9wdXAocG9wdXBfZGIzMWNlZTZjNDAxNDZlMjk4MTU1MjhkZGE2M2EyM2IpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjlmN2JiZWYxYWNkNDkyMDkxY2FjMTZhNGMwNWI1NWQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NTcxLC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmZiZTVhZThjYzAxNDE3ZWFiZjgyNmUxMjA1N2Q0NzkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTkyYTgzMTVhM2I3NDliMGE3MmNkZDRhZTJhOGE3MTMgPSAkKCc8ZGl2IGlkPSJodG1sX2E5MmE4MzE1YTNiNzQ5YjBhNzJjZGQ0YWUyYThhNzEzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgRGFuZm9ydGggV2VzdCwgUml2ZXJkYWxlLCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2ZmYmU1YWU4Y2MwMTQxN2VhYmY4MjZlMTIwNTdkNDc5LnNldENvbnRlbnQoaHRtbF9hOTJhODMxNWEzYjc0OWIwYTcyY2RkNGFlMmE4YTcxMyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82OWY3YmJlZjFhY2Q0OTIwOTFjYWMxNmE0YzA1YjU1ZC5iaW5kUG9wdXAocG9wdXBfZmZiZTVhZThjYzAxNDE3ZWFiZjgyNmUxMjA1N2Q0NzkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjFjYTY5NTMxNzQzNDg4YjgwMzg3MjVkNTg0NmZiYTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDcxNzY4LC03OS4zODE1NzY0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xYWVlMzIzMTlhNWE0Njc0YWFhNjY4OWM3Y2FiZmNhYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lYjNmMmJhYTkzMTg0ZjYyOTNjYWU5YWI2Yzc5NDUxNSA9ICQoJzxkaXYgaWQ9Imh0bWxfZWIzZjJiYWE5MzE4NGY2MjkzY2FlOWFiNmM3OTQ1MTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRvcm9udG8gRG9taW5pb24gQ2VudHJlLCBEZXNpZ24gRXhjaGFuZ2UsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzFhZWUzMjMxOWE1YTQ2NzRhYWE2Njg5YzdjYWJmY2FiLnNldENvbnRlbnQoaHRtbF9lYjNmMmJhYTkzMTg0ZjYyOTNjYWU5YWI2Yzc5NDUxNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82MWNhNjk1MzE3NDM0ODhiODAzODcyNWQ1ODQ2ZmJhNy5iaW5kUG9wdXAocG9wdXBfMWFlZTMyMzE5YTVhNDY3NGFhYTY2ODljN2NhYmZjYWIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzVmMmE4ZmIzNjRmNDIyODg5ZTNiZGQ0N2M5Y2FhYzEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzY4NDcyLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84NWQwNjYyODkwYTQ0NmNkOTc5ZTliZGQzM2NjMjdkNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82MmY5MjU4MThhNWQ0NTgzODAxZDQxNTM5MmUwODQ3YyA9ICQoJzxkaXYgaWQ9Imh0bWxfNjJmOTI1ODE4YTVkNDU4MzgwMWQ0MTUzOTJlMDg0N2MiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJyb2NrdG9uLCBQYXJrZGFsZSBWaWxsYWdlLCBFeGhpYml0aW9uIFBsYWNlLCBXZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg1ZDA2NjI4OTBhNDQ2Y2Q5NzllOWJkZDMzY2MyN2Q0LnNldENvbnRlbnQoaHRtbF82MmY5MjU4MThhNWQ0NTgzODAxZDQxNTM5MmUwODQ3Yyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NWYyYThmYjM2NGY0MjI4ODllM2JkZDQ3YzljYWFjMS5iaW5kUG9wdXAocG9wdXBfODVkMDY2Mjg5MGE0NDZjZDk3OWU5YmRkMzNjYzI3ZDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmEyN2JjOWE1ZjA2NGNhOTg1YWI3NDE5ZWU4YjhkZGYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTExMTE3MDAwMDAwMDQsLTc5LjI4NDU3NzJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjkyMjkxMzU5Mjc3NDRkNGJkNjE2ZjY4NTAwNzI5YjIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTMzMzNhMWNjYTM1NGQ3Mzg2MzQwNGIwMzBlZmVkZWEgPSAkKCc8ZGl2IGlkPSJodG1sXzkzMzMzYTFjY2EzNTRkNzM4NjM0MDRiMDMwZWZlZGVhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Hb2xkZW4gTWlsZSwgQ2xhaXJsZWEsIE9ha3JpZGdlLCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjkyMjkxMzU5Mjc3NDRkNGJkNjE2ZjY4NTAwNzI5YjIuc2V0Q29udGVudChodG1sXzkzMzMzYTFjY2EzNTRkNzM4NjM0MDRiMDMwZWZlZGVhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJhMjdiYzlhNWYwNjRjYTk4NWFiNzQxOWVlOGI4ZGRmLmJpbmRQb3B1cChwb3B1cF9mOTIyOTEzNTkyNzc0NGQ0YmQ2MTZmNjg1MDA3MjliMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wM2Y2M2IyOTcyY2Q0NzM5OTc5ZDI1YWZkOTZhZGFmYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc1NzQ5MDIsLTc5LjM3NDcxNDA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YwMjA3MWZmNmQ2OTQ3ODU4YmY3ZjNhOGU0NjQwMDU5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJhNjQyNTcwODM3NzRkNjRhNDFkODMxNmY3OWZkNTZhID0gJCgnPGRpdiBpZD0iaHRtbF8yYTY0MjU3MDgzNzc0ZDY0YTQxZDgzMTZmNzlmZDU2YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+WW9yayBNaWxscywgU2lsdmVyIEhpbGxzLCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mMDIwNzFmZjZkNjk0Nzg1OGJmN2YzYThlNDY0MDA1OS5zZXRDb250ZW50KGh0bWxfMmE2NDI1NzA4Mzc3NGQ2NGE0MWQ4MzE2Zjc5ZmQ1NmEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDNmNjNiMjk3MmNkNDczOTk3OWQyNWFmZDk2YWRhZmIuYmluZFBvcHVwKHBvcHVwX2YwMjA3MWZmNmQ2OTQ3ODU4YmY3ZjNhOGU0NjQwMDU5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI1MmFhOTJhNDdiNTRiMzI5YjdhM2NhMDI0MmM0YmM5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM5MDE0NiwtNzkuNTA2OTQzNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85M2M5NDI2NWFiN2M0OTJjYmUxZThkMDcxY2NkMTI0YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jMGIwZTRlM2JlMDc0OGMwODUxNTQ2MjBjM2IyOGJlNiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzBiMGU0ZTNiZTA3NDhjMDg1MTU0NjIwYzNiMjhiZTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvd25zdmlldywgTm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOTNjOTQyNjVhYjdjNDkyY2JlMWU4ZDA3MWNjZDEyNGIuc2V0Q29udGVudChodG1sX2MwYjBlNGUzYmUwNzQ4YzA4NTE1NDYyMGMzYjI4YmU2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI1MmFhOTJhNDdiNTRiMzI5YjdhM2NhMDI0MmM0YmM5LmJpbmRQb3B1cChwb3B1cF85M2M5NDI2NWFiN2M0OTJjYmUxZThkMDcxY2NkMTI0Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jZmMzYjY5ZGFkZDY0ZjRkOGQ1ZjQ2YjA5ZTI0OWQyYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2ODk5ODUsLTc5LjMxNTU3MTU5OTk5OTk4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzg0NDEyZTIzMThmODRiMzRiZjE1ZjFlZjA2OTdmMzRiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzNkOTRhOGQyOTA3ZTQ4M2U5ZThjMjg0NDY3YzUwYWFmID0gJCgnPGRpdiBpZD0iaHRtbF8zZDk0YThkMjkwN2U0ODNlOWU4YzI4NDQ2N2M1MGFhZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SW5kaWEgQmF6YWFyLCBUaGUgQmVhY2hlcyBXZXN0LCBFYXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg0NDEyZTIzMThmODRiMzRiZjE1ZjFlZjA2OTdmMzRiLnNldENvbnRlbnQoaHRtbF8zZDk0YThkMjkwN2U0ODNlOWU4YzI4NDQ2N2M1MGFhZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jZmMzYjY5ZGFkZDY0ZjRkOGQ1ZjQ2YjA5ZTI0OWQyYi5iaW5kUG9wdXAocG9wdXBfODQ0MTJlMjMxOGY4NGIzNGJmMTVmMWVmMDY5N2YzNGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfN2QxNGRkZTk5YjgzNDU3MDhjYTBhNzAxYWZlYzE0OTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDgxOTg1LC03OS4zNzk4MTY5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80YTI2Mzk2NTU5M2I0ZDA4YTJhNTE2YWJiYTllOGQ3OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mOTIyNmIwZjU1MWM0MThhOTc1ZjRiMTViM2M1YWQ4YiA9ICQoJzxkaXYgaWQ9Imh0bWxfZjkyMjZiMGY1NTFjNDE4YTk3NWY0YjE1YjNjNWFkOGIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNvbW1lcmNlIENvdXJ0LCBWaWN0b3JpYSBIb3RlbCwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNGEyNjM5NjU1OTNiNGQwOGEyYTUxNmFiYmE5ZThkNzkuc2V0Q29udGVudChodG1sX2Y5MjI2YjBmNTUxYzQxOGE5NzVmNGIxNWIzYzVhZDhiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdkMTRkZGU5OWI4MzQ1NzA4Y2EwYTcwMWFmZWMxNDkzLmJpbmRQb3B1cChwb3B1cF80YTI2Mzk2NTU5M2I0ZDA4YTJhNTE2YWJiYTllOGQ3OSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83NGIwY2IwMDE0MTc0OGUzOTA5NTZlNDlhMTM1OTllNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMzc1NjIwMDAwMDAwNiwtNzkuNDkwMDczOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iMzBlNzg3NTMwZWM0NTIyODZkZTEwODE4ZWEzYzEyMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82NGU0N2NlNWUzYTY0Zjk3YTllNjYxZGQ3MjQwZDVkZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNjRlNDdjZTVlM2E2NGY5N2E5ZTY2MWRkNzI0MGQ1ZGQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRoIFBhcmssIE1hcGxlIExlYWYgUGFyaywgVXB3b29kIFBhcmssIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2IzMGU3ODc1MzBlYzQ1MjI4NmRlMTA4MThlYTNjMTIxLnNldENvbnRlbnQoaHRtbF82NGU0N2NlNWUzYTY0Zjk3YTllNjYxZGQ3MjQwZDVkZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NGIwY2IwMDE0MTc0OGUzOTA5NTZlNDlhMTM1OTllNi5iaW5kUG9wdXAocG9wdXBfYjMwZTc4NzUzMGVjNDUyMjg2ZGUxMDgxOGVhM2MxMjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYmEwZTBhMGRkNDliNDZlNzhmZjg0MmY1NDE0OTU4ZTYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTYzMDMzLC03OS41NjU5NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82Y2FiODcwODMxYTA0OTI2YWQwMTU2OWE1YmFkNjgwNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82MGZiMjhjNTFlNmM0NjMxYWIxM2YzMjNlZTQyMzY0ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNjBmYjI4YzUxZTZjNDYzMWFiMTNmMzIzZWU0MjM2NGQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkh1bWJlciBTdW1taXQsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzZjYWI4NzA4MzFhMDQ5MjZhZDAxNTY5YTViYWQ2ODA3LnNldENvbnRlbnQoaHRtbF82MGZiMjhjNTFlNmM0NjMxYWIxM2YzMjNlZTQyMzY0ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iYTBlMGEwZGQ0OWI0NmU3OGZmODQyZjU0MTQ5NThlNi5iaW5kUG9wdXAocG9wdXBfNmNhYjg3MDgzMWEwNDkyNmFkMDE1NjlhNWJhZDY4MDcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWYwOWZkMmE5Y2Q0NGMwZmExMmQwZTg1M2I0OGI1NzkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTYzMTYsLTc5LjIzOTQ3NjA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NjZjliYzY3Mjc5NDQ0NTNiY2NiYmFhZDNkMjZkZDlhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQwM2I4NDY0OWI4NzRhYTZiMGYwMTVmNmZjYTExNWJlID0gJCgnPGRpdiBpZD0iaHRtbF80MDNiODQ2NDliODc0YWE2YjBmMDE1ZjZmY2ExMTViZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2xpZmZzaWRlLCBDbGlmZmNyZXN0LCBTY2FyYm9yb3VnaCBWaWxsYWdlIFdlc3QsIFNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jY2Y5YmM2NzI3OTQ0NDUzYmNjYmJhYWQzZDI2ZGQ5YS5zZXRDb250ZW50KGh0bWxfNDAzYjg0NjQ5Yjg3NGFhNmIwZjAxNWY2ZmNhMTE1YmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWYwOWZkMmE5Y2Q0NGMwZmExMmQwZTg1M2I0OGI1NzkuYmluZFBvcHVwKHBvcHVwX2NjZjliYzY3Mjc5NDQ0NTNiY2NiYmFhZDNkMjZkZDlhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJiODZiYTBiOWMxNDRjMGZhMjU5ZDljNmI1ZGExNDAyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzg5MDUzLC03OS40MDg0OTI3OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yMjczYzQ4Y2Y2MmY0MmFiOTMwZjFmNTBhOGQzOGM4OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMTUzY2RjYjlmNzk0ZjU5YjAyNzExYTQ5NmQzNzE4MiA9ICQoJzxkaXYgaWQ9Imh0bWxfYTE1M2NkY2I5Zjc5NGY1OWIwMjcxMWE0OTZkMzcxODIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldpbGxvd2RhbGUsIE5ld3RvbmJyb29rLCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yMjczYzQ4Y2Y2MmY0MmFiOTMwZjFmNTBhOGQzOGM4OC5zZXRDb250ZW50KGh0bWxfYTE1M2NkY2I5Zjc5NGY1OWIwMjcxMWE0OTZkMzcxODIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmI4NmJhMGI5YzE0NGMwZmEyNTlkOWM2YjVkYTE0MDIuYmluZFBvcHVwKHBvcHVwXzIyNzNjNDhjZjYyZjQyYWI5MzBmMWY1MGE4ZDM4Yzg4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2FlM2ZhNGMzYTlkMjRjZWZhMjY0MDA0NTE3ZTE5YzUwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzI4NDk2NCwtNzkuNDk1Njk3NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWU0MjlmYjdkOTM3NDM0NzhiNDgzZjM2MzBiYzRmMjEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmNmY2IyNzI0ZDMyNDU5OWJlZjc3ZDdlYTU4NTA5YTYgPSAkKCc8ZGl2IGlkPSJodG1sX2JjZmNiMjcyNGQzMjQ1OTliZWY3N2Q3ZWE1ODUwOWE2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb3duc3ZpZXcsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVlNDI5ZmI3ZDkzNzQzNDc4YjQ4M2YzNjMwYmM0ZjIxLnNldENvbnRlbnQoaHRtbF9iY2ZjYjI3MjRkMzI0NTk5YmVmNzdkN2VhNTg1MDlhNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hZTNmYTRjM2E5ZDI0Y2VmYTI2NDAwNDUxN2UxOWM1MC5iaW5kUG9wdXAocG9wdXBfNWU0MjlmYjdkOTM3NDM0NzhiNDgzZjM2MzBiYzRmMjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWRmMWIyYTU5MmMxNGFiMzgyOTY5MDgyOTcwMmFhMjggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTk1MjU1LC03OS4zNDA5MjNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmQ4YTg2ZWY1ZWNiNGQxMGJjNDU5ZmMzZjZmZmE3NmIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmRjNDZkMDBjODdkNDA5ODk3M2YxNjBiZTVlYmQ1NTUgPSAkKCc8ZGl2IGlkPSJodG1sX2JkYzQ2ZDAwYzg3ZDQwOTg5NzNmMTYwYmU1ZWJkNTU1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdHVkaW8gRGlzdHJpY3QsIEVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmQ4YTg2ZWY1ZWNiNGQxMGJjNDU5ZmMzZjZmZmE3NmIuc2V0Q29udGVudChodG1sX2JkYzQ2ZDAwYzg3ZDQwOTg5NzNmMTYwYmU1ZWJkNTU1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzlkZjFiMmE1OTJjMTRhYjM4Mjk2OTA4Mjk3MDJhYTI4LmJpbmRQb3B1cChwb3B1cF8yZDhhODZlZjVlY2I0ZDEwYmM0NTlmYzNmNmZmYTc2Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80NGMyZmE4N2RiYTY0MzFhYjUwZGIyMTM2MDlmN2VmNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjczMzI4MjUsLTc5LjQxOTc0OTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMjhhYWUzNTIxMTdhNGM1NGEzNWQ4MDVhNjFhNmIyOGIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZWRjNmQxYzA5NGQ4NDFmN2JlOGE2ZmNiZjhkZmFiY2QgPSAkKCc8ZGl2IGlkPSJodG1sX2VkYzZkMWMwOTRkODQxZjdiZThhNmZjYmY4ZGZhYmNkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CZWRmb3JkIFBhcmssIExhd3JlbmNlIE1hbm9yIEVhc3QsIE5vcnRoIFlvcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzI4YWFlMzUyMTE3YTRjNTRhMzVkODA1YTYxYTZiMjhiLnNldENvbnRlbnQoaHRtbF9lZGM2ZDFjMDk0ZDg0MWY3YmU4YTZmY2JmOGRmYWJjZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NGMyZmE4N2RiYTY0MzFhYjUwZGIyMTM2MDlmN2VmNy5iaW5kUG9wdXAocG9wdXBfMjhhYWUzNTIxMTdhNGM1NGEzNWQ4MDVhNjFhNmIyOGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDhkMmFkZmM0Zjk1NDNkNDhhODk0MTU5OGM4NGQwZjUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTExMTU4LC03OS40NzYwMTMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wMThjZGY0YjIyNjY0MmRmYWM4ZDM1NWI5YzljMTQ5YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MWUxMzMwMjVkZWM0NDA3ODQ0OTAwZGY1MTVhZTI4MyA9ICQoJzxkaXYgaWQ9Imh0bWxfNzFlMTMzMDI1ZGVjNDQwNzg0NDkwMGRmNTE1YWUyODMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlbCBSYXksIE1vdW50IERlbm5pcywgS2VlbHNkYWxlIGFuZCBTaWx2ZXJ0aG9ybiwgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDE4Y2RmNGIyMjY2NDJkZmFjOGQzNTViOWM5YzE0OWIuc2V0Q29udGVudChodG1sXzcxZTEzMzAyNWRlYzQ0MDc4NDQ5MDBkZjUxNWFlMjgzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Q4ZDJhZGZjNGY5NTQzZDQ4YTg5NDE1OThjODRkMGY1LmJpbmRQb3B1cChwb3B1cF8wMThjZGY0YjIyNjY0MmRmYWM4ZDM1NWI5YzljMTQ5Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80MjIyMDVhNDBmYmU0MDNkODBjNjdjNjgxZWUyNzMwZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNDc2NTksLTc5LjUzMjI0MjQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzllYjUwNzkyZDAxMzQzMDhiOGU1ZmQ5Zjk2NzEyZmU5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzlhMGQ0MDNkZTE3OTRkOTA4ZTNmNGIwZTdjZGMyYWQ2ID0gJCgnPGRpdiBpZD0iaHRtbF85YTBkNDAzZGUxNzk0ZDkwOGUzZjRiMGU3Y2RjMmFkNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SHVtYmVybGVhLCBFbWVyeSwgTm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWViNTA3OTJkMDEzNDMwOGI4ZTVmZDlmOTY3MTJmZTkuc2V0Q29udGVudChodG1sXzlhMGQ0MDNkZTE3OTRkOTA4ZTNmNGIwZTdjZGMyYWQ2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQyMjIwNWE0MGZiZTQwM2Q4MGM2N2M2ODFlZTI3MzBmLmJpbmRQb3B1cChwb3B1cF85ZWI1MDc5MmQwMTM0MzA4YjhlNWZkOWY5NjcxMmZlOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83NGNjNmUwYTljNTk0ZWU3OGNiNjlmMWZjMmI0Y2NkMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY5MjY1NzAwMDAwMDAwNCwtNzkuMjY0ODQ4MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yMjVmMDRmNzIzOWU0YmViYjY1OGI5YjgyN2I5NWE2ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kNTk2NDU3NDljOGY0OGQ2OTBkNWFiYzQzNTljOGY0NSA9ICQoJzxkaXYgaWQ9Imh0bWxfZDU5NjQ1NzQ5YzhmNDhkNjkwZDVhYmM0MzU5YzhmNDUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJpcmNoIENsaWZmLCBDbGlmZnNpZGUgV2VzdCwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzIyNWYwNGY3MjM5ZTRiZWJiNjU4YjliODI3Yjk1YTZlLnNldENvbnRlbnQoaHRtbF9kNTk2NDU3NDljOGY0OGQ2OTBkNWFiYzQzNTljOGY0NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NGNjNmUwYTljNTk0ZWU3OGNiNjlmMWZjMmI0Y2NkMi5iaW5kUG9wdXAocG9wdXBfMjI1ZjA0ZjcyMzllNGJlYmI2NThiOWI4MjdiOTVhNmUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWNjMmRiNzFjOWVmNDcyY2I4Y2MyNWRkOThlZTc1MmMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NzAxMTk5LC03OS40MDg0OTI3OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yNDQ3ZGJiNmE3ZWY0YTFlOGRiZGNkYWRlNTIwNDk4ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jMzVkNjljY2U1Njg0MWRmYjFkYTBlYmUwMTdiNjI0ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzM1ZDY5Y2NlNTY4NDFkZmIxZGEwZWJlMDE3YjYyNGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldpbGxvd2RhbGUsIFdpbGxvd2RhbGUgRWFzdCwgTm9ydGggWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjQ0N2RiYjZhN2VmNGExZThkYmRjZGFkZTUyMDQ5OGYuc2V0Q29udGVudChodG1sX2MzNWQ2OWNjZTU2ODQxZGZiMWRhMGViZTAxN2I2MjRmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FjYzJkYjcxYzllZjQ3MmNiOGNjMjVkZDk4ZWU3NTJjLmJpbmRQb3B1cChwb3B1cF8yNDQ3ZGJiNmE3ZWY0YTFlOGRiZGNkYWRlNTIwNDk4Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80OTdhMzVkYzE3YjE0YTVlYTU1MjAzMzBkMjVkYzk4MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc2MTYzMTMsLTc5LjUyMDk5OTQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JhZTRmODgyNmRmODQwOWRiYzk1YThiYjEzYWZkN2JiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU3MDE3OWJhMmU1MzRiZTJhMjcxM2QyNjkyYWE0NmU0ID0gJCgnPGRpdiBpZD0iaHRtbF81NzAxNzliYTJlNTM0YmUyYTI3MTNkMjY5MmFhNDZlNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnN2aWV3LCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iYWU0Zjg4MjZkZjg0MDlkYmM5NWE4YmIxM2FmZDdiYi5zZXRDb250ZW50KGh0bWxfNTcwMTc5YmEyZTUzNGJlMmEyNzEzZDI2OTJhYTQ2ZTQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDk3YTM1ZGMxN2IxNGE1ZWE1NTIwMzMwZDI1ZGM5ODEuYmluZFBvcHVwKHBvcHVwX2JhZTRmODgyNmRmODQwOWRiYzk1YThiYjEzYWZkN2JiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM3MDczOTEwYzVhMzQ3ZjViMzYzOTkxY2E4Yzk2YzdmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzI4MDIwNSwtNzkuMzg4NzkwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iOWEzMGQ5M2RlMzQ0NzlmYmQwODdmZGZmM2UwZGNmNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hZjI5Nzg4ZGJhMmQ0YjBlYTU3ODJmZTgzNmQ1ODJlNCA9ICQoJzxkaXYgaWQ9Imh0bWxfYWYyOTc4OGRiYTJkNGIwZWE1NzgyZmU4MzZkNTgyZTQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxhd3JlbmNlIFBhcmssIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYjlhMzBkOTNkZTM0NDc5ZmJkMDg3ZmRmZjNlMGRjZjUuc2V0Q29udGVudChodG1sX2FmMjk3ODhkYmEyZDRiMGVhNTc4MmZlODM2ZDU4MmU0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM3MDczOTEwYzVhMzQ3ZjViMzYzOTkxY2E4Yzk2YzdmLmJpbmRQb3B1cChwb3B1cF9iOWEzMGQ5M2RlMzQ0NzlmYmQwODdmZGZmM2UwZGNmNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zZjhmN2MyZDZjMGM0NzcyYmYyYTA4YTIyNGY3MDlkNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMTY5NDgsLTc5LjQxNjkzNTU5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzRmNDI3MDViYTg3YzQzYWY5MGU1NzQyODJiMTIzNWY2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzdjOTRmMjBkYTRjZDRhN2I5ZDc0NGRkYzM4ODJkZjQ2ID0gJCgnPGRpdiBpZD0iaHRtbF83Yzk0ZjIwZGE0Y2Q0YTdiOWQ3NDRkZGMzODgyZGY0NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um9zZWxhd24sIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNGY0MjcwNWJhODdjNDNhZjkwZTU3NDI4MmIxMjM1ZjYuc2V0Q29udGVudChodG1sXzdjOTRmMjBkYTRjZDRhN2I5ZDc0NGRkYzM4ODJkZjQ2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzNmOGY3YzJkNmMwYzQ3NzJiZjJhMDhhMjI0ZjcwOWQ0LmJpbmRQb3B1cChwb3B1cF80ZjQyNzA1YmE4N2M0M2FmOTBlNTc0MjgyYjEyMzVmNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yYjE5MGIxMThmNzM0NzUyOTY2OWQ0ZGNiNjk2YzA3ZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3MzE4NTI5OTk5OTk5LC03OS40ODcyNjE5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xN2QxOTAzMjlhMzk0ZTI1OTExNTRjYjc5NjFiZDgzYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80ZDg0OTUxMWM5ODA0NjNjYmM1ZTI0M2MyMzZmZDQ5MCA9ICQoJzxkaXYgaWQ9Imh0bWxfNGQ4NDk1MTFjOTgwNDYzY2JjNWUyNDNjMjM2ZmQ0OTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJ1bm55bWVkZSwgVGhlIEp1bmN0aW9uIE5vcnRoLCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xN2QxOTAzMjlhMzk0ZTI1OTExNTRjYjc5NjFiZDgzYi5zZXRDb250ZW50KGh0bWxfNGQ4NDk1MTFjOTgwNDYzY2JjNWUyNDNjMjM2ZmQ0OTApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmIxOTBiMTE4ZjczNDc1Mjk2NjlkNGRjYjY5NmMwN2QuYmluZFBvcHVwKHBvcHVwXzE3ZDE5MDMyOWEzOTRlMjU5MTE1NGNiNzk2MWJkODNiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzNmOWVjNGYxNDE4MDRhMWViNzQ0YjMwOGQ2NDBiZjk3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA2ODc2LC03OS41MTgxODg0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84Zjg4YjRkYTg0YjY0YTk2YjQwOGRiMTM5Y2VhYzVjMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xOTUzMWRkNWQxNTU0ZTM4ODc2NzVjYjM2NzNiMGVjNCA9ICQoJzxkaXYgaWQ9Imh0bWxfMTk1MzFkZDVkMTU1NGUzODg3Njc1Y2IzNjczYjBlYzQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldlc3RvbiwgWW9yazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGY4OGI0ZGE4NGI2NGE5NmI0MDhkYjEzOWNlYWM1YzEuc2V0Q29udGVudChodG1sXzE5NTMxZGQ1ZDE1NTRlMzg4NzY3NWNiMzY3M2IwZWM0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzNmOWVjNGYxNDE4MDRhMWViNzQ0YjMwOGQ2NDBiZjk3LmJpbmRQb3B1cChwb3B1cF84Zjg4YjRkYTg0YjY0YTk2YjQwOGRiMTM5Y2VhYzVjMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85Yzc3N2E1OGNjNGM0OTFiOTUxNmY2OTkzMGZkOWJiMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc1NzQwOTYsLTc5LjI3MzMwNDAwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzlmZTkyMzNiZDExMzQ2YjM5NDQ4MGU1OGQ2NWZiMzBjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2I1ZmYwMGY5YjM0OTRhZmJiZjE3YTQyYjMwMTUyODQ1ID0gJCgnPGRpdiBpZD0iaHRtbF9iNWZmMDBmOWIzNDk0YWZiYmYxN2E0MmIzMDE1Mjg0NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG9yc2V0IFBhcmssIFdleGZvcmQgSGVpZ2h0cywgU2NhcmJvcm91Z2ggVG93biBDZW50cmUsIFNjYXJib3JvdWdoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85ZmU5MjMzYmQxMTM0NmIzOTQ0ODBlNThkNjVmYjMwYy5zZXRDb250ZW50KGh0bWxfYjVmZjAwZjliMzQ5NGFmYmJmMTdhNDJiMzAxNTI4NDUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWM3NzdhNThjYzRjNDkxYjk1MTZmNjk5MzBmZDliYjMuYmluZFBvcHVwKHBvcHVwXzlmZTkyMzNiZDExMzQ2YjM5NDQ4MGU1OGQ2NWZiMzBjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzllZGI0YzZhNDVlYTQzNWZhZjg4MWIyMjFlMjA2NzllID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUyNzU4Mjk5OTk5OTk2LC03OS40MDAwNDkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U2OTNmZmE4NjkyMDRmNmQ4MjhhMDkxOGZiMzhlM2IwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzNhYTJhMDQ3ZjViODQxNTA4YzE4Yzc1MDVhYzU1ZTA2ID0gJCgnPGRpdiBpZD0iaHRtbF8zYWEyYTA0N2Y1Yjg0MTUwOGMxOGM3NTA1YWM1NWUwNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+WW9yayBNaWxscyBXZXN0LCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lNjkzZmZhODY5MjA0ZjZkODI4YTA5MThmYjM4ZTNiMC5zZXRDb250ZW50KGh0bWxfM2FhMmEwNDdmNWI4NDE1MDhjMThjNzUwNWFjNTVlMDYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWVkYjRjNmE0NWVhNDM1ZmFmODgxYjIyMWUyMDY3OWUuYmluZFBvcHVwKHBvcHVwX2U2OTNmZmE4NjkyMDRmNmQ4MjhhMDkxOGZiMzhlM2IwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E1YjM3OTIxY2EzNzQwMWI5ZWIwNThjNTg2NzlhZTU2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzEyNzUxMSwtNzkuMzkwMTk3NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yZjQ3YzQyYjI1MTc0NjRjOTc3ZWQyYjJiMmM1Yzg3MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81Mzk3ZTlmYThhNTA0NDkwYmYzNGU4ZDQ3MzRjODQyMSA9ICQoJzxkaXYgaWQ9Imh0bWxfNTM5N2U5ZmE4YTUwNDQ5MGJmMzRlOGQ0NzM0Yzg0MjEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRhdmlzdmlsbGUgTm9ydGgsIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmY0N2M0MmIyNTE3NDY0Yzk3N2VkMmIyYjJjNWM4NzEuc2V0Q29udGVudChodG1sXzUzOTdlOWZhOGE1MDQ0OTBiZjM0ZThkNDczNGM4NDIxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2E1YjM3OTIxY2EzNzQwMWI5ZWIwNThjNTg2NzlhZTU2LmJpbmRQb3B1cChwb3B1cF8yZjQ3YzQyYjI1MTc0NjRjOTc3ZWQyYjJiMmM1Yzg3MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NDU0ZGI5YjZiNjc0ZmUwOWQ1ZGFiOTJiYTc2MzA0ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY5Njk0NzYsLTc5LjQxMTMwNzIwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzdmZjE4YmRlOGIyMzQ0OTI5NzQ0NGFkM2NjODAzM2Q3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYzYzA2ZjVjNjg3ODQzODY4OGE1NjNjNmIyOGI2OGVlID0gJCgnPGRpdiBpZD0iaHRtbF82M2MwNmY1YzY4Nzg0Mzg2ODhhNTYzYzZiMjhiNjhlZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Rm9yZXN0IEhpbGwgTm9ydGggJmFtcDsgV2VzdCwgRm9yZXN0IEhpbGwgUm9hZCBQYXJrLCBDZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdmZjE4YmRlOGIyMzQ0OTI5NzQ0NGFkM2NjODAzM2Q3LnNldENvbnRlbnQoaHRtbF82M2MwNmY1YzY4Nzg0Mzg2ODhhNTYzYzZiMjhiNjhlZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82NDU0ZGI5YjZiNjc0ZmUwOWQ1ZGFiOTJiYTc2MzA0ZS5iaW5kUG9wdXAocG9wdXBfN2ZmMThiZGU4YjIzNDQ5Mjk3NDQ0YWQzY2M4MDMzZDcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZThmNTFlMmVkMjk5NDQyZWE3YzM5ODQxNGM1ZDRlZmYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjE2MDgzLC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wYjI2N2ZkMmRmYTE0NTkxOTNiZTVkOTY1YWI5OTJkZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80ODUzN2E0MzI5OWU0NDY2YjQzYzM4ODNmNDgzMzVkOCA9ICQoJzxkaXYgaWQ9Imh0bWxfNDg1MzdhNDMyOTllNDQ2NmI0M2MzODgzZjQ4MzM1ZDgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhpZ2ggUGFyaywgVGhlIEp1bmN0aW9uIFNvdXRoLCBXZXN0IFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzBiMjY3ZmQyZGZhMTQ1OTE5M2JlNWQ5NjVhYjk5MmRmLnNldENvbnRlbnQoaHRtbF80ODUzN2E0MzI5OWU0NDY2YjQzYzM4ODNmNDgzMzVkOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lOGY1MWUyZWQyOTk0NDJlYTdjMzk4NDE0YzVkNGVmZi5iaW5kUG9wdXAocG9wdXBfMGIyNjdmZDJkZmExNDU5MTkzYmU1ZDk2NWFiOTkyZGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTdkMjAyNGUwNmMyNDhmNGIyNzI1MWVhZTc1MjFiY2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTYzMTksLTc5LjUzMjI0MjQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzhlMTEzZTM1YzJjODQ5MmFiOWM2ZGEwNjQ5MTNmZTY2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzMyYTYyOTJmZGQzZTQ4NjRiNGY0MjIwOGU0OWU0MTU1ID0gJCgnPGRpdiBpZD0iaHRtbF8zMmE2MjkyZmRkM2U0ODY0YjRmNDIyMDhlNDllNDE1NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdG1vdW50LCBFdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzhlMTEzZTM1YzJjODQ5MmFiOWM2ZGEwNjQ5MTNmZTY2LnNldENvbnRlbnQoaHRtbF8zMmE2MjkyZmRkM2U0ODY0YjRmNDIyMDhlNDllNDE1NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xN2QyMDI0ZTA2YzI0OGY0YjI3MjUxZWFlNzUyMWJjYi5iaW5kUG9wdXAocG9wdXBfOGUxMTNlMzVjMmM4NDkyYWI5YzZkYTA2NDkxM2ZlNjYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzhhMDM0NTYxOWM0NDhhMjgxMTFkM2U2YjczMmNhNGIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTAwNzE1MDAwMDAwMDQsLTc5LjI5NTg0OTFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODZiZDk5M2U3OGI4NDdhYWE0MTNiMzk3ODhiNWZmZWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmRmZGJjY2U0MGNiNDBiZmI2NzJiZTZhOTY4ZjQ3NzQgPSAkKCc8ZGl2IGlkPSJodG1sXzZkZmRiY2NlNDBjYjQwYmZiNjcyYmU2YTk2OGY0Nzc0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXhmb3JkLCBNYXJ5dmFsZSwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg2YmQ5OTNlNzhiODQ3YWFhNDEzYjM5Nzg4YjVmZmVmLnNldENvbnRlbnQoaHRtbF82ZGZkYmNjZTQwY2I0MGJmYjY3MmJlNmE5NjhmNDc3NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zOGEwMzQ1NjE5YzQ0OGEyODExMWQzZTZiNzMyY2E0Yi5iaW5kUG9wdXAocG9wdXBfODZiZDk5M2U3OGI4NDdhYWE0MTNiMzk3ODhiNWZmZWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDhkN2NhZjM1OWIwNDdlZmE4ZTdiM2E1MzJlMjhkYzcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43ODI3MzY0LC03OS40NDIyNTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzMxNjYwZGQ2NTIzYzRlNzdiODYzZWY4ZWZkODZlNjVkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUxMDFkOTJlZTliYzQ3M2JiMTRjOTMwMGZlNjQzZWRjID0gJCgnPGRpdiBpZD0iaHRtbF81MTAxZDkyZWU5YmM0NzNiYjE0YzkzMDBmZTY0M2VkYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2lsbG93ZGFsZSwgV2lsbG93ZGFsZSBXZXN0LCBOb3J0aCBZb3JrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zMTY2MGRkNjUyM2M0ZTc3Yjg2M2VmOGVmZDg2ZTY1ZC5zZXRDb250ZW50KGh0bWxfNTEwMWQ5MmVlOWJjNDczYmIxNGM5MzAwZmU2NDNlZGMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDhkN2NhZjM1OWIwNDdlZmE4ZTdiM2E1MzJlMjhkYzcuYmluZFBvcHVwKHBvcHVwXzMxNjYwZGQ2NTIzYzRlNzdiODYzZWY4ZWZkODZlNjVkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzc4MDI0M2Y1MTE2NDQwNGJhMmE5M2RlMTBhMTFmMTVhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzE1MzgzNCwtNzkuNDA1Njc4NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDRhMmFmOWI0NGUyNGQ1NDliMmZiYjk3YjJlMGIwODYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTk3YmFhZmZiNzllNDAzNWE2Y2UxMjNhOTdiZWVmNTIgPSAkKCc8ZGl2IGlkPSJodG1sX2E5N2JhYWZmYjc5ZTQwMzVhNmNlMTIzYTk3YmVlZjUyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBUb3JvbnRvIFdlc3QsIExhd3JlbmNlIFBhcmssIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDRhMmFmOWI0NGUyNGQ1NDliMmZiYjk3YjJlMGIwODYuc2V0Q29udGVudChodG1sX2E5N2JhYWZmYjc5ZTQwMzVhNmNlMTIzYTk3YmVlZjUyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzc4MDI0M2Y1MTE2NDQwNGJhMmE5M2RlMTBhMTFmMTVhLmJpbmRQb3B1cChwb3B1cF8wNGEyYWY5YjQ0ZTI0ZDU0OWIyZmJiOTdiMmUwYjA4Nik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85ZWFlYzAyMGUzNTA0NDZhYWUyZjE0NTM0YmI2ZGVjMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3MjcwOTcsLTc5LjQwNTY3ODQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzE4MDQ3YWQxMzg1YjQ0ZTBhOTFiOTI4YzZkYzkxZjQxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhjMzQ4Yjg4NDhmMzQ2MjZhMTg1OTc0NzY4YzBjYTNmID0gJCgnPGRpdiBpZD0iaHRtbF84YzM0OGI4ODQ4ZjM0NjI2YTE4NTk3NDc2OGMwY2EzZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VGhlIEFubmV4LCBOb3J0aCBNaWR0b3duLCBZb3JrdmlsbGUsIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTgwNDdhZDEzODViNDRlMGE5MWI5MjhjNmRjOTFmNDEuc2V0Q29udGVudChodG1sXzhjMzQ4Yjg4NDhmMzQ2MjZhMTg1OTc0NzY4YzBjYTNmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzllYWVjMDIwZTM1MDQ0NmFhZTJmMTQ1MzRiYjZkZWMwLmJpbmRQb3B1cChwb3B1cF8xODA0N2FkMTM4NWI0NGUwYTkxYjkyOGM2ZGM5MWY0MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xODM4OWRlNDhhNDA0Y2YzOWMxNjhmYjM0YjllNGUwYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODk1OTcsLTc5LjQ1NjMyNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMTNlYjg3YWJiNjk0MmY4YmRlMjM4ZWJmNjIwMGNhYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80MTExOWFkZWExMjk0M2IzYmUyZTMzY2RlNzFhNjAwOSA9ICQoJzxkaXYgaWQ9Imh0bWxfNDExMTlhZGVhMTI5NDNiM2JlMmUzM2NkZTcxYTYwMDkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhcmtkYWxlLCBSb25jZXN2YWxsZXMsIFdlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDEzZWI4N2FiYjY5NDJmOGJkZTIzOGViZjYyMDBjYWEuc2V0Q29udGVudChodG1sXzQxMTE5YWRlYTEyOTQzYjNiZTJlMzNjZGU3MWE2MDA5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzE4Mzg5ZGU0OGE0MDRjZjM5YzE2OGZiMzRiOWU0ZTBjLmJpbmRQb3B1cChwb3B1cF9kMTNlYjg3YWJiNjk0MmY4YmRlMjM4ZWJmNjIwMGNhYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83NDNiYzRkNTE1ODY0YWEwYWZkZjk4YTQzNTM4ZDIyYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYzNjk2NTYsLTc5LjYxNTgxODk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFjYjY4YmVjZjFiMzQ2MTQ4ZmViZTFjZTlmN2JkMjhiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzE2N2VmNTc4OTc0ZDQ4YzI4MDljN2FmMzkxN2M5MWRkID0gJCgnPGRpdiBpZD0iaHRtbF8xNjdlZjU3ODk3NGQ0OGMyODA5YzdhZjM5MTdjOTFkZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2FuYWRhIFBvc3QgR2F0ZXdheSBQcm9jZXNzaW5nIENlbnRyZSwgTWlzc2lzc2F1Z2E8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzFjYjY4YmVjZjFiMzQ2MTQ4ZmViZTFjZTlmN2JkMjhiLnNldENvbnRlbnQoaHRtbF8xNjdlZjU3ODk3NGQ0OGMyODA5YzdhZjM5MTdjOTFkZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NDNiYzRkNTE1ODY0YWEwYWZkZjk4YTQzNTM4ZDIyYS5iaW5kUG9wdXAocG9wdXBfMWNiNjhiZWNmMWIzNDYxNDhmZWJlMWNlOWY3YmQyOGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDYyY2Y3YzIxNzRiNDA0MjhjNDU5NmY5ODM4YTQ5MGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODg5MDU0LC03OS41NTQ3MjQ0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jNGZiOTgxODY4ZDU0NjBmOThjYTk2NWNiZmQ0NzNiZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jOWVhYmE3N2YzNGM0NGYzYTU3OTM0MDkwZTYwZDIzYiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzllYWJhNzdmMzRjNDRmM2E1NzkzNDA5MGU2MGQyM2IiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktpbmdzdmlldyBWaWxsYWdlLCBTdC4gUGhpbGxpcHMsIE1hcnRpbiBHcm92ZSBHYXJkZW5zLCBSaWNodmlldyBHYXJkZW5zLCBFdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M0ZmI5ODE4NjhkNTQ2MGY5OGNhOTY1Y2JmZDQ3M2JlLnNldENvbnRlbnQoaHRtbF9jOWVhYmE3N2YzNGM0NGYzYTU3OTM0MDkwZTYwZDIzYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wNjJjZjdjMjE3NGI0MDQyOGM0NTk2Zjk4MzhhNDkwZC5iaW5kUG9wdXAocG9wdXBfYzRmYjk4MTg2OGQ1NDYwZjk4Y2E5NjVjYmZkNDczYmUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDczYjNkZDEyNDg3NGQxNTg1YzFmMDQwZTc1MmQ5MTUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43OTQyMDAzLC03OS4yNjIwMjk0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yN2RjNDhmOWRkYzk0OTM1YmM3YmZjNjIwOTI4NjRlMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iNDg5NDNjNDRjZmE0OTc2OTA0ZDk5NzRlOWQwYjk0NCA9ICQoJzxkaXYgaWQ9Imh0bWxfYjQ4OTQzYzQ0Y2ZhNDk3NjkwNGQ5OTc0ZTlkMGI5NDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkFnaW5jb3VydCwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzI3ZGM0OGY5ZGRjOTQ5MzViYzdiZmM2MjA5Mjg2NGUwLnNldENvbnRlbnQoaHRtbF9iNDg5NDNjNDRjZmE0OTc2OTA0ZDk5NzRlOWQwYjk0NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kNzNiM2RkMTI0ODc0ZDE1ODVjMWYwNDBlNzUyZDkxNS5iaW5kUG9wdXAocG9wdXBfMjdkYzQ4ZjlkZGM5NDkzNWJjN2JmYzYyMDkyODY0ZTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYTUxNjcyZjc4YTgyNGIxZWFjYzQ3ZmRkZDA3ZDc2ZjMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDQzMjQ0LC03OS4zODg3OTAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzAwNjA5MWMyM2I2ZTQ4MGY5MjI4YjNiNjdlNTdlNDAwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzIwYTY1Njg4NWRlYzQ4MjY4NWViNDljY2UxNDIyY2RmID0gJCgnPGRpdiBpZD0iaHRtbF8yMGE2NTY4ODVkZWM0ODI2ODVlYjQ5Y2NlMTQyMmNkZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RGF2aXN2aWxsZSwgQ2VudHJhbCBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wMDYwOTFjMjNiNmU0ODBmOTIyOGIzYjY3ZTU3ZTQwMC5zZXRDb250ZW50KGh0bWxfMjBhNjU2ODg1ZGVjNDgyNjg1ZWI0OWNjZTE0MjJjZGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTUxNjcyZjc4YTgyNGIxZWFjYzQ3ZmRkZDA3ZDc2ZjMuYmluZFBvcHVwKHBvcHVwXzAwNjA5MWMyM2I2ZTQ4MGY5MjI4YjNiNjdlNTdlNDAwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzZkMDIyZmFmMjJlZTRmMDdhOGE5ZTRhZTRhYzcyN2UzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYyNjk1NiwtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83NGFlMmM4MzBiNWM0NGQwODJkNjY3NmYwMWM3ZjNjYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81ZGY4NDY2NWE4MmQ0Mjk4OWU0Y2NmZDRhMmIzZWNiOSA9ICQoJzxkaXYgaWQ9Imh0bWxfNWRmODQ2NjVhODJkNDI5ODllNGNjZmQ0YTJiM2VjYjkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlVuaXZlcnNpdHkgb2YgVG9yb250bywgSGFyYm9yZCwgRG93bnRvd24gVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzRhZTJjODMwYjVjNDRkMDgyZDY2NzZmMDFjN2YzY2Iuc2V0Q29udGVudChodG1sXzVkZjg0NjY1YTgyZDQyOTg5ZTRjY2ZkNGEyYjNlY2I5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzZkMDIyZmFmMjJlZTRmMDdhOGE5ZTRhZTRhYzcyN2UzLmJpbmRQb3B1cChwb3B1cF83NGFlMmM4MzBiNWM0NGQwODJkNjY3NmYwMWM3ZjNjYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jNmVlZmIyNmRhODc0NGRlYWRiZTZhYmYwZjczYzJiMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MTU3MDYsLTc5LjQ4NDQ0OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYmE1NGViODFiYjRkNGVlNDkxZDVlNTc5NDEyYjM1ZDMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2JlOWMxYWJmMDRmNGZlMmJiZmU5OGYyMjU2MTE3MzIgPSAkKCc8ZGl2IGlkPSJodG1sXzNiZTljMWFiZjA0ZjRmZTJiYmZlOThmMjI1NjExNzMyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SdW5ueW1lZGUsIFN3YW5zZWEsIFdlc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmE1NGViODFiYjRkNGVlNDkxZDVlNTc5NDEyYjM1ZDMuc2V0Q29udGVudChodG1sXzNiZTljMWFiZjA0ZjRmZTJiYmZlOThmMjI1NjExNzMyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2M2ZWVmYjI2ZGE4NzQ0ZGVhZGJlNmFiZjBmNzNjMmIwLmJpbmRQb3B1cChwb3B1cF9iYTU0ZWI4MWJiNGQ0ZWU0OTFkNWU1Nzk0MTJiMzVkMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hMDExMGMxZTBmMTI0Y2Y2YTEzYWMwMmMzMmY4NGMzZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4MTYzNzUsLTc5LjMwNDMwMjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTZiYzI2NGNiNGM4NDFlZTgxZTc0MmI5YmVjMDFiNDIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGM0YTNmZjFmMTZjNDU0MmE5YjE5NTM0MGYzYTZkMzAgPSAkKCc8ZGl2IGlkPSJodG1sXzhjNGEzZmYxZjE2YzQ1NDJhOWIxOTUzNDBmM2E2ZDMwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DbGFya3MgQ29ybmVycywgVGFtIE8mIzM5O1NoYW50ZXIsIFN1bGxpdmFuLCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTZiYzI2NGNiNGM4NDFlZTgxZTc0MmI5YmVjMDFiNDIuc2V0Q29udGVudChodG1sXzhjNGEzZmYxZjE2YzQ1NDJhOWIxOTUzNDBmM2E2ZDMwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2EwMTEwYzFlMGYxMjRjZjZhMTNhYzAyYzMyZjg0YzNlLmJpbmRQb3B1cChwb3B1cF9lNmJjMjY0Y2I0Yzg0MWVlODFlNzQyYjliZWMwMWI0Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lZDM3ZmJhNTNjOGI0OGMzOWEyNWRlYjE5OGJiNzBmZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4OTU3NDMsLTc5LjM4MzE1OTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzc5NjljYTgzZTAxNDRiYjA5ZDI3MTM0Njg3MWQ1YmMyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzAyNGEyNTcyZTA5MzQ5Nzc4ODJiZDg2NzEyMDQ3MzAzID0gJCgnPGRpdiBpZD0iaHRtbF8wMjRhMjU3MmUwOTM0OTc3ODgyYmQ4NjcxMjA0NzMwMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TW9vcmUgUGFyaywgU3VtbWVyaGlsbCBFYXN0LCBDZW50cmFsIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzc5NjljYTgzZTAxNDRiYjA5ZDI3MTM0Njg3MWQ1YmMyLnNldENvbnRlbnQoaHRtbF8wMjRhMjU3MmUwOTM0OTc3ODgyYmQ4NjcxMjA0NzMwMyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lZDM3ZmJhNTNjOGI0OGMzOWEyNWRlYjE5OGJiNzBmZS5iaW5kUG9wdXAocG9wdXBfNzk2OWNhODNlMDE0NGJiMDlkMjcxMzQ2ODcxZDViYzIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTllNTcyOWVlNTU3NDcwMmIzZjIxN2ViNjA1ZGVlYzAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTMyMDU3LC03OS40MDAwNDkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZhNGVmYWYwMTAyMDRiNzFiZWM5ZmQ0MDI0ODhjZTYyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2RlZjI1Yjc1ZjE2MTQ0ZTI5MjUxZDZiZjgxYTRiNGUxID0gJCgnPGRpdiBpZD0iaHRtbF9kZWYyNWI3NWYxNjE0NGUyOTI1MWQ2YmY4MWE0YjRlMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+S2Vuc2luZ3RvbiBNYXJrZXQsIENoaW5hdG93biwgR3JhbmdlIFBhcmssIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzZhNGVmYWYwMTAyMDRiNzFiZWM5ZmQ0MDI0ODhjZTYyLnNldENvbnRlbnQoaHRtbF9kZWYyNWI3NWYxNjE0NGUyOTI1MWQ2YmY4MWE0YjRlMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85OWU1NzI5ZWU1NTc0NzAyYjNmMjE3ZWI2MDVkZWVjMC5iaW5kUG9wdXAocG9wdXBfNmE0ZWZhZjAxMDIwNGI3MWJlYzlmZDQwMjQ4OGNlNjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWI0ZDliMzJlZWFhNGY1YWFkNzdiOTE1ZDdkYTU2NmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MTUyNTIyLC03OS4yODQ1NzcyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzg2ODM1OTFmYmIzZjQzZGRhNmYxNDljYWQwNWJkNzUxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ZmMzc0YmU1ODk5MjQ1YTNhYjUyMDUzZGZhMDI1ZGQzID0gJCgnPGRpdiBpZD0iaHRtbF9mZjM3NGJlNTg5OTI0NWEzYWI1MjA1M2RmYTAyNWRkMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWlsbGlrZW4sIEFnaW5jb3VydCBOb3J0aCwgU3RlZWxlcyBFYXN0LCBMJiMzOTtBbW9yZWF1eCBFYXN0LCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODY4MzU5MWZiYjNmNDNkZGE2ZjE0OWNhZDA1YmQ3NTEuc2V0Q29udGVudChodG1sX2ZmMzc0YmU1ODk5MjQ1YTNhYjUyMDUzZGZhMDI1ZGQzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFiNGQ5YjMyZWVhYTRmNWFhZDc3YjkxNWQ3ZGE1NjZiLmJpbmRQb3B1cChwb3B1cF84NjgzNTkxZmJiM2Y0M2RkYTZmMTQ5Y2FkMDViZDc1MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80OWMwMTI2ZDEwZWU0OGM1OWZlODQyYzk4ZjFhNDczNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4NjQxMjI5OTk5OTk5LC03OS40MDAwNDkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JjM2IwNTJiYjE1MjQ1ZDA4MzQ1ZDBkYjczZWJlNWFiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MwODFiZWZlYzM0MjRiZmRiY2FjZTA5Yzk0YmYxZDQyID0gJCgnPGRpdiBpZD0iaHRtbF9jMDgxYmVmZWMzNDI0YmZkYmNhY2UwOWM5NGJmMWQ0MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3VtbWVyaGlsbCBXZXN0LCBSYXRobmVsbHksIFNvdXRoIEhpbGwsIEZvcmVzdCBIaWxsIFNFLCBEZWVyIFBhcmssIENlbnRyYWwgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmMzYjA1MmJiMTUyNDVkMDgzNDVkMGRiNzNlYmU1YWIuc2V0Q29udGVudChodG1sX2MwODFiZWZlYzM0MjRiZmRiY2FjZTA5Yzk0YmYxZDQyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQ5YzAxMjZkMTBlZTQ4YzU5ZmU4NDJjOThmMWE0NzM0LmJpbmRQb3B1cChwb3B1cF9iYzNiMDUyYmIxNTI0NWQwODM0NWQwZGI3M2ViZTVhYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xMjgxYWQ3MjlkZDI0NDk5YmMyZDcwMDc3YTRmZDFkYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODk0NjcsLTc5LjM5NDQxOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjBiYmI5MmYwNzdkNDYyMjhlMmZmNjYzMzdmYjg1Y2YgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzZhODdiZTUwNGRhNDMzZjljZGMyNWU2ZWQzMzBhMDkgPSAkKCc8ZGl2IGlkPSJodG1sX2M2YTg3YmU1MDRkYTQzM2Y5Y2RjMjVlNmVkMzMwYTA5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DTiBUb3dlciwgS2luZyBhbmQgU3BhZGluYSwgUmFpbHdheSBMYW5kcywgSGFyYm91cmZyb250IFdlc3QsIEJhdGh1cnN0IFF1YXksIFNvdXRoIE5pYWdhcmEsIElzbGFuZCBhaXJwb3J0LCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMGJiYjkyZjA3N2Q0NjIyOGUyZmY2NjMzN2ZiODVjZi5zZXRDb250ZW50KGh0bWxfYzZhODdiZTUwNGRhNDMzZjljZGMyNWU2ZWQzMzBhMDkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTI4MWFkNzI5ZGQyNDQ5OWJjMmQ3MDA3N2E0ZmQxZGMuYmluZFBvcHVwKHBvcHVwX2IwYmJiOTJmMDc3ZDQ2MjI4ZTJmZjY2MzM3ZmI4NWNmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzBjOThhOTM2NzE1OTQ1NzZhYjFjMGM4MTFjZmYzYThhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjA1NjQ2NiwtNzkuNTAxMzIwNzAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfN2Y4YTcyYjYzYTliNDhmYWEyYmVjMzRhNGY4MjJjZTQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNWFmZWNmMTg0ZjRkNDU5MzkyYTRmYjU3MTY2OTEzZmYgPSAkKCc8ZGl2IGlkPSJodG1sXzVhZmVjZjE4NGY0ZDQ1OTM5MmE0ZmI1NzE2NjkxM2ZmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OZXcgVG9yb250bywgTWltaWNvIFNvdXRoLCBIdW1iZXIgQmF5IFNob3JlcywgRXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83ZjhhNzJiNjNhOWI0OGZhYTJiZWMzNGE0ZjgyMmNlNC5zZXRDb250ZW50KGh0bWxfNWFmZWNmMTg0ZjRkNDU5MzkyYTRmYjU3MTY2OTEzZmYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGM5OGE5MzY3MTU5NDU3NmFiMWMwYzgxMWNmZjNhOGEuYmluZFBvcHVwKHBvcHVwXzdmOGE3MmI2M2E5YjQ4ZmFhMmJlYzM0YTRmODIyY2U0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RhNWNmOWY3NzA5NzQ4ZjA5N2YzY2ZmOTYyNjUzYmQwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM5NDE2Mzk5OTk5OTk2LC03OS41ODg0MzY5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2I0NThkYzE2MWMyZjRkNDk4OTg1YjgxZmY3MWI3ZTYxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzkxZjdlNmQzMWU2NjRkYTE4NGY5N2QwZGI2NGY5MGI5ID0gJCgnPGRpdiBpZD0iaHRtbF85MWY3ZTZkMzFlNjY0ZGExODRmOTdkMGRiNjRmOTBiOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U291dGggU3RlZWxlcywgU2lsdmVyc3RvbmUsIEh1bWJlcmdhdGUsIEphbWVzdG93biwgTW91bnQgT2xpdmUsIEJlYXVtb25kIEhlaWdodHMsIFRoaXN0bGV0b3duLCBBbGJpb24gR2FyZGVucywgRXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iNDU4ZGMxNjFjMmY0ZDQ5ODk4NWI4MWZmNzFiN2U2MS5zZXRDb250ZW50KGh0bWxfOTFmN2U2ZDMxZTY2NGRhMTg0Zjk3ZDBkYjY0ZjkwYjkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGE1Y2Y5Zjc3MDk3NDhmMDk3ZjNjZmY5NjI2NTNiZDAuYmluZFBvcHVwKHBvcHVwX2I0NThkYzE2MWMyZjRkNDk4OTg1YjgxZmY3MWI3ZTYxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E1ZTQzOWQ3YjkyMzRjMmY5MDNjYTYyZTM2ZGU0MTMxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzk5NTI1MjAwMDAwMDA1LC03OS4zMTgzODg3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzEyNmE0ZGVjMzgyYjQ4ZTdiZjI0YjQ3ZGE4NGNlZmJhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2E4N2FmYzdlZDQ1YTRmYWI5ZDQ5NWUwYjY2YTViNGZmID0gJCgnPGRpdiBpZD0iaHRtbF9hODdhZmM3ZWQ0NWE0ZmFiOWQ0OTVlMGI2NmE1YjRmZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3RlZWxlcyBXZXN0LCBMJiMzOTtBbW9yZWF1eCBXZXN0LCBTY2FyYm9yb3VnaDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTI2YTRkZWMzODJiNDhlN2JmMjRiNDdkYTg0Y2VmYmEuc2V0Q29udGVudChodG1sX2E4N2FmYzdlZDQ1YTRmYWI5ZDQ5NWUwYjY2YTViNGZmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2E1ZTQzOWQ3YjkyMzRjMmY5MDNjYTYyZTM2ZGU0MTMxLmJpbmRQb3B1cChwb3B1cF8xMjZhNGRlYzM4MmI0OGU3YmYyNGI0N2RhODRjZWZiYSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xZmQ4OTM2ZjBjYTA0MzE2YTMxMGYxMDYzNWNhNTVhOCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3OTU2MjYsLTc5LjM3NzUyOTQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2VmZGJlMGM3NDU3YjQ0ZTg5ZjdkNWI1ZWFhNzc2YzAzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2YwZTQ4MTRhZTIyYjQxNDRiNWViMTAxYjFmZjFkYTJiID0gJCgnPGRpdiBpZD0iaHRtbF9mMGU0ODE0YWUyMmI0MTQ0YjVlYjEwMWIxZmYxZGEyYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um9zZWRhbGUsIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2VmZGJlMGM3NDU3YjQ0ZTg5ZjdkNWI1ZWFhNzc2YzAzLnNldENvbnRlbnQoaHRtbF9mMGU0ODE0YWUyMmI0MTQ0YjVlYjEwMWIxZmYxZGEyYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZmQ4OTM2ZjBjYTA0MzE2YTMxMGYxMDYzNWNhNTVhOC5iaW5kUG9wdXAocG9wdXBfZWZkYmUwYzc0NTdiNDRlODlmN2Q1YjVlYWE3NzZjMDMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGEwNjBiNDc2MmVlNDZmZjk4NWM0MTEzZjg2ZjZlY2MgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDY0MzUyLC03OS4zNzQ4NDU5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jYzlhYjU3ODNhNmU0YjEwYWRkYzVmNmE4ZGViN2YwZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84MzA3MTc3OTk3Zjg0ODEyOTE0N2JlNjEzYjFhMTg4OCA9ICQoJzxkaXYgaWQ9Imh0bWxfODMwNzE3Nzk5N2Y4NDgxMjkxNDdiZTYxM2IxYTE4ODgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0biBBIFBPIEJveGVzLCBEb3dudG93biBUb3JvbnRvPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jYzlhYjU3ODNhNmU0YjEwYWRkYzVmNmE4ZGViN2YwZi5zZXRDb250ZW50KGh0bWxfODMwNzE3Nzk5N2Y4NDgxMjkxNDdiZTYxM2IxYTE4ODgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNGEwNjBiNDc2MmVlNDZmZjk4NWM0MTEzZjg2ZjZlY2MuYmluZFBvcHVwKHBvcHVwX2NjOWFiNTc4M2E2ZTRiMTBhZGRjNWY2YThkZWI3ZjBmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzgwNDI1NzMzNjg0ZTRiMzRhZjM4NWIwNjFlMzJlN2M4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjAyNDEzNzAwMDAwMDEsLTc5LjU0MzQ4NDA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2VmOWY3ZTE4ZjJkMjRjZDM4MWNhMDQxN2JhMjNlYjhiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzMzOTc4OThiNDgwMTQyMWZiN2ZhYzRlOWNjYTdkNWM3ID0gJCgnPGRpdiBpZD0iaHRtbF8zMzk3ODk4YjQ4MDE0MjFmYjdmYWM0ZTljY2E3ZDVjNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QWxkZXJ3b29kLCBMb25nIEJyYW5jaCwgRXRvYmljb2tlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lZjlmN2UxOGYyZDI0Y2QzODFjYTA0MTdiYTIzZWI4Yi5zZXRDb250ZW50KGh0bWxfMzM5Nzg5OGI0ODAxNDIxZmI3ZmFjNGU5Y2NhN2Q1YzcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODA0MjU3MzM2ODRlNGIzNGFmMzg1YjA2MWUzMmU3YzguYmluZFBvcHVwKHBvcHVwX2VmOWY3ZTE4ZjJkMjRjZDM4MWNhMDQxN2JhMjNlYjhiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzcwZGY5Yjk3MzQ2ZDRmZGZhOTZmY2I5MGY5NjBkYWYyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzA2NzQ4Mjk5OTk5OTk0LC03OS41OTQwNTQ0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzhjY2JiYWI2OTkwNDRmZjA4MTYxZDE5OWE0YmYxNDAwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzU0ZjBjY2EyZjMyMDQyNzRhMDUyY2RmNTljMTZjMDkxID0gJCgnPGRpdiBpZD0iaHRtbF81NGYwY2NhMmYzMjA0Mjc0YTA1MmNkZjU5YzE2YzA5MSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGh3ZXN0LCBXZXN0IEh1bWJlciAtIENsYWlydmlsbGUsIEV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGNjYmJhYjY5OTA0NGZmMDgxNjFkMTk5YTRiZjE0MDAuc2V0Q29udGVudChodG1sXzU0ZjBjY2EyZjMyMDQyNzRhMDUyY2RmNTljMTZjMDkxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzcwZGY5Yjk3MzQ2ZDRmZGZhOTZmY2I5MGY5NjBkYWYyLmJpbmRQb3B1cChwb3B1cF84Y2NiYmFiNjk5MDQ0ZmYwODE2MWQxOTlhNGJmMTQwMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81NGRmMGI4MTAzZTg0NDgxODE2ODA2OTRkZGU1ZTViZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjgzNjEyNDcwMDAwMDAwNiwtNzkuMjA1NjM2MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODNmMTA1ZDYxNGYzNDUyNzg1MjNiOTYxMjQzMjg2ZTQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWU2MzI3MDkwOGMxNDBmNmE5NjM0N2I5Y2IxZTA2OWQgPSAkKCc8ZGl2IGlkPSJodG1sXzFlNjMyNzA5MDhjMTQwZjZhOTYzNDdiOWNiMWUwNjlkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5VcHBlciBSb3VnZSwgU2NhcmJvcm91Z2g8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzgzZjEwNWQ2MTRmMzQ1Mjc4NTIzYjk2MTI0MzI4NmU0LnNldENvbnRlbnQoaHRtbF8xZTYzMjcwOTA4YzE0MGY2YTk2MzQ3YjljYjFlMDY5ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81NGRmMGI4MTAzZTg0NDgxODE2ODA2OTRkZGU1ZTViZC5iaW5kUG9wdXAocG9wdXBfODNmMTA1ZDYxNGYzNDUyNzg1MjNiOTYxMjQzMjg2ZTQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTk3ZTIwNzk1NWFlNGUyYWFjNmExZTE3NTFiOTBmZjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njc5NjcsLTc5LjM2NzY3NTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjE5ZDc5ZjUyYTQ0NDA3MDliYmRhOTNkMjA0ZDM0YWQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzE0ZjZlY2I0NzQyNDA3MTgxYzc1NWI2ZmJlYmI4NzggPSAkKCc8ZGl2IGlkPSJodG1sXzcxNGY2ZWNiNDc0MjQwNzE4MWM3NTViNmZiZWJiODc4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdC4gSmFtZXMgVG93biwgQ2FiYmFnZXRvd24sIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YxOWQ3OWY1MmE0NDQwNzA5YmJkYTkzZDIwNGQzNGFkLnNldENvbnRlbnQoaHRtbF83MTRmNmVjYjQ3NDI0MDcxODFjNzU1YjZmYmViYjg3OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85OTdlMjA3OTU1YWU0ZTJhYWM2YTFlMTc1MWI5MGZmMi5iaW5kUG9wdXAocG9wdXBfZjE5ZDc5ZjUyYTQ0NDA3MDliYmRhOTNkMjA0ZDM0YWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjlmZWUzYzJiNDZjNGJkNzhmYTMxMDE1Mzk4ZDU4YzcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDg0MjkyLC03OS4zODIyODAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NkNTk0YzEwZGQ4ODRmNDJhYTZhOGNkNDkwZDc0NTMxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ZmOGU1YzU1ZDczODQxM2Y4YmQ5NTVjNGYyNjJmM2QwID0gJCgnPGRpdiBpZD0iaHRtbF9mZjhlNWM1NWQ3Mzg0MTNmOGJkOTU1YzRmMjYyZjNkMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Rmlyc3QgQ2FuYWRpYW4gUGxhY2UsIFVuZGVyZ3JvdW5kIGNpdHksIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NkNTk0YzEwZGQ4ODRmNDJhYTZhOGNkNDkwZDc0NTMxLnNldENvbnRlbnQoaHRtbF9mZjhlNWM1NWQ3Mzg0MTNmOGJkOTU1YzRmMjYyZjNkMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yOWZlZTNjMmI0NmM0YmQ3OGZhMzEwMTUzOThkNThjNy5iaW5kUG9wdXAocG9wdXBfY2Q1OTRjMTBkZDg4NGY0MmFhNmE4Y2Q0OTBkNzQ1MzEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNWI1MTljOGQyNjU0NDc0ODgyMjM3YmUzYTNhMzJlNmQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTM2NTM2MDAwMDAwMDUsLTc5LjUwNjk0MzZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjE0NjgxMTY3OTA2NDM1MWEzMzEwMjA2ZWI2ZjYzNjkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2VmZjljMzJjMGUzNGFiNWJlYjMxMWNhY2YwYWIzMWQgPSAkKCc8ZGl2IGlkPSJodG1sXzNlZmY5YzMyYzBlMzRhYjViZWIzMTFjYWNmMGFiMzFkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgS2luZ3N3YXksIE1vbnRnb21lcnkgUm9hZCwgT2xkIE1pbGwgTm9ydGgsIEV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNjE0NjgxMTY3OTA2NDM1MWEzMzEwMjA2ZWI2ZjYzNjkuc2V0Q29udGVudChodG1sXzNlZmY5YzMyYzBlMzRhYjViZWIzMTFjYWNmMGFiMzFkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzViNTE5YzhkMjY1NDQ3NDg4MjIzN2JlM2EzYTMyZTZkLmJpbmRQb3B1cChwb3B1cF82MTQ2ODExNjc5MDY0MzUxYTMzMTAyMDZlYjZmNjM2OSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80N2RkN2MxYjcxZWU0MDRkOTUwYWI0NjhiOTc3OTAxNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2NTg1OTksLTc5LjM4MzE1OTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2I0NjgzMjNkOTBiZDQxZGE5MjRkMjhjNDk4MzEwMjU5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MwMjk0NzQ1ODExZjQ4M2M4YzI3MjI4OTFlNTliNmVmID0gJCgnPGRpdiBpZD0iaHRtbF9jMDI5NDc0NTgxMWY0ODNjOGMyNzIyODkxZTU5YjZlZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2h1cmNoIGFuZCBXZWxsZXNsZXksIERvd250b3duIFRvcm9udG88L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I0NjgzMjNkOTBiZDQxZGE5MjRkMjhjNDk4MzEwMjU5LnNldENvbnRlbnQoaHRtbF9jMDI5NDc0NTgxMWY0ODNjOGMyNzIyODkxZTU5YjZlZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80N2RkN2MxYjcxZWU0MDRkOTUwYWI0NjhiOTc3OTAxNC5iaW5kUG9wdXAocG9wdXBfYjQ2ODMyM2Q5MGJkNDFkYTkyNGQyOGM0OTgzMTAyNTkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzI0ZDhlYjg3YjEyNGEwM2JhNmY1YmQzYzgxNTJiMjggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjI3NDM5LC03OS4zMjE1NThdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYjQ0ODdhMDk5OWNjNDgwMDhkYWI1ZDA1ZDRjNjA0MDcpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODMyODg3Y2Q1YTQ1NDU5YTg2MzI3ZTRhNDM4OWNjNjcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmQwMzU2YmVhZGQ0NGJmMjliOGViY2U1NTQ2MmZhZmMgPSAkKCc8ZGl2IGlkPSJodG1sX2ZkMDM1NmJlYWRkNDRiZjI5YjhlYmNlNTU0NjJmYWZjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CdXNpbmVzcyByZXBseSBtYWlsIFByb2Nlc3NpbmcgQ2VudHJlLCBTb3V0aCBDZW50cmFsIExldHRlciBQcm9jZXNzaW5nIFBsYW50IFRvcm9udG8sIEVhc3QgVG9yb250bzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODMyODg3Y2Q1YTQ1NDU5YTg2MzI3ZTRhNDM4OWNjNjcuc2V0Q29udGVudChodG1sX2ZkMDM1NmJlYWRkNDRiZjI5YjhlYmNlNTU0NjJmYWZjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzMyNGQ4ZWI4N2IxMjRhMDNiYTZmNWJkM2M4MTUyYjI4LmJpbmRQb3B1cChwb3B1cF84MzI4ODdjZDVhNDU0NTlhODYzMjdlNGE0Mzg5Y2M2Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iMTE4MWU2N2ZjODI0ZWUzYWIwZjg3MjY4ZjZmZTlkZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYzNjI1NzksLTc5LjQ5ODUwOTA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2I0NDg3YTA5OTljYzQ4MDA4ZGFiNWQwNWQ0YzYwNDA3KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVhMDU0YmZlYjJjMDQwYTc5NTdkY2MxYWY2NTg0ZDM5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2M1YWRhOTkyOTgwYzQwNzI4MDE2MmRjNDIwZGUyMTU1ID0gJCgnPGRpdiBpZD0iaHRtbF9jNWFkYTk5Mjk4MGM0MDcyODAxNjJkYzQyMGRlMjE1NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+T2xkIE1pbGwgU291dGgsIEtpbmcmIzM5O3MgTWlsbCBQYXJrLCBTdW5ueWxlYSwgSHVtYmVyIEJheSwgTWltaWNvIE5FLCBUaGUgUXVlZW5zd2F5IEVhc3QsIFJveWFsIFlvcmsgU291dGggRWFzdCwgS2luZ3N3YXkgUGFyayBTb3V0aCBFYXN0LCBFdG9iaWNva2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVhMDU0YmZlYjJjMDQwYTc5NTdkY2MxYWY2NTg0ZDM5LnNldENvbnRlbnQoaHRtbF9jNWFkYTk5Mjk4MGM0MDcyODAxNjJkYzQyMGRlMjE1NSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iMTE4MWU2N2ZjODI0ZWUzYWIwZjg3MjY4ZjZmZTlkZi5iaW5kUG9wdXAocG9wdXBfNWEwNTRiZmViMmMwNDBhNzk1N2RjYzFhZjY1ODRkMzkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDNlMmQ3YmQ0YTY3NDRlMjg1ZmM5MTk4MjVkYzQ4YjAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Mjg4NDA4LC03OS41MjA5OTk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9iNDQ4N2EwOTk5Y2M0ODAwOGRhYjVkMDVkNGM2MDQwNyk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jYjg1Nzg0ODMwYTk0ZmY0YTc3NjA5YzAzNDk0MDBkNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lZjA0OWZkMzk4NDQ0ZWUwOGUzZTNhNmVmNzY3Y2Q5ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfZWYwNDlmZDM5ODQ0NGVlMDhlM2UzYTZlZjc2N2NkOWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1pbWljbyBOVywgVGhlIFF1ZWVuc3dheSBXZXN0LCBTb3V0aCBvZiBCbG9vciwgS2luZ3N3YXkgUGFyayBTb3V0aCBXZXN0LCBSb3lhbCBZb3JrIFNvdXRoIFdlc3QsIEV0b2JpY29rZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2I4NTc4NDgzMGE5NGZmNGE3NzYwOWMwMzQ5NDAwZDYuc2V0Q29udGVudChodG1sX2VmMDQ5ZmQzOTg0NDRlZTA4ZTNlM2E2ZWY3NjdjZDlkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2QzZTJkN2JkNGE2NzQ0ZTI4NWZjOTE5ODI1ZGM0OGIwLmJpbmRQb3B1cChwb3B1cF9jYjg1Nzg0ODMwYTk0ZmY0YTc3NjA5YzAzNDk0MDBkNik7CgogICAgICAgICAgICAKICAgICAgICAKPC9zY3JpcHQ+ onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
CLIENT_ID = 'TTZNLNIM0I10RWP34ZTLOZZ5RAI1RO0MHJKALZNIPRHZC215' # your Foursquare ID
CLIENT_SECRET = 'S2OS1CRAGER4NXWO2XM1DULEZIS3STLXQYMMIIDYHNSLR3GO' # your Foursquare Secret
VERSION = '20180605' # Foursquare API version
LIMIT = 100 # A default Foursquare API limit value

print('Your credentails:')
print('CLIENT_ID: ' + CLIENT_ID)
print('CLIENT_SECRET:' + CLIENT_SECRET)
```

    Your credentails:
    CLIENT_ID: TTZNLNIM0I10RWP34ZTLOZZ5RAI1RO0MHJKALZNIPRHZC215
    CLIENT_SECRET:S2OS1CRAGER4NXWO2XM1DULEZIS3STLXQYMMIIDYHNSLR3GO



```python
toronto_df.head()
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
      <th>PostalCode</th>
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>M3A</td>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M4A</td>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>M5A</td>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M6A</td>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M7A</td>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
  </tbody>
</table>
</div>



Filtering and Grouping Data


```python
toronto_filtered_df = toronto_df.drop('PostalCode', 1)

```


```python
toronto_filtered_df.head()
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
      <th>Borough</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>North York</td>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>1</th>
      <td>North York</td>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Downtown Toronto</td>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>North York</td>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Downtown Toronto</td>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
  </tbody>
</table>
</div>




```python
toronto_grouped = toronto_filtered_df.groupby('Neighbourhood').mean().reset_index()
toronto_grouped
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
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agincourt</td>
      <td>43.794200</td>
      <td>-79.262029</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alderwood, Long Branch</td>
      <td>43.602414</td>
      <td>-79.543484</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bathurst Manor, Wilson Heights, Downsview North</td>
      <td>43.754328</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bayview Village</td>
      <td>43.786947</td>
      <td>-79.385975</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bedford Park, Lawrence Manor East</td>
      <td>43.733283</td>
      <td>-79.419750</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Berczy Park</td>
      <td>43.644771</td>
      <td>-79.373306</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Birch Cliff, Cliffside West</td>
      <td>43.692657</td>
      <td>-79.264848</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Brockton, Parkdale Village, Exhibition Place</td>
      <td>43.636847</td>
      <td>-79.428191</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Business reply mail Processing Centre, South C...</td>
      <td>43.662744</td>
      <td>-79.321558</td>
    </tr>
    <tr>
      <th>9</th>
      <td>CN Tower, King and Spadina, Railway Lands, Har...</td>
      <td>43.628947</td>
      <td>-79.394420</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Caledonia-Fairbanks</td>
      <td>43.689026</td>
      <td>-79.453512</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Canada Post Gateway Processing Centre</td>
      <td>43.636966</td>
      <td>-79.615819</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Cedarbrae</td>
      <td>43.773136</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Central Bay Street</td>
      <td>43.657952</td>
      <td>-79.387383</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Christie</td>
      <td>43.669542</td>
      <td>-79.422564</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Church and Wellesley</td>
      <td>43.665860</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Clarks Corners, Tam O'Shanter, Sullivan</td>
      <td>43.781638</td>
      <td>-79.304302</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Cliffside, Cliffcrest, Scarborough Village West</td>
      <td>43.716316</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Commerce Court, Victoria Hotel</td>
      <td>43.648198</td>
      <td>-79.379817</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Davisville</td>
      <td>43.704324</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Davisville North</td>
      <td>43.712751</td>
      <td>-79.390197</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Del Ray, Mount Dennis, Keelsdale and Silverthorn</td>
      <td>43.691116</td>
      <td>-79.476013</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Don Mills</td>
      <td>43.735903</td>
      <td>-79.346555</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Dorset Park, Wexford Heights, Scarborough Town...</td>
      <td>43.757410</td>
      <td>-79.273304</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Downsview</td>
      <td>43.741654</td>
      <td>-79.497101</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Dufferin, Dovercourt Village</td>
      <td>43.669005</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>26</th>
      <td>East Toronto, Broadview North (Old East York)</td>
      <td>43.685347</td>
      <td>-79.338106</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Eringate, Bloordale Gardens, Old Burnhamthorpe...</td>
      <td>43.643515</td>
      <td>-79.577201</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Fairview, Henry Farm, Oriole</td>
      <td>43.778517</td>
      <td>-79.346556</td>
    </tr>
    <tr>
      <th>29</th>
      <td>First Canadian Place, Underground city</td>
      <td>43.648429</td>
      <td>-79.382280</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Forest Hill North &amp; West, Forest Hill Road Park</td>
      <td>43.696948</td>
      <td>-79.411307</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Garden District, Ryerson</td>
      <td>43.657162</td>
      <td>-79.378937</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Glencairn</td>
      <td>43.709577</td>
      <td>-79.445073</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Golden Mile, Clairlea, Oakridge</td>
      <td>43.711112</td>
      <td>-79.284577</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Guildwood, Morningside, West Hill</td>
      <td>43.763573</td>
      <td>-79.188711</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Harbourfront East, Union Station, Toronto Islands</td>
      <td>43.640816</td>
      <td>-79.381752</td>
    </tr>
    <tr>
      <th>36</th>
      <td>High Park, The Junction South</td>
      <td>43.661608</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Hillcrest Village</td>
      <td>43.803762</td>
      <td>-79.363452</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Humber Summit</td>
      <td>43.756303</td>
      <td>-79.565963</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Humberlea, Emery</td>
      <td>43.724766</td>
      <td>-79.532242</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Humewood-Cedarvale</td>
      <td>43.693781</td>
      <td>-79.428191</td>
    </tr>
    <tr>
      <th>41</th>
      <td>India Bazaar, The Beaches West</td>
      <td>43.668999</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Islington Avenue, Humber Valley Village</td>
      <td>43.667856</td>
      <td>-79.532242</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Kennedy Park, Ionview, East Birchmount Park</td>
      <td>43.727929</td>
      <td>-79.262029</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Kensington Market, Chinatown, Grange Park</td>
      <td>43.653206</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Kingsview Village, St. Phillips, Martin Grove ...</td>
      <td>43.688905</td>
      <td>-79.554724</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Lawrence Manor, Lawrence Heights</td>
      <td>43.718518</td>
      <td>-79.464763</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Lawrence Park</td>
      <td>43.728020</td>
      <td>-79.388790</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Leaside</td>
      <td>43.709060</td>
      <td>-79.363452</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Little Portugal, Trinity</td>
      <td>43.647927</td>
      <td>-79.419750</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Malvern, Rouge</td>
      <td>43.806686</td>
      <td>-79.194353</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Milliken, Agincourt North, Steeles East, L'Amo...</td>
      <td>43.815252</td>
      <td>-79.284577</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Mimico NW, The Queensway West, South of Bloor,...</td>
      <td>43.628841</td>
      <td>-79.520999</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Moore Park, Summerhill East</td>
      <td>43.689574</td>
      <td>-79.383160</td>
    </tr>
    <tr>
      <th>54</th>
      <td>New Toronto, Mimico South, Humber Bay Shores</td>
      <td>43.605647</td>
      <td>-79.501321</td>
    </tr>
    <tr>
      <th>55</th>
      <td>North Park, Maple Leaf Park, Upwood Park</td>
      <td>43.713756</td>
      <td>-79.490074</td>
    </tr>
    <tr>
      <th>56</th>
      <td>North Toronto West, Lawrence Park</td>
      <td>43.715383</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Northwest, West Humber - Clairville</td>
      <td>43.706748</td>
      <td>-79.594054</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Northwood Park, York University</td>
      <td>43.767980</td>
      <td>-79.487262</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Old Mill South, King's Mill Park, Sunnylea, Hu...</td>
      <td>43.636258</td>
      <td>-79.498509</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Parkdale, Roncesvalles</td>
      <td>43.648960</td>
      <td>-79.456325</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Parkview Hill, Woodbine Gardens</td>
      <td>43.706397</td>
      <td>-79.309937</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Parkwoods</td>
      <td>43.753259</td>
      <td>-79.329656</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Queen's Park, Ontario Provincial Government</td>
      <td>43.662301</td>
      <td>-79.389494</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Regent Park, Harbourfront</td>
      <td>43.654260</td>
      <td>-79.360636</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Richmond, Adelaide, King</td>
      <td>43.650571</td>
      <td>-79.384568</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Rosedale</td>
      <td>43.679563</td>
      <td>-79.377529</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Roselawn</td>
      <td>43.711695</td>
      <td>-79.416936</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Rouge Hill, Port Union, Highland Creek</td>
      <td>43.784535</td>
      <td>-79.160497</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Runnymede, Swansea</td>
      <td>43.651571</td>
      <td>-79.484450</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Runnymede, The Junction North</td>
      <td>43.673185</td>
      <td>-79.487262</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Scarborough Village</td>
      <td>43.744734</td>
      <td>-79.239476</td>
    </tr>
    <tr>
      <th>72</th>
      <td>South Steeles, Silverstone, Humbergate, Jamest...</td>
      <td>43.739416</td>
      <td>-79.588437</td>
    </tr>
    <tr>
      <th>73</th>
      <td>St. James Town</td>
      <td>43.651494</td>
      <td>-79.375418</td>
    </tr>
    <tr>
      <th>74</th>
      <td>St. James Town, Cabbagetown</td>
      <td>43.667967</td>
      <td>-79.367675</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Steeles West, L'Amoreaux West</td>
      <td>43.799525</td>
      <td>-79.318389</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Stn A PO Boxes</td>
      <td>43.646435</td>
      <td>-79.374846</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Studio District</td>
      <td>43.659526</td>
      <td>-79.340923</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Summerhill West, Rathnelly, South Hill, Forest...</td>
      <td>43.686412</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>79</th>
      <td>The Annex, North Midtown, Yorkville</td>
      <td>43.672710</td>
      <td>-79.405678</td>
    </tr>
    <tr>
      <th>80</th>
      <td>The Beaches</td>
      <td>43.676357</td>
      <td>-79.293031</td>
    </tr>
    <tr>
      <th>81</th>
      <td>The Danforth West, Riverdale</td>
      <td>43.679557</td>
      <td>-79.352188</td>
    </tr>
    <tr>
      <th>82</th>
      <td>The Kingsway, Montgomery Road, Old Mill North</td>
      <td>43.653654</td>
      <td>-79.506944</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Thorncliffe Park</td>
      <td>43.705369</td>
      <td>-79.349372</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Toronto Dominion Centre, Design Exchange</td>
      <td>43.647177</td>
      <td>-79.381576</td>
    </tr>
    <tr>
      <th>85</th>
      <td>University of Toronto, Harbord</td>
      <td>43.662696</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Upper Rouge</td>
      <td>43.836125</td>
      <td>-79.205636</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Victoria Village</td>
      <td>43.725882</td>
      <td>-79.315572</td>
    </tr>
    <tr>
      <th>88</th>
      <td>West Deane Park, Princess Gardens, Martin Grov...</td>
      <td>43.650943</td>
      <td>-79.554724</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Westmount</td>
      <td>43.696319</td>
      <td>-79.532242</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Weston</td>
      <td>43.706876</td>
      <td>-79.518188</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Wexford, Maryvale</td>
      <td>43.750072</td>
      <td>-79.295849</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Willowdale, Newtonbrook</td>
      <td>43.789053</td>
      <td>-79.408493</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Willowdale, Willowdale East</td>
      <td>43.770120</td>
      <td>-79.408493</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Willowdale, Willowdale West</td>
      <td>43.782736</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Woburn</td>
      <td>43.770992</td>
      <td>-79.216917</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Woodbine Heights</td>
      <td>43.695344</td>
      <td>-79.318389</td>
    </tr>
    <tr>
      <th>97</th>
      <td>York Mills West</td>
      <td>43.752758</td>
      <td>-79.400049</td>
    </tr>
    <tr>
      <th>98</th>
      <td>York Mills, Silver Hills</td>
      <td>43.757490</td>
      <td>-79.374714</td>
    </tr>
  </tbody>
</table>
</div>



Creating Clusters


```python
# set number of clusters
kclusters = 5

#toronto_grouped_clustering = toronto_grouped.drop('Neighbourhood', 1)

# run k-means clustering
kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(toronto_grouped_clustering)

# check cluster labels generated for each row in the dataframe
kmeans.labels_[0:10] 
```




    array([4, 2, 3, 3, 3, 0, 1, 0, 1, 0], dtype=int32)




```python
toronto_grouped.shape
```




    (99, 3)




```python
# add clustering labels
toronto_grouped.insert(0, 'Cluster Labels', kmeans.labels_)

#manhattan_merged = manhattan_data
```


```python
toronto_grouped.head() # check the last columns!
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
      <th>Cluster Labels</th>
      <th>Neighbourhood</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>Agincourt</td>
      <td>43.794200</td>
      <td>-79.262029</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Alderwood, Long Branch</td>
      <td>43.602414</td>
      <td>-79.543484</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Bathurst Manor, Wilson Heights, Downsview North</td>
      <td>43.754328</td>
      <td>-79.442259</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Bayview Village</td>
      <td>43.786947</td>
      <td>-79.385975</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>Bedford Park, Lawrence Manor East</td>
      <td>43.733283</td>
      <td>-79.419750</td>
    </tr>
  </tbody>
</table>
</div>



Creating Clustered Map


```python
# create map
map_clusters = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i + x + (i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(toronto_grouped['Latitude'], toronto_grouped['Longitude'], toronto_grouped['Neighbourhood'], toronto_grouped['Cluster Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_clusters)
       
map_clusters
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZScsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDMuNjUzNDgxNywtNzkuMzgzOTM0N10sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfODg2YzQzNTE0ODQyNDUwMjhlNjZlYzg0MDQ0ZWExZjQgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk0YWFmNDdhYWQ1ZDRmMmZiODhiMmU5MDBmM2M2MjFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzk0MjAwMywtNzkuMjYyMDI5NDAwMDAwMDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzk4NzQzZTgxZmRmNDU0NTg5ZGJkZWM2YmEwMTc0ZGMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGE1MjcyZmJhOTYzNGY2OGFkMDcxZTRjOTg2YjBmYmYgPSAkKCc8ZGl2IGlkPSJodG1sXzhhNTI3MmZiYTk2MzRmNjhhZDA3MWU0Yzk4NmIwZmJmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5BZ2luY291cnQgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83OTg3NDNlODFmZGY0NTQ1ODlkYmRlYzZiYTAxNzRkYy5zZXRDb250ZW50KGh0bWxfOGE1MjcyZmJhOTYzNGY2OGFkMDcxZTRjOTg2YjBmYmYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTRhYWY0N2FhZDVkNGYyZmI4OGIyZTkwMGYzYzYyMWQuYmluZFBvcHVwKHBvcHVwXzc5ODc0M2U4MWZkZjQ1NDU4OWRiZGVjNmJhMDE3NGRjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI4OGI4ODg5YjAxMjQ5MTVhOGJiYzFkNDE4MDk3NjhmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjAyNDEzNzAwMDAwMDEsLTc5LjU0MzQ4NDA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y3NzQ3NTc0ODkwNjQ4MzFhMjk2MWU3NjY5MzAyYTBiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk3ZmQ1MjY5OTBjYjRjNmM5NGYyZGI0YmE4OTU2YzM3ID0gJCgnPGRpdiBpZD0iaHRtbF85N2ZkNTI2OTkwY2I0YzZjOTRmMmRiNGJhODk1NmMzNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QWxkZXJ3b29kLCBMb25nIEJyYW5jaCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y3NzQ3NTc0ODkwNjQ4MzFhMjk2MWU3NjY5MzAyYTBiLnNldENvbnRlbnQoaHRtbF85N2ZkNTI2OTkwY2I0YzZjOTRmMmRiNGJhODk1NmMzNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yODhiODg4OWIwMTI0OTE1YThiYmMxZDQxODA5NzY4Zi5iaW5kUG9wdXAocG9wdXBfZjc3NDc1NzQ4OTA2NDgzMWEyOTYxZTc2NjkzMDJhMGIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOGNmNzFiOTJmMmZmNDBjZjhhM2RhYTIxNWMzMTNlYzIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTQzMjgzLC03OS40NDIyNTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzAwZDVlZWEzZjk1MTQ1N2U5YzQ1Njc3OWQ4NTZjYzQyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzEyYzE2MTQ2NDRjNjRhMWY5ZGUxYmFlNmZjYTM3ZmFlID0gJCgnPGRpdiBpZD0iaHRtbF8xMmMxNjE0NjQ0YzY0YTFmOWRlMWJhZTZmY2EzN2ZhZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmF0aHVyc3QgTWFub3IsIFdpbHNvbiBIZWlnaHRzLCBEb3duc3ZpZXcgTm9ydGggQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wMGQ1ZWVhM2Y5NTE0NTdlOWM0NTY3NzlkODU2Y2M0Mi5zZXRDb250ZW50KGh0bWxfMTJjMTYxNDY0NGM2NGExZjlkZTFiYWU2ZmNhMzdmYWUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGNmNzFiOTJmMmZmNDBjZjhhM2RhYTIxNWMzMTNlYzIuYmluZFBvcHVwKHBvcHVwXzAwZDVlZWEzZjk1MTQ1N2U5YzQ1Njc3OWQ4NTZjYzQyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Y1MTFhMWQzOThiNjRhOTY4ZTRiM2E5ZTc5YzFjOGIxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzg2OTQ3MywtNzkuMzg1OTc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzM2NDdiNTNmMzNmYTQyZGJiNmRmMjM2NTNhZTYzNmQzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzFiNjgyZmUwOWJhNDQ2ZmJiZjMyNjcxYTI1MGZhMWVhID0gJCgnPGRpdiBpZD0iaHRtbF8xYjY4MmZlMDliYTQ0NmZiYmYzMjY3MWEyNTBmYTFlYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmF5dmlldyBWaWxsYWdlIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMzY0N2I1M2YzM2ZhNDJkYmI2ZGYyMzY1M2FlNjM2ZDMuc2V0Q29udGVudChodG1sXzFiNjgyZmUwOWJhNDQ2ZmJiZjMyNjcxYTI1MGZhMWVhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y1MTFhMWQzOThiNjRhOTY4ZTRiM2E5ZTc5YzFjOGIxLmJpbmRQb3B1cChwb3B1cF8zNjQ3YjUzZjMzZmE0MmRiYjZkZjIzNjUzYWU2MzZkMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kMGZkNmZjMTg0ODU0ZTkyODhjMzcyMzcxOGNmNWEwYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjczMzI4MjUsLTc5LjQxOTc0OTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzFiYjZjOGJhMjVkNDE4MTk3OWY2NTQwNzBiOTBiNTAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGE4YWMyYWM1OTY5NGRkZDhmNDA2MWQxN2FmNDFiN2IgPSAkKCc8ZGl2IGlkPSJodG1sX2RhOGFjMmFjNTk2OTRkZGQ4ZjQwNjFkMTdhZjQxYjdiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CZWRmb3JkIFBhcmssIExhd3JlbmNlIE1hbm9yIEVhc3QgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83MWJiNmM4YmEyNWQ0MTgxOTc5ZjY1NDA3MGI5MGI1MC5zZXRDb250ZW50KGh0bWxfZGE4YWMyYWM1OTY5NGRkZDhmNDA2MWQxN2FmNDFiN2IpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDBmZDZmYzE4NDg1NGU5Mjg4YzM3MjM3MThjZjVhMGEuYmluZFBvcHVwKHBvcHVwXzcxYmI2YzhiYTI1ZDQxODE5NzlmNjU0MDcwYjkwYjUwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzhhYTA4NDFlZGRmOTRiM2M4MDE0MWUxNTY2ZDNlOTQ0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ0NzcwNzk5OTk5OTk2LC03OS4zNzMzMDY0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZiNWY5NmFjNzkyYzQ5ZWQ5YzRmNDNkNTJkN2EzODA1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQwOTUzNjdmMWIxMTQzMjA4MjNkZGRjYWY3NmE4ZWU1ID0gJCgnPGRpdiBpZD0iaHRtbF80MDk1MzY3ZjFiMTE0MzIwODIzZGRkY2FmNzZhOGVlNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmVyY3p5IFBhcmsgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82YjVmOTZhYzc5MmM0OWVkOWM0ZjQzZDUyZDdhMzgwNS5zZXRDb250ZW50KGh0bWxfNDA5NTM2N2YxYjExNDMyMDgyM2RkZGNhZjc2YThlZTUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGFhMDg0MWVkZGY5NGIzYzgwMTQxZTE1NjZkM2U5NDQuYmluZFBvcHVwKHBvcHVwXzZiNWY5NmFjNzkyYzQ5ZWQ5YzRmNDNkNTJkN2EzODA1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzgyZWZlY2I3YzJlOTRjYzY4ODhmZGVmOTZlNmJjNTJlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjkyNjU3MDAwMDAwMDA0LC03OS4yNjQ4NDgxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2IwNTU0ODFjY2E3MjQwZTJhOGM3MDRjMGJhMGNmNDhmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzg4ZjIxYTM3YTkxMDRjYjM4ZWViZjRkMDhlNmZlZDE3ID0gJCgnPGRpdiBpZD0iaHRtbF84OGYyMWEzN2E5MTA0Y2IzOGVlYmY0ZDA4ZTZmZWQxNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmlyY2ggQ2xpZmYsIENsaWZmc2lkZSBXZXN0IENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYjA1NTQ4MWNjYTcyNDBlMmE4YzcwNGMwYmEwY2Y0OGYuc2V0Q29udGVudChodG1sXzg4ZjIxYTM3YTkxMDRjYjM4ZWViZjRkMDhlNmZlZDE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgyZWZlY2I3YzJlOTRjYzY4ODhmZGVmOTZlNmJjNTJlLmJpbmRQb3B1cChwb3B1cF9iMDU1NDgxY2NhNzI0MGUyYThjNzA0YzBiYTBjZjQ4Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNDRkNDU2NzIwZjY0NTVkOWRkYzkyNDE0YTk0M2VmMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYzNjg0NzIsLTc5LjQyODE5MTQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZkMDJmZThlNWVhNTQxNjc5ZjcyM2YzMTBjN2RmZjMwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2U5N2NiNGZmNGU3ODQ2NmY5NGE5NjE1YjExMWZhNjQzID0gJCgnPGRpdiBpZD0iaHRtbF9lOTdjYjRmZjRlNzg0NjZmOTRhOTYxNWIxMTFmYTY0MyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QnJvY2t0b24sIFBhcmtkYWxlIFZpbGxhZ2UsIEV4aGliaXRpb24gUGxhY2UgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82ZDAyZmU4ZTVlYTU0MTY3OWY3MjNmMzEwYzdkZmYzMC5zZXRDb250ZW50KGh0bWxfZTk3Y2I0ZmY0ZTc4NDY2Zjk0YTk2MTViMTExZmE2NDMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDQ0ZDQ1NjcyMGY2NDU1ZDlkZGM5MjQxNGE5NDNlZjEuYmluZFBvcHVwKHBvcHVwXzZkMDJmZThlNWVhNTQxNjc5ZjcyM2YzMTBjN2RmZjMwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RlYzAzNTBlMTM4MDRiYjVhYTEzZTA2OTEyMjMxNzIwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYyNzQzOSwtNzkuMzIxNTU4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FmZTc4MDZjNzUxNzRkZDFhMjYwZDZjMTcwODI2ZjE1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZiNjJjMDA5NGEzMDRmMGViOGRjYjIxN2JmNDkyNTBhID0gJCgnPGRpdiBpZD0iaHRtbF82YjYyYzAwOTRhMzA0ZjBlYjhkY2IyMTdiZjQ5MjUwYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QnVzaW5lc3MgcmVwbHkgbWFpbCBQcm9jZXNzaW5nIENlbnRyZSwgU291dGggQ2VudHJhbCBMZXR0ZXIgUHJvY2Vzc2luZyBQbGFudCBUb3JvbnRvIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYWZlNzgwNmM3NTE3NGRkMWEyNjBkNmMxNzA4MjZmMTUuc2V0Q29udGVudChodG1sXzZiNjJjMDA5NGEzMDRmMGViOGRjYjIxN2JmNDkyNTBhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2RlYzAzNTBlMTM4MDRiYjVhYTEzZTA2OTEyMjMxNzIwLmJpbmRQb3B1cChwb3B1cF9hZmU3ODA2Yzc1MTc0ZGQxYTI2MGQ2YzE3MDgyNmYxNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83MTFhOWI0NGVlOGE0YWM3YTc0ZGE0NzEzZWEzNjEyYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjYyODk0NjcsLTc5LjM5NDQxOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZGM3NzU2MjQxOGY1NDA4ZDllYTQ5MjQzMDkwMDQyM2QgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTJmNmIyOGVjODM2NDFhOWI2ZjE5ODNhYzQ4MmM1ZWYgPSAkKCc8ZGl2IGlkPSJodG1sXzUyZjZiMjhlYzgzNjQxYTliNmYxOTgzYWM0ODJjNWVmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DTiBUb3dlciwgS2luZyBhbmQgU3BhZGluYSwgUmFpbHdheSBMYW5kcywgSGFyYm91cmZyb250IFdlc3QsIEJhdGh1cnN0IFF1YXksIFNvdXRoIE5pYWdhcmEsIElzbGFuZCBhaXJwb3J0IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGM3NzU2MjQxOGY1NDA4ZDllYTQ5MjQzMDkwMDQyM2Quc2V0Q29udGVudChodG1sXzUyZjZiMjhlYzgzNjQxYTliNmYxOTgzYWM0ODJjNWVmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzcxMWE5YjQ0ZWU4YTRhYzdhNzRkYTQ3MTNlYTM2MTJhLmJpbmRQb3B1cChwb3B1cF9kYzc3NTYyNDE4ZjU0MDhkOWVhNDkyNDMwOTAwNDIzZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85ZTRkMzJkYmJkZGI0N2VjOTViYmFlMjM5ZGYzNTBiNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4OTAyNTYsLTc5LjQ1MzUxMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xOGM0OWI2MmUxNDE0MDUwYjYwODdmNGZiZGNlYTY5YyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMDc1OGUzNGRhNDY0MjQxOGJlM2U5ZWYxNGFkNTliMCA9ICQoJzxkaXYgaWQ9Imh0bWxfYTA3NThlMzRkYTQ2NDI0MThiZTNlOWVmMTRhZDU5YjAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNhbGVkb25pYS1GYWlyYmFua3MgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xOGM0OWI2MmUxNDE0MDUwYjYwODdmNGZiZGNlYTY5Yy5zZXRDb250ZW50KGh0bWxfYTA3NThlMzRkYTQ2NDI0MThiZTNlOWVmMTRhZDU5YjApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWU0ZDMyZGJiZGRiNDdlYzk1YmJhZTIzOWRmMzUwYjQuYmluZFBvcHVwKHBvcHVwXzE4YzQ5YjYyZTE0MTQwNTBiNjA4N2Y0ZmJkY2VhNjljKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U0MTk3ZGY5MzlkNDRmZGFiZTczNzk0N2UwY2M2OGJhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjM2OTY1NiwtNzkuNjE1ODE4OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMGQxNzdkYmM5NDQ5NGIwNzk2Nzk3NjY3YWNhMDc4YzIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOWJjNWFhM2JmYjVhNDUyMTkwZWFlM2M3ZDFkMGY2OGEgPSAkKCc8ZGl2IGlkPSJodG1sXzliYzVhYTNiZmI1YTQ1MjE5MGVhZTNjN2QxZDBmNjhhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DYW5hZGEgUG9zdCBHYXRld2F5IFByb2Nlc3NpbmcgQ2VudHJlIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGQxNzdkYmM5NDQ5NGIwNzk2Nzk3NjY3YWNhMDc4YzIuc2V0Q29udGVudChodG1sXzliYzVhYTNiZmI1YTQ1MjE5MGVhZTNjN2QxZDBmNjhhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2U0MTk3ZGY5MzlkNDRmZGFiZTczNzk0N2UwY2M2OGJhLmJpbmRQb3B1cChwb3B1cF8wZDE3N2RiYzk0NDk0YjA3OTY3OTc2NjdhY2EwNzhjMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zZGY0MmI0ZjFiOGM0OGY5YWM5MDdiOWExNWM2ODU5NCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc3MzEzNiwtNzkuMjM5NDc2MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWVmMTYxOTc4NzYyNGRmYWEyMDM2YjJkYjE5OTMxYTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTBlNjdhYzZlODYxNGM3MDlhYjU2ZDM4MzdlZmNlMTggPSAkKCc8ZGl2IGlkPSJodG1sX2UwZTY3YWM2ZTg2MTRjNzA5YWI1NmQzODM3ZWZjZTE4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DZWRhcmJyYWUgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZWYxNjE5Nzg3NjI0ZGZhYTIwMzZiMmRiMTk5MzFhNi5zZXRDb250ZW50KGh0bWxfZTBlNjdhYzZlODYxNGM3MDlhYjU2ZDM4MzdlZmNlMTgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfM2RmNDJiNGYxYjhjNDhmOWFjOTA3YjlhMTVjNjg1OTQuYmluZFBvcHVwKHBvcHVwXzVlZjE2MTk3ODc2MjRkZmFhMjAzNmIyZGIxOTkzMWE2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVkNjEzY2E2NDZiMzQyYmY4NzE2MmJmNmU3YTg2NWM5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU3OTUyNCwtNzkuMzg3MzgyNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wZDFhNjAzZGEwNjk0ODA1ODBlNTg3YjkwODA2MGE0MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hZmFjYjcyYTdkNjA0NDU4YThkZjAzYzRlZTE1YzM2MyA9ICQoJzxkaXYgaWQ9Imh0bWxfYWZhY2I3MmE3ZDYwNDQ1OGE4ZGYwM2M0ZWUxNWMzNjMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNlbnRyYWwgQmF5IFN0cmVldCBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzBkMWE2MDNkYTA2OTQ4MDU4MGU1ODdiOTA4MDYwYTQzLnNldENvbnRlbnQoaHRtbF9hZmFjYjcyYTdkNjA0NDU4YThkZjAzYzRlZTE1YzM2Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81ZDYxM2NhNjQ2YjM0MmJmODcxNjJiZjZlN2E4NjVjOS5iaW5kUG9wdXAocG9wdXBfMGQxYTYwM2RhMDY5NDgwNTgwZTU4N2I5MDgwNjBhNDMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWY2OTM1ZDYyNWNmNDM1ZThkZWVhY2VlMjlhZGI5MTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njk1NDIsLTc5LjQyMjU2MzddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzdjZDdhNWVkYjI0NDcwNmEwZjcxOTM2ZTAxNTYxN2UgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGY4OTU4NGQ4OGZiNDIwOTg2NjdlNmI1MmU5ZjMyOTIgPSAkKCc8ZGl2IGlkPSJodG1sXzhmODk1ODRkODhmYjQyMDk4NjY3ZTZiNTJlOWYzMjkyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DaHJpc3RpZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2M3Y2Q3YTVlZGIyNDQ3MDZhMGY3MTkzNmUwMTU2MTdlLnNldENvbnRlbnQoaHRtbF84Zjg5NTg0ZDg4ZmI0MjA5ODY2N2U2YjUyZTlmMzI5Mik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZjY5MzVkNjI1Y2Y0MzVlOGRlZWFjZWUyOWFkYjkxNy5iaW5kUG9wdXAocG9wdXBfYzdjZDdhNWVkYjI0NDcwNmEwZjcxOTM2ZTAxNTYxN2UpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjU5NmE0OGNkNzM2NDJlZWI4OTIwODAxOGI1OGE2ZjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjU4NTk5LC03OS4zODMxNTk5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNTdkMmYxMmRlZmM0Y2MwYmQ1ZmFhZjRlOWE4ZTEwMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wZDI4NzMwMDk5MDc0NGE1OTNiOWIwOThkMTdjZDU3ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfMGQyODczMDA5OTA3NDRhNTkzYjliMDk4ZDE3Y2Q1N2YiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNodXJjaCBhbmQgV2VsbGVzbGV5IENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTU3ZDJmMTJkZWZjNGNjMGJkNWZhYWY0ZTlhOGUxMDAuc2V0Q29udGVudChodG1sXzBkMjg3MzAwOTkwNzQ0YTU5M2I5YjA5OGQxN2NkNTdmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzY1OTZhNDhjZDczNjQyZWViODkyMDgwMThiNThhNmY2LmJpbmRQb3B1cChwb3B1cF8xNTdkMmYxMmRlZmM0Y2MwYmQ1ZmFhZjRlOWE4ZTEwMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xYzE0NWMxYWM1YjI0OGEyYTM5MGNhMmZjNjkxODEzNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4MTYzNzUsLTc5LjMwNDMwMjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOWUzYmQwMGY0ZTQ3NGZmOTlkZjVjZmExMTExMjgzNzEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmI5MDdlYjhiYmRhNDUxYThjYWI1ZGM4YmMzN2ViZmMgPSAkKCc8ZGl2IGlkPSJodG1sX2JiOTA3ZWI4YmJkYTQ1MWE4Y2FiNWRjOGJjMzdlYmZjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DbGFya3MgQ29ybmVycywgVGFtIE8mIzM5O1NoYW50ZXIsIFN1bGxpdmFuIENsdXN0ZXIgNDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWUzYmQwMGY0ZTQ3NGZmOTlkZjVjZmExMTExMjgzNzEuc2V0Q29udGVudChodG1sX2JiOTA3ZWI4YmJkYTQ1MWE4Y2FiNWRjOGJjMzdlYmZjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFjMTQ1YzFhYzViMjQ4YTJhMzkwY2EyZmM2OTE4MTM2LmJpbmRQb3B1cChwb3B1cF85ZTNiZDAwZjRlNDc0ZmY5OWRmNWNmYTExMTEyODM3MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lYjQzZDIxOTBiOTg0NzdiYTBmMmIzODlkNmRhODgyNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxNjMxNiwtNzkuMjM5NDc2MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzZhNGFhMDM3MDFiNGUwNWE0OGY0OTc3OWEzZDBjYzAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOWYyNzU0MzNjNWM4NGQzNjg1MmJhMGQwYjgwYmNmMWQgPSAkKCc8ZGl2IGlkPSJodG1sXzlmMjc1NDMzYzVjODRkMzY4NTJiYTBkMGI4MGJjZjFkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DbGlmZnNpZGUsIENsaWZmY3Jlc3QsIFNjYXJib3JvdWdoIFZpbGxhZ2UgV2VzdCBDbHVzdGVyIDQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzc2YTRhYTAzNzAxYjRlMDVhNDhmNDk3NzlhM2QwY2MwLnNldENvbnRlbnQoaHRtbF85ZjI3NTQzM2M1Yzg0ZDM2ODUyYmEwZDBiODBiY2YxZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lYjQzZDIxOTBiOTg0NzdiYTBmMmIzODlkNmRhODgyNy5iaW5kUG9wdXAocG9wdXBfNzZhNGFhMDM3MDFiNGUwNWE0OGY0OTc3OWEzZDBjYzApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmI0OGViOTk2MzllNDE0ZjhjN2NjM2Y1MzUyNzNlYzYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDgxOTg1LC03OS4zNzk4MTY5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lMWRmNTI0MmJmZTg0NjA3OGZhNzZiNzQ1OGNhZGU2ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jYWU0Y2I5ZGNlYWQ0MTJhOGU5YTFjMDIyNjkyZjZlZCA9ICQoJzxkaXYgaWQ9Imh0bWxfY2FlNGNiOWRjZWFkNDEyYThlOWExYzAyMjY5MmY2ZWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNvbW1lcmNlIENvdXJ0LCBWaWN0b3JpYSBIb3RlbCBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2UxZGY1MjQyYmZlODQ2MDc4ZmE3NmI3NDU4Y2FkZTZlLnNldENvbnRlbnQoaHRtbF9jYWU0Y2I5ZGNlYWQ0MTJhOGU5YTFjMDIyNjkyZjZlZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yYjQ4ZWI5OTYzOWU0MTRmOGM3Y2MzZjUzNTI3M2VjNi5iaW5kUG9wdXAocG9wdXBfZTFkZjUyNDJiZmU4NDYwNzhmYTc2Yjc0NThjYWRlNmUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMjI5NTgxZWZmYzRlNDAwYjg5ZDFjY2U5YWY4ZWVjMmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDQzMjQ0LC03OS4zODg3OTAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzg5MzAwN2Y5ZTBlZTQwOTY4YTJlNjJmYTBhY2YwNjlmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2E0YTczZjYxNjFiNTQ3YWI4Mjk0MmVmNjk4ZDk4ZDgyID0gJCgnPGRpdiBpZD0iaHRtbF9hNGE3M2Y2MTYxYjU0N2FiODI5NDJlZjY5OGQ5OGQ4MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RGF2aXN2aWxsZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg5MzAwN2Y5ZTBlZTQwOTY4YTJlNjJmYTBhY2YwNjlmLnNldENvbnRlbnQoaHRtbF9hNGE3M2Y2MTYxYjU0N2FiODI5NDJlZjY5OGQ5OGQ4Mik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yMjk1ODFlZmZjNGU0MDBiODlkMWNjZTlhZjhlZWMyYi5iaW5kUG9wdXAocG9wdXBfODkzMDA3ZjllMGVlNDA5NjhhMmU2MmZhMGFjZjA2OWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODM0NTY3MTJiYTdiNGRlYjhjMjI0NzQwNmQwYjYwMmQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTI3NTExLC03OS4zOTAxOTc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U1ZmJlMWIyYmExNTQ1ZTk5MDhhM2YwYjdlODZlODdiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzg0MDQ3ZDg2MWI0NjRkZjNhZjBlNmQ5ODA1OTg2YmM2ID0gJCgnPGRpdiBpZD0iaHRtbF84NDA0N2Q4NjFiNDY0ZGYzYWYwZTZkOTgwNTk4NmJjNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RGF2aXN2aWxsZSBOb3J0aCBDbHVzdGVyIDM8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2U1ZmJlMWIyYmExNTQ1ZTk5MDhhM2YwYjdlODZlODdiLnNldENvbnRlbnQoaHRtbF84NDA0N2Q4NjFiNDY0ZGYzYWYwZTZkOTgwNTk4NmJjNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84MzQ1NjcxMmJhN2I0ZGViOGMyMjQ3NDA2ZDBiNjAyZC5iaW5kUG9wdXAocG9wdXBfZTVmYmUxYjJiYTE1NDVlOTkwOGEzZjBiN2U4NmU4N2IpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTMwNzYwZDdkNDBjNDUzOGIxMzM2NTQ3NzYxODBmZjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTExMTU4LC03OS40NzYwMTMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jYzNiMDFhMTI4M2E0MWY0YTZmYjk5MjhlNGNlZjg2NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84Y2IyMDZhNDJiYjA0OWU3YjkyZmJiZGE2YTMwNzhjYyA9ICQoJzxkaXYgaWQ9Imh0bWxfOGNiMjA2YTQyYmIwNDllN2I5MmZiYmRhNmEzMDc4Y2MiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlbCBSYXksIE1vdW50IERlbm5pcywgS2VlbHNkYWxlIGFuZCBTaWx2ZXJ0aG9ybiBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NjM2IwMWExMjgzYTQxZjRhNmZiOTkyOGU0Y2VmODY2LnNldENvbnRlbnQoaHRtbF84Y2IyMDZhNDJiYjA0OWU3YjkyZmJiZGE2YTMwNzhjYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85MzA3NjBkN2Q0MGM0NTM4YjEzMzY1NDc3NjE4MGZmNC5iaW5kUG9wdXAocG9wdXBfY2MzYjAxYTEyODNhNDFmNGE2ZmI5OTI4ZTRjZWY4NjYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWUwNjg0ZDE5OTdlNDZiZTk1MDlmNjQ3MzkwNTk0NDIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MzU5MDI3NSwtNzkuMzQ2NTU1NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mMDgxMTgyMmIwZjE0OGYyODdmNDUxZGYzZmJhOTNmNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82Y2EwMWQwOTljODI0ZDM2YTk5MjMzOGFiMmJiOWMxMyA9ICQoJzxkaXYgaWQ9Imh0bWxfNmNhMDFkMDk5YzgyNGQzNmE5OTIzMzhhYjJiYjljMTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvbiBNaWxscyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YwODExODIyYjBmMTQ4ZjI4N2Y0NTFkZjNmYmE5M2Y1LnNldENvbnRlbnQoaHRtbF82Y2EwMWQwOTljODI0ZDM2YTk5MjMzOGFiMmJiOWMxMyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZTA2ODRkMTk5N2U0NmJlOTUwOWY2NDczOTA1OTQ0Mi5iaW5kUG9wdXAocG9wdXBfZjA4MTE4MjJiMGYxNDhmMjg3ZjQ1MWRmM2ZiYTkzZjUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfM2VlYmMxM2YwZWQ1NGY2NmI3Zjk4ZGQ2MDgwZWZhNzUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTc0MDk2LC03OS4yNzMzMDQwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lMGY4Y2E5MzRhNWY0NWRmYjFiMDJmNjEzNzIwZTZiZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81ODBlYjYxNjJlNWQ0ZTQzYTg5YjQ1ODQwNWFjNjdlNyA9ICQoJzxkaXYgaWQ9Imh0bWxfNTgwZWI2MTYyZTVkNGU0M2E4OWI0NTg0MDVhYzY3ZTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvcnNldCBQYXJrLCBXZXhmb3JkIEhlaWdodHMsIFNjYXJib3JvdWdoIFRvd24gQ2VudHJlIENsdXN0ZXIgNDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTBmOGNhOTM0YTVmNDVkZmIxYjAyZjYxMzcyMGU2YmUuc2V0Q29udGVudChodG1sXzU4MGViNjE2MmU1ZDRlNDNhODliNDU4NDA1YWM2N2U3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzNlZWJjMTNmMGVkNTRmNjZiN2Y5OGRkNjA4MGVmYTc1LmJpbmRQb3B1cChwb3B1cF9lMGY4Y2E5MzRhNWY0NWRmYjFiMDJmNjEzNzIwZTZiZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yNDJiNzJhOTY0MWM0NTNjYTFiZDhkZjU4MjFhY2QxZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc0MTY1Mzg3NTAwMDAwNCwtNzkuNDk3MTAwOTI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzExZWU3ZjYxZDQxZTQxMTRhNzRkMDVkY2NlZjBkNmI3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzliMGVkMjZlYzc5NDQxNzhhMTZlOTJlYTFjMGMzMzJkID0gJCgnPGRpdiBpZD0iaHRtbF85YjBlZDI2ZWM3OTQ0MTc4YTE2ZTkyZWExYzBjMzMyZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RG93bnN2aWV3IENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTFlZTdmNjFkNDFlNDExNGE3NGQwNWRjY2VmMGQ2Yjcuc2V0Q29udGVudChodG1sXzliMGVkMjZlYzc5NDQxNzhhMTZlOTJlYTFjMGMzMzJkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI0MmI3MmE5NjQxYzQ1M2NhMWJkOGRmNTgyMWFjZDFlLmJpbmRQb3B1cChwb3B1cF8xMWVlN2Y2MWQ0MWU0MTE0YTc0ZDA1ZGNjZWYwZDZiNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mN2IwYjU3OGUyMDE0MTY0YTJhYTI4NjhlYmU4ZmY2OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2OTAwNTEwMDAwMDAxLC03OS40NDIyNTkzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2MwMWJlZTIwNzFkZDQwYzRhNzE2MjkyYTBlNzQwNWFkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2VkMjhmNWMzOTY2YjRiOTE5MWJmODczODI1YTNmODc0ID0gJCgnPGRpdiBpZD0iaHRtbF9lZDI4ZjVjMzk2NmI0YjkxOTFiZjg3MzgyNWEzZjg3NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RHVmZmVyaW4sIERvdmVyY291cnQgVmlsbGFnZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2MwMWJlZTIwNzFkZDQwYzRhNzE2MjkyYTBlNzQwNWFkLnNldENvbnRlbnQoaHRtbF9lZDI4ZjVjMzk2NmI0YjkxOTFiZjg3MzgyNWEzZjg3NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mN2IwYjU3OGUyMDE0MTY0YTJhYTI4NjhlYmU4ZmY2OS5iaW5kUG9wdXAocG9wdXBfYzAxYmVlMjA3MWRkNDBjNGE3MTYyOTJhMGU3NDA1YWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODM4ZGFlZjhhOTM3NDkxYTliNTY4NWE3MTQwMTc1Y2EgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODUzNDcsLTc5LjMzODEwNjVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzA3NDgxMWZiYzk4NDlkZjk3ODhjM2FjY2MwYTg1NjIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzJlOWE0MDU4MmFhNDgwOThhNzFiYWFmZjM4YTE2ZTcgPSAkKCc8ZGl2IGlkPSJodG1sXzMyZTlhNDA1ODJhYTQ4MDk4YTcxYmFhZmYzOGExNmU3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FYXN0IFRvcm9udG8sIEJyb2FkdmlldyBOb3J0aCAoT2xkIEVhc3QgWW9yaykgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jMDc0ODExZmJjOTg0OWRmOTc4OGMzYWNjYzBhODU2Mi5zZXRDb250ZW50KGh0bWxfMzJlOWE0MDU4MmFhNDgwOThhNzFiYWFmZjM4YTE2ZTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODM4ZGFlZjhhOTM3NDkxYTliNTY4NWE3MTQwMTc1Y2EuYmluZFBvcHVwKHBvcHVwX2MwNzQ4MTFmYmM5ODQ5ZGY5Nzg4YzNhY2NjMGE4NTYyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQxMTRhYzA4N2NmYTQ0MTA5Zjg5NzA5MTM2YzU2NGE5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQzNTE1MiwtNzkuNTc3MjAwNzk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOWMyZDhhYmMyYzg3NDZhMDg1YjliZDJjNzhmNjY5ZGIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMThlNDk5Y2MyMzQ4NGQ5ZGI1OTdiZmU0NjNkOTQ2NGYgPSAkKCc8ZGl2IGlkPSJodG1sXzE4ZTQ5OWNjMjM0ODRkOWRiNTk3YmZlNDYzZDk0NjRmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FcmluZ2F0ZSwgQmxvb3JkYWxlIEdhcmRlbnMsIE9sZCBCdXJuaGFtdGhvcnBlLCBNYXJrbGFuZCBXb29kIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWMyZDhhYmMyYzg3NDZhMDg1YjliZDJjNzhmNjY5ZGIuc2V0Q29udGVudChodG1sXzE4ZTQ5OWNjMjM0ODRkOWRiNTk3YmZlNDYzZDk0NjRmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQxMTRhYzA4N2NmYTQ0MTA5Zjg5NzA5MTM2YzU2NGE5LmJpbmRQb3B1cChwb3B1cF85YzJkOGFiYzJjODc0NmEwODViOWJkMmM3OGY2NjlkYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNzQzZDgyNTk3MmQ0Nzc1YTZjOGZlZDlkNjU4NzVkYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc3ODUxNzUsLTc5LjM0NjU1NTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODUxMzQ1NjNiNmJkNGNjNDkxYjVkYzE2M2E4NWM1MzMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzBkYzZmNzI4MjhkNDJmYmFmNWRhODY0MTliODdmYWQgPSAkKCc8ZGl2IGlkPSJodG1sXzcwZGM2ZjcyODI4ZDQyZmJhZjVkYTg2NDE5Yjg3ZmFkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5GYWlydmlldywgSGVucnkgRmFybSwgT3Jpb2xlIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODUxMzQ1NjNiNmJkNGNjNDkxYjVkYzE2M2E4NWM1MzMuc2V0Q29udGVudChodG1sXzcwZGM2ZjcyODI4ZDQyZmJhZjVkYTg2NDE5Yjg3ZmFkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA3NDNkODI1OTcyZDQ3NzVhNmM4ZmVkOWQ2NTg3NWRhLmJpbmRQb3B1cChwb3B1cF84NTEzNDU2M2I2YmQ0Y2M0OTFiNWRjMTYzYTg1YzUzMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iZTk0YTcyYzMxMjI0Y2ViYjQ0YjQyNDE1NTVmMGU2YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0ODQyOTIsLTc5LjM4MjI4MDJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzUyZjEwZGFjNjAxNDAxNThjZGI2MjAyNDI1YmYzNTggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDc3MGRmOTkzZjkzNGI1MjhhOTkwNGZjYWQwNjlhYjcgPSAkKCc8ZGl2IGlkPSJodG1sXzA3NzBkZjk5M2Y5MzRiNTI4YTk5MDRmY2FkMDY5YWI3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5GaXJzdCBDYW5hZGlhbiBQbGFjZSwgVW5kZXJncm91bmQgY2l0eSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzc1MmYxMGRhYzYwMTQwMTU4Y2RiNjIwMjQyNWJmMzU4LnNldENvbnRlbnQoaHRtbF8wNzcwZGY5OTNmOTM0YjUyOGE5OTA0ZmNhZDA2OWFiNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iZTk0YTcyYzMxMjI0Y2ViYjQ0YjQyNDE1NTVmMGU2YS5iaW5kUG9wdXAocG9wdXBfNzUyZjEwZGFjNjAxNDAxNThjZGI2MjAyNDI1YmYzNTgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNmUyNGU2ZjZiYjliNGUwODg3ZTEyNmEzZWRjYTA2N2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTY5NDc2LC03OS40MTEzMDcyMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kNTdhZWQ3NDNjYWM0ZmI5OTc2ZmNhNDhhODkxNDQyNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82MzhmMmU0YjBlNTc0MDY4ODRhNzgxNzMwMjk5MGNjYSA9ICQoJzxkaXYgaWQ9Imh0bWxfNjM4ZjJlNGIwZTU3NDA2ODg0YTc4MTczMDI5OTBjY2EiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZvcmVzdCBIaWxsIE5vcnRoICZhbXA7IFdlc3QsIEZvcmVzdCBIaWxsIFJvYWQgUGFyayBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q1N2FlZDc0M2NhYzRmYjk5NzZmY2E0OGE4OTE0NDI2LnNldENvbnRlbnQoaHRtbF82MzhmMmU0YjBlNTc0MDY4ODRhNzgxNzMwMjk5MGNjYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82ZTI0ZTZmNmJiOWI0ZTA4ODdlMTI2YTNlZGNhMDY3Yi5iaW5kUG9wdXAocG9wdXBfZDU3YWVkNzQzY2FjNGZiOTk3NmZjYTQ4YTg5MTQ0MjYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWUwMDU3Y2QyZmE3NGI5YjlhYmVmMDU3NzBhMGIxZTQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTcxNjE4LC03OS4zNzg5MzcwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84NTRlZWQ0MzkxNjU0ZDQyYTU5NGU0NTU1Nzg2ZjRjZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yMTNhZTNkMGUyYzc0NzI0YjdmMTNiM2EzMzBkNzc0OSA9ICQoJzxkaXYgaWQ9Imh0bWxfMjEzYWUzZDBlMmM3NDcyNGI3ZjEzYjNhMzMwZDc3NDkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkdhcmRlbiBEaXN0cmljdCwgUnllcnNvbiBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg1NGVlZDQzOTE2NTRkNDJhNTk0ZTQ1NTU3ODZmNGNlLnNldENvbnRlbnQoaHRtbF8yMTNhZTNkMGUyYzc0NzI0YjdmMTNiM2EzMzBkNzc0OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85ZTAwNTdjZDJmYTc0YjliOWFiZWYwNTc3MGEwYjFlNC5iaW5kUG9wdXAocG9wdXBfODU0ZWVkNDM5MTY1NGQ0MmE1OTRlNDU1NTc4NmY0Y2UpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDI3YjRkMmY2NTQ0NDA2YjljYjljNDU5YzBiNjI3MDYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDk1NzcsLTc5LjQ0NTA3MjU5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZlZjExMDMyMzc3MTRlYThiNDJmZTU3YTkwODRkZWY0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QwZGY2MzUwMzVmMjRjZjI5YmY1N2M4YjQ3MmIxNmExID0gJCgnPGRpdiBpZD0iaHRtbF9kMGRmNjM1MDM1ZjI0Y2YyOWJmNTdjOGI0NzJiMTZhMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R2xlbmNhaXJuIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNmVmMTEwMzIzNzcxNGVhOGI0MmZlNTdhOTA4NGRlZjQuc2V0Q29udGVudChodG1sX2QwZGY2MzUwMzVmMjRjZjI5YmY1N2M4YjQ3MmIxNmExKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzAyN2I0ZDJmNjU0NDQwNmI5Y2I5YzQ1OWMwYjYyNzA2LmJpbmRQb3B1cChwb3B1cF82ZWYxMTAzMjM3NzE0ZWE4YjQyZmU1N2E5MDg0ZGVmNCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kYzhmYjdjM2JlMTM0NDgzYjMzMGQxMjEwZDhhZmZhYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxMTExMTcwMDAwMDAwNCwtNzkuMjg0NTc3Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hMzYwZjZiZTVmZDU0YzY2OWVjYzUwZDE4YzQ5OTUwOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80YWFiZDk1OTViYTM0MGU1YWRkOTVmZjkwMjBiZTQ4MCA9ICQoJzxkaXYgaWQ9Imh0bWxfNGFhYmQ5NTk1YmEzNDBlNWFkZDk1ZmY5MDIwYmU0ODAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkdvbGRlbiBNaWxlLCBDbGFpcmxlYSwgT2FrcmlkZ2UgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hMzYwZjZiZTVmZDU0YzY2OWVjYzUwZDE4YzQ5OTUwOS5zZXRDb250ZW50KGh0bWxfNGFhYmQ5NTk1YmEzNDBlNWFkZDk1ZmY5MDIwYmU0ODApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGM4ZmI3YzNiZTEzNDQ4M2IzMzBkMTIxMGQ4YWZmYWEuYmluZFBvcHVwKHBvcHVwX2EzNjBmNmJlNWZkNTRjNjY5ZWNjNTBkMThjNDk5NTA5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVkZDA1NjJkNTgxMDQ5NGI5ZjlhY2FmNDk3NmQyMzIyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzYzNTcyNiwtNzkuMTg4NzExNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hMTZlYmZhMzA2NzQ0Yjk5OGFjYTZkMDI4YTU3MWI2YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82YWJhMDFlZTVkNTc0NjYzYTA3MDlkZmMxM2VhMzE2ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNmFiYTAxZWU1ZDU3NDY2M2EwNzA5ZGZjMTNlYTMxNmYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkd1aWxkd29vZCwgTW9ybmluZ3NpZGUsIFdlc3QgSGlsbCBDbHVzdGVyIDQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2ExNmViZmEzMDY3NDRiOTk4YWNhNmQwMjhhNTcxYjZiLnNldENvbnRlbnQoaHRtbF82YWJhMDFlZTVkNTc0NjYzYTA3MDlkZmMxM2VhMzE2Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81ZGQwNTYyZDU4MTA0OTRiOWY5YWNhZjQ5NzZkMjMyMi5iaW5kUG9wdXAocG9wdXBfYTE2ZWJmYTMwNjc0NGI5OThhY2E2ZDAyOGE1NzFiNmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTVkNjljYThkNDU0NGExODgyOGVmODA4NzFlYzc5NTggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDA4MTU3LC03OS4zODE3NTIyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNmQ1YmFiOGZjYzY0ZTA4YTJjMzkwOTVhYjkzNmIwNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zZmM2OWJkNDI2Y2Q0ZGVjYTlkZDMyM2QxNmI0NzVkNiA9ICQoJzxkaXYgaWQ9Imh0bWxfM2ZjNjliZDQyNmNkNGRlY2E5ZGQzMjNkMTZiNDc1ZDYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkhhcmJvdXJmcm9udCBFYXN0LCBVbmlvbiBTdGF0aW9uLCBUb3JvbnRvIElzbGFuZHMgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xNmQ1YmFiOGZjYzY0ZTA4YTJjMzkwOTVhYjkzNmIwNS5zZXRDb250ZW50KGh0bWxfM2ZjNjliZDQyNmNkNGRlY2E5ZGQzMjNkMTZiNDc1ZDYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTVkNjljYThkNDU0NGExODgyOGVmODA4NzFlYzc5NTguYmluZFBvcHVwKHBvcHVwXzE2ZDViYWI4ZmNjNjRlMDhhMmMzOTA5NWFiOTM2YjA1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2QwYzEzZTUyZTUyYzQzNWRhODQ5ZWFiZjUwODkyMjE5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjYxNjA4MywtNzkuNDY0NzYzMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTkyZjI2ZjVkMmViNGY1MGE4NTIzODY0YTIzMzNlOTkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODIwOGRjYWM3NzlmNGM0YThjMmY0MTc5MTA4N2QxNWMgPSAkKCc8ZGl2IGlkPSJodG1sXzgyMDhkY2FjNzc5ZjRjNGE4YzJmNDE3OTEwODdkMTVjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IaWdoIFBhcmssIFRoZSBKdW5jdGlvbiBTb3V0aCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2U5MmYyNmY1ZDJlYjRmNTBhODUyMzg2NGEyMzMzZTk5LnNldENvbnRlbnQoaHRtbF84MjA4ZGNhYzc3OWY0YzRhOGMyZjQxNzkxMDg3ZDE1Yyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kMGMxM2U1MmU1MmM0MzVkYTg0OWVhYmY1MDg5MjIxOS5iaW5kUG9wdXAocG9wdXBfZTkyZjI2ZjVkMmViNGY1MGE4NTIzODY0YTIzMzNlOTkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTg2YzcxNDQxOTU0NGI2Y2IwMTc1OWJhOWY2MjkxNGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MDM3NjIyLC03OS4zNjM0NTE3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQ2YWQwNDZjOTNjMDQwNzRiZTk0MTFhN2FmNmEwZjUzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYzMmI2Njg5OWJhNDQ0MTA4YjZjM2VjYmU3N2IxZmNiID0gJCgnPGRpdiBpZD0iaHRtbF82MzJiNjY4OTliYTQ0NDEwOGI2YzNlY2JlNzdiMWZjYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGlsbGNyZXN0IFZpbGxhZ2UgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80NmFkMDQ2YzkzYzA0MDc0YmU5NDExYTdhZjZhMGY1My5zZXRDb250ZW50KGh0bWxfNjMyYjY2ODk5YmE0NDQxMDhiNmMzZWNiZTc3YjFmY2IpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTg2YzcxNDQxOTU0NGI2Y2IwMTc1OWJhOWY2MjkxNGQuYmluZFBvcHVwKHBvcHVwXzQ2YWQwNDZjOTNjMDQwNzRiZTk0MTFhN2FmNmEwZjUzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RiN2MwMjBmOGVmODQyODliMmJjYWI2MGQ3MzMzZmFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzU2MzAzMywtNzkuNTY1OTYzMjk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDVjNTU2ZTRiZWE1NGFjMDlkMjJlMzljNjgyOTE5NmMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODJmMjE4NWVlMDFmNDQzNmI2ZGIxYTcxMDRkZjkzZTYgPSAkKCc8ZGl2IGlkPSJodG1sXzgyZjIxODVlZTAxZjQ0MzZiNmRiMWE3MTA0ZGY5M2U2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IdW1iZXIgU3VtbWl0IENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDVjNTU2ZTRiZWE1NGFjMDlkMjJlMzljNjgyOTE5NmMuc2V0Q29udGVudChodG1sXzgyZjIxODVlZTAxZjQ0MzZiNmRiMWE3MTA0ZGY5M2U2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2RiN2MwMjBmOGVmODQyODliMmJjYWI2MGQ3MzMzZmFkLmJpbmRQb3B1cChwb3B1cF9kNWM1NTZlNGJlYTU0YWMwOWQyMmUzOWM2ODI5MTk2Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hN2E4NTllYmNjYmM0MzM3ODJiMDA2MmE5YzE4M2Q3OCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNDc2NTksLTc5LjUzMjI0MjQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YzNjg4MGMyYmE3NTQyN2ZhYzk4ODIxYWNhNDQzMTI3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzc4OTcxYWQzMDA4NTRmYThhMWEyYTY0MTc3YTNiNzk4ID0gJCgnPGRpdiBpZD0iaHRtbF83ODk3MWFkMzAwODU0ZmE4YTFhMmE2NDE3N2EzYjc5OCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SHVtYmVybGVhLCBFbWVyeSBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YzNjg4MGMyYmE3NTQyN2ZhYzk4ODIxYWNhNDQzMTI3LnNldENvbnRlbnQoaHRtbF83ODk3MWFkMzAwODU0ZmE4YTFhMmE2NDE3N2EzYjc5OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hN2E4NTllYmNjYmM0MzM3ODJiMDA2MmE5YzE4M2Q3OC5iaW5kUG9wdXAocG9wdXBfZjM2ODgwYzJiYTc1NDI3ZmFjOTg4MjFhY2E0NDMxMjcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjVkMTM0NjcwMzhkNGRmM2JmN2QyNzYwNmIwNjkwMGIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTM3ODEzLC03OS40MjgxOTE0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81YTU0ZWQ5M2M1NzE0NmY2OWY3MTFlY2Y1Nzk5N2ZlMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hNjMzYzdiZGUxMmI0Njk4YjIwMDQxZGRkM2JlMjdiMyA9ICQoJzxkaXYgaWQ9Imh0bWxfYTYzM2M3YmRlMTJiNDY5OGIyMDA0MWRkZDNiZTI3YjMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkh1bWV3b29kLUNlZGFydmFsZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVhNTRlZDkzYzU3MTQ2ZjY5ZjcxMWVjZjU3OTk3ZmUwLnNldENvbnRlbnQoaHRtbF9hNjMzYzdiZGUxMmI0Njk4YjIwMDQxZGRkM2JlMjdiMyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mNWQxMzQ2NzAzOGQ0ZGYzYmY3ZDI3NjA2YjA2OTAwYi5iaW5kUG9wdXAocG9wdXBfNWE1NGVkOTNjNTcxNDZmNjlmNzExZWNmNTc5OTdmZTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNWQ2ZjI3NGJmZjFjNDFiYzliNjQ2ZjA2YTZlNTlmN2UgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njg5OTg1LC03OS4zMTU1NzE1OTk5OTk5OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82NjYwYzQ2Y2M5NTU0MTg0YTc3MDllZDg4ZGZlOTdhMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wY2EwNThkZTUyZDY0MmJjODM2MGZkODk3MjQ4NzVlMCA9ICQoJzxkaXYgaWQ9Imh0bWxfMGNhMDU4ZGU1MmQ2NDJiYzgzNjBmZDg5NzI0ODc1ZTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkluZGlhIEJhemFhciwgVGhlIEJlYWNoZXMgV2VzdCBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY2NjBjNDZjYzk1NTQxODRhNzcwOWVkODhkZmU5N2EwLnNldENvbnRlbnQoaHRtbF8wY2EwNThkZTUyZDY0MmJjODM2MGZkODk3MjQ4NzVlMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81ZDZmMjc0YmZmMWM0MWJjOWI2NDZmMDZhNmU1OWY3ZS5iaW5kUG9wdXAocG9wdXBfNjY2MGM0NmNjOTU1NDE4NGE3NzA5ZWQ4OGRmZTk3YTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzdkZmYyNDVhZDAxNGI1NWE0ZDgxMTNiMDRhOGQ0NjUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Njc4NTU2LC03OS41MzIyNDI0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hNTNkZDFmMjQyMmU0NDkxOTBhN2E2NDZmYjE3ZTY3NSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zZGQyYzM4NjEwZWI0MjJiOWJiMjBhOGQzMGIxNGI2YiA9ICQoJzxkaXYgaWQ9Imh0bWxfM2RkMmMzODYxMGViNDIyYjliYjIwYThkMzBiMTRiNmIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPklzbGluZ3RvbiBBdmVudWUsIEh1bWJlciBWYWxsZXkgVmlsbGFnZSBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E1M2RkMWYyNDIyZTQ0OTE5MGE3YTY0NmZiMTdlNjc1LnNldENvbnRlbnQoaHRtbF8zZGQyYzM4NjEwZWI0MjJiOWJiMjBhOGQzMGIxNGI2Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zN2RmZjI0NWFkMDE0YjU1YTRkODExM2IwNGE4ZDQ2NS5iaW5kUG9wdXAocG9wdXBfYTUzZGQxZjI0MjJlNDQ5MTkwYTdhNjQ2ZmIxN2U2NzUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzVlOWYzN2RiMWE4NDRjNTg5ZDJjZWYyYmIzZDljNjEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43Mjc5MjkyLC03OS4yNjIwMjk0MDAwMDAwMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xZTQxYzc4OGY5YjM0NTc4YTMwOWZkNmI0ODFhZGMyMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84Njc2ZGViZGU4NTM0ZjU4OTZjYmY1ZjNlMDczNGQzMyA9ICQoJzxkaXYgaWQ9Imh0bWxfODY3NmRlYmRlODUzNGY1ODk2Y2JmNWYzZTA3MzRkMzMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktlbm5lZHkgUGFyaywgSW9udmlldywgRWFzdCBCaXJjaG1vdW50IFBhcmsgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xZTQxYzc4OGY5YjM0NTc4YTMwOWZkNmI0ODFhZGMyMi5zZXRDb250ZW50KGh0bWxfODY3NmRlYmRlODUzNGY1ODk2Y2JmNWYzZTA3MzRkMzMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzVlOWYzN2RiMWE4NDRjNTg5ZDJjZWYyYmIzZDljNjEuYmluZFBvcHVwKHBvcHVwXzFlNDFjNzg4ZjliMzQ1NzhhMzA5ZmQ2YjQ4MWFkYzIyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVlMzA4ZjNlYzJjNzQ4ZjM5YWQyZDAwYzIxMmQwNjgwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUzMjA1NywtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZmU1NDI3MDQzYzk0YWJmOTdmOTA2NTBiM2QwNmU2YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xMmEyNTYxZTZhNGE0MDY1YTFiNWZjZGU4YjI0NmI1YyA9ICQoJzxkaXYgaWQ9Imh0bWxfMTJhMjU2MWU2YTRhNDA2NWExYjVmY2RlOGIyNDZiNWMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktlbnNpbmd0b24gTWFya2V0LCBDaGluYXRvd24sIEdyYW5nZSBQYXJrIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2ZlNTQyNzA0M2M5NGFiZjk3ZjkwNjUwYjNkMDZlNmIuc2V0Q29udGVudChodG1sXzEyYTI1NjFlNmE0YTQwNjVhMWI1ZmNkZThiMjQ2YjVjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzVlMzA4ZjNlYzJjNzQ4ZjM5YWQyZDAwYzIxMmQwNjgwLmJpbmRQb3B1cChwb3B1cF9jZmU1NDI3MDQzYzk0YWJmOTdmOTA2NTBiM2QwNmU2Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wYmE0MDFhZWM1YmQ0Y2QwYjFhMWQ3NTMzMTk3Y2E1ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY4ODkwNTQsLTc5LjU1NDcyNDQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzI1NGMzZGFmOTdjOTQwYWI4MzlhZmE0NDc5MTViODY2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2IzM2I5Yjc1MDZjZDQ1ZTk5MTdhMzE0YjY4ZDYwYTg0ID0gJCgnPGRpdiBpZD0iaHRtbF9iMzNiOWI3NTA2Y2Q0NWU5OTE3YTMxNGI2OGQ2MGE4NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+S2luZ3N2aWV3IFZpbGxhZ2UsIFN0LiBQaGlsbGlwcywgTWFydGluIEdyb3ZlIEdhcmRlbnMsIFJpY2h2aWV3IEdhcmRlbnMgQ2x1c3RlciAyPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yNTRjM2RhZjk3Yzk0MGFiODM5YWZhNDQ3OTE1Yjg2Ni5zZXRDb250ZW50KGh0bWxfYjMzYjliNzUwNmNkNDVlOTkxN2EzMTRiNjhkNjBhODQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGJhNDAxYWVjNWJkNGNkMGIxYTFkNzUzMzE5N2NhNWUuYmluZFBvcHVwKHBvcHVwXzI1NGMzZGFmOTdjOTQwYWI4MzlhZmE0NDc5MTViODY2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk3Y2Q4OWQ4YmM5NDQ2MjlhYWQ5NzI1YjFjNjhmM2I3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzE4NTE3OTk5OTk5OTk2LC03OS40NjQ3NjMyOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODBmZmI0IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwZmZiNCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yNWJmZDgzZTU2N2Q0NjQ0OTUwMWJlOTAyZGQ4N2Q4ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mNDEwMmRjM2I1MzA0NDQ2OTc2Yjc2ZGU4ZGJhY2M4OSA9ICQoJzxkaXYgaWQ9Imh0bWxfZjQxMDJkYzNiNTMwNDQ0Njk3NmI3NmRlOGRiYWNjODkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxhd3JlbmNlIE1hbm9yLCBMYXdyZW5jZSBIZWlnaHRzIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjViZmQ4M2U1NjdkNDY0NDk1MDFiZTkwMmRkODdkOGYuc2V0Q29udGVudChodG1sX2Y0MTAyZGMzYjUzMDQ0NDY5NzZiNzZkZThkYmFjYzg5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk3Y2Q4OWQ4YmM5NDQ2MjlhYWQ5NzI1YjFjNjhmM2I3LmJpbmRQb3B1cChwb3B1cF8yNWJmZDgzZTU2N2Q0NjQ0OTUwMWJlOTAyZGQ4N2Q4Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81NjlhZjg3MWNlYzk0Y2M4YTA5OTI1OWUzNWExOTI5MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyODAyMDUsLTc5LjM4ODc5MDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWUyODZiY2Q5YjVmNDBlOGI4ODY2ZDE3Y2JkYjcyNzEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODVlMmQ3MWU0NzgzNGZlYmIxZjJlMGJmNWU5MjQ1ZGQgPSAkKCc8ZGl2IGlkPSJodG1sXzg1ZTJkNzFlNDc4MzRmZWJiMWYyZTBiZjVlOTI0NWRkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MYXdyZW5jZSBQYXJrIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWUyODZiY2Q5YjVmNDBlOGI4ODY2ZDE3Y2JkYjcyNzEuc2V0Q29udGVudChodG1sXzg1ZTJkNzFlNDc4MzRmZWJiMWYyZTBiZjVlOTI0NWRkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU2OWFmODcxY2VjOTRjYzhhMDk5MjU5ZTM1YTE5MjkxLmJpbmRQb3B1cChwb3B1cF9lZTI4NmJjZDliNWY0MGU4Yjg4NjZkMTdjYmRiNzI3MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mYTFlY2NjYjFlZmI0MGI2Yjg4ZmMxZTJhMGNmZmNkYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwOTA2MDQsLTc5LjM2MzQ1MTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDU1MTA4MTRmOTFlNDllYTk4ZTY0OTQzOTkzNmU1NDkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmI1NzNhZjQwOGY4NDg5OTg2MTc2ZWI2MDAxYmY0MmUgPSAkKCc8ZGl2IGlkPSJodG1sX2ZiNTczYWY0MDhmODQ4OTk4NjE3NmViNjAwMWJmNDJlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MZWFzaWRlIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDU1MTA4MTRmOTFlNDllYTk4ZTY0OTQzOTkzNmU1NDkuc2V0Q29udGVudChodG1sX2ZiNTczYWY0MDhmODQ4OTk4NjE3NmViNjAwMWJmNDJlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZhMWVjY2NiMWVmYjQwYjZiODhmYzFlMmEwY2ZmY2RiLmJpbmRQb3B1cChwb3B1cF8wNTUxMDgxNGY5MWU0OWVhOThlNjQ5NDM5OTM2ZTU0OSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iNzhjYzg3YjRhNTk0Njk5YmE0N2ExMTA1NWQwYTMzNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY0NzkyNjcwMDAwMDAwNiwtNzkuNDE5NzQ5N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kY2UxZWQ4OThjOTY0Y2YzYmZkYTA4NWUyZjhmNGE1OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82ZjU3M2Q2YmYzYTI0ODI5ODBkYzIyM2Q3ZTA4MjNkZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNmY1NzNkNmJmM2EyNDgyOTgwZGMyMjNkN2UwODIzZGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxpdHRsZSBQb3J0dWdhbCwgVHJpbml0eSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2RjZTFlZDg5OGM5NjRjZjNiZmRhMDg1ZTJmOGY0YTU4LnNldENvbnRlbnQoaHRtbF82ZjU3M2Q2YmYzYTI0ODI5ODBkYzIyM2Q3ZTA4MjNkZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iNzhjYzg3YjRhNTk0Njk5YmE0N2ExMTA1NWQwYTMzNS5iaW5kUG9wdXAocG9wdXBfZGNlMWVkODk4Yzk2NGNmM2JmZGEwODVlMmY4ZjRhNTgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODIzOWIwYWIzYTMxNDgxMmE2YTZiNWE0MWJhNTI4YjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My44MDY2ODYyOTk5OTk5OTYsLTc5LjE5NDM1MzQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZmIzNjAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmZiMzYwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVkZTNmZTE2ZTBkYjQ3YmVhZjg2MGE3ZTQ2NTI1MjFiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJlYzY3NTFlZDcxYTRmNGJiYzRlM2I0ZWZiOTU0MTk2ID0gJCgnPGRpdiBpZD0iaHRtbF8yZWM2NzUxZWQ3MWE0ZjRiYmM0ZTNiNGVmYjk1NDE5NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFsdmVybiwgUm91Z2UgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZGUzZmUxNmUwZGI0N2JlYWY4NjBhN2U0NjUyNTIxYi5zZXRDb250ZW50KGh0bWxfMmVjNjc1MWVkNzFhNGY0YmJjNGUzYjRlZmI5NTQxOTYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODIzOWIwYWIzYTMxNDgxMmE2YTZiNWE0MWJhNTI4YjIuYmluZFBvcHVwKHBvcHVwXzVkZTNmZTE2ZTBkYjQ3YmVhZjg2MGE3ZTQ2NTI1MjFiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U5NWYxMDY2NTk2YjQ2NWZiMTllMmY0ZjFjYzNhYzBlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuODE1MjUyMiwtNzkuMjg0NTc3Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yMjY0MTRiZDk1Yzg0MzEwYmE2MzJjM2Q4ZWE3YjU3OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80NTBkNDA2NDU3MzE0ZGJiYTQ1MWQ5ZDdhNjQ5ZTRlZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNDUwZDQwNjQ1NzMxNGRiYmE0NTFkOWQ3YTY0OWU0ZWUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1pbGxpa2VuLCBBZ2luY291cnQgTm9ydGgsIFN0ZWVsZXMgRWFzdCwgTCYjMzk7QW1vcmVhdXggRWFzdCBDbHVzdGVyIDQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzIyNjQxNGJkOTVjODQzMTBiYTYzMmMzZDhlYTdiNTc5LnNldENvbnRlbnQoaHRtbF80NTBkNDA2NDU3MzE0ZGJiYTQ1MWQ5ZDdhNjQ5ZTRlZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lOTVmMTA2NjU5NmI0NjVmYjE5ZTJmNGYxY2MzYWMwZS5iaW5kUG9wdXAocG9wdXBfMjI2NDE0YmQ5NWM4NDMxMGJhNjMyYzNkOGVhN2I1NzkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjQ4MmRhODkxM2IzNDcwMTk2Y2ExMmY2MGViZWNkOTYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Mjg4NDA4LC03OS41MjA5OTk0MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xYTAyN2VmOGRjMGU0NDE2YjdlOGU1ZWUyYWUwYzcwYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hYzM5ZjVkNWFlMjI0MjVmYmUwMGVmYjMwMGZiYmRmNCA9ICQoJzxkaXYgaWQ9Imh0bWxfYWMzOWY1ZDVhZTIyNDI1ZmJlMDBlZmIzMDBmYmJkZjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1pbWljbyBOVywgVGhlIFF1ZWVuc3dheSBXZXN0LCBTb3V0aCBvZiBCbG9vciwgS2luZ3N3YXkgUGFyayBTb3V0aCBXZXN0LCBSb3lhbCBZb3JrIFNvdXRoIFdlc3QgQ2x1c3RlciAyPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xYTAyN2VmOGRjMGU0NDE2YjdlOGU1ZWUyYWUwYzcwYi5zZXRDb250ZW50KGh0bWxfYWMzOWY1ZDVhZTIyNDI1ZmJlMDBlZmIzMDBmYmJkZjQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZjQ4MmRhODkxM2IzNDcwMTk2Y2ExMmY2MGViZWNkOTYuYmluZFBvcHVwKHBvcHVwXzFhMDI3ZWY4ZGMwZTQ0MTZiN2U4ZTVlZTJhZTBjNzBiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI5Njk0NTg4OTYwNDQwYmI5ZmE2NzM3NjI5YzY3NDEzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjg5NTc0MywtNzkuMzgzMTU5OTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNGJlYThiMjU0Y2U1NDRiMGE5OTlhNzA1MDlkYzQ1MDAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzBiOTk1YmIyYTc1NDQzMWE4MDA4MmYxNGViYTQxM2YgPSAkKCc8ZGl2IGlkPSJodG1sX2MwYjk5NWJiMmE3NTQ0MzFhODAwODJmMTRlYmE0MTNmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Nb29yZSBQYXJrLCBTdW1tZXJoaWxsIEVhc3QgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80YmVhOGIyNTRjZTU0NGIwYTk5OWE3MDUwOWRjNDUwMC5zZXRDb250ZW50KGh0bWxfYzBiOTk1YmIyYTc1NDQzMWE4MDA4MmYxNGViYTQxM2YpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjk2OTQ1ODg5NjA0NDBiYjlmYTY3Mzc2MjljNjc0MTMuYmluZFBvcHVwKHBvcHVwXzRiZWE4YjI1NGNlNTQ0YjBhOTk5YTcwNTA5ZGM0NTAwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzZlYWZhMDNlMmVkNTQ2MWFhODQ0MjA2YThiYzU2ZGYyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjA1NjQ2NiwtNzkuNTAxMzIwNzAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjU4ZDNkY2EzYjkxNDU2Zjg1MjY1MGYyNWYyZjc5YWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmNkMGNjMzJjZWQ1NDA3ZGFkNjM3NGJkMTA4NmU0NjEgPSAkKCc8ZGl2IGlkPSJodG1sX2JjZDBjYzMyY2VkNTQwN2RhZDYzNzRiZDEwODZlNDYxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OZXcgVG9yb250bywgTWltaWNvIFNvdXRoLCBIdW1iZXIgQmF5IFNob3JlcyBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY1OGQzZGNhM2I5MTQ1NmY4NTI2NTBmMjVmMmY3OWFmLnNldENvbnRlbnQoaHRtbF9iY2QwY2MzMmNlZDU0MDdkYWQ2Mzc0YmQxMDg2ZTQ2MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82ZWFmYTAzZTJlZDU0NjFhYTg0NDIwNmE4YmM1NmRmMi5iaW5kUG9wdXAocG9wdXBfNjU4ZDNkY2EzYjkxNDU2Zjg1MjY1MGYyNWYyZjc5YWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTk2ZWJmYmZhNGJkNDc5ZmE1MTc2ZTE5YTUyZDIwZDggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTM3NTYyMDAwMDAwMDYsLTc5LjQ5MDA3MzhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTE0ODQ1ODU3MmY3NDg3Yjk1ODU1YWFmNGM5MDEwYTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2E0YTUxY2M0ZGVkNDVmYzhkOWI4OGRkNjg5Zjg5YzEgPSAkKCc8ZGl2IGlkPSJodG1sXzNhNGE1MWNjNGRlZDQ1ZmM4ZDliODhkZDY4OWY4OWMxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ob3J0aCBQYXJrLCBNYXBsZSBMZWFmIFBhcmssIFVwd29vZCBQYXJrIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTE0ODQ1ODU3MmY3NDg3Yjk1ODU1YWFmNGM5MDEwYTYuc2V0Q29udGVudChodG1sXzNhNGE1MWNjNGRlZDQ1ZmM4ZDliODhkZDY4OWY4OWMxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU5NmViZmJmYTRiZDQ3OWZhNTE3NmUxOWE1MmQyMGQ4LmJpbmRQb3B1cChwb3B1cF81MTQ4NDU4NTcyZjc0ODdiOTU4NTVhYWY0YzkwMTBhNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zNzRmNzY3Mjk3NTk0ZmEyYjFlM2VkZTMxMmRmOWU4YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcxNTM4MzQsLTc5LjQwNTY3ODQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFiY2Y3M2VmMThiNzQyZTk4NWRkZWNkZDI1OThiOTliID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2NhZWZkNWJhMWU1YjRlYzQ5YjQyNjJkOWNkM2Q5YzE3ID0gJCgnPGRpdiBpZD0iaHRtbF9jYWVmZDViYTFlNWI0ZWM0OWI0MjYyZDljZDNkOWMxNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGggVG9yb250byBXZXN0LCBMYXdyZW5jZSBQYXJrIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWJjZjczZWYxOGI3NDJlOTg1ZGRlY2RkMjU5OGI5OWIuc2V0Q29udGVudChodG1sX2NhZWZkNWJhMWU1YjRlYzQ5YjQyNjJkOWNkM2Q5YzE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM3NGY3NjcyOTc1OTRmYTJiMWUzZWRlMzEyZGY5ZThhLmJpbmRQb3B1cChwb3B1cF8xYmNmNzNlZjE4Yjc0MmU5ODVkZGVjZGQyNTk4Yjk5Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83YjFhM2M3OGRkMWQ0YTkxYmQ3MjQ2MjUyM2JmZjIyMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNjc0ODI5OTk5OTk5NCwtNzkuNTk0MDU0NF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNWU0N2VmMTc3MTk0ZDNmYTRmMmFlYjQzZWY0OWNiNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85MmIxMGNlZTc1MzY0YjVhOGVlNGNiYTc3NTAzZDQ4ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfOTJiMTBjZWU3NTM2NGI1YThlZTRjYmE3NzUwM2Q0OGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5vcnRod2VzdCwgV2VzdCBIdW1iZXIgLSBDbGFpcnZpbGxlIENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjVlNDdlZjE3NzE5NGQzZmE0ZjJhZWI0M2VmNDljYjQuc2V0Q29udGVudChodG1sXzkyYjEwY2VlNzUzNjRiNWE4ZWU0Y2JhNzc1MDNkNDhmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdiMWEzYzc4ZGQxZDRhOTFiZDcyNDYyNTIzYmZmMjIyLmJpbmRQb3B1cChwb3B1cF9mNWU0N2VmMTc3MTk0ZDNmYTRmMmFlYjQzZWY0OWNiNCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yZjBhNTkwYzAzNTk0Mjc0OWU4YjcwOWM5YWQzOWE0YiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc2Nzk4MDMsLTc5LjQ4NzI2MTkwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzJmYmY3YWQ0YTZiMjRmMDhiMzRkNWFiYzUyZDBiMDY5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzAyNjk4NGYxMGQ5NTQ3MWM5YWU3ZDhmZGE0Y2FhNWNmID0gJCgnPGRpdiBpZD0iaHRtbF8wMjY5ODRmMTBkOTU0NzFjOWFlN2Q4ZmRhNGNhYTVjZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9ydGh3b29kIFBhcmssIFlvcmsgVW5pdmVyc2l0eSBDbHVzdGVyIDM8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzJmYmY3YWQ0YTZiMjRmMDhiMzRkNWFiYzUyZDBiMDY5LnNldENvbnRlbnQoaHRtbF8wMjY5ODRmMTBkOTU0NzFjOWFlN2Q4ZmRhNGNhYTVjZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yZjBhNTkwYzAzNTk0Mjc0OWU4YjcwOWM5YWQzOWE0Yi5iaW5kUG9wdXAocG9wdXBfMmZiZjdhZDRhNmIyNGYwOGIzNGQ1YWJjNTJkMGIwNjkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTUwN2Q0OGRmNzlkNGFlYjg3MDBmNmQyNGU4OTI1NGIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42MzYyNTc5LC03OS40OTg1MDkwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yMDE5Mzc2Y2I3NWI0MTZmYTg4NWU2ZjAwNjlmMTU5ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80NzM4MWI4NzkzOGM0Yjc5ODJhMTEwMjg0ZTE1YmE1NyA9ICQoJzxkaXYgaWQ9Imh0bWxfNDczODFiODc5MzhjNGI3OTgyYTExMDI4NGUxNWJhNTciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk9sZCBNaWxsIFNvdXRoLCBLaW5nJiMzOTtzIE1pbGwgUGFyaywgU3VubnlsZWEsIEh1bWJlciBCYXksIE1pbWljbyBORSwgVGhlIFF1ZWVuc3dheSBFYXN0LCBSb3lhbCBZb3JrIFNvdXRoIEVhc3QsIEtpbmdzd2F5IFBhcmsgU291dGggRWFzdCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzIwMTkzNzZjYjc1YjQxNmZhODg1ZTZmMDA2OWYxNTlkLnNldENvbnRlbnQoaHRtbF80NzM4MWI4NzkzOGM0Yjc5ODJhMTEwMjg0ZTE1YmE1Nyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xNTA3ZDQ4ZGY3OWQ0YWViODcwMGY2ZDI0ZTg5MjU0Yi5iaW5kUG9wdXAocG9wdXBfMjAxOTM3NmNiNzViNDE2ZmE4ODVlNmYwMDY5ZjE1OWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZWI1Y2NjNjYzMTFhNDk3ZDhmNGRkNGZiZjQ2ZWI5NDIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDg5NTk3LC03OS40NTYzMjVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTUxOTcxY2Y5MTkzNDEwNjg0NDk3OGI1YmUyNGE3NGUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODZkYWZhNzllNjc2NGMwMWIxZjI4OWUyNDU0OWUzNTYgPSAkKCc8ZGl2IGlkPSJodG1sXzg2ZGFmYTc5ZTY3NjRjMDFiMWYyODllMjQ1NDllMzU2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QYXJrZGFsZSwgUm9uY2VzdmFsbGVzIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTUxOTcxY2Y5MTkzNDEwNjg0NDk3OGI1YmUyNGE3NGUuc2V0Q29udGVudChodG1sXzg2ZGFmYTc5ZTY3NjRjMDFiMWYyODllMjQ1NDllMzU2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ViNWNjYzY2MzExYTQ5N2Q4ZjRkZDRmYmY0NmViOTQyLmJpbmRQb3B1cChwb3B1cF9hNTE5NzFjZjkxOTM0MTA2ODQ0OTc4YjViZTI0YTc0ZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wODgxMTUyZWNiMGQ0NTlmODlkYTFkNGJiZmJmNWVjNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNjM5NzIsLTc5LjMwOTkzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82NmU4OGYxMjZmMGE0MTc3YmNmMGIwZTdkYjZhYWQ3NSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82N2E1ZTk4N2QzZDc0YjdjOTM2MWVkMWQ4MzQwMmQzZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNjdhNWU5ODdkM2Q3NGI3YzkzNjFlZDFkODM0MDJkM2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhcmt2aWV3IEhpbGwsIFdvb2RiaW5lIEdhcmRlbnMgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NmU4OGYxMjZmMGE0MTc3YmNmMGIwZTdkYjZhYWQ3NS5zZXRDb250ZW50KGh0bWxfNjdhNWU5ODdkM2Q3NGI3YzkzNjFlZDFkODM0MDJkM2QpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDg4MTE1MmVjYjBkNDU5Zjg5ZGExZDRiYmZiZjVlYzUuYmluZFBvcHVwKHBvcHVwXzY2ZTg4ZjEyNmYwYTQxNzdiY2YwYjBlN2RiNmFhZDc1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzkxYWE1NDA5OTE4ZjQ4ZmE5MjRlZDY5ZTI1YTQ2NTc0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUzMjU4NiwtNzkuMzI5NjU2NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNjk2ZWVlN2MwODA0NzEwOWIzNWI2OGU2ZmMyYTlmMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wNzA4YzY4ODlhN2Q0NmI4OWNmMDg2OTk1ZmZjYmU1NiA9ICQoJzxkaXYgaWQ9Imh0bWxfMDcwOGM2ODg5YTdkNDZiODljZjA4Njk5NWZmY2JlNTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhcmt3b29kcyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE2OTZlZWU3YzA4MDQ3MTA5YjM1YjY4ZTZmYzJhOWYwLnNldENvbnRlbnQoaHRtbF8wNzA4YzY4ODlhN2Q0NmI4OWNmMDg2OTk1ZmZjYmU1Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85MWFhNTQwOTkxOGY0OGZhOTI0ZWQ2OWUyNWE0NjU3NC5iaW5kUG9wdXAocG9wdXBfMTY5NmVlZTdjMDgwNDcxMDliMzViNjhlNmZjMmE5ZjApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzYzMjA4ZDU3ZmNmNDY0YmJlYmE2OWExOWU5MTE3YzYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NjIzMDE1LC03OS4zODk0OTM4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y0YTUxMGZjODQ4YTRlY2ZiMTBhNjcyZjkzNGJkOWRjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2VkODBkNDE4MzE1NDQ5YjU4ZTRjMWJmYmJkODA4ZDJlID0gJCgnPGRpdiBpZD0iaHRtbF9lZDgwZDQxODMxNTQ0OWI1OGU0YzFiZmJiZDgwOGQyZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UXVlZW4mIzM5O3MgUGFyaywgT250YXJpbyBQcm92aW5jaWFsIEdvdmVybm1lbnQgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mNGE1MTBmYzg0OGE0ZWNmYjEwYTY3MmY5MzRiZDlkYy5zZXRDb250ZW50KGh0bWxfZWQ4MGQ0MTgzMTU0NDliNThlNGMxYmZiYmQ4MDhkMmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzYzMjA4ZDU3ZmNmNDY0YmJlYmE2OWExOWU5MTE3YzYuYmluZFBvcHVwKHBvcHVwX2Y0YTUxMGZjODQ4YTRlY2ZiMTBhNjcyZjkzNGJkOWRjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdmMTc5YzAyZDgwYzQ5ZjQ4MWU2YmU4MGJjOTk1ZjhkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjU0MjU5OSwtNzkuMzYwNjM1OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81ZmMyYWIyODIzNmU0OGFiYjEyYjEyNTI4ZmIzMGM3ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NmY4OTBlOGUxMDM0YmU0YTY4OTU4NjYyYzgyNTdkMyA9ICQoJzxkaXYgaWQ9Imh0bWxfNzZmODkwZThlMTAzNGJlNGE2ODk1ODY2MmM4MjU3ZDMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJlZ2VudCBQYXJrLCBIYXJib3VyZnJvbnQgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZmMyYWIyODIzNmU0OGFiYjEyYjEyNTI4ZmIzMGM3ZS5zZXRDb250ZW50KGh0bWxfNzZmODkwZThlMTAzNGJlNGE2ODk1ODY2MmM4MjU3ZDMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2YxNzljMDJkODBjNDlmNDgxZTZiZTgwYmM5OTVmOGQuYmluZFBvcHVwKHBvcHVwXzVmYzJhYjI4MjM2ZTQ4YWJiMTJiMTI1MjhmYjMwYzdlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdiMDY3NDhhMzVmODQwYzU5NjkxMWVmYjMzNmY3MzAwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUwNTcxMjAwMDAwMDEsLTc5LjM4NDU2NzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTZiZDJkZWZhOTQ3NGIwNDgxNDI0ZmQzNzIzOWIzZDIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDE1Y2E1ZjIxOGMxNGRlODg0ODdiZDYyZWI3MWM1ZGEgPSAkKCc8ZGl2IGlkPSJodG1sXzQxNWNhNWYyMThjMTRkZTg4NDg3YmQ2MmViNzFjNWRhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SaWNobW9uZCwgQWRlbGFpZGUsIEtpbmcgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lNmJkMmRlZmE5NDc0YjA0ODE0MjRmZDM3MjM5YjNkMi5zZXRDb250ZW50KGh0bWxfNDE1Y2E1ZjIxOGMxNGRlODg0ODdiZDYyZWI3MWM1ZGEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2IwNjc0OGEzNWY4NDBjNTk2OTExZWZiMzM2ZjczMDAuYmluZFBvcHVwKHBvcHVwX2U2YmQyZGVmYTk0NzRiMDQ4MTQyNGZkMzcyMzliM2QyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVkMzM0OTY0MjBjNTQyOGM5MmY2NWNiYzhjZTNlNzc5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc5NTYyNiwtNzkuMzc3NTI5NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWIyNzBkMWMzODgzNGFkM2I2ODNjMThjY2IyZDNiZmYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzI0YWI0MjJmNzYzNDY0ZThkMGJhOWFjNjVlYmQ0MTcgPSAkKCc8ZGl2IGlkPSJodG1sXzcyNGFiNDIyZjc2MzQ2NGU4ZDBiYTlhYzY1ZWJkNDE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Sb3NlZGFsZSBDbHVzdGVyIDA8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2ViMjcwZDFjMzg4MzRhZDNiNjgzYzE4Y2NiMmQzYmZmLnNldENvbnRlbnQoaHRtbF83MjRhYjQyMmY3NjM0NjRlOGQwYmE5YWM2NWViZDQxNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81ZDMzNDk2NDIwYzU0MjhjOTJmNjVjYmM4Y2UzZTc3OS5iaW5kUG9wdXAocG9wdXBfZWIyNzBkMWMzODgzNGFkM2I2ODNjMThjY2IyZDNiZmYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDFiNDZhMmZiYzA2NGM5NWE5ZWI5NDk0MDNiNTVlYmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MTE2OTQ4LC03OS40MTY5MzU1OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODBmZmI0IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwZmZiNCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80Njc4OGJkMTlkMTE0MjIxOWExZjYxYzAzMjdlYWEwYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mZGVhMzYyNzUyODA0ZDVhODA3NDY0YjAzOWNkNzM5OCA9ICQoJzxkaXYgaWQ9Imh0bWxfZmRlYTM2Mjc1MjgwNGQ1YTgwNzQ2NGIwMzljZDczOTgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJvc2VsYXduIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDY3ODhiZDE5ZDExNDIyMTlhMWY2MWMwMzI3ZWFhMGIuc2V0Q29udGVudChodG1sX2ZkZWEzNjI3NTI4MDRkNWE4MDc0NjRiMDM5Y2Q3Mzk4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2QxYjQ2YTJmYmMwNjRjOTVhOWViOTQ5NDAzYjU1ZWJhLmJpbmRQb3B1cChwb3B1cF80Njc4OGJkMTlkMTE0MjIxOWExZjYxYzAzMjdlYWEwYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yOTU3ZmRmZmUzNDU0MTkyYTgxOTY0MzhmY2Y1MDFhOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4NDUzNTEsLTc5LjE2MDQ5NzA5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZmIzNjAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmZiMzYwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2QyNTIwY2Q0MWUzMDQzZTM5ZGNjNDc2MjY3ZjE0OWE0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzViNTE5YjM2M2JiOTQxNzM5MzU4YmNiYjM0MTEwNzYyID0gJCgnPGRpdiBpZD0iaHRtbF81YjUxOWIzNjNiYjk0MTczOTM1OGJjYmIzNDExMDc2MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um91Z2UgSGlsbCwgUG9ydCBVbmlvbiwgSGlnaGxhbmQgQ3JlZWsgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kMjUyMGNkNDFlMzA0M2UzOWRjYzQ3NjI2N2YxNDlhNC5zZXRDb250ZW50KGh0bWxfNWI1MTliMzYzYmI5NDE3MzkzNThiY2JiMzQxMTA3NjIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjk1N2ZkZmZlMzQ1NDE5MmE4MTk2NDM4ZmNmNTAxYTkuYmluZFBvcHVwKHBvcHVwX2QyNTIwY2Q0MWUzMDQzZTM5ZGNjNDc2MjY3ZjE0OWE0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U1ZTI5NGMxNWM0NTRhZjE5ZDBhOTFlOTBlMjg3YWMxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjUxNTcwNiwtNzkuNDg0NDQ5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80ODExODRhN2FlZmI0N2YzYmQ4MjVlOGUyNjk3OGFiMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yNzRkOWM4ODY2MjM0ZTg5OTliNzZjMDhhOTFmZTIwMSA9ICQoJzxkaXYgaWQ9Imh0bWxfMjc0ZDljODg2NjIzNGU4OTk5Yjc2YzA4YTkxZmUyMDEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJ1bm55bWVkZSwgU3dhbnNlYSBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ4MTE4NGE3YWVmYjQ3ZjNiZDgyNWU4ZTI2OTc4YWIyLnNldENvbnRlbnQoaHRtbF8yNzRkOWM4ODY2MjM0ZTg5OTliNzZjMDhhOTFmZTIwMSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lNWUyOTRjMTVjNDU0YWYxOWQwYTkxZTkwZTI4N2FjMS5iaW5kUG9wdXAocG9wdXBfNDgxMTg0YTdhZWZiNDdmM2JkODI1ZThlMjY5NzhhYjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGM3YjlmZGE3N2JlNGY1OGJmZDI5Y2JkMTYwYjBkODEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NzMxODUyOTk5OTk5OSwtNzkuNDg3MjYxOTAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzgyYWFhNDU2MmQwNDllMDgwOWY2N2MzMDdlYTA1N2MgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjE5ZThiMjU5NTQ5NDhlOWE1NmYwMGE4YjBkYWI0NzAgPSAkKCc8ZGl2IGlkPSJodG1sXzIxOWU4YjI1OTU0OTQ4ZTlhNTZmMDBhOGIwZGFiNDcwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SdW5ueW1lZGUsIFRoZSBKdW5jdGlvbiBOb3J0aCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzM4MmFhYTQ1NjJkMDQ5ZTA4MDlmNjdjMzA3ZWEwNTdjLnNldENvbnRlbnQoaHRtbF8yMTllOGIyNTk1NDk0OGU5YTU2ZjAwYThiMGRhYjQ3MCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80YzdiOWZkYTc3YmU0ZjU4YmZkMjljYmQxNjBiMGQ4MS5iaW5kUG9wdXAocG9wdXBfMzgyYWFhNDU2MmQwNDllMDgwOWY2N2MzMDdlYTA1N2MpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGJhNDFjNmNkMDhlNGQ1ZTk3ODViZjIwZGE0MTBiM2MgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NDQ3MzQyLC03OS4yMzk0NzYwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iNTNjNWVlOTFjYjk0ZmI3ODAwMjBiNjFiNjZhOTgzYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jMTlkZmQ2MDk4NmY0ODUzOWZhMzQ1M2JlNWRiNzQxZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYzE5ZGZkNjA5ODZmNDg1MzlmYTM0NTNiZTVkYjc0MWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNjYXJib3JvdWdoIFZpbGxhZ2UgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iNTNjNWVlOTFjYjk0ZmI3ODAwMjBiNjFiNjZhOTgzYS5zZXRDb250ZW50KGh0bWxfYzE5ZGZkNjA5ODZmNDg1MzlmYTM0NTNiZTVkYjc0MWQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNGJhNDFjNmNkMDhlNGQ1ZTk3ODViZjIwZGE0MTBiM2MuYmluZFBvcHVwKHBvcHVwX2I1M2M1ZWU5MWNiOTRmYjc4MDAyMGI2MWI2NmE5ODNhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzA3NDNmYmY4M2I0NzQ3OWZiYmFmNGY4NmM2NzlkODE1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzM5NDE2Mzk5OTk5OTk2LC03OS41ODg0MzY5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzEyNTUwYzc3N2Y1NTQ4NGFhMWMzOTAxYzVkOGM1MzRkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzY3ODMwYWFlNmRlYzRjNTc5MWFhZjM5NDc1MGRlMDcxID0gJCgnPGRpdiBpZD0iaHRtbF82NzgzMGFhZTZkZWM0YzU3OTFhYWYzOTQ3NTBkZTA3MSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U291dGggU3RlZWxlcywgU2lsdmVyc3RvbmUsIEh1bWJlcmdhdGUsIEphbWVzdG93biwgTW91bnQgT2xpdmUsIEJlYXVtb25kIEhlaWdodHMsIFRoaXN0bGV0b3duLCBBbGJpb24gR2FyZGVucyBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzEyNTUwYzc3N2Y1NTQ4NGFhMWMzOTAxYzVkOGM1MzRkLnNldENvbnRlbnQoaHRtbF82NzgzMGFhZTZkZWM0YzU3OTFhYWYzOTQ3NTBkZTA3MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wNzQzZmJmODNiNDc0NzlmYmJhZjRmODZjNjc5ZDgxNS5iaW5kUG9wdXAocG9wdXBfMTI1NTBjNzc3ZjU1NDg0YWExYzM5MDFjNWQ4YzUzNGQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzczYmZjNTA2N2U2NDAyMzg0ZWQyNjRiMjA1ZjFhMjcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NTE0OTM5LC03OS4zNzU0MTc5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzZlMjQxMzQ0Y2MwOTQ1Y2Y4ZjFmZjBmZjYwY2U0ZmU4ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ3OWRkMWM3YTM3ZDQ4ZWI4NjYwMWNlMmRmNzMwMGUxID0gJCgnPGRpdiBpZD0iaHRtbF80NzlkZDFjN2EzN2Q0OGViODY2MDFjZTJkZjczMDBlMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QuIEphbWVzIFRvd24gQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82ZTI0MTM0NGNjMDk0NWNmOGYxZmYwZmY2MGNlNGZlOC5zZXRDb250ZW50KGh0bWxfNDc5ZGQxYzdhMzdkNDhlYjg2NjAxY2UyZGY3MzAwZTEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzczYmZjNTA2N2U2NDAyMzg0ZWQyNjRiMjA1ZjFhMjcuYmluZFBvcHVwKHBvcHVwXzZlMjQxMzQ0Y2MwOTQ1Y2Y4ZjFmZjBmZjYwY2U0ZmU4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzYzZWVkZjU2ZjkzMzQ1MjlhNzhkMTU3NTIzMzJkZTE3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjY3OTY3LC03OS4zNjc2NzUzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Q4NTFjNmQ1OGZhZjQ4OTA5NWViZjQ5OGYyNGUzZjgwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzBhZWQ4Yzc2N2IzMjQzNjdhYTVhZjU3ODIxZTEyNDBjID0gJCgnPGRpdiBpZD0iaHRtbF8wYWVkOGM3NjdiMzI0MzY3YWE1YWY1NzgyMWUxMjQwYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QuIEphbWVzIFRvd24sIENhYmJhZ2V0b3duIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDg1MWM2ZDU4ZmFmNDg5MDk1ZWJmNDk4ZjI0ZTNmODAuc2V0Q29udGVudChodG1sXzBhZWQ4Yzc2N2IzMjQzNjdhYTVhZjU3ODIxZTEyNDBjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzYzZWVkZjU2ZjkzMzQ1MjlhNzhkMTU3NTIzMzJkZTE3LmJpbmRQb3B1cChwb3B1cF9kODUxYzZkNThmYWY0ODkwOTVlYmY0OThmMjRlM2Y4MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wMzVmNWY2ZTY4NmY0Y2M5ODgwODQ5YmRjODkyOTdiNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc5OTUyNTIwMDAwMDAwNSwtNzkuMzE4Mzg4N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNDdmOTBiNDI1Njg0NTQxOTIzMDIyNDk1OThiZTM4ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85MGNmZDZkNzJmYTg0NTUzOTFmYzAxMjc3YjZlYWE4ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfOTBjZmQ2ZDcyZmE4NDU1MzkxZmMwMTI3N2I2ZWFhOGQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0ZWVsZXMgV2VzdCwgTCYjMzk7QW1vcmVhdXggV2VzdCBDbHVzdGVyIDQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y0N2Y5MGI0MjU2ODQ1NDE5MjMwMjI0OTU5OGJlMzhmLnNldENvbnRlbnQoaHRtbF85MGNmZDZkNzJmYTg0NTUzOTFmYzAxMjc3YjZlYWE4ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wMzVmNWY2ZTY4NmY0Y2M5ODgwODQ5YmRjODkyOTdiNC5iaW5kUG9wdXAocG9wdXBfZjQ3ZjkwYjQyNTY4NDU0MTkyMzAyMjQ5NTk4YmUzOGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZGY1YTk0NDU3MDI0NDExZjhhMDMwOTM0NmQwZmYxYmUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42NDY0MzUyLC03OS4zNzQ4NDU5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xZGQyNjdhOTYzYjA0ZTc5OTBhNmRiZTk4MmFkMGVhYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80ZDkwNzViZDYwNjg0NjY0YjQ2NDM3Zjg1MjFmNWZlMCA9ICQoJzxkaXYgaWQ9Imh0bWxfNGQ5MDc1YmQ2MDY4NDY2NGI0NjQzN2Y4NTIxZjVmZTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0biBBIFBPIEJveGVzIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMWRkMjY3YTk2M2IwNGU3OTkwYTZkYmU5ODJhZDBlYWMuc2V0Q29udGVudChodG1sXzRkOTA3NWJkNjA2ODQ2NjRiNDY0MzdmODUyMWY1ZmUwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2RmNWE5NDQ1NzAyNDQxMWY4YTAzMDkzNDZkMGZmMWJlLmJpbmRQb3B1cChwb3B1cF8xZGQyNjdhOTYzYjA0ZTc5OTBhNmRiZTk4MmFkMGVhYyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zNjMwM2FmMWI3ODg0MDZhOGQ1Mjk4OTExZDEyZWY2OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1OTUyNTUsLTc5LjM0MDkyM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lOTIyMzk0YTllNzQ0YzBiODc5Nzc0OTNkYWRmOGNlNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lZWViNDliNzAxYTg0NTBhOTNhNzc2ZWNlZjBjMjY0YiA9ICQoJzxkaXYgaWQ9Imh0bWxfZWVlYjQ5YjcwMWE4NDUwYTkzYTc3NmVjZWYwYzI2NGIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0dWRpbyBEaXN0cmljdCBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2U5MjIzOTRhOWU3NDRjMGI4Nzk3NzQ5M2RhZGY4Y2U3LnNldENvbnRlbnQoaHRtbF9lZWViNDliNzAxYTg0NTBhOTNhNzc2ZWNlZjBjMjY0Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zNjMwM2FmMWI3ODg0MDZhOGQ1Mjk4OTExZDEyZWY2OS5iaW5kUG9wdXAocG9wdXBfZTkyMjM5NGE5ZTc0NGMwYjg3OTc3NDkzZGFkZjhjZTcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzEyZTUwMWNkMDY1NGFjNWFkNmQxNGZlYWRlMDVhNTUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42ODY0MTIyOTk5OTk5OSwtNzkuNDAwMDQ5M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85Yjc4ZjZmOWQ3ZGE0M2EzYmExMTc3MTcxYTBkZThmZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85Y2IwNWI0MzMwYzA0ZDYzOTY4MDYxMWFlYjUyZTM4ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfOWNiMDViNDMzMGMwNGQ2Mzk2ODA2MTFhZWI1MmUzOGQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN1bW1lcmhpbGwgV2VzdCwgUmF0aG5lbGx5LCBTb3V0aCBIaWxsLCBGb3Jlc3QgSGlsbCBTRSwgRGVlciBQYXJrIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOWI3OGY2ZjlkN2RhNDNhM2JhMTE3NzE3MWEwZGU4ZmUuc2V0Q29udGVudChodG1sXzljYjA1YjQzMzBjMDRkNjM5NjgwNjExYWViNTJlMzhkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzcxMmU1MDFjZDA2NTRhYzVhZDZkMTRmZWFkZTA1YTU1LmJpbmRQb3B1cChwb3B1cF85Yjc4ZjZmOWQ3ZGE0M2EzYmExMTc3MTcxYTBkZThmZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MTczNDY2M2I2ZjM0MDFiYjJkZTVjY2M4OGU2OGQxNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY3MjcwOTcsLTc5LjQwNTY3ODQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZjAwMDAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmYwMDAwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2IwZGJmNjIyYWU5NDRjN2NiNWIyYTYzOGFiN2RlYmRmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzE4MzZhMmIzYTE1MjQ1OGFhZWYxNjNhNjBmZjAxZWJiID0gJCgnPGRpdiBpZD0iaHRtbF8xODM2YTJiM2ExNTI0NThhYWVmMTYzYTYwZmYwMWViYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VGhlIEFubmV4LCBOb3J0aCBNaWR0b3duLCBZb3JrdmlsbGUgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMGRiZjYyMmFlOTQ0YzdjYjViMmE2MzhhYjdkZWJkZi5zZXRDb250ZW50KGh0bWxfMTgzNmEyYjNhMTUyNDU4YWFlZjE2M2E2MGZmMDFlYmIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTE3MzQ2NjNiNmYzNDAxYmIyZGU1Y2NjODhlNjhkMTcuYmluZFBvcHVwKHBvcHVwX2IwZGJmNjIyYWU5NDRjN2NiNWIyYTYzOGFiN2RlYmRmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRmZjg3ZTFkNTlkZTRmNjg4ZTRlYTk1NzBlMmFhNzllID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjc2MzU3Mzk5OTk5OTksLTc5LjI5MzAzMTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTMxMDJhYTM1ZjA0NGFkMDg1NWExZTkwNGZjNzVhYTMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGFmNDc4YjNmMjAyNDhiZTk2NzNhODZjMzQzMDJlYWYgPSAkKCc8ZGl2IGlkPSJodG1sX2RhZjQ3OGIzZjIwMjQ4YmU5NjczYTg2YzM0MzAyZWFmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgQmVhY2hlcyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2UzMTAyYWEzNWYwNDRhZDA4NTVhMWU5MDRmYzc1YWEzLnNldENvbnRlbnQoaHRtbF9kYWY0NzhiM2YyMDI0OGJlOTY3M2E4NmMzNDMwMmVhZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80ZmY4N2UxZDU5ZGU0ZjY4OGU0ZWE5NTcwZTJhYTc5ZS5iaW5kUG9wdXAocG9wdXBfZTMxMDJhYTM1ZjA0NGFkMDg1NWExZTkwNGZjNzVhYTMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTdjNTExYWEwMzdjNGRmOGE3ZTc2MGExM2UzNzM1MzggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42Nzk1NTcxLC03OS4zNTIxODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDU5MjlkMWU5ZTNmNDY2OGE2NjkwMjRjY2FlNTgyNDQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDBiYzA1NzVkMTUzNDExYWE2MDMwZjFmMmU1NzA5ZGQgPSAkKCc8ZGl2IGlkPSJodG1sX2QwYmMwNTc1ZDE1MzQxMWFhNjAzMGYxZjJlNTcwOWRkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgRGFuZm9ydGggV2VzdCwgUml2ZXJkYWxlIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDU5MjlkMWU5ZTNmNDY2OGE2NjkwMjRjY2FlNTgyNDQuc2V0Q29udGVudChodG1sX2QwYmMwNTc1ZDE1MzQxMWFhNjAzMGYxZjJlNTcwOWRkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2U3YzUxMWFhMDM3YzRkZjhhN2U3NjBhMTNlMzczNTM4LmJpbmRQb3B1cChwb3B1cF80NTkyOWQxZTllM2Y0NjY4YTY2OTAyNGNjYWU1ODI0NCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hYTU1OWEwOTE0MTY0NTUwYjIzZTBmN2E4NTJlMjllNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MzY1MzYwMDAwMDAwNSwtNzkuNTA2OTQzNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMDBiNWViIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzAwYjVlYiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hZjRhMjY1M2QxOWQ0NDFhOGM4MDVmYWJlYWQ1MzI4MiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80NTE1N2VmOTNkYjc0MzkzYTVlNjAwYWIxY2JhNDRjYSA9ICQoJzxkaXYgaWQ9Imh0bWxfNDUxNTdlZjkzZGI3NDM5M2E1ZTYwMGFiMWNiYTQ0Y2EiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBLaW5nc3dheSwgTW9udGdvbWVyeSBSb2FkLCBPbGQgTWlsbCBOb3J0aCBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2FmNGEyNjUzZDE5ZDQ0MWE4YzgwNWZhYmVhZDUzMjgyLnNldENvbnRlbnQoaHRtbF80NTE1N2VmOTNkYjc0MzkzYTVlNjAwYWIxY2JhNDRjYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hYTU1OWEwOTE0MTY0NTUwYjIzZTBmN2E4NTJlMjllNi5iaW5kUG9wdXAocG9wdXBfYWY0YTI2NTNkMTlkNDQxYThjODA1ZmFiZWFkNTMyODIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGVhOTg1OWVjYTYwNGU2ZDg4Mzc0YTk3ZGJhYmZiYjMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43MDUzNjg5LC03OS4zNDkzNzE5MDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83YWRiMWZmZDk4Yzc0ZDYxYmNmZTM3YTc2MjU3NDAxZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMjkwMjRjNTVjZjc0ZTEyYWNmYmNjYWViY2RmZWU0MyA9ICQoJzxkaXYgaWQ9Imh0bWxfMzI5MDI0YzU1Y2Y3NGUxMmFjZmJjY2FlYmNkZmVlNDMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRob3JuY2xpZmZlIFBhcmsgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83YWRiMWZmZDk4Yzc0ZDYxYmNmZTM3YTc2MjU3NDAxZi5zZXRDb250ZW50KGh0bWxfMzI5MDI0YzU1Y2Y3NGUxMmFjZmJjY2FlYmNkZmVlNDMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGVhOTg1OWVjYTYwNGU2ZDg4Mzc0YTk3ZGJhYmZiYjMuYmluZFBvcHVwKHBvcHVwXzdhZGIxZmZkOThjNzRkNjFiY2ZlMzdhNzYyNTc0MDFmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q4MjIxYTI2YmMzZjRkM2M5NGEyODI2YzE5NjNiOTE3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjQ3MTc2OCwtNzkuMzgxNTc2NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTZkNjNkMzcyNDdkNDEzZWI0YjA2MTlkMDQxYmJjZDAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDUzODM0NjhmZDAyNDg1YTg2OTFiNjU3NzZlMjBlOGEgPSAkKCc8ZGl2IGlkPSJodG1sXzQ1MzgzNDY4ZmQwMjQ4NWE4NjkxYjY1Nzc2ZTIwZThhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ub3JvbnRvIERvbWluaW9uIENlbnRyZSwgRGVzaWduIEV4Y2hhbmdlIENsdXN0ZXIgMDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTZkNjNkMzcyNDdkNDEzZWI0YjA2MTlkMDQxYmJjZDAuc2V0Q29udGVudChodG1sXzQ1MzgzNDY4ZmQwMjQ4NWE4NjkxYjY1Nzc2ZTIwZThhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Q4MjIxYTI2YmMzZjRkM2M5NGEyODI2YzE5NjNiOTE3LmJpbmRQb3B1cChwb3B1cF9hNmQ2M2QzNzI0N2Q0MTNlYjRiMDYxOWQwNDFiYmNkMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kNTI5MWE2YjM0YTE0MjFiYWNjOWQ5ZDU0OTgwMjRmMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY2MjY5NTYsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmMDAwMCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZjAwMDAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWE5OGE0ZjVlNGMxNDQ0MjkzNTFmOTQ3OTNhMDA1MmQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmVmNWE4MDBkMTdiNDA0ZTgwM2QwMTQ1MThmZjBlOTcgPSAkKCc8ZGl2IGlkPSJodG1sX2ZlZjVhODAwZDE3YjQwNGU4MDNkMDE0NTE4ZmYwZTk3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Vbml2ZXJzaXR5IG9mIFRvcm9udG8sIEhhcmJvcmQgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81YTk4YTRmNWU0YzE0NDQyOTM1MWY5NDc5M2EwMDUyZC5zZXRDb250ZW50KGh0bWxfZmVmNWE4MDBkMTdiNDA0ZTgwM2QwMTQ1MThmZjBlOTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDUyOTFhNmIzNGExNDIxYmFjYzlkOWQ1NDk4MDI0ZjAuYmluZFBvcHVwKHBvcHVwXzVhOThhNGY1ZTRjMTQ0NDI5MzUxZjk0NzkzYTAwNTJkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VkNDkyOTc2Mzg1OTQwNzFhYTBiMmM0NjJmMWRkZjliID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuODM2MTI0NzAwMDAwMDA2LC03OS4yMDU2MzYwOTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmZiMzYwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmYjM2MCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83YWNiNjMzMzQ0MGM0YTg5ODlhYWRmMDY5YmE1YzhkZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wMzcyYTA5YzA0OWI0N2EzODI4MzFhYzM2NWUyZmE1MiA9ICQoJzxkaXYgaWQ9Imh0bWxfMDM3MmEwOWMwNDliNDdhMzgyODMxYWMzNjVlMmZhNTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlVwcGVyIFJvdWdlIENsdXN0ZXIgNDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfN2FjYjYzMzM0NDBjNGE4OTg5YWFkZjA2OWJhNWM4ZGYuc2V0Q29udGVudChodG1sXzAzNzJhMDljMDQ5YjQ3YTM4MjgzMWFjMzY1ZTJmYTUyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2VkNDkyOTc2Mzg1OTQwNzFhYTBiMmM0NjJmMWRkZjliLmJpbmRQb3B1cChwb3B1cF83YWNiNjMzMzQ0MGM0YTg5ODlhYWRmMDY5YmE1YzhkZik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84NWZmNzM0NWU3MDE0NWU4OGZkODA0M2VlMGU1YTNjYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcyNTg4MjI5OTk5OTk5NSwtNzkuMzE1NTcxNTk5OTk5OThdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmNhZmU1OTk1MmNhNGNmMTlkZDA0NDUxYTZmNTkzYmMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzgxNGE3YzBmYmZlNDk1ZDkzMzM0YjI2ZDI2NWE0ZjAgPSAkKCc8ZGl2IGlkPSJodG1sXzc4MTRhN2MwZmJmZTQ5NWQ5MzMzNGIyNmQyNjVhNGYwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5WaWN0b3JpYSBWaWxsYWdlIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmNhZmU1OTk1MmNhNGNmMTlkZDA0NDUxYTZmNTkzYmMuc2V0Q29udGVudChodG1sXzc4MTRhN2MwZmJmZTQ5NWQ5MzMzNGIyNmQyNjVhNGYwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzg1ZmY3MzQ1ZTcwMTQ1ZTg4ZmQ4MDQzZWUwZTVhM2NiLmJpbmRQb3B1cChwb3B1cF9mY2FmZTU5OTUyY2E0Y2YxOWRkMDQ0NTFhNmY1OTNiYyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84MWE2ODcxYWFjZGM0NzFhODMxMjRkNDRmZWVmZjA2NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjY1MDk0MzIsLTc5LjU1NDcyNDQwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVlYmVlNGI1NzA1MDQ1MTFhZWFjNzhkZGNhOWUxZTVmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2M5ODc2OGE1N2Y2MDQ2YjY4MWY4YjZmZTk4MTNkZGZhID0gJCgnPGRpdiBpZD0iaHRtbF9jOTg3NjhhNTdmNjA0NmI2ODFmOGI2ZmU5ODEzZGRmYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdCBEZWFuZSBQYXJrLCBQcmluY2VzcyBHYXJkZW5zLCBNYXJ0aW4gR3JvdmUsIElzbGluZ3RvbiwgQ2xvdmVyZGFsZSBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVlYmVlNGI1NzA1MDQ1MTFhZWFjNzhkZGNhOWUxZTVmLnNldENvbnRlbnQoaHRtbF9jOTg3NjhhNTdmNjA0NmI2ODFmOGI2ZmU5ODEzZGRmYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84MWE2ODcxYWFjZGM0NzFhODMxMjRkNDRmZWVmZjA2NS5iaW5kUG9wdXAocG9wdXBfNWViZWU0YjU3MDUwNDUxMWFlYWM3OGRkY2E5ZTFlNWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWIyODZkODExNDBjNDBiYTg4N2RiZjE0Mjk2NjNjM2QgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My42OTYzMTksLTc5LjUzMjI0MjQwMDAwMDAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMwMGI1ZWIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjMDBiNWViIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzgzNWZiYjE4OTkyMDQxZjRhZmYzYmMxYzQ2M2FhZjQ3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ZmZmFhOTFiZjc5NTQ5MDY4ZjY5MTQ5ZjMwNzc1MTE4ID0gJCgnPGRpdiBpZD0iaHRtbF9mZmZhYTkxYmY3OTU0OTA2OGY2OTE0OWYzMDc3NTExOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2VzdG1vdW50IENsdXN0ZXIgMjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODM1ZmJiMTg5OTIwNDFmNGFmZjNiYzFjNDYzYWFmNDcuc2V0Q29udGVudChodG1sX2ZmZmFhOTFiZjc5NTQ5MDY4ZjY5MTQ5ZjMwNzc1MTE4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzliMjg2ZDgxMTQwYzQwYmE4ODdkYmYxNDI5NjYzYzNkLmJpbmRQb3B1cChwb3B1cF84MzVmYmIxODk5MjA0MWY0YWZmM2JjMWM0NjNhYWY0Nyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lMjA0YTkwMTViZDA0MGMxYmNiNzYzNmNiMmMwZGJjMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjcwNjg3NiwtNzkuNTE4MTg4NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzU3Yjk5NWNiNTMxNDczODhmNTRkZGYxNWYwYTk5ZDcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTU5OWRlNDAwNmEwNDJlYTkxYzYzYjgzYTlhMDQ4OTkgPSAkKCc8ZGl2IGlkPSJodG1sX2E1OTlkZTQwMDZhMDQyZWE5MWM2M2I4M2E5YTA0ODk5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XZXN0b24gQ2x1c3RlciAyPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zNTdiOTk1Y2I1MzE0NzM4OGY1NGRkZjE1ZjBhOTlkNy5zZXRDb250ZW50KGh0bWxfYTU5OWRlNDAwNmEwNDJlYTkxYzYzYjgzYTlhMDQ4OTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTIwNGE5MDE1YmQwNDBjMWJjYjc2MzZjYjJjMGRiYzMuYmluZFBvcHVwKHBvcHVwXzM1N2I5OTVjYjUzMTQ3Mzg4ZjU0ZGRmMTVmMGE5OWQ3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk0NzRmZmEyM2RkZDQ5ZGI5NTE2MTAyMmYwZmY4NGUwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzUwMDcxNTAwMDAwMDA0LC03OS4yOTU4NDkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZmIzNjAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmZiMzYwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFjMGRmNGJhZmNiNTQ1NDViMDIzODE1MzNiZTZiMGM0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzNiZjNlYWMyMTBhNTQzNzlhN2VjOTNmNDk4ZGQ0Y2RiID0gJCgnPGRpdiBpZD0iaHRtbF8zYmYzZWFjMjEwYTU0Mzc5YTdlYzkzZjQ5OGRkNGNkYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2V4Zm9yZCwgTWFyeXZhbGUgQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xYzBkZjRiYWZjYjU0NTQ1YjAyMzgxNTMzYmU2YjBjNC5zZXRDb250ZW50KGh0bWxfM2JmM2VhYzIxMGE1NDM3OWE3ZWM5M2Y0OThkZDRjZGIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTQ3NGZmYTIzZGRkNDlkYjk1MTYxMDIyZjBmZjg0ZTAuYmluZFBvcHVwKHBvcHVwXzFjMGRmNGJhZmNiNTQ1NDViMDIzODE1MzNiZTZiMGM0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q4YzUyMTk2ZTA3OTQ0NTdhZmYyNDBiZTFkNjNiMjc4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzg5MDUzLC03OS40MDg0OTI3OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODBmZmI0IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwZmZiNCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9mYjk2MjQ5YzJkMjQ0NDE3YjIzZWQ1MjExYmE4YjQwZSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mOGMyZDg5Zjc1ZWM0N2RlYTI5MDZhZWM2NGQwYTEyMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80MTA5MTVjZGY0NDE0ODcxYjQ1ZjdlMDFhZTllNDllYiA9ICQoJzxkaXYgaWQ9Imh0bWxfNDEwOTE1Y2RmNDQxNDg3MWI0NWY3ZTAxYWU5ZTQ5ZWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldpbGxvd2RhbGUsIE5ld3RvbmJyb29rIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjhjMmQ4OWY3NWVjNDdkZWEyOTA2YWVjNjRkMGExMjAuc2V0Q29udGVudChodG1sXzQxMDkxNWNkZjQ0MTQ4NzFiNDVmN2UwMWFlOWU0OWViKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Q4YzUyMTk2ZTA3OTQ0NTdhZmYyNDBiZTFkNjNiMjc4LmJpbmRQb3B1cChwb3B1cF9mOGMyZDg5Zjc1ZWM0N2RlYTI5MDZhZWM2NGQwYTEyMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84MmY3ZDUxNWU2OTE0MTIwODU5YWYyMzUwZWJkNmQzZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc3MDExOTksLTc5LjQwODQ5Mjc5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MGZmYjQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODBmZmI0IiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U2ZThkZjI5MjRiYTQ4Y2I4YTBjZmU0M2MyYjM2NTdkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZlODNlZGI2N2YxNzRkNWU4OWU2NTNjNjIyYjVlZmRiID0gJCgnPGRpdiBpZD0iaHRtbF82ZTgzZWRiNjdmMTc0ZDVlODllNjUzYzYyMmI1ZWZkYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2lsbG93ZGFsZSwgV2lsbG93ZGFsZSBFYXN0IENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTZlOGRmMjkyNGJhNDhjYjhhMGNmZTQzYzJiMzY1N2Quc2V0Q29udGVudChodG1sXzZlODNlZGI2N2YxNzRkNWU4OWU2NTNjNjIyYjVlZmRiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgyZjdkNTE1ZTY5MTQxMjA4NTlhZjIzNTBlYmQ2ZDNkLmJpbmRQb3B1cChwb3B1cF9lNmU4ZGYyOTI0YmE0OGNiOGEwY2ZlNDNjMmIzNjU3ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yYzgzMGRjMGIxMDQ0NjVjODQ0MmQ0YWM5NzQzOWE5MCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzQzLjc4MjczNjQsLTc5LjQ0MjI1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2ZlOTc5MDk1NGMxNGFlNzkwYzc2MDgwNzc4ODQ4NDYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjQ5YmQ1YzMyODYyNGFjOWEwNDA4ZWFjNzY2MDk1YjUgPSAkKCc8ZGl2IGlkPSJodG1sXzI0OWJkNWMzMjg2MjRhYzlhMDQwOGVhYzc2NjA5NWI1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XaWxsb3dkYWxlLCBXaWxsb3dkYWxlIFdlc3QgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jZmU5NzkwOTU0YzE0YWU3OTBjNzYwODA3Nzg4NDg0Ni5zZXRDb250ZW50KGh0bWxfMjQ5YmQ1YzMyODYyNGFjOWEwNDA4ZWFjNzY2MDk1YjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmM4MzBkYzBiMTA0NDY1Yzg0NDJkNGFjOTc0MzlhOTAuYmluZFBvcHVwKHBvcHVwX2NmZTk3OTA5NTRjMTRhZTc5MGM3NjA4MDc3ODg0ODQ2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZiNjg0M2Q0NTk1NjQ0ZGRiNzgxMzdhMTBlNmRkODkyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzcwOTkyMSwtNzkuMjE2OTE3NDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiI2ZmYjM2MCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiNmZmIzNjAiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjA3MWM0NDc5YzdmNGQwNDg3MTlhODM5OTdmY2Q3NjcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2I4Njg4ODU2ZmY3NDdkYzhjNDRmZjMwMDY0NmZjNTEgPSAkKCc8ZGl2IGlkPSJodG1sXzNiODY4ODg1NmZmNzQ3ZGM4YzQ0ZmYzMDA2NDZmYzUxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Xb2J1cm4gQ2x1c3RlciA0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMDcxYzQ0NzljN2Y0ZDA0ODcxOWE4Mzk5N2ZjZDc2Ny5zZXRDb250ZW50KGh0bWxfM2I4Njg4ODU2ZmY3NDdkYzhjNDRmZjMwMDY0NmZjNTEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZmI2ODQzZDQ1OTU2NDRkZGI3ODEzN2ExMGU2ZGQ4OTIuYmluZFBvcHVwKHBvcHVwX2IwNzFjNDQ3OWM3ZjRkMDQ4NzE5YTgzOTk3ZmNkNzY3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk4YTQ3Yjc5NGFmNTRlYTBhY2I3ODJkZmI1MzJiZjk3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNjk1MzQzOTAwMDAwMDA1LC03OS4zMTgzODg3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2ZiOTYyNDljMmQyNDQ0MTdiMjNlZDUyMTFiYThiNDBlKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzMwMTE0NTcwMGNkODQ2MWI4MDkzMTI4ZTJkNTgxMTI2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2FjMzcwMTljODNlMTQ4ZjVhM2JkNjUzOWY1M2M1NzUxID0gJCgnPGRpdiBpZD0iaHRtbF9hYzM3MDE5YzgzZTE0OGY1YTNiZDY1MzlmNTNjNTc1MSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V29vZGJpbmUgSGVpZ2h0cyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzMwMTE0NTcwMGNkODQ2MWI4MDkzMTI4ZTJkNTgxMTI2LnNldENvbnRlbnQoaHRtbF9hYzM3MDE5YzgzZTE0OGY1YTNiZDY1MzlmNTNjNTc1MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85OGE0N2I3OTRhZjU0ZWEwYWNiNzgyZGZiNTMyYmY5Ny5iaW5kUG9wdXAocG9wdXBfMzAxMTQ1NzAwY2Q4NDYxYjgwOTMxMjhlMmQ1ODExMjYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjQ4M2QzZWY4MzgxNGQ1MzkzOWI5YThkNmQwODJjYzkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs0My43NTI3NTgyOTk5OTk5OTYsLTc5LjQwMDA0OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYzdjNjY1NjI0MTVlNDcxY2JkZjA4NzUyYjZkMGQ0MTggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMTk4YWM1YzhmZTkwNGE4MWE3NWJmMDI3NTRkYTM1MTUgPSAkKCc8ZGl2IGlkPSJodG1sXzE5OGFjNWM4ZmU5MDRhODFhNzViZjAyNzU0ZGEzNTE1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrIE1pbGxzIFdlc3QgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jN2M2NjU2MjQxNWU0NzFjYmRmMDg3NTJiNmQwZDQxOC5zZXRDb250ZW50KGh0bWxfMTk4YWM1YzhmZTkwNGE4MWE3NWJmMDI3NTRkYTM1MTUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjQ4M2QzZWY4MzgxNGQ1MzkzOWI5YThkNmQwODJjYzkuYmluZFBvcHVwKHBvcHVwX2M3YzY2NTYyNDE1ZTQ3MWNiZGYwODc1MmI2ZDBkNDE4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2YwZDEwMzYyZmUxMTQ5NWQ5N2NiZTBlM2YxMDVjODQwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNDMuNzU3NDkwMiwtNzkuMzc0NzE0MDk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwZmZiNCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MGZmYjQiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZmI5NjI0OWMyZDI0NDQxN2IyM2VkNTIxMWJhOGI0MGUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMjA3MDE4NjRlMWVmNDRmZmI4ZWU1YmIxZmIyOWNhZTUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTBiM2RhMzBmMTI3NDhhY2E3MGFkMDJhNjcyY2YxYWEgPSAkKCc8ZGl2IGlkPSJodG1sX2UwYjNkYTMwZjEyNzQ4YWNhNzBhZDAyYTY3MmNmMWFhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Zb3JrIE1pbGxzLCBTaWx2ZXIgSGlsbHMgQ2x1c3RlciAzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yMDcwMTg2NGUxZWY0NGZmYjhlZTViYjFmYjI5Y2FlNS5zZXRDb250ZW50KGh0bWxfZTBiM2RhMzBmMTI3NDhhY2E3MGFkMDJhNjcyY2YxYWEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZjBkMTAzNjJmZTExNDk1ZDk3Y2JlMGUzZjEwNWM4NDAuYmluZFBvcHVwKHBvcHVwXzIwNzAxODY0ZTFlZjQ0ZmZiOGVlNWJiMWZiMjljYWU1KTsKCiAgICAgICAgICAgIAogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python

```
