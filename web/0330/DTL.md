# 1. DTL

DTL은 **D**jango **T**emplate **L**anguage의 줄임말로 django내에서 웹 문서 작성을 편리하게 할 수 있도록 도와준다.  구체적으로 DTL은 HTML문서내에서 파이썬 문법을 쓸 수 있도록 도와준다.  그러나 문법은 파이썬, HTML과 다르므로 숙지하고 문서를 참고 할 필요가 있다.



## 1.1 변수의 사용

HTML문서에서 기본적인 작성과 변수의 사용을 구분하기 위해 `{{ 변수명 }}`이 사용된다. 어떤 변수를 사용하고 싶다면 앞선 이중 중괄호 안에 변수의 이름을 넣어 사용 할 수 있다. 예시는 다음과 같다.

```html
<p>영화이름: {{ title }}</p>
```



변수에는 파이썬의 객체처럼 사용할 수 있는 태그가 있다. 예를 들어 `다크나이트`라는 문자열이 들어있는 변수 `title`이 존재한다면, 다음과 같이 변수의 길이를 표현 할 수 있다.

```html
<p>다크나이트는 {{ title|length }}글자 입니다.</p>
```

다음과 같은 빌트인 태그들은 장고 문서에서 찾아 볼 수 있다.  [링크](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/)



파이썬에서는 인덱스를 접근 할 때 `[]`를 사용하지만 DTL에서는 `.`으로 접근한다.

```html
<p>영화의 첫 글자는 {{ title.0 }} </p>
```



## 1.2 문법의 사용

문법의 사용에도 구분자가 있다. `{% 문법 %}`과 같은 형태로 사용되며 반드시 **쌍이 되는 닫는 코드를 기술**해 주어야 한다.



### for

반복문의 경우 파이썬의 `for-in` 형식을 사용한다.

  ```html
  {% for article in articles %}
  	<p>article.0</p>
  	<p>article.1</p>
  {% endfor %}
  ```



range와 같은 문법을 제공하지 않기 때문에 반복문에서 사용 할 수 있는 요소들이 존재한다.

- `forloop`
  - forloop.counter:  for문의 현재 반복횟수를 반환 (1부터 시작)
  - forloop.counter0 : for문의 현재 반복횟수를 반환 (0부터 시작)
  - forloop.revcounter: 마지막 인덱스 - 현재 인덱스를 반환 (1부터 시작하며 0도 존재)
  - forloop.first: 첫 번째 반복일 경우 True를 반환
  - forloop.last: 마지막 반복일 경우 True를 반환



## if

조건문의 경우 python과 동일하게 `if`, `elif`, `else`를 사용하며 닫는 코드를 기술하는 것만 주의한다.



```html
{% if forloop.first %}
	<p>첫 번째</p>
{% endif %}
```



## extends

여러 html문서를 만들 때 공통되는 부분을 재사용이 가능토록 한다. 예를 들어 `community.html`, `home.html` 두 가지 html을 작성한다면, html의 Doctype부분과 nav등의 레이아웃은 공통된 부분으로 중복된다. `extends`를 활용하면 이런 코드를 중복하지 않을 수 있다.

먼저 중복되는 부분, 예를 들어 nav를 구성하는 코드 별도의 html에 작성한다.

```html
<!-- base.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <nav>nav입니다.</nav>
  {% block content %}
  {% endblock}
</body>
</html>
```

이 때 중복요소 외에 다른 코드가 들어갈 부분을 `block`으로 지정한다. `block {block명}`의 형식이며 block이름은 확장하는 html문서에서 반드시 같은 이름을 사용해야 한다.

```html
<!-- home.html -->
{% extends "base.html" %}
{% block content %}
<!-- 내용부분 -->
{% endblock}
```

`home.html`에서 다음과 같이 확장대상 html을 `extends`하고 `block`을 가져와 정의하면 `block`부분이 치환되어 `base.html`에 들어가는 것과 같은 결과가 나타난다.

