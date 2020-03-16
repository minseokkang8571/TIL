# 1. 어려웠던 부분

- 두 문서의 크기가 달라서 인지 nav와 footer의 크기가 달라짐. 이로인해 화면전환시 nav와footer의 크기가 변함.
- 가운데 정렬



# 2. 학습한 내용



## 2.1 가운데 정렬

- `table`을 이용: div를 큰 테이블로 만들고 다단 레이아웃을 셀로 처리

  ```html
  div {
  	display: table;
  	width: 100%;
  	text-align: center;
  }
  p {
  	display: table-cell;
  	vertical-align: middle;
  }
  ```

- `flex`를 이용

  ```html
  div {
  	display: flex;
  	justify-content: center;
  	align-items: center;
  	flex-direction: column;
  }
  ```

- `line-height`를 이용: 자식요소의 모든 부분에 해당 되므로 주의해야함.

  ```javascript
  line-height: {부모의 height}px;
  ```

  

## 2.2 둥근 경계

`border-radius` 속성을 이용해 경계선을 둥글게 할 수 있음.

```javascript
#go-button {
    display: inline-block;
    color: white;
    background-color: rgb(15, 124, 201);
    padding: 10px 10px;
    border-radius: 8px;
}
```



## 2.3 바 고정

`position`속성을 이용해 요소를 고정할 수 있음.

```javascript
nav {
    position: fixed;
}
# 또는
nav {
    position: sticky;
}
```



## 2.3 레이아웃 구성

레이아웃을 구성할 때 `button`과 `글`이 문서의 가운데에 오도록 위치해라. 라는 조건이 있을 경우 먼저 `button`과 `글`을 하나의 범주 (div, section 등)으로 묶어야 함.

또한, 별도의 설정이 없을시 자식은 부모의 너비를, 부모는 자식의 높이를 사용하므로 유의해야함.

