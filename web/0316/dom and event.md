# 1. DOM

DOM은 문서객체모델로  문서를 구조화하여 표현하며 문서 구조, 스타일 그리고 내용 등을 변경할 수 있도록 도운다. 

주요 객체로는 **window**, **document**, navigator, location, history, screen 등이 있음



# 1.1 DOM 접근

- 단일 Node
  - document.getElementByID()
  - **document.querySelector(selector)**
- HTMLCollection(live): 조건문, for문 사용시 실시간으로 값이 바뀜.
  - document.getElementsByTagName()
  - document.getElementsByClassName()
- NodeList(non-live): live와 달리 값이 변하지 않음.
  - **document.quertSelectorAll()**



# 2. event

브라우저에서 발생한 이벤트에 대한 행위를 정의할 수 있다.



## 2.1 발생 가능한 이벤트의 종류

- load  페이지, 윈도우가 브라우저로 읽혀짐
- move 윈도우나 프레임을 움직임
- mouseover 마우스를 올려놓음
- mousedown 마우스를 누름
- mouseup 마우스를 눌렀다 뗌
- dbclick 더블클릭
- submit 폼을 제출
- select 입력양식이 선택 됨

이 외에도 blur, change, focus, resize, dragdrop, error 등 많은 이벤트가 존재한다.

위와 같은 이벤트들은 다음과 같이 사용할 수 있다. 다음 코드에선 버튼을 누르면 함수가 동작하게 된다. 

```html
<input type="button" onclick=({함수})>
```

```javascript
document.querySelector("submitButton").onclick = {함수}
```



## 2.2 event 리스너

위의 예시 처럼 이벤트를 동작시킬 수 있지만 이벤트를 더 유동적으로 쓰기 위해 리스너로 등록하여 사용할 수 있다.

리스너를 통한 이벤트의 등록에는 `addEventListener`가 쓰이며 방법은 다음과 같다.

```javascript
document.querySelector("submitButton").addEventListener("click", {함수})
```

`addEventListener`는 세개의 인자를 가지는데 순서대로 이벤트, 이벤트 발생 후 호출 될 함수, 전파의 방향이다. 



### event propagation

여기서 전파란 이벤트가 상위, 하위 노드로 전달되는 것을 의미하며 하위 노드에서 상위 노드로 전파되는 **버블링**(false)이 default값이다. 세 번째 인자에 true를 주면 역으로 전파되는 **캡쳐**를 사용할 수 있다.

또한, 이벤트의 전파를 막는 것도 가능하다. `stopPropagation()` API를 사용하면 요소의 이벤트만 동작시키며 상위, 하위 노드에 이벤트를 전파하지 않는다.



## 2.3 event 위임

유사 항목에 일일이 이벤트리스너를 등록하는 번거로움 없이 상위 노드에 이벤트리스너를 등록하는 방법이다. 다음 예시는 `itemList`에 속해있는 버튼들에 `onclick`이벤트가 일어날 경우 alert이 일어나도록 하는 예시이다.

```html
<ul class="itemList">
    <li>
    	<input type="checkbox" id="item1">
    </li>
    <li>
    	<input type="checkbox" id="item2">
    </li>
</ul>
```

```javascript
var itemList = document.querySelector(".itemList");
itemList.addEventListener("click", function(event) {
    alert("clicked");
})
```

