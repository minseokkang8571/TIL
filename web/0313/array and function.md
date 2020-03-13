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

배열의 인덱스를 이용해 값의 유무에 상관없이 원소를 추가 가능하다.

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

