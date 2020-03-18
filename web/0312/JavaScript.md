# 1. JavaScript

객체 기반의 스크립트 언어로서, 스크립트 언어기 때문에 편집기만 있으면 프로그래밍을 할 수 있다는 특징이 있다. 웹에서는 웹 페이지의 기능을 구현하는 역할을 한다.



## 1.1 문법

- 자료형: 자료형은 정수, 문자열, boolean형 등이 있으나 따로 변수의 형을 지정해서 선언할 필요없이 `var`, `let`과 같은 키워드로 선언한다.
- 함수의 선언: `function`이라는 키워드로 선언을 한다. 함수의 이름 뒤에는 `()`안에 매개변수를 전달할 수 있다.
- 단항연산자: c++와 같음.



### 객체

자바스크립트는 객체 기반이므로 내장 객체 또한 포함되어 있다. 하지만 class가 존재하지 않아 모조 클래스를 사용하는 것이 특징이다. 먼저 객체에 대한 개념을 알아본다.

- property: 객체 변수, 예를 들어 square라는 객체에는 width라는 property가 있다.

- method: 객체 함수, 예를 들어 square라는 객체에는 area()라는 method가 있다. 

- 내장객체: 단순 데이터 타입(Number, String..), Date, Math, RegExp(정규표현식)등이 있다.

#### 객체의 생성

객체의 생성은 literal방식과 new를 사용하는 방식으로 나뉜다.

- literal

  	```javascript
var square = {
    width: 50, height: 100, faceColor: "yellow", borderColor: "black"
    area: function() {return this.width * this.height;}
}
    
  ```

- new

  ```javascript
  function square(w, h) {
  this.width = w;
  this.height = h;
  this.faceColor = "yellow";
  this.borderColor = "black";
  this.position = {x: 100, y: 100};
  }
  
  var mysquare = new square(20, 30);
  ```



## 1.2 정의

css와 마찬가지로 html파일에서의 정의와 **파일을 불러오는 방식**이 존재한다.

### HTML에서의 정의

css처럼 html 파일 내에서 스크립트의 내용을 정의할 수 있다. 이 때 `<script>`라는 태크를 사용한다. 다음은 간단한 계산기를 만드는 웹 페이지의 예시이다.

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="utf-8">
        <script>
            function multi(){
                var first_num = cal.num1.value;
                var second_num = cal.num2.value;
                cal.result.value = first_num * second_num;
            }
        </script>
        <title>계산기</title> 
    </head>
    <body>
        <form id="cal">
            <input type="number" name="num1" value="0"> x
            <input type="number" name="num2" value="0"> =
            <input type="text" name="result">
            <input type="button" value="계산하기" onclick="multi()">
        </form>
    </body>
</html>
```



위의 코드에서는 form에 `cal`이라는 id를 부여하므로서 `multi()`함수내에서 id로 접근했지만, id의 부여가 없을 경우 다음과 같이 접근해야한다.

```html
<script>
    function multi(){
        var first_num = document.forms[0].num1.value;
        var second_num = document.forms[0].num2.value;
        document.forms[0].result.value = first_num * second_num;
    }
</script>
```



### 파일 불러오기

`.js`를 확장자로 갖는 파일을 작성한 뒤 html파일에서 다음과 같이 기술한다.

```html
<script src="{파일이름}"></script>
```



