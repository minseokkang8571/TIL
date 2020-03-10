# 1. HTML5

HTML은 Hipey Text Markup Language의 줄임말로 하이퍼텍스트를 나타내기 위해 만들어진 언어이다. 서버에서 HTML형식의 문서를 보내면 웹 브라우저가 이를 해석하여 클라이언트에게 보여준다. HTML5는 기존의 HTML보다 간결한 코드를 가지고 있다는 특징을 가진다.



## 1.1 문서의 작성

### DOCTYPE

HTML 문서의 최상단에는 해당 문서가 HTML이라는 것을 알리는 `DOCTYPE`부터 시작한다. HTML의 태그들은 시작과 끝을 알리기위해 내용을 감싸는 형식을 가진다.

``` html
<!DOCTYPE html>
...
</html>
```



### HEAD

그 다음으로 문서의 시작부인 `head`가 들어간다. `head`의 내용은 페이지에는 표시 되지 않으며, 페이지에 대한 metadata를 포함한다. 예시는 다음과 같으며 `{}`부분에는 페이지의 제목이 들어가게 된다.

```html
<head>
	<meta charset="utf-8">
	<title>{}</title>
</head>
```

#### meta의 종류

- description - <meta name="description" content="{}"
- keywords - <meta name="keywords " content="{}"
- stylesheet - <meta name="stylesheet " href="{}"

meta: 기존의 HTML요소가 내용을 감싸는 형식이라면 meta요소는 속성과 속성 값의 정보를 담고 있을 뿐 내용을 감싸지는 않는다. +이러한 형식을 Empty요소라고 한다



### BODY

실질적으로 페이지에 보여지는 내용은 `body`를 통해서 기술된다.

`body`의 형태 역시 `head`와 같다.

``` html
<body>
...    
</body>
```

#### 태그

`body`태그의 기술은 링크로 대체한다.

