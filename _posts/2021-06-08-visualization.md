---
title:  "Visualization using PYTHON"
excerpt: "data visualization with crawled data from website using python"
toc: true
toc_sticky: true

categories:
  - PYTHON

tags:
  - python
  - webcrawling
  - data
  - analysis
  - selenium
  - pandas
  - numpy
  - dataframe
  - visualization
  - folium

---

### first you need to import libraries and packages 
```python
    import pandas as pd
    import folium
```

Read data that has already been pre-processed.
```python
    df = pd.read_csv("지역화페 전처리 완성본.csv")
    df.head()
```

But there is always small detail that I forgot to revise 🥲, but remember, this is the beauty of data preprocessing !

```python
df['업종소분류']=df['업종소분류'].replace(['일식?회집'],['일식회집'])
df
```

Now let's narrow the data down to a specific location/street/etc.<br/>
In this case "영통동"

```python
df = df[df["동명"].str.contains("영통동")].reset_index(drop=True)
df.head()
```

In order to examine location data in different industries in more specific detail, it will all organized separately.
<br/>like below

```python
df_k=df[df['업종소분류'].str.contains('일반한식')]
df_k
df_w=df[df['업종소분류'].str.contains('서양음식')]
df_w
df_s=df[df['업종소분류'].str.contains('스넥')]
df_s
df_j=df[df['업종소분류'].str.contains('주점')]
df_j
df_i=df[df['업종소분류'].str.contains('일식회집')]
df_i
df_c=df[df['업종소분류'].str.contains('중국식')]
df_c
df_b=df[df['업종소분류'].str.contains('칵테일바')]
df_b
```

Now all data have been narrowed and separated well. <br/>
It is time to visualize the data 😄


We will be using `folium`.
I think the perfect place to start a new chinese restaurant would be an area where there are no Chinese restaurants,<br/>
But full of other commercial districts.
<br/>Therefore, I will be focus on area where there there is no chinese restaurants within a 300-meter radius.

```python
map = folium.Map(location=[37.251077, 127.075972], zoom_start=15)
```

Below shows where other commercial districts are located

```python
for i in range(0,len(df)):
    folium.CircleMarker(
        [df.iloc[i]["위도"], df.iloc[i]["경도"]],
        radius=5,color="blue",opacity=1,
        popup = df.iloc[i]["상호명"]
    ).add_to(map)
```
Below dark blue outline of the circle shows the radius (300m)

```python
for i in range(0,len(df_c)):
       folium.Circle(
        [df_c.iloc[i]["위도"], df_c.iloc[i]["경도"]],
         radius=300,color="darkblue",fill_color="darkblue",fill=True,fill_opacity=.2,
     ).add_to(map)
```

This red dots are the chinese restaurants that are currently in business

```python
for i in range(0,len(df_c)):
    folium.CircleMarker(
        [df_c.iloc[i]["위도"], df_c.iloc[i]["경도"]],
        radius=4,color="lightred",fill_color="red",fill=True,fill_opacity=1,
        popup = df_c.iloc[i]["상호명"]
    ).add_to(map)
    
map
```

<img width="524" alt="chinese" src="https://user-images.githubusercontent.com/75202769/121371740-bda68f00-c978-11eb-8722-2e40b807810e.png">

In the same way, let's find out where a Japanese restaurant should be built.<br/>
the code is same with the chinese restaurant, but the data will be changed ( df_c to df_j ) <br/>

Let's find the right place for Japanese Restaurant !
<br/>As I mentioned before, a place where there are no Japanese restaurants with suitable commercial districts is a good condition for Japanese restaurants to enter.

```python
map = folium.Map(location=[37.251077, 127.075972], zoom_start=15)


for i in range(0,len(df)):
    folium.CircleMarker(
        [df.iloc[i]["위도"], df.iloc[i]["경도"]],
        radius=5,color="blue",opacity=1,
        popup = df.iloc[i]["상호명"]
    ).add_to(map)

for i in range(0,len(df_i)):
       folium.Circle(
        [df_i.iloc[i]["위도"], df_i.iloc[i]["경도"]],
         radius=300,color="darkblue",fill_color="darkblue",fill=True,fill_opacity=.2,
     ).add_to(map)
    
for i in range(0,len(df_i)):
    folium.CircleMarker(
        [df_i.iloc[i]["위도"], df_i.iloc[i]["경도"]],
        radius=4,color="red",fill_color="red",fill=True,fill_opacity=1,
        popup = df_i.iloc[i]["상호명"]
    ).add_to(map)
    
map
```
At a glance, it is notable in the image below, You can find out where to open a Japanese restaurant.

<img width="537" alt="Japanese" src="https://user-images.githubusercontent.com/75202769/121374687-0f501900-c97b-11eb-9c74-915603c55677.png">

Bye!
