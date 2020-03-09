# HTML5

HTML5는 기존의 HTML보다 간결한 코드를 가지고 있다.



- meta: 기존의 HTML요소가 내용을 감싸는 형식이라면 meta요소는 속성과 속성 값의 정보를 담고 있을 뿐 내용을 감싸지는 않는다. **+ 이러한 형식을 Empty요소라고 한다**



## 문서의 작성

### DOCTYPE

HTML 문서의 최상단에는 해당 문서가 html이라는 것을 알리는 `DOCTYPE`부터 시작한다. HTML의 태그들은 시작과 끝을 알리기위해 내용을 감싸는 형식을 가진다.

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

### BODY

실질적으로 페이지에 보여지는 내용은 `body`를 통해서 기술된다.

`body`의 형태 역시 `head`와 같다.

``` html
<body>
...    
</body>
```

### 태그

`body`태그의 기술은 링크로 대체한다.

- [body태그링크]([https://ofcourse.kr/html-course/body-%ED%83%9C%EA%B7%B8](https://ofcourse.kr/html-course/body-태그))



## Visual Code 패키지

visual code는 html작성 편의를 위한 패키지가 있으며 다음과 같다.

- HTML Snippets
- Open in brower
- Auto rename tag, Auto close tag