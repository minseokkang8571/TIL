# 1. CSS

Cascading Style Sheets의 약자로 HTML 요소에 스타일을 적용하기 위해 사용한다. 글자의 크기, 색, 행간 등 시각적 특성을 원하는 바대로 변경 할 수 있다. 하나의 CSS 파일은 다수의 HTML에서 사용할 수 있어 확장이 용이하다.



## 1.1 작성 방법

1. HTML요소에 직접 CSS정의: HTML 요소의 스타일 속성 이용
2. CSS를 모아서 정의: HTML헤더 부분에 style 요소 사용
3. **CSS파일을 만들어 HTML에 연결** 
   - HTML이 서버에서 웹 브라우저로 로드 될 때 CSS파일도 함께 로드



### 기술방식

- 기본 기술

  - 형태: 선택자 {속성 선언} 
  - 예: h1{font-size: 1.5em;}

- 선택자, 속성선언

  - 선택자: 어떤 속성이 적용되는지 정의하는 요소. 단순한 요소명, 클래스, ID 또는 복잡한 논리형으로 다양하다.
  - 속성선언: 선택자를 통해 선택된 HTML요소에 적용할 스타일 내용
  - 하나의 선택자는 하나의 속성 선언을 가지며 각각의 속성선언은 `;`로 구분한다.

  ```html
  h1{
  	width: 300px;
  	height: 50px;
  	font-size: 1.5em;
  	color: red
  	font-weight: bold;
  }
  ```



### 외부 CSS파일 HTML문서 연결

1. HTML코드 작성
2. 비어있는 파일 작성 확장자 `.css`로 저장
3. CSS파일 경로 지정 (HTML 코드 <head> 부분에 link사용)
4. CSS파일 에서 CSS스타일 지정

- link요소: HTML문서와 외부 리소스 연결을 위해 사용, 대부분 CSS파일 연결

- link속성:

  - rel: 생략 불가한 속성으로 HTML과의 파일 간 관계를 나타낸다. CSS의 경우 다음과 같이 기술한다.

    ```html
    <link rel="stylesheet"...
    ```

  - href: 연결하고자 하는 리소스 파일의 경로를 지정한다.

    ```html
    <link rel="stylesheet" href="css/style.css">
    ```



### 선택자

HTML 요소를 선택하기 위해 사용되는 CSS구문

- 타입 선택자: HTML요소명, 요소의 클래스 속성 값, ID값을 나타내며 가장 기초적인 선택자. h1, p 등이 있다

- 클래스 선택자: 요소들을 원하는 그룹으로 묶을 수 있는 선택자

  ```HTML
  <p class="mycomment">개인적인 코멘트 입니다.</p>
  <div class="mycomment">
      <img src="img/fugi.jpg">
      <p class="fignumber">그림 13</p>
  </div>
  ----------------------------------------------
  .mycomment{ color:gray;
  			font-weight: bold;}
  .fignumber{ font-size:0.8em;
  			float: right;}
  ```

- ID선택자: HTML문서 내의 유일한 값으로 고유의 요소를 식별할 수 있게함. 예시와 같이`#:{id}`로 사용

  ```HTML
  <div id="pinkbox">이것은 핑크 상자입니다.</div>
  -----------------------------------------------
  #pinkbox { width: 200px;
  		height: 50px;}
  ```

- 고급 선택자: HTML문서의 요소들의 계층 관계를 이용하여 선택하는 선택자

  - 하위 선택자: 어떤 요소 하위에 있는 특정 자손 요소 선택. 다음 예시는 `article `요소 하위에 있는 모든 `h1`요소를 핑크색으로 만드는 예시이다.

    ```HTML
    article h1{
    	color: pink;
    }
    ```

  - 직계 자손 선택자: 바로 하위에 있는 요소를 선택하는 선택자. 다음 예시는 `article`요소의 직계 자손인 `h1`요소를 핑크색으로 만드는 예시이다.

    ```HTML
    article>h1{
    	color: pink;
    }
    ```
    
  - 형제 선택자: 형제 관계의 요소를 선택하는 선택자. 다음 예시는 `h1`과 형제관계인 모든 `p`요소를 핑크색으로 만드는 예시이다.
  
    ```html
    h1~p{
    	color:pink;
    }
    ```
  
  - 인접 형제 선택자: 바로 이웃한 형제 관계의 요소를 선택하는 선택자. 
  
    ```html
    h1+p{
    	color:pink;
    }
    -----------------------------------------
    <h1>아티클 헤딩</h1>
    <p>첫 단락</p> <<<< 해당 p만 선택
    <p>두번째 단락</p>
    ```
  
