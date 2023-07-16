---
title: 网络数据采集
date: 2023-06-06 20:39:00
tags: [Python,数据采集,爬虫]
---

网络数据采集大多数使用爬虫实现，爬虫是一种自动化获取互联网信息的程序，它可以模拟人的行为，访问网页、提取数据并进行处理分析。使用 Python 编程语言编写爬虫程序非常方便和灵活，因为它具有简洁的语法和丰富的第三方库。

## 基本原理

Python 爬虫的基本原理包括以下几个步骤：

1. 发起 HTTP 请求：使用`Python`的网络库（如`requests`）发起 HTTP 请求，向目标网页发送请求并获取响应。

2. 解析网页内容：使用`HTML`解析库（如`BeautifulSoup`）解析网页的`HTML`内容，提取出需要的数据。

3. 数据处理和存储：对提取的数据进行处理和清洗，可以使用`Python`的数据处理库（如`pandas`）进行数据分析和处理。将数据存储到文件、数据库或其他数据存储介质中。

4. 可选步骤：爬虫还可以进行更高级的操作，例如处理`JavaScript`动态渲染的网页、使用代理、处理验证码等。

## Python 爬虫的常用库

以下是一些在`Python`爬虫中常用的库：

- **requests：** 用于发送 HTTP 请求和处理响应。
- **BeautifulSoup：** 用于解析 HTML 和 XML 文件，提取需要的数据。
- **Scrapy：** 一个功能强大的爬虫框架，提供了高度可定制的爬虫功能。
- **Selenium：** 用于处理 JavaScript 渲染的网页，模拟用户操作。
- **pandas：** 用于数据处理和分析。
- **MongoDB：** 一种流行的 NoSQL 数据库，常用于存储爬虫获取的数据。

## 示例代码

以下是一个简单的使用`Python`和`requests`库编写的爬虫示例代码，用于获取网页内容：

```python
import requests

# 发起 HTTP 请求
response = requests.get('http://example.com')

# 获取网页内容
content = response.text

# 打印网页内容
print(content)
```

## 例子

### 获取百度实时热点排行榜信息
```python
import requests
from lxml import etree

# 处理字符串中的空白符，并拼接字符串
def processing(strs):
    s = ''  # 定义保存内容的字符串
    for n in strs:
        n = ''.join(n.split(" "))  # 去除空白符
        s = s + n  # 拼接字符串
    return s  # 返回拼接后的字符串

# 定义解析页面函数，用来获取百度实时热点排行榜信息
def get_hotspot_info(url, headers):
    response = requests.get(url=url, headers=headers)  # 发送网络请求
    html = etree.HTML(response.text)  # 解析HTML字符串
    div_all = html.xpath('//div[@class="category-wrap_iQLoo horizontal_1eKyQ"]')  # 获取实时热点相关所有信息
    for div in div_all:
        rank = div.xpath('.//div[contains(@class, "index_1Ew5p")]')[0].xpath("string(.)") # 获取实时热点排名
        rank = processing(rank)  # 处理实时热点排名
        title = div.xpath('.//div[@class="c-single-text-ellipsis"]')[0].xpath("string(.)")  # 获取实时热点标题
        title = processing(title)  # 处理实时热点标题
        index = div.xpath('.//div[@class="hot-index_1Bl1a"]')[0].xpath("string(.)")  # 获取实时热点热搜指数
        index = processing(index)  # 处理实时热点热搜指数
        record = rank + '\t' + title + '\t' + index  # 拼接百度实时热点排行榜信息
        print(record)  # 输出


# 程序入口
if __name__ == '__main__':
    url = 'https://top.baidu.com/board?tab=realtime'  # 百度实时热点排行榜链接
    h = {
        'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36 Edg/113.0.1774.57'
    }
    get_hotspot_info(url, h) # 调用爬虫方法，获取百度实时热点排行榜信息
```

