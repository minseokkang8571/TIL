# 1. class

자바스크립트는 class라는 개념이 존재하지 않지만 prototype을 이용해 class를 구현할 수 있다. 



## 1.1 프로토타입

스크립트에서 함수를 선언하면 함수멤버로 prototype 속성이 만들어지며, 해당 속성은 다른곳에 생성된 함수 이름의 프로토타입 객체를 참조한다. 만약 Person이라는 이름의 함수를 선언하면 다음과 같은 구조가 만들어진다.

Person.prototype > Person.prototype 객체 < Person 인스턴스

즉, 프로토타입 객체는 new와 Person함수를 통해 만들어지는 생성된 객체의 원형이 되는 객체이며 prototype 속성으로서 접근할 수 있다. 다음은 프로토타입 동작에 대한 예시이다.

```javascript
function Person(){}

Person.prototype.getType = function(){
    return "인간";
}
var min = new Person();
var kim = new Person();

console.log(min.getType()); // 인간
console.log(kim.getType()); // 인간
```

prototype 속성의 추가, 수정 그리고 삭제의 경우 `Person.prototype.getType`으로 선언한 것 처럼 prototype속성을 이용해야한다.

하지만 멤버 읽기의 경우, `min.getType()`처럼 함수를 호출하면, 먼저 Person함수의 메소드내에서 getType()의 여부를 확인한 뒤 Person.prototype의 속성을 확인한다.



## 1.2 구현

위의 프로토타입을 이용하여 부모 자식 클래스 관계를 나타낼 수 있게 된다. 하지만 다음과 같은 문제가 발생할 수 있다.

```javascript
function Person(name){
    this.name = name || "아무개";
}

Person.prototype.getName = function() {
    return this.name;
};

function Korean(name){}
Korean.prototype = new Person();

var min = new Korean("민석");
var kim = new Korean();

console.log(min.getName()); // 아무개
console.log(kim.getName()); // 아무개
```

해당 코드에서 의도한 것은 Person을 부모로 두는 Korean 클래스를 생성하여 생성자에 `민석`이라는 값을 넣는 것이다. 하지만 Person 프로토타입 객체의 getName 함수를 사용해보면 생성자를 통한 이름의 할당이 제대로 되지 않은 것을 확인 할 수 있다.

의도대로 결과를 내기위해서는 매번 Person의 함수를 호출하거나 다음과 같은 방법을 사용해야한다.

```javascript
function Person(name){
    this.name = name || "아무개";
}

Person.prototype.getName = function() {
    return this.name;
};

function Korean(name){
	Person.apply(this, arguments); // Korean의 this를 Person 생성자에 바인딩
}
Korean.prototype = new Person();

var min = new Korean("민석");
var kim = new Korean();

console.log(min.getName()); // 민석
console.log(kim.getName()); // 아무개
```

위의 코드 처럼 자식 클래스의 this를 부모 클래스에 바인딩하게 되면 this로 이루어진, 생성자의 속성들을 모두 상속 받을 수 있어진다. 하지만 위와 같이 구현 했을 경우 생성자를 통해 적용된 Korean의 속성과 Person객체로 부터 전달받은 prototype내의 속성을 둘 다 가지게 되어 값이 중복된다.

이러한 문제를 해결하기 위해서는 위의 코드의 일부를 다음과 같이 변경해야한다.

```javascript
Korean.prototype = Person.prototype;
```

위와 같이 코드를 수정하면 값의 중복은 없어지지만 참조를 전달하므로 문제가 생기게 된다. 참조가 아닌 값을 전달하여 이 문제를 해결 할 수 있다.

```javascript
function copyObject(obj) {
    var tempFunc = function() {}
    tempFunc.prototype = obj;
    return new tempFunc();
}

Korean.prototype = copyObject(Person.prototype);	
```

완성된 상속 코드의 전문은 다음과 같다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    function Person(name){
      this.name = name || "아무개";
    }
    function Korean(name){
      this.lang = "한국어"
      Person.apply(this, arguments);
    }
    function copyObject(obj) {
      var tempFunc = function() {}
      tempFunc.prototype = obj;
      return new tempFunc();
    }

    Person.prototype.getName = function() {
      return this.name;
    };
    Korean.prototype = copyObject(Person.prototype);
    Korean.prototype.addHabit = function(habit) {
      this.habit = habit;
    };
    var min = new Korean("민석");
    min.addHabit("게임");
    var kim = new Korean();

    console.log(min.getName());
    console.log(min.lang);
    console.log(min.habit);
    console.log(kim.getName());
  </script>
</body>
</html>
```

