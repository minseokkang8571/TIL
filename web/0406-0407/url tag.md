# 1. url tag

url을 하드코딩하지않고 유연하게 사용하기 위해 url tag를 사용 할 수 있다.

django에서 url은 프로젝트 `urls.py`에서 각각의 앱 url을 include해, app에서 해당 url을 관리하므로 app마다 namespace를 지정해줘야한다. 또한 namespace내에서 url이 어떤 이름으로 쓰일 지를 지정해줘야한다.

```python
# app/urls.py

app_name = "my_app"
urlpatterns = { 
	path("just_redirect/", views.just_redirect)
    path("", views.index, name="index")
}
```



위와 같이 지정해준 namespace를 우린 `:`을 통해 접근 할 수 있다. `views.py`를 보자. 아래 코드는 `my_app`이라는 namespace의 `index`라는 이름을 가진 url로 redirect를 하는 함수이다.

```python
# app/views.py

def just_redirect(request):
    return redirect("my_app:index")
```



우리는 `url tag`를 이용해 python파일 뿐만 아니라 DTL, 즉 html파일에서도 이런 방식으로 코딩 할 수 있다.

```html
{% url 'my_app:index' %}
```



만약 url내에 variable routing 변수가 존재한다면 다음과 같이 기술한다.

```html
{% url 'my_app:delete' variable1 variable2 ... %}
```