- [body태그링크]([https://ofcourse.kr/html-course/body-%ED%83%9C%EA%B7%B8](https://ofcourse.kr/html-course/body-태그))



## 1.2 Visual Code 패키지

visual code는 html작성 편의를 위한 패키지가 있으며 다음과 같다.

- HTML Snippets
- Open in brower
- Auto rename tag, Auto close tag



## 1.3 문서 구조화

 현재 웹 구성에서 HTML은 웹 페이지의 모든 것을 정의하는 것이 아니라 문서의 구조만을 정의한다. 이로서 웹 접근성 증대, 낮은 유지 비용 발생, 문서 크기 감소 등의 이점을 갖는다.

|       HTML       |             CSS             |
| :--------------: | :-------------------------: |
| 문서 구조만 정의 | 문서 레이아웃과 디자인 정의 |



### 레벨 요소

- 블록 레벨 요소: 줄 바꿈이 일어나는 단락 요소. 제목, 문단, 인용문에 쓰이며 블록 레벨 요소 안에 인라인 레벨 요소 또는 다른 블록 레벨 요소가 들어 갈 수 있음
- 인라인 레벨 요소: 줄바꿈이 일어나지 않는 행의 일부 요소. 강조, 링크, 이미지에 쓰이며 인라인 레벨 요소 안에는 인라인 레벨 요소만 들어 갈 수 있다.



### 섹션 요소

#### section

의미가 연결되는 콘텐츠의 집합. 자체적으로 제목과 내용을 가질 수 있는 하나의 독립적 컨텐츠 단위를 일컬음. 다음 예와 같이 두개의 콘텐츠로 구분 할 수 있음.

``` html
<section>
    <h1>예식</h1>
</section>
<section>
    <h1>졸업생</h1>
</section>
```



#### nav

네비게이션 링크로 구성되는 섹션. CSS와 JavaScript 적용 시 다양한 동적 네비게이션 메뉴를 만들 수 있다. 예시는 다음과 같다.

``` html
<nav>
	<ul>
        	<li><a href="index.html">홈</a></li>
        	<li><a href="about.html">회사소개</a></li>
        	<li><a href="product.html">제품</a></li>
    </ul>
</nav>
```



#### article

독립적으로 배포 혹은 재사용 가능한 섹션. 신문, 잡지 기사, 블로그 글 등. 아티클 요소에는 제목, header, footer, 그리고 섹션 요소까지 포함된다. 사용 예는 다음과 같다.

``` html
<article>
	<header>
    	<h1>삶에서 가장 중요한 원칙</h1>
    </header>
    	<p>당신 주변에 어디라도..</p>
    <footer>
        <a href="?comments=1">show comments...</a>
    </footer>
</article>
```



#### aside

문서의 주요 컨텐츠와 별개의 영역을 정의. 일반적으로 사이드 바의 형태를 띔.



#### address

가장 가까운 조상 요소인 article 또는 body요소의 연락처 정보를 의미함.



### 그룹 컨텐츠 요소

그룹으로 내용을 분리하기 위해 사용하는 요소. 컨텐츠의 역할을 정의함.



#### p

컨텐츠 문단을 의미하는 요소. 문단과 인용에 쓰인다. p보다 적합한 요소가 있는 경우 구분해서 사용할 필요가 있다.

- P 요소로 처리한 경우(부적절)

  ``` html
  <section>
  	<p>최종 수정일: 2001-04-23</p>
  	<p>저자: kim12503@naver.com</p>
  </section>
  ```

- footer와 address를 사용한 경우(적절)

  ```html
  <section>
  	<footer>최종 수정일: 2001-04-23</footer>
  	<address>저자: kim12503@naver.com</address>
  </section>
  ```



### hr

문단 레벨에서 주제를 분리하는 요소. 섹션 내에서 다른 화제로의 전환 등. 마침 요소가 없이 단일 요소로 사용되며 예제는 다음과 같음.

```html
<hr>
```



#### 목록

텍스트 컨텐츠를 설명하기 위해 자주 사용되는 구조. 순서 있는 목록, 순서 없는 목록, 정의 목록으로 구분한다.

1. 순서가 있는 목록

   ``` html
   <ol>
       <li>청중의 이해가 빨라야 한다.</li>
       <li>청중의 흥미를 이끌어내야 한다.</li>
   </ol>
   ```

2. 순서가 없는 목록

   ```html
   <ul>
       <li>청중의 이해가 빨라야 한다.</li>
       <li>청중의 흥미를 이끌어내야 한다.</li>
   </ul>
   ```

3. 정의 목록

   ```html
   <dl>
       <dt>제목 요소</dt>
       <dd>데이터 요소</dd>
   </dl>
   ```



#### 기타 그룹핑

1. figure, fugcaption: 사진, 일러스트, 비디오 등을 표시하고 주석을 다는 요소
2. div: 컨텐츠의 구분만을 위한 요소
3. id: HTML 코드 내 유일한 식별자
4. class: 여러 요소에 동일한 스타일을 적용하기 위해 사용하는 요소
5. title: 요소의 조언 정보



## 1.4 form

HTML의 서식을 정의하는 요소. action, method의 속성을 가진다.

- get: 클라이언트는 서버에 폼 데이터를 이름과 값이 결합된 스트링으로 전달한다. 데이터의 이름과 값은 `&`을 이용하여 연결하며 action 속성의 인터넷 주소값 뒤에 붙어 URL을 만들 수 있다. 웹 브라우저에서 접근가능하다. 다음 예시는 서버에 클라이언트의 이름과 나이를 전달하는 예시이다.

  ```html
  client
  http://www.test.com/customReg.html?custom_name=kimchulsu$custom_age=30
  ------------------------------------
  server
  <form action="customReg.html" method="get">
      <p><label>고객이름:<input name="custom_name"></label></p>
      <p><label>고객나이:<input name="custom_age"></label></p>
  </form>
  ```

- post: 클라이언트와 서버간의 약속된 형식으로 인코딩을 하여 서버로 전송한다. 인터넷 주소에 전송 값이 나타나지 않는다. get과 달리 전송 내용은 가려지지만 링크로 접근 불가하다.

- input:  다양한 타입을 가지는 입력 데이터 필드

  - type: 입력값의 유형에 따라 지정가능
    - <input type="text"> 일반적인 줄바꿈이 없는 출력
    - <input type="password"> 화면에 입력된 텍스트가 보이지 않음
    - <input type="summit"> 서식을 제출하는 단추를 만듦
    - <input type="reset"> 서식을 초기화 하는 단추를 만듦
  - name: 서버에 전달될 서식 값의 이름
  - required: 반드시 입력되어야 하는 서식의 경우, 유효성 검사에서 처리함.
  - placeholder: 입력 폼에 짧은 힌트를 제공함.
  - autofocus: 폼이 로딩되면 커서가 autofocus 속성이 있는 요소로 이동

  ```html
  <p><label>고객이름:<input name="custom_name" placeholder="이름은 꼭 입력하세요" required autofocus></label></p>
  ```

- label: 서식 입력 요소의 캡션

  ```html
  <lable for="name">이름:</lable><input id="name" type="text">
  ```

  