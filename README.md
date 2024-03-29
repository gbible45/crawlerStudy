# 웹 크롤러 란?
## 웹 크롤러 란?
-  웹 크롤러는 조직적, 자동화된 방법으로 월드 와이드 웹을 탐색하는 컴퓨터 프로그램이다.
- 웹 크롤러가 하는 작업을 '웹 크롤링' 혹은 '스파이더링'이라 부른다.
- 검색 엔진과 같은 여러 사이트에서는 데이터의 최신 상태 유지를 위해 웹 크롤링한다

## 크롤링 기법
- 1. 정적 페이지 : HTML 페이지를 가져와서 파싱하고, 필요한 데이터만 추출하는 기법
- 2. Open Api Open : API(Rest API) 를 제공하는 서비스에 Open API를 호출해서, 받은 데이터 중 필요한 데이터만 추출하는 기법
- 3. 동적페이지 : Selenium 등 브라우저를 프로그래밍으로 조작해서, 필요한 데이터만 추출하는 기법

## Python 기초 문법
- 왕초보를 위한 Python : https://wikidocs.net/book/2

## BeautifulSoup 사용법
### 1) BeautifulSoup import
```python
import requests
from bs4 import BeautifulSoup
```
### 2) HTML 페이지 요청 및 파싱
```python
res = requests.get('http://v.media.daum.net/v/20170615203441266')
print(res.content)
soup = BeautifulSoup(res.content, 'html.parser')
```
### 3) 필요한 데이터 검색 및 추출
```python
title = soup.find('title')
print(title.get_text())
```

### 4) 태그로 검색 방법
```python
html = """
<html>
    <body>
        <h1 id='title'>타이틀 문구</h1>
        <p class='cssstyle'>웹페이지에서 필요한 데이터를 추출 내용</p>
        <p id='body' align='center'>
           여러줄 데이터내용
           여러줄 데이터내용
           여러줄 데이터내용
           여러줄 데이터내용
        </p>
    </body>
</html>
"""
soup = BeautifulSoup(html, "html.parser")

title_data = soup.find('h1')

print(title_data)
print(title_data.string)
print(title_data.get_text())

# 가장 먼저 검색되는 태그를 반환
paragraph_data = soup.find('p')

print(paragraph_data)
print(paragraph_data.string)
print(paragraph_data.get_text())

```

## selenium 사용법
### 1) 라이브러리 import 방법 로컬
#### 1-1) 주피터 사용시
```python
# set options to be headless, ..
try:
    from selenium import webdriver
except:
    !pip install selenium
    from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

try:
    from webdriver_manager.chrome import ChromeDriverManager
except:
    !pip install webdriver_manager
    from webdriver_manager.chrome import ChromeDriverManager

# options 정의
options = webdriver.ChromeOptions()
# options.add_argument('window-size=1920,1080')
# options.add_argument('headless') # 창 감춤
options.add_argument('start-maximized')
```

#### 1-2) google colab 사용시
```python
import sys

# install chromium, its driver, and selenium
if 'google.colab' in sys.modules:
    !apt-get update
    !apt install chromium-chromedriver
    !cp /usr/lib/chromium-browser/chromedriver /usr/bin
    !pip install selenium

# set options to be headless, ..
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

### options 정의
options = webdriver.ChromeOptions()
options.add_argument('--headless')
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')

```

### 2) 브라우저 컨트롤
#### 2-1) 브라우저 띄우기
```python
# 주피터 사용시
chrome = webdriver.Chrome(ChromeDriverManager().install(), options=options)
# google colab 사용시
chrome = webdriver.Chrome('chromedriver', options=options)
```

#### 2-2) 브라우저 닫기
```python
chrome.close() #현재 탭 닫기
chrome.quit()  #브라우저 닫기
```
                     
#### 2-3) 뒤로가기 / 앞으로가기
```python
chrome.back() #뒤로가기
chrome.forward() #앞으로가기
```

### 3) 엘레먼트 컨트롤
#### 3-1) 엘레먼트 접근하는 방법
```python
# sample 임 쿼리에 맞는 데이터가 있어야 작동
chrome.find_element(By.XPATH, '/html/body/div[2]/div[2]/div[1]/div/div[3]/form/fieldset/button/span[2]') #xpath 로 접근
chrome.find_element(By.CLASS_NAME, 'ico_search_submit')  #class 속성으로 접근
chrome.find_element(By.ID, 'ke_kbd_btn') #id 속성으로 접근
chrome.find_element(By.LINK_TEXT, '회원가입')    #링크가 달려 있는 텍스트로 접근
chrome.find_element(By.CSS_SELECTOR, '#account > div > a')   #css 셀렉터로 접근
chrome.find_element(By.NAME, 'join') #name 속성으로 접근
chrome.find_element(By.PARTIAL_LINK_TEXT, '가입')  #링크가 달려 있는 엘레먼트에 텍스트 일부만 적어서 해당 엘레먼트에 접근
chrome.find_element(By.TAG_NAME, 'input')    #태그 이름으로 접근

chrome.find_element_by_tag_name('input').find_element_by_tag_name('a')  #input 태그 하위태그인 a 태그에 접근

# 단일
chrome.find_element(BY.~~~~~~~~, 'query')
# 복수
chrome.find_elements(BY.~~~~~~~, 'query')
```

#### 3-2) 엘레먼트 액션
```python
# 엘레먼트 클릭
chrome.find_element(By.ID, 'ke_kbd_btn').click()

# 텍스트 입력
chrome.find_element(By.ID, 'ke_awd2_btn').send_keys('텍스트 입력')

# 텍스트 삭제
chrome.find_element(By.ID, 'ke_awd2_btn').clear()
```
#### 3-3) 단축키 입력 방법
```python
from selenium.webdriver.common.keys import Keys
# 컨트롤+V
chrome.find_element(By.ID, 'ke_kbd_btn').send_keys(Keys.CONTROL + 'v')

# 다른 방법
from selenium.webdriver import ActionChains

ActionChains(chrome).key_down(Keys.CONTROL).send_keys('V').key_up(Keys.CONTROL).perform() 
#위에서 chrome 대신 엘리먼트를 입력해도 좋음.
```

#### 3-4) 이동
```python
# Frame 이동
#이동할 프레임 엘리먼트 지정
element = chrome.find_element(By.TAG_NAME, 'iframe')

#프레임 이동
chrome.switch_to.frame(element)

#프레임에서 빠져나오기
chrome.switch_to.default_content()
```
#### 3-5) 경고창 (alert) 처리
```python
## 경고창 이동
chrome.switch_to.alert

## 경고창 수락 / 거절
from selenium.webdriver.common.alert import Alert

Alert(chrome).accept()    #경고창 수락 누름
Alert(chrome).dismiss()   #경고창 거절 누름
print(Alert(chrome).text  # 경고창 텍스트 얻음
```
#### 3-6) 쿠키
```python
#쿠키값 얻기
chrome.get_cookies()

#쿠키 추가
chrome.add_cookie()

#쿠키 전부 삭제
chrome.delete_all_cookies()

#특정 쿠기 삭제
chrome.delete_cookie(cookiename)
```
#### 3-7) 자바스크립트 코드 실행
```python
#브라우저 스크롤 최하단으로 이동
chrome.execute_script('window.scrollTo(0, document.body.scrollHeight);')

#새 탭 열기
chrome.execute_script('window.open("https://naver.com");')
```

## google colab 사용시
- google colab : https://colab.research.google.com/notebooks/intro.ipynb

## jupyter lab 사용시
- 아나콘다 설치 방법 : https://annajang.tistory.com/26


