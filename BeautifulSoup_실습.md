##  BeautifulSoup 실습 - 네이버 증건 거래 상위 수집
- 라이브러리 import
```python
#-*- coding: utf-8 -*-
try:
    import requests
except:
    !pip install requests
    import requests

try:
    from bs4 import BeautifulSoup
except:
    !pip install beautifulsoup4
    from bs4 import BeautifulSoup

import time
```
- html 요청 및 파싱
```python
# 해더
headers = {
    'Content-Type': 'application/json; charset=utf-8',
    'user-agen': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36'
}
# 요청
response = requests.get("https://finance.naver.com/sise/sise_quant.nhn", headers=headers)
# html 파싱
soup = BeautifulSoup(response.text, 'html.parser')
```
- 데이터 추출
```python
data_trs = soup.select("div.box_type_l table.type_2 > tr")

print(data_trs)

print(len(data_trs))

print(data_trs[2])

print(data_trs[2].select('td'))

print(data_trs[2].select('td').__len__())

print(data_trs[2].select('td')[3])

print(data_trs[2].select('td')[0].text)

print(data_trs[2].select('td')[1])

data_trs[2].select('td')[1].select('a')[0].attrs['href']

href = data_trs[2].select('td')[1].select('a')[0].attrs['href']

href[href.find('code=') + len('code='):]

print(data_trs[2].select('td')[2].text.replace(",", ""))

print(data_trs[2].select('td')[3].text)

print(data_trs[2].select('td')[3].text.strip())

print(data_trs[2].select('td')[4].text.strip().replace("%", ""))
print(data_trs[2].select('td')[5].text.strip().replace(",", ""))
print(data_trs[2].select('td')[6].text.strip().replace(",", ""))
print(data_trs[2].select('td')[7].text.strip().replace(",", ""))
print(data_trs[2].select('td')[8].text.strip().replace(",", ""))
print(data_trs[2].select('td')[9].text.strip().replace(",", ""))
print(data_trs[2].select('td')[10].text.strip().replace(",", ""))
print(data_trs[2].select('td')[11].text.strip().replace(",", ""))

```
