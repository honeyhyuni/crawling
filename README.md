# crawling 이란?
크롤링(crawling) 은 웹 페이지를 그대로 가져와서 거기서 데이터를 추출해 내는 행위다. 크롤링하는 소프트웨어는 크롤러(crawler)라고 부른다.

## 정적 웹 크롤링
정적 웹 크롤링은 아주 간단합니다.</br>

1단계. 원하는 웹 페이지의 html문서를 싹 긁어온다.</br>
2단계. 긁어온 html 문서를 **파싱(Parsing)** 한다.</br>
3단계. 파싱한 html 문서에서 원하는 것을 골라서 사용한다.</br>

우선 anaconda 에 "pip install requests" 와 "pip install beautifulsoup4" 를 입력하여 requests 와 beatufulsoup 를 설치합니다.</br>
BeautifulSoup4 패키지는 매우 길고 정신없는 html 문서를 잘 정리되고 다루기 쉬운 형태로 만들어 원하는 것만 쏙쏙 가져올 때 사용합니다.</br>
이 작업을 **파싱(Parsing)** 이라고도 부릅니다.

request 와 beautifulsoup 를 import 해준다
```
from bs4 import BeautifulSoup
import urllib.request
```
정적 크롤링
```
dic = {1: 26, 2: 17, 3: 9, 4: 11, 5: 6, 6: 6, 7: 6, 8: 17, 9: 45, 10: 19, 11: 16, 12: 17, 13: 16,
      14: 23, 15: 25, 16: 23, 17: 3}  # sido2 를 돌기 위한 딕셔너리 
# 교촌(정적) 웹 사이트를 전부 돌면서 크롤링을 하기 위한 이중 for 문
for page in range(1,18):
    for page2 in range(1, dic[page]):
        gyochon_url = 'https://kyochon.com/shop/domestic.asp?sido1=%d&sido2=%d&txtsearch=' %(page, page2) 
        html = urllib.request.urlopen(gyochon_url)
        soupGyochon = BeautifulSoup(html, 'html.parser') # html을 잘 정리된 형태로 변환
        
        # 해당 웹사이트의 html 을 잘 파악하여 추출해야한다.
        store_gyochon = soupGyochon.find(class_ = 'shopSchList') 
            for store in store_gyochon.find_all(class_ = 'store_item'):
            ...
            ...  
```
- find( ): 하나만 찾는 것 
- find_all( ): 모두 다 찾는 것

주의할 것은 find로 찾는 것이 여러개라면 가장 첫 번째 것만 가져온다는 것, find_all 로 찾아온 값은 배열로 저장된다.
## 동적 웹 크롤링

### selenium 이란?
크롬과 크롬 드라이버 시간에 설치했던 chromedriver를 이용해 chrome을 제어하기 위해 사용합니다.
크롤링을 하다보면 무엇인가 입력하거나 특정 버튼을 눌러야 하는 상황이 발생합니다. **사람이 그러한 행동을 하는 대신 컴퓨터가 할 수 있도록 해주는 패키지가 selenium** 입니다.

우선 anaconda 에 "pip install selenium" 를 입력하여 selenium 을 설치해 줍니다.

webdriver 를 import 해줍니다.
`from selenium import webdriver`

컴퓨터가 대신 chrome 을 크롤링 하기 위한 webdriver 를 지정해줍니다
`wd = webdriver.Chrome('./chromedriver.exe')`
전 현재 디렉토리 밑에 chormedriver.exe 가 설치되어있어 이렇게 지정 # webdriver.Chrome(경로명./chromedriver.exe)

```
for i in range(1,500):        
        wd.get('https://tomntoms.com/store/domestic_store_search.html') # 탐앤탐스(동적) 웹 사이트를 크롤링하기 위해 경로 지정
        try:
            wd.execute_script('select_one(%d)' %i) # 얻고싶은 데이터가 저장 되어있는 js 경로를 파악후 실행
            html2 = wd.page_source
            soup2 = BeautifulSoup(html2,'html.parser') # html을 잘 정리된 형태로 변환
            tomtom_info = soup2.find(class_='pop-content');
            tomtom = tomtom_info.select('div > .in-bx > .desc')
            ...
            ...   
        except:  # 동적이므로 숫자가 일정하지 않을수 있음.
            continue 
```
- select( )는 find_all( )과 같은 개념
- select_one( )은 find( )와 같은 개념

select( ) 함수는 괄호( )안에 CSS 선택자라는 것을 넣어서 원하는 정보를 찾는 것이 특징

