python

```
# -*- coding: utf-8 -*-

import requests
from urllib import request
from lxml import etree
import re
import os

HEADERS = {
    "User-Agent": 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36',
    "Host": 'www.doutula.com',
    "Accept": 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8'

}

def parse_url(url):
    response = requests.get(url, headers=HEADERS)
    text = response.text
    html = etree.HTML(text)
    imgs = html.xpath('//div[@class="page-content text-center"]//img[@class != "gif"]')
    for img in imgs:
        img_url = img.get("data-original")
        img_name = img.get("alt")
        img_name = re.sub(r'[\？?，。、*\.]', '', img_name)
        suffix = os.path.splitext(img_url)[1]
        filename = img_name + suffix
        request.urlretrieve(img_url, 'images/' + filename)

def spider():
    for i in range(1, 101):
        url = "http://www.doutula.com/photo/list/?page=%d" %i
        parse_url(url)
        break

if __name__ == '__main__':
    spider()
```
