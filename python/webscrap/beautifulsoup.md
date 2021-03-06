# Beautiful Soup

HTML 소스코드를 해석하는 것을 파싱(parsing)이라고 한다.  
HTML 문서에서 정보를 추출하기 위해서 beautifulsoup 라이브러리를 이용한다.  

### Install
~~~
pip install beautifulsoup4
~~~


### Code
~~~
import requests
from bs4 import BeautifulSoup

url  = "https://en.wikipedia.org/wiki/Seoul_Metropolitan_Subway"
resp = requests.get(url)
html_src = resp.text

soup = BeautifulSoup(html_src, 'html.parser')
print(type(soup))
print('\n')

print(soup.head)
print('\n')

print(soup.body)
print('\n')

print('title 태그 요소 : ', soup.title)
print('title 태그 이름 : ', soup.title.name)
print('title 태그 문자열 : ', soup.title.string)
~~~

* `requests`와 `bs4` 모듈을 불러온다.  
* url을 설정하고 `requests`에서 GET 요청을 통해 응답한 객체의 내용을 resp에 담는다.  
* 응답 객체의 text 속성에서 HTML 코드를 추출하여 html_src에 담는다. 
* HTML을 해석하는 적절한 해석기(parser)를 사용한다. `html.parser`  
* type은 beautiful soup 클래스임을 확인할 수 있다.  
* soup.head : 웹문서의 <head>...</head> 부분의 내용을 출력한다.  
* soup.body : 웹문서의 <body>...</body> 부분의 내용을 출력한다.  
* soup.title : `title`태그의 내용을 출력한다.  
* soup.title.name : name 속성을 지정하여 태그의 이름을 출력한다.  
* soup.title.string : 태그를 제외하고 태그안에 존재하는 텍스트만을 출력한다.  


### Image Code
~~~
import requests
from bs4 import BeautifulSoup

url  = "https://en.wikipedia.org/wiki/Seoul_Metropolitan_Subway"
resp = requests.get(url)
html_src = resp.text

soup = BeautifulSoup(html_src, 'html.parser')

first_img = soup.find(name='img')
print(first_img)
print("\n")

target_img = soup.find(name='img', attrs={'alt':'Seoul-Metro-2004-20070722.jpg'})
print(target_img)
~~~

* `find` 메소드를 이용해서 찾고자 하는 태그의 이름 `img`를 name 매개변수로 지정한다.  
* 가장 처음으로 나오는 img태그의 내용을 출력한다. 
* `find` 메소드에 `attrs`매개변수를 이용해서 {'속성이름':'속성 값'}의 딕셔너리 구조로 고유의 속성값을 지정한다.  
* 지정한 속성 값을 기준으로 처음 나오는 태그를 찾아 출력한다.  


### SavingImageCode
~~~
import requests
from bs4 import BeautifulSoup

url  = "https://en.wikipedia.org/wiki/Seoul_Metropolitan_Subway"
resp = requests.get(url)
html_src = resp.text

soup = BeautifulSoup(html_src, 'html.parser')

target_img = soup.find(name='img', attrs={'alt':'Seoul-Metro-2004-20070722.jpg'})
print("HTML 요소 : ", target_img)
print("\n")

target_img_src = target_img.get('src')
print("이미지 파일 경로 : ", target_img_src)
print("\n")

target_img_resp = requests.get('http:' + target_img_src)
out_file_path = "./output/download_image.jpg"

with open(out_file_path, 'wb') as out_file:
    out_file.write(target_img_resp.content)
    print("이미지 파일을 저장하였습니다.")
~~~
* 앞 내용은 위와 동일하고.   
* target_img_src(이미지 파일 경로)에 target_img에 대한 src를 GET 요청해서 저장한다.  
* out_file_path로, 현재 디렉토리 아래 output/ 에 넣어줄 경로 및 파일 이름을 설정해주었다.  
* target_img_src에 http:를 앞에 붙여 target_img_resp 에 저장한 후 이 target_img_resp에 content에는 이미지 파일이 바이너리 형태로 저장되어 있다.  
* write명령으로 이미지 파일을 외부 파일로 저장한다. 

### Hyperlink
~~~
import requests, re
from bs4 import BeautifulSoup

url = "http://en.wikipedia.org/wiki/Seoul_Metropolitan_Subway"
resp = requests.get(url)
html_src = resp.text
soup = BeautifulSoup(html_src, 'html.parser')

links = soup.find_all("a")
print("하이퍼 링크의 개수 : ", len(links))
print("\n")
print("첫 3개의 원소 : ", links[:-3])
print("\n")

wiki_links = soup.find_all(name="a", href=re.compile("/wiki/"), limit=3)
print("/wiki/ 문자열이 포함된 하이퍼링크 : ", wiki_links)
print("\n")

external_links = soup.find_all(name="a", attrs={"class":"external text"}, limit=3)
print("class 속성으로 추출한 하이퍼링크 : ", external_links)
print("\n")
~~~
*
