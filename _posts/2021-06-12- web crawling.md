---
title:  "Web Crawling using PYTHON"
excerpt: "crawl a website using python"
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
  
---







### In this section, we will crawl data from a website


The web site we will be crawl is "dining code", click below link and see how the homepage looks like

https://www.diningcode.com/list.php?query=%EC%88%98%EC%9B%90%20%EC%9D%BC%EC%8B%9D

 Before we get into crawl data, we need to know which data should be crawled and why.
 In this project, we will crawl all the data regarding menus. To decide menu for our new Japanese restaurants. 
 For example, what kind of menu is common and which one is rare. 


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
    ........
     '쫄면, 차돌박이, 초밥',
     '회전초밥, 스시',
     '규카츠, 루프탑, 사시미']

