### In this section, we will crawl data from a website


#### The web site we will be crawl is "dining code", click below link and see how the homepage looks like

https://www.diningcode.com/list.php?query=%EC%88%98%EC%9B%90%20%EC%9D%BC%EC%8B%9D

#### Before we get into crawl data, we need to know which data should be crawled and why.
#### In this project, we will crawl all the data regarding menus. To decide menu for our new Japanese restaurants. 
#### For example, what kind of menu is common and which one is rare. 


```python
# To crawl data from website, we need to approach the website with "selenium", becuase we need all page's data, not one page. 
# So, click "see more" ("더보기") button should be done by "selenium"

from selenium import webdriver
import time
url = "https://www.diningcode.com/list.php?query=%EC%88%98%EC%9B%90%20%EC%9D%BC%EC%8B%9D"


# To use webdriver you need to write path of the chromedriver, <- about this chromedriver, will make other post :)
driver = webdriver.Chrome("/Users/jenna/Downloads/chromedriver")
driver.implicitly_wait(30)
driver.get(url)

#this part shows that the "see more" btn will be clicked until it has no more "see more" button
while True:
    try:
        see_more = driver.find_element_by_css_selector("div.list-more-btn")
        see_more.click()
        time.sleep(1)
    except:
        break

# After the page has been all open by the "see_more" button, 
# Now it is time to crawl menus

# To avoid too much data, will only see first menu as a sample
menus=[]
for content in driver.find_elements_by_tag_name('span.stxt'):
    menu=content.text
    menus.append(menu)
    

```


```python
# Results!!
menus
```




    ['도로초밥, 활어초밥',
     '가츠동, 일식, 일식당',
     '초밥, 활어초밥, 계란초밥',
     '크림카레우동, 크림우동, 일식당',
     '회전초밥, 스시, 일식당',
     '캘리포니아롤, 스시, 초밥',
     '사케동, 일식집, 연어사시미',
     '판모밀, 돈까스, 일식당',
     '크림카레우동, 일식당',
     '부타동, 차슈덮밥, 일식',
     '복죽, 매생이전, 사시미',
     '대창덮밥, 라멘, 일식',
     '샐러드바, 뷔페, 스시',
     '초밥, 스시, 회전초밥',
     '사케동, 일식, 일식당',
     '텐동, 일본식, 일식',
     '뭉티기, 육회, 사시미',
     '오마카세, 초밥, 사시미',
     '에비텐동, 일식당',
     '카이센동, 초밥, 스시',
     '스시, 초밥',
     '혼마구로, 사케동, 일식당',
     '오마카세, 스시, 일식',
     '오마카세, 스시, 사시미',
     '초밥, 스시',
     '물회, 스시, 초밥',
     '회무한리필, 횟집, 사시미',
     '무한리필횟집, 선어회, 사시미',
     '일식집, 일식',
     '초밥, 스시, 계란초밥',
     '초밥, 스시',
     '텐동, 일식당',
     '치즈돈카츠, 돈카츠, 일식당',
     '텐동, 일식집, 일식당',
     '오마카세, 사시미, 스시',
     '초밥, 스시',
     '스끼야끼, 사시미, 일식',
     '회전초밥, 스시',
     '오마카세, 사시미, 스시',
     '오뎅바, 이자카야, 일식당',
     '스시부페, 스시',
     '일식, 일식집',
     '참치, 초밥, 광어초밥',
     '돈부리, 냉모밀, 일식',
     '초밥, 스시, 활어초밥',
     '오마카세, 사시미, 스시',
     '해산물뷔페, 뷔페, 초밥',
     '회전초밥, 스시, 초밥',
     '규카츠, 벤또, 일식당',
     '차돌박이, 초밥',
     '초밥뷔페, 초밥',
     '스시, 회전초밥',
     '초밥뷔페, 초밥',
     '초밥, 스시, 스시전문',
     '참치회, 초밥, 스시',
     '모츠나베, 오코노미야끼, 일식',
     '돈까스, 우동, 일식당',
     '화로구이, 일본가정식, 일식당',
     '회전초밥, 스시',
     '일본가정식, 돈부리, 일식당',
     '초밥, 스시',
     '초밥, 스시',
     '스시, 초밥',
     '이자카야, 사시미',
     '초밥, 스시',
     '활어회, 일식당',
     '도로초밥, 초밥',
     '활어초밥, 초밥',
     '카페, 이자카야, 사시미',
     '초밥, 회전초밥',
     '참치, 초밥, 스시',
     '튀김덮밥, 텐동, 일식당',
     '술집, 이자카야, 사시미',
     '사케동, 연어, 일식당',
     '일식, 초밥',
     '대하, 횟집, 초밥',
     '밀푀유나베, 화로구이, 초밥',
     '일본라멘, 라멘, 일식집',
     '유부, 유부초밥, 초밥',
     '일식집, 일식',
     '비빔당면, 차돌박이, 초밥',
     '솥밥, 카이센동, 초밥',
     '돈카츠, 돈까스, 초밥',
     '초밥, 스시',
     '연어사시미, 연어, 사시미',
     '쫄면, 일식당',
     '술집, 이자카야, 일식당',
     '초밥, 참치초밥',
     '횟집, 초밥, 초밥전문',
     '나베, 이자카야, 사시미',
     '황태구이, 일식당',
     '일식당, 돈가스',
     '이자카야, 사시미',
     '스시, 초밥',
     '회전초밥, 스시, 초밥',
     '돈카츠, 모밀, 일식',
     '돼지부속, 특수부위, 사시미',
     '쫄면, 차돌박이, 초밥',
     '회전초밥, 스시',
     '규카츠, 루프탑, 사시미']




```python

```


```python

```
