# 1. array

자바스크립트에서 배열은 `object`, 즉 객체이다.



## 1.1 배열의 생성

literal, new를 사용해서 생성가능하며, 여타 언어들과 다르게 배열의 크기를 지정할 필요가 없다. 다음은 배열 생성의 예시들이다.

- literal

	```javascript
var fixedEmpty = [,,,];
	```

- new

	```javascript
var emptyArray = new Array();
var fixedArray = new Array(10); # 길이가 10인 배열 생성
	```



## 1.2 배열 원소 접근

자바스크립트에서는 배열의 원소가 함수 일 수 있으므로 다음과 같이 접근한다.

```javascript
var testArray = [1, function(){return "six"}];
# 이 때 배열의 원소로 정의된 함수는 이름이 없다.

testArray.[1]();
```



### 배열의 순회 접근

for/in 문이나 length 메소드를 이용해 순회할 수 있다.

- for/in

  ```javascript
  var numberString = ["one", "two", "three"];
  for (var a in numberString) {
      document.writeln("<p>" + numberString[a] + "</p>")
  }
  ```

- length

  ```javascript
  var numberString = ["one", "two", "three"];
  for (var i = 0; i < numberString.length; i++) {
      document.writeln("<p>" + numberString[i] + "</p>")   
  }
  ```



### 배열의 원소 추가/삭제

배열의 인덱스를 이용해 **값의 유무에 상관없이** 원소를 추가 가능하다.

```javascript
var array = new Array();
array[0] = 5;
array[array.length] = 6;
```

배열은 객체이므로 원소 삭제 또한 객체의 property를 삭제하는 방법과 동일하다.

```javascript
delete array[1];
```



## 1.3 배열의 메소드

- toString(): 배열을 string으로 변환.
- sort(): 배열을 오름차순으로 정렬, 기본이 문자정렬이기때문에 숫자를 정렬할 시 전달인자가 필요함.
- reverse(): 배열을 내림차순으로 정렬.

- concat(): 배열을 오름차순으로 정렬하고 정렬 배열을 반환.
- slice(): 배열의 일부분을 반환.
- splice(): 배열의 일부분을 삭제, 삽입.
- push(): 배열의 마지막 원소 추가.
- pop(): 배열의 마지막 원소 제거, 제거된 원소 반환.

- unshift(): 배열의 맨 앞 원소 추가, 배열 전체 길이 반환.
- shift(): 배열의 맨 앞의 원소 제거, 제거된 원소 반환.



# 2. function

자바스크립트는 함수형 프로그래밍언어로, 함수가 단순히 반복을 위한 요소가 아니다. 함수가 변수로 처리되어 배열에 들어갈수도있으며 이름이 없는 임시함수의 형태로도 쓰일 수 있다. (때문에 자바스크립트의 함수를 일급함수라고 칭한다.) 이처럼 자바스크립트에서의 함수는 중요하다.



## 2.1 함수 선언식과 표현식

- 함수 선언식

  ```javascript
  function {함수명}() {
      
  }
  ```

- 함수 표현식

  ```javascript
  var {함수명} = function () {
      
  };
  ```



### 선언식과 표현식의 차이점

함수 선언식은 호이스팅에 영향을 받지만, 함수 표현식은 호이스팅에 영향을 받지 않는다. 함수 선언식의 경우 코드를 구현한 위치에 상관없이 자바스크립트의 특징인 호이스팅에 따라 코드의 순서가 맨 위로 앞당겨 진다.



#### 호이스팅

아래 예시는 호이스팅에 의해 logMessage() 함수가 위로 앞당겨지는 예시이다.

```javascript
logMessage();
sumNumbers();

function logMessage() {
  return 'worked';
}

var sumNumbers = function () {
  return 10 + 20;
};
```

해당 코드를 자바스크립트 해석기는 다음과 같이 인식한다.

```javascript
function logMessage() {
  return 'worked';
}

logMessage();
sumNumbers();

var sumNumbers = function () {
  return 10 + 20;
};
```

 

### 표현식의 장점

호이스팅에 영향을 받지 않는 것 외에도 유용하게 쓰이는 경우는 다음과 같다.

- 클로저로 사용: 클로저의 경우 3번째 목차에서 기술한다.
- 콜백으로 사용: 다른 함수의 인자로 사용되는 함수.



## 2.2 다인수 함수

인수가 두개 이상인 함수에서는 인수의 `default`값은 undefine으로 설정된다. 함수의 정의보다 적은 수의 인수가 전달될 때는 undefined값을 가지게 된다.

```javascript
<body>
    <script>
        function test(name, job) {
            document.write('<h1>My name is <span>' + name + 
                           '</span>. I am a <span>' + job +
                           '</span></h1>');
        }
    test("john", "student");
    test("mark", "singer");
    </script>
</body>
--------------------------------------
My name is John. I am a student
My name is mark. I am a undefined
```



## 2.3 중첩 함수

중첩 함수는 함수내에서 다른 함수를 정의하고 사용하는 것을 의미한다.

```javascri
var global_name = "홍길동";
function printName() { 
	var outer_name = "김철수"; 
	function showName() { 
		var inner_name = "김영희"; 
		console.log(global_name); 
		console.log(outer_name); 
		console.log(inner_name); 
    } 
    showName(); 
} 
printName(); 
-------------------------------------------
홍길동
김철수
김영희


```



# 3. 추가 개념

해당 문서에서 설명이 더 필요한 부분을 추가 기술한다.

출처: https://offbyone.tistory.com/135 [쉬고 싶은 개발자]



## 3.1 일급 시민

**"프로그래밍 언어를 디자인 할때 주어진 프로그래밍언어에서 일급 시민(또는 일급 타입, 일급 객체, 일급 엔티티, 일급 값)은 다른 엔티티들이 일반적으로 이용 가능한 모든 연산을 지원하는 엔티티를 뜻합니다. 여기서의 연산은 전형적으로 인자로 전달되고, 함수의 반환값으로 사용되고, 수정되고, 변수에 할당하는 것을 포함합니다."**

위의 일급 객체의 정의에 자바스크립트의 함수는 포함되며 이를 **일급 함수**라고 한다.



## 3.2 closer

자신을 포함하고 있는 외부 함수의 인자, 지역변수 등을 외부 함수가 종료된 후에도 사용할 수 있는 함수로 앞선 변수들은 **자유 변수**라고 한다.

이렇게 클로저에 의해 자유 변수가 되는 것을 캡쳐라고 하며, 해당 자유 변수는 외부에서 직접 접근할 수 없고, 오직 클로저만을 통해 사용할 수 있다. 이는 객체 지향언어의 private 멤버 변수와 같은 효과를 낸다.

이처럼 자유 변수를 가지는 코드를 **클로저(closer)**라고 한다.

```javascript
function makeGreeting(name) {
    var greeting = "안녕! ";

    return function() {
        console.log(greeting + name);
    };
}

var g1 = makeGreeting("홍길동");
var g2 = makeGreeting("김철수");

g1();
g2();

결과)
안녕! 홍길동
안녕! 김철수
```

