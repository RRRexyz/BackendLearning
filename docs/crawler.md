# 爬虫

‌爬虫是一种自动化程序，用于从互联网上抓取信息。‌它通过*模拟浏览器行为*，向网站发送请求，获取资源后分析并提取有用的数据，然后存储起来供后续使用。‌

爬虫的基本工作流程包括以下几个步骤：首先，通过HTTP协议向目标站点发送请求；接着，等待目标站点的响应并获取响应内容；然后，使用正则表达式或其他解析工具解析获取的内容；最后，将解析后的数据保存到本地或远程服务器中。

# url
URL（Uniform Resource Locator）是互联网上资源的地址，它由三部分组成：协议、域名、路径。通俗来说就是你在浏览器地址栏输入的网址。

例如：[https://www.baidu.com/](https://www.baidu.com/)


## User-Agent
User-Agent 即用户代理，简称“UA”，它是一个特殊字符串头。网站服务器通过识别 “UA”来确定用户所使用的操作系统版本、CPU 类型、浏览器版本等信息。而网站服务器则通过判断 UA 来给客户端发送不同的页面。

我们知道，网络爬虫使用程序代码来访问网站，而非人类亲自点击访问，因此爬虫程序也被称为“网络机器人”。绝大多数网站都具备一定的反爬能力，禁止网爬虫大量地访问网站，以免给网站服务器带来压力。User-Agent 就是反爬策略的第一步。

在发送HTTP请求时，需要在请求头中加入User-Agent字段，其内容通常是浏览器的标识信息，这样网页服务器才会对我们的请求进行响应。

常见的User-Agent有：

- Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36 Edg/130.0.0.0

## 认识网页结构
网页一般由三部分组成，分别是 HTML（超文本标记语言）、CSS（层叠样式表）和 JScript（活动脚本语言）。

网页请求的过程分为两个环节：

- Request （请求）：每一个展示在用户面前的网页都必须经过这一步，也就是向服务器发送访问请求。
  
- Response（响应）：服务器在接收到用户的请求后，会验证请求的有效性，然后向用户（客户端）发送响应的内容，客户端接收服务器响应的内容，将内容展示出来，就是我们所熟悉的网页请求，如图 8 所示。

最常见的两种网络请求：

- GET：一般用于获取或者查询资源信息，响应速度快。

- POST：相比 GET 方式，多了以表单形式上传参数的功能，因此除查询信息外，还可以修改信息。

## Requests
Requests 是一个 Python 库，是一个为人类设计的简单而优雅的 HTTP 库。我们可以用它来模拟浏览器行为，发送 HTTP 请求，获取响应内容，并分析提取有用的数据。


## Beautiful Soup
Beautiful Soup 是 Python 库，用于解析 HTML 和 XML 文档并快速提取其中的数据。

1. 创建 `BeautifulSoup` 对象。

2. 使用 `find()` 方法或 `find_all()` 方法查找标签或标签集合。

3. 使用 `select()` 方法或 `select_one()` 方法查找 CSS 选择器。

4. 使用 `get_text()` 方法提取标签内的文本。

5. 使用 `get()` 方法提取标签的属性。


------

## 作业

### 要求
请你写一个爬虫程序，爬取顶点小说上龙王传说的全部章节内容
网址：https://www.ddyucshu.cc/1_1589/


提示：

1.先配置一个虚拟环境安装好需要用到的库。

2.你需要爬取到每一章节的链接，然后再爬取每一章节的内容。
点进一个具体章节，你会看到地址栏的url将发生一些变化，比如：
[https://www.ddyveshu.cc/1_1589/97906157.html](https://www.ddyveshu.cc/1_1589/97906157.html)。
其中97906157就是这一章节的id。
所以，你需要写一个程序，先爬取首页，找到所有章节的链接，记录下来，然后再分别爬取每一个章节链接后的页面内容。

3.由于我们要爬取大量章节的内容，因此会频繁地访问网站，因此有可能被网站认定为爬虫而禁止访问。为此我们可以准备一批不同的User-Agent，在爬取不同章节时从里面随机选择一个，这样网站会认为我们是来自不同设备的访问源，从而不会禁止访问。

User-Agent大全：[https://blog.csdn.net/weixin_69039688/article/details/137566214](https://blog.csdn.net/weixin_69039688/article/details/137566214)

3.由于该小说有一千九百八十一章，爬取全部章节将花费大量的时间，因此你可以只爬取一部分章节。


### 参考代码

```
import requests
from bs4 import BeautifulSoup
import random

headers_list = [
        {
            'user-agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; SM-G955U Build/R16NW) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 10; SM-G981B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.162 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (iPad; CPU OS 13_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/87.0.4280.77 Mobile/15E148 Safari/604.1'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0; Pixel 2 Build/OPD3.170816.012) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.109 Safari/537.36 CrKey/1.54.248666'
        }, {
            'user-agent': 'Mozilla/5.0 (X11; Linux aarch64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.188 Safari/537.36 CrKey/1.54.250320'
        }, {
            'user-agent': 'Mozilla/5.0 (BB10; Touch) AppleWebKit/537.10+ (KHTML, like Gecko) Version/10.0.9.2372 Mobile Safari/537.10+'
        }, {
            'user-agent': 'Mozilla/5.0 (PlayBook; U; RIM Tablet OS 2.1.0; en-US) AppleWebKit/536.2+ (KHTML like Gecko) Version/7.2.1.0 Safari/536.2+'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; U; Android 4.3; en-us; SM-N900T Build/JSS15J) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; U; Android 4.1; en-us; GT-N7100 Build/JRO03C) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; U; Android 4.0; en-us; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 7.0; SM-G950U Build/NRD90M) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.84 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; SM-G965U Build/R16NW) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.111 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.1.0; SM-T837A) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.80 Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; U; en-us; KFAPWI Build/JDQ39) AppleWebKit/535.19 (KHTML, like Gecko) Silk/3.13 Safari/535.19 Silk-Accelerated=true'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; U; Android 4.4.2; en-us; LGMS323 Build/KOT49I.MS32310c) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Windows Phone 10.0; Android 4.2.1; Microsoft; Lumia 550) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2486.0 Mobile Safari/537.36 Edge/14.14263'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 6.0.1; Moto G (4)) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 6.0.1; Nexus 10 Build/MOB31T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 4.4.2; Nexus 4 Build/KOT49H) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; Nexus 5X Build/OPR4.170623.006) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 7.1.1; Nexus 6 Build/N6F26U) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; Nexus 6P Build/OPP3.170518.006) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 6.0.1; Nexus 7 Build/MOB30X) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 520)'
        }, {
            'user-agent': 'Mozilla/5.0 (MeeGo; NokiaN9) AppleWebKit/534.13 (KHTML, like Gecko) NokiaBrowser/8.5.0 Mobile Safari/534.13'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 9; Pixel 3 Build/PQ1A.181105.017.A1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.158 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 10; Pixel 4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 11; Pixel 3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.181 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0; Pixel 2 Build/OPD3.170816.012) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (Linux; Android 8.0.0; Pixel 2 XL Build/OPD1.170816.004) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
        }, {
            'user-agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_1 like Mac OS X) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.0 Mobile/14E304 Safari/602.1'
        }, {
            'user-agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'
        }, {
            'user-agent': 'Mozilla/5.0 (iPad; CPU OS 11_0 like Mac OS X) AppleWebKit/604.1.34 (KHTML, like Gecko) Version/11.0 Mobile/15A5341f Safari/604.1'
        }
    ]

def get_novel_chapters() -> list:
    root_url = "https://www.ddyueshu.com/1_1589/"
    res = requests.get(root_url, headers=random.choice(headers_list))
    res.encoding = "gbk"
    soup = BeautifulSoup(res.text, "html.parser")

    data = []
    for dd in soup.find_all("dd"):
        link = dd.find("a")
        if not link:
            continue

        data.append((f"https://www.ddyueshu.com{link['href']}", link.get_text()))
    return data


def get_chapter_content(url: str) -> str:
    r = requests.get(url, headers=random.choice(headers_list))
    r.encoding = 'gbk'
    soup = BeautifulSoup(r.text, "html.parser")
    text = soup.find("div", id='content').get_text()
    text = text.replace("　　", "\n\n　　")
    return text



if __name__ == '__main__':
    novel_chapters = get_novel_chapters()

    for chapter in novel_chapters:
        url, title = chapter
        with open(f"{title}.txt", "w") as f:
            f.write(get_chapter_content(url))
```