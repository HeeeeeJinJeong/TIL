크롤러 : 웹 페이지에 있는 자료를 자동으로 수집하는 프로그램

ex) 검색엔진
-> Robot.txt : 검색엔진에게 어디까지 검색을 허용할 것이냐?
-> 브라우저가 혹은 크롤러 어떤식으로 서버에 접근해서 데이터를 가져가느냐?
1. 주소를 입력하면 해당 서버로 접근한다.
2. 웹 서버 프로그램이 해당 주소에 맞는 내용을 전달한다.

requests (urllib의 wrapper class)

3. 받은 내용을 해석해서 화면에 보여 준다. 
  3-1. 받은 내용을 해석해서 내가 원하는 데이터를 찾는다. 뽑아낸다.
BeautifulSoupa
-> HTML코드의 해석
-> CSS Selector 만드는 방법

모듈 설치
1. requests : ✗ pip install requests
2. BeautifulSoup4  : ✗ pip install BeautifulSoup4

selector
HTML 태그
1. Container tag : <div td=“sad”>내용</div>
2. Empty tag : <img src=“img 주소”>

태그는 속성을 갖는다.
```shell
<span class="ah_k" queryid="C1556071729834957034">어벤져스 엔드게임 쿠키영상</span>
```

```shell
<tag id=“super” class”test” data-rel=“” data-num=“”>cont</tag>
```
* 단일 셀렉터
tag 이름 : div

id : #super

class : .test

* 다중 셀렉터
~ 안에 있는 무엇(띄어쓰기나 >로 구분)

div ul li.ah_k -> div li.ah_k : 중간 경로 생략 가능, 그냥 ~안에

div > ul > li.ah_k : 중간 경로 생략 불가능

~ 이고, ~인 것(.으로 구분)
span.ah_k.ah_some


~의 근처에 있는 것

* openpyxl
```python
# Workbook : file
# Worksheet : sheet
# 내용을 저장하기 위해 파일 만들기 메모리 상에
wb = Workbook()
ws1 = wb.active # 제일 첫번째 시트
ws2 = wb.create_sheet(title='second sheet')

ws1.cell(row=1, column=1, value='test') # A1  ws1['A1']
ws2.cell(row=3, column=7, value='second sheet')

wb.save('test.xlsx')
```