### 获取网易新闻热点排行Top10信息
```python
import requests
from bs4 import BeautifulSoup

# 定义解析页面函数，用来获取网易新闻热点排行Top10信息
def get_news_info(url, headers):
    response = requests.get(url=url, headers=headers)  # 发送网络请求
    soup = BeautifulSoup(response.text, "lxml")  # 创建一个BeautifulSoup对象，获取页面正文
    all_news = soup.find('div', class_="mod_hot_rank").find('ul').find_all('li')  # 获取网易新闻热点排行Top10内容
    news_list = []  # 创建空列表
    for news in all_news:
        news_rank = news.em.string  # 获取新闻排名
        news_title = news.a.string  # 获取新闻标题
        posts_num = news.span.string  # 获取新闻跟帖数
        news_url = news.a.get('href')  # 获取新闻链接
        news_list.append([news_rank, news_title, posts_num, news_url])  # 把每条新闻的排名、标题、跟帖数和链接添加到一个列表中，再追加到一个大列表中
    return news_list  # 返回列表

# 程序入口
if __name__ == '__main__':
    url = 'https://news.163.com/'  # 网易新闻首页链接
    # 定义请求头信息
    headers = {
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/71.0.3578.80 Chrome/71.0.3578.80 Safari/537.36'}
    news_list = get_news_info(url, headers)  # 调用爬虫方法，获取网易新闻热点排行Top10
    print(news_list)  # 输出网易新闻热点排行Top10信息
```

### 获取百度贴吧中"police"贴吧帖子标题、作者、链接和创建时间
```python
import requests
import re

# 请求页面的函数
def get_page(url):
    # 定义请求头信息
    headers = {
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/71.0.3578.80 Chrome/71.0.3578.80 Safari/537.36'}
    try:
        response = requests.get(url=url, headers=headers)  # 发送网络请求
        if response.status_code == 200:  # 判断请求是否成功
            return response.text  # 以文本形式返回整个HTML页面
    except:
        print('请求页面错误！！！')

# 定义解析贴吧网页的爬虫函数，用来获取"police吧"帖子标题、作者、链接和创建时间
def get_posts_info(html):
    posts_title = re.findall(r'class="j_th_tit ">(.+?)</a>', html)   # 帖子标题
    posts_author = re.findall(r'title="主题作者:(.+?)"', html)  # 帖子作者
    posts_href = re.findall(r'href="/p/(.+?)"', html)  # 帖子链接
    post_createtime = re.findall(r'title="创建时间">(.+?)<', html)  # 帖子创建时间
    #print('帖子标题：', posts_title)
    #print('帖子作者：', posts_author)
    #print('帖子链接：', posts_href)
    #print('帖子创建时间：', post_createtime)
    return list(zip(posts_title, posts_author, posts_href, post_createtime))

def save_as_txt(text):   
    with open("posts.txt","a",encoding='utf-8') as f:
        f.write(text + '\n')

# 程序入口
if __name__ == '__main__':
    base_url = 'https://tieba.baidu.com/f?kw=police&ie=utf-8&pn={page}'  # "police吧"基础URL地址
    i = 0
    for i in range(0, 201, 50):  # 每页间隔50，实现循环，共5页
        page_url = base_url.format(page = i)  # 通过format替换切换页码的URL地址
        html = get_page(page_url)  # 调用请求页面的函数，获取整个HTML页面
        posts_data = get_posts_info(html)  # 调用解析贴吧网页的爬虫函数，获取"police"贴吧帖子标题、作者、链接和创建时间
        
        for data in posts_data:
            title = data[0]
            author = data[1]
            href = 'https://tieba.baidu.com/p/' + data[2]
            time = data[3]
            text = title + '\t' + author + '\t' + href + '\t' + time
            save_as_txt(text)
            print("已写入数据：",i)
            i += 1
```

## 注意

#### 俗话说爬虫写得好，牢饭吃到饱。使用爬虫必须遵循以下几点:
- ##### 不要爬敏感数据、隐私数据、非公开数据。
- ##### 不要干扰被爬取网站或系统的正常运营。
- ##### 不要规避网站经营者设置的反爬虫措施或者破解服务器防抓取措施，非法获取相关信息。