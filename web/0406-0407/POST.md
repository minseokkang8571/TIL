# 1. POST

http의 전송 방식은 `GET`, `POST` 두 가지가 존재한다. 별도의 설정없이 form에서 `action`을 보내게 되면 `GET`의 형태로 url이 보내지는데 이 때 url 주소를 보면 보내지는 변수의 값들이 외부로 노출되는 형태가 된다.

예를 들어 네이버에서 **GET형식**이라는 키워드로 검색을 해보자.

```
https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query=GET형식
```



url을 보면 query라는 이름의 인풋에 **GET형식**이 담겨 전달되는 것을 볼 수 있다. 

`POST`형식은 이러한 외부로의 노출을 기피하는 방식으로 url에 값이 직접 노출되지 않는 방식을 의미한다. 우리가 django에서 `POST`를 사용하기 위해서는 기존의 `GET`방식에서 method의 직접지정, 토큰의 추가가 필요하다.



## 1.1 django에서의 POST사용

url의 형식을 `POST`로 바꾸기 위해, 먼저 form의 형식을 바꿔 줄 필요가 있다. 우리는 method를 `POST`로 지정하여 이를 변경가능하다. 방법은 매우 간단한데 form태그 내부에 **method='POST'**를 추가하여 형식을 바꾸면 된다.

```html
<form action="" method='POST'
```



보내는 url의 형식을 바꾸었기 때문에 받는 입장에서도 `GET`이 아닌 `POST`의 형태로 데이터를 받게 해야한다. 기존의 `views.py`에서 request를 받을 때 우리는 `request.GET.get()`이라는 함수로 데이터를 받아왔다. 이 때 함수내의 `GET`이 method방식을 의미하므로, 이것을 다음과 같이 바꿔준다.

```python
title = request.POST.get('title')
```



### 토큰의 사용

`POST`의 경우 보안을 중요시 하므로 **토큰**이라는 개념이 추가로 사용된다. 토큰은 암호화 기법으로 form내부에 작성하여 사용한다.

```html
<form action="" method="POST">
    {% csrf_token %}
</form>
```



## 1.2 GET, POST분기

하나의 목적으로 두개의 url을 보낼 때 우리는 `GET`과 `POST` 두가지 방식으로 분기하여 하나의 views함수에서 처리 할 수 있다. 예를 들어 특정한 model에 들어 갈 데이터를 사용자로부터 받기 위해 form을 노출하는 url과 앞선 과정에서 받아온 데이터를 model의 저장하는 경우, 하나의 목적을 가지고 url을 나눠 쓸 필요가 없다. 

즉, 사용자에게 form화면을 render해주는 요청을 `GET`으로, 데이터를 수신 받는 요청을 `POST`로 분기하여 코드를 작성해 볼 수 있다. 이를 위해선 두 경우 모두 같은 url을 받아 동일한 views함수를 실행하도록 한다. 해당 예시에선`create`라는 이름의 함수를 사용하도록 한다.

```python
# views.py

def create(request):
    if request.method == "POST":
    	article = Article()
        article.title = request.POST.get('title')
        article.content = request.POST.get('content')
        article.save()
        return redirect('articles:index')
    else:
        return render(request, 'new.html')
```

