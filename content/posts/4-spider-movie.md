---
title: '爬取豆瓣电影 Top 250'
date: 2020-06-26
published: true
slug: 4-spider-movie
tags: ['爬虫']
cover_image: "./images/p4.jpg"
canonical_url: false
description: '记录一次爬虫练习，爬取豆瓣电影 Top 250 并存入表格中。'
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" height="500" width="750"  src="//music.163.com/outchain/player?type=2&id=29999850&auto=1&height=66"></iframe>

# 前言

记录一次爬虫练习，爬取豆瓣电影 Top 250 并存入表格中。

起因是同学发过来一段代码，让我帮忙改改 bug，采用的是 [requests-html](https://github.com/psf/requests-html) 来提取信息，但当我下载这个包时环境出现了问题。

于是我将其修改成了用 BeautifulSoup 来提取信息。然后采用 pandas 将如数据存入 xlsx 表格中。是一次很好的学习案例，从中再次练习了 bs 和正则表达式的使用。同时也让我意识到以后要多从实践中学习，纸上得来终觉浅。

之前没有注意过这个包，查了一下，发现还挺强大。

>  requests-html 是基于现有的框架 PyQuery、Requests、lxml、beautifulsoup4等库进行了二次封装，作者将Requests设计的简单强大的优点带到了该项目中。

# 注意

注意 BeautifulSoup 的使用，`title = soup.find(property="v:itemreviewed").text` 根据属性的实际值可以锁定到具体位置。
 
# 完整代码

```python
import requests
import re
import time
import pandas as pd
import os
from bs4 import BeautifulSoup

def writeFile():
    movInfoAll = pd.DataFrame()
    urls = [f'https://movie.douban.com/top250?start={25*i}&filter=' for i in range(0,1)]
    for url in urls:
        # 请求头
        headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36'}
        ret = requests.get(url,headers=headers)
        html = str(ret.content,encoding='utf-8')
        pattern = ' <div class="pic">.*?href="(.*?)">'
        links = re.findall(pattern,html,re.S)
        title_list = []
        years_list = []
        score_list = []
        for link in links:
            
            res = requests.get(link, headers=headers)
            html = str(res.content,encoding='utf-8')
            soup = BeautifulSoup(html, 'html.parser')
            
            # 电影名
            title = soup.find(property="v:itemreviewed").text
            title_list.append(title)
            
            # 电影年份
            year = soup.find(property="v:initialReleaseDate").text
            years_list.append(year)

            # 电影评分
            score = soup.find(property="v:average").text
            score_list.append(score)

            print(title, year, score)

        movInfo = pd.DataFrame([title_list, years_list,score_list]).T # 将数据存入二维表格中
        movInfo.columns = ['影名', '年份','评分'] # 二维表格的第一行
        movInfoAll = pd.concat([movInfoAll, movInfo])
        time.sleep(2) # 休眠两秒

    # 写入文件
    movInfoAll.to_excel('Moviesall_douban.xlsx', sheet_name='豆瓣电影Top250')


if __name__ == '__main__':
    writeFile()
```