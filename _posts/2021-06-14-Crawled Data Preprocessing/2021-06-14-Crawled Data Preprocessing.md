```python
from wordcloud import WordCloud 
import matplotlib.pyplot as plt 
import nltk 
from nltk.corpus import stopwords
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

import matplotlib as mpl #->as this data is Korean
import matplotlib.font_manager as fm  #-> select Korean font from mac

import seaborn as sns


#settings for Korean
import matplotlib as mpl
mpl.rc('font', family='AppleGothic')
plt.rcParams['axes.unicode_minus'] = False 
```

# In previous section, we crawled data from a website called "diningcode".
# And we save the data as csv file.


```python
#call menu.csv file for preprocessing 
menus.to_csv("menus.csv", sep=",", encoding="utf-8")
```


```python
menus= pd.read_csv("menus.csv", index_col=0)
menus

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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>도로초밥, 활어초밥</td>
    </tr>
    <tr>
      <th>1</th>
      <td>가츠동, 일식, 일식당</td>
    </tr>
    <tr>
      <th>2</th>
      <td>초밥, 활어초밥, 계란초밥</td>
    </tr>
    <tr>
      <th>3</th>
      <td>크림카레우동, 크림우동, 일식당</td>
    </tr>
    <tr>
      <th>4</th>
      <td>회전초밥, 스시, 일식당</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>돈카츠, 모밀, 일식</td>
    </tr>
    <tr>
      <th>96</th>
      <td>돼지부속, 특수부위, 사시미</td>
    </tr>
    <tr>
      <th>97</th>
      <td>쫄면, 차돌박이, 초밥</td>
    </tr>
    <tr>
      <th>98</th>
      <td>회전초밥, 스시</td>
    </tr>
    <tr>
      <th>99</th>
      <td>규카츠, 루프탑, 사시미</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 1 columns</p>
</div>




```python
from tqdm import trange, notebook

menus=pd.DataFrame(menus)
menus.columns=["메뉴"]

menus = menus["메뉴"].str.split(",")
menus

#to remove [] <- this
menu_list = []
for i in menus:
    for  z  in  i:
        menu_list.append(z.replace("[]",""))
        
menu_list
menu_list= pd.DataFrame(menu_list)
menu_list.head()
#this data is very long, for viewers, shorten using head()
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>도로초밥</td>
    </tr>
    <tr>
      <th>1</th>
      <td>활어초밥</td>
    </tr>
    <tr>
      <th>2</th>
      <td>가츠동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>일식</td>
    </tr>
    <tr>
      <th>4</th>
      <td>일식당</td>
    </tr>
  </tbody>
</table>
</div>




```python
len(menu_list)
```




    264




```python
menu_list.columns=["메뉴"]
menu_list.head()
#there are plenty, for readers shorten the data using head()
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
      <th>메뉴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>도로초밥</td>
    </tr>
    <tr>
      <th>1</th>
      <td>활어초밥</td>
    </tr>
    <tr>
      <th>2</th>
      <td>가츠동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>일식</td>
    </tr>
    <tr>
      <th>4</th>
      <td>일식당</td>
    </tr>
  </tbody>
</table>
</div>




```python
# see how does all the menus are look like
menu_list["메뉴"].unique()
```




    array(['도로초밥', ' 활어초밥', '가츠동', ' 일식', ' 일식당', '초밥', ' 계란초밥', '크림카레우동',
           ' 크림우동', '회전초밥', ' 스시', '캘리포니아롤', ' 초밥', '사케동', ' 일식집', ' 연어사시미',
           '판모밀', ' 돈까스', '부타동', ' 차슈덮밥', '복죽', ' 매생이전', ' 사시미', '대창덮밥',
           ' 라멘', '샐러드바', ' 뷔페', ' 회전초밥', '텐동', ' 일본식', '뭉티기', ' 육회', '오마카세',
           '에비텐동', '카이센동', '스시', '혼마구로', ' 사케동', '물회', '회무한리필', ' 횟집',
           '무한리필횟집', ' 선어회', '일식집', '치즈돈카츠', ' 돈카츠', '스끼야끼', '오뎅바', ' 이자카야',
           '스시부페', '일식', '참치', ' 광어초밥', '돈부리', ' 냉모밀', '해산물뷔페', '규카츠', ' 벤또',
           '차돌박이', '초밥뷔페', ' 스시전문', '참치회', '모츠나베', ' 오코노미야끼', '돈까스', ' 우동',
           '화로구이', ' 일본가정식', '일본가정식', ' 돈부리', '이자카야', '활어회', '활어초밥', '카페',
           '튀김덮밥', ' 텐동', '술집', ' 연어', '대하', '밀푀유나베', ' 화로구이', '일본라멘', '유부',
           ' 유부초밥', '비빔당면', ' 차돌박이', '솥밥', ' 카이센동', '돈카츠', '연어사시미', '쫄면',
           ' 참치초밥', '횟집', ' 초밥전문', '나베', '황태구이', '일식당', ' 돈가스', ' 모밀', '돼지부속',
           ' 특수부위', ' 루프탑'], dtype=object)




```python
#there are some datas should be corrected due to small spaces