- 의사 클래스: 가짜 또는 모조 클래스, 클래스의 특징을 가지며 `:{의사클래스명}`으로 나타낸다.

  - 링크 의사 클래스: 한 번도 선택하지 않은 링크를 선택하는 선택자. `link`는 한 번도 클릭하지 않은 링크를 선택하며, `visited`는 방문한 이력이 있는 링크를 선택한다.

    ```html
    <a href="{링크}">링크</a>
    ---------------------------
    a:link{color:pink;}
    a:visited{color:lightgray}
    ```
  
- 선택자 우선순위: impotant - 인라인 - id - 클래스 - 요소



## 1.2 CSS의 화면 정의 내용

- 텍스트의 크기 및 스타일
- 요소들의 레이아웃
- 그림, 섹션 요소들의 크기와 테두리, 배경 색 또는 이미지
- 화면 전환이나 요소들의 움직임



### 서체 및 텍스트

- 폰트 패밀리: 비슷한 모양의 서체를 묶어서 제시. 다음 예시에서 컴퓨터에`serif`체가 없을 경우 `Arial`체 그리고 `sans-serif`체 순으로 지정된다. 패밀리내에 사용가능한 서체가 없을 경우 브라우저의 기본서체를 사용한다.

  ```html
  p{
  	font-family: serif, Arial, sans-serif
  }
  ```

- 서체 크기 조절: 단위로는 절대크기값 pt, 상대크기값 em, px 그리고 %등이 있음.

    ```html
    p{
        font-size: 11pt;
    }
    ```
    
- 서체 변형: 비스듬하게, 굵게 등 서체를 변형

    - Itatlic: 영문에서 자주 사용되는 비스듬한 모양의 서체

        ```html
        a{
        	font-style: italic;
        }
        ```

    - Bold: 굵은 서체

        ```html
        a{
        font-weight: bold;
        }
        ```

    - Small cap: 텍스트의 크기로 소문자를 표기

        ```html
        a{
        	font-variant: small-caps;
        }
        ```

- 텍스트 간격 조절

    - 자간 설정: 글자 간의 간격

        ```html
        a{
        	letter-spacing: -0.02em;
        }
        ```

    - 단어 간격설정

        ```
        a{
        	word-sapcing: 0.2em;
        }
        ```

    - 행간 설정

        ```html
        a{
        	line-height: 2em;
        }
        ```

- 텍스트 정렬

    - 가로정렬: 속성으로 left, right, center, justify(양끝정렬) 그리고 auto(기본정렬)이 있다. **블록레벨 요소에만 적용된다**. 인라인 요소에서 사용하기 위해서는 부모 요소를 블록으로 생성하고 정렬을 적용해야한다.

        ```
        p{
            text-align: center;
        }
        ```
    
        ```html
        <div style=text-align: left>
            <span>왼쪽</span>
        </div>
        ```
    
    - 세로정렬: 인라인 요소와 테이블 셀에 적용되며 속성으로 text-top, top, text-bottom, bottom, super(위첨자), sub(아래첨자)의 속성을 가진다.
    
        ```html
        #musical{
        	vertical-align: text-top;
        }
        ```
    
- 글자색 표현

    - 16진수표현: `color: #FFFFFF`
    - 10진수표현: `color: rgb(255,255,255)`
    - HSL 표현: 색조, 채도, 명조로 표현 `color: hsl(120, 100%, 25%)`
    - 색상 키워드: `color: red;`

- 배경 색 표현

    | 단락의 배경 색 설정         | 문서 전체 배경 색 설정         |
    | --------------------------- | ------------------------------ |
    | `p{background-color:gray;}` | `body{background-color:gray;}` |

- 배경 이미지 표현: 디폴트가 반복으로 되어있으며 반복을 하지 않으려면 다음 예시처럼 기술해야한다. 또한 스크롤에 따른 배경 이미지 고정도 설정할 수 있다.

    ```
    body{
    	background-image: url("images/flower.png");
    	background-repeat: no-repeat;
    	background-attachment: fixed;
    	background-position: 30px, 30px;
    	background-size: 50%, 50%;
    }
    ```

    