menu_list["메뉴"]=menu_list["메뉴"].replace([' 초밥'],['초밥'])
menu_list["메뉴"]
menu_list["메뉴"]=menu_list["메뉴"].replace([' 스시'],['스시'])
menu_list["메뉴"]

#also, some words mean same things, so revise accordingly
menu_list["메뉴"]=menu_list["메뉴"].replace(['스시'],['초밥'])
menu_list["메뉴"]

```




    0       도로초밥
    1       활어초밥
    2        가츠동
    3         일식
    4        일식당
           ...  
    259     회전초밥
    260       초밥
    261      규카츠
    262      루프탑
    263      사시미
    Name: 메뉴, Length: 264, dtype: object




```python
menu_list
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
      <th>메뉴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>도로초밥</td>
    </tr>
    <tr>
      <th>1</th>
      <td>활어초밥</td>
    </tr>
    <tr>
      <th>2</th>
      <td>가츠동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>일식</td>
    </tr>
    <tr>
      <th>4</th>
      <td>일식당</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>회전초밥</td>
    </tr>
    <tr>
      <th>260</th>
      <td>초밥</td>
    </tr>
    <tr>
      <th>261</th>
      <td>규카츠</td>
    </tr>
    <tr>
      <th>262</th>
      <td>루프탑</td>
    </tr>
    <tr>
      <th>263</th>
      <td>사시미</td>
    </tr>
  </tbody>
</table>
<p>264 rows × 1 columns</p>
</div>




```python
#also, Japanese, Japanesfood is not menu, so remove those data, using "-"
menu_list= menu_list[-menu_list["메뉴"].str.contains("일식")].reset_index(drop=True)
menu_list
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
      <th>메뉴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>도로초밥</td>
    </tr>
    <tr>
      <th>1</th>
      <td>활어초밥</td>
    </tr>
    <tr>
      <th>2</th>
      <td>가츠동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>초밥</td>
    </tr>
    <tr>
      <th>4</th>
      <td>활어초밥</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>216</th>
      <td>회전초밥</td>
    </tr>
    <tr>
      <th>217</th>
      <td>초밥</td>
    </tr>
    <tr>
      <th>218</th>
      <td>규카츠</td>
    </tr>
    <tr>
      <th>219</th>
      <td>루프탑</td>
    </tr>
    <tr>
      <th>220</th>
      <td>사시미</td>
    </tr>
  </tbody>
</table>
<p>221 rows × 1 columns</p>
</div>




```python
menu_list_num=menu_list.value_counts()
menu_list_num=pd.DataFrame(menu_list_num)
print(menu_list_num.info())
menu_list_num.columns=["갯수"]
menu_list_num=menu_list_num.head(5)
menu_list_num
```

    <class 'pandas.core.frame.DataFrame'>
    MultiIndex: 93 entries, ('초밥',) to ('횟집',)
    Data columns (total 1 columns):
     #   Column  Non-Null Count  Dtype
    ---  ------  --------------  -----
     0   0       93 non-null     int64
    dtypes: int64(1)
    memory usage: 4.1+ KB
    None





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
      <th>갯수</th>
    </tr>
    <tr>
      <th>메뉴</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>초밥</th>
      <td>74</td>
    </tr>
    <tr>
      <th>사시미</th>
      <td>18</td>
    </tr>
    <tr>
      <th>오마카세</th>
      <td>6</td>
    </tr>
    <tr>
      <th>회전초밥</th>
      <td>6</td>
    </tr>
    <tr>
      <th>이자카야</th>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
#visualize the top 5 data

#plt.rcParams.update({'font.size':15 })
menu_list_num.plot(kind='barh',color='salmon', width=0.5, figsize=(8,8))
plt.title('다이닝코드 "수원일식" TOP 100 중 가장 많은 메뉴')

```




    Text(0.5, 1.0, '다이닝코드 "수원일식" TOP 100 중 가장 많은 메뉴')


_posts/2021-06-14-Crawled Data Preprocessing/output_12_1.png

    


