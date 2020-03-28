# 1. django 설치

django는 파이썬으로 작성된 오픈 소스 웹 어플리케이션 프레임워크로, 관계형 데이터베이스 형태의 모델(**M**odel), 사용자 인터페이스인 템플릿(**T**emplates) 그리고 HTTP 요청을 처리하는 웹 템플릿 시스템인 뷰(**V**iew)로 이루어진다.

![그림1](images/그림1.png)

django의 **MTV**처리과정은 다음과 같다.

1. 클라이언트에게 요청받은 URL을 URLConf모듈을 이용하여 분석
2. URL과 일치하는 View실행
3. View로직을 실행하고 DB처리가 필요할 시 모델을 통해 처리 후 결과를 반환 받음
4. 로직처리가 완료된 후 Templates를 사용하여 클라이언트에게 전송할 HTML 생성
5. HTML 전송



## 1.1 설치

```bash
$ django install django==2.1.15
```



## 1.2 프로젝트 생성

서버 디렉토리를 만들고 싶은 경로에서 다음 명령어를 실행한다.

```bash
$ django-admin startproject {프로젝트명}
```



해당 문서에서는 **프로젝트명을 intro**라고 가정하고 작성한다. 프로젝트를 생성하면 프로젝트명과 같은 이름의 폴더가 생성되며 해당 폴더의 디렉토리는 다음과 같다.

```bahs
db.sqlite3  intro/  manage.py*  pages/
```



하위 디렉토리인 intro에는 `init.py`, `setting.py`, `urls.py` 등 프로젝트 동작에 대한 설정을 하는 파일들이 존재하며 서버의 동작은 `manage.py`를 사용한다. 초기에 서버를 실행 할 시 서버 접근 권한이 막혀있어 이를 `setting.py`에 들어가 변경해주어야한다. 추가로 기초 언어를 한글로 설정할 수 있다.

```python
#L28
ALLOWED_HOSTS =["{허용 ip}"]
#L107
LANGUAGE_CODE = "Ko-KR"
```



설정을 마치고 나면, 다시 `manage.py`가 있는 `intro` 디렉토리로 돌아가 서버를 실행시킬 수 있다. 해당 명령어는 서버를 8080포트로 연다는 의미이다.

```bash
$ python manage.py runserver 8080
```



## 1.3 app정의

django 서버에서 실질적인 기능을 구현하기 위해 app을 생성해야 한다. app은 프로젝트 없이 실행되지않으며`manage.py`를 통해 생성한다.

```bash
$ python manage.py startapp {앱이름}
```



앱을 생성한 이후에는 프로젝트에 app을 등록해주어야 한다. 이 작업은 `setting.py`에서 이루어진다. 해당 문서에서는 **앱이름을 pages**로 한다.**이 설정이 이루어지지 않으면 app은 동작하지 않기 때문에 주의!**

```python
#L33
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    "pages",
]
```



# 2. -



## 2.1 요청에 따른 응답 페이지

요청에 따른 응답을 하는 페이지를 구성하기 위해선 앞서 언급된 MTV구조를 만들어야 한다. 이 후의 작업들은 순서는 중요하지 않다. 

- `urls.py`

  ```python
  from django.contrib import admin
  from django.urls import path
  from pages import views
  
  urlpatterns = [
      path("lotto/", views.lotto),
      path('admin/', admin.site.urls),
  ]
  ```

  `urls.py`파일에 작동 app의 `views.py`를 import하고 url을 등록한다. 이 때 path()의 두번째 인자는 `views.py`에서 동작할 함수이다.

- `views.py`

  ```python
  from django.shortcuts import render
  
  # Create your views here.
  
  def lotto(request):
      import random
      pick = random.sample(range(1, 46), 6)
      context = {
          "pick":pick
      }
      return render(request, "lotto.html", context)
  ```

  `views.py`에서는 로직을 작성해준다. 위의 코드는 로또 번호를 임의로 생성해 `lotto.html`파일과 함께 반환하는 코드이다.

- `templates`

  `views.py`에서 넘겨진 로직은 템플릿에 의해 HTML로 쓰여지고 전송된다. 이를 위해 app의 하위 디렉토리로 `templates`를 만들어주어야 한다. `templates`폴더 내부에는 html을 작성하고 lotto함수에서 반환한 context변수를 이용해 출력한다.

  ```html
  {{ pick }}
  <!-- 출력은 반환 딕셔너리의 key를 이용한다 !-->
  ```




## 2.2 variable routing

url에 변수 요소의 값을 넣어 응답시 변수에 따른 결과를 받을 수 있다.

- `url.py`: 기존의 url에 추가로 변수의 구조를 작성한다.

  ```python
  path("{url}/<{변수형}:{변수값}/", {함수})
  # 예를 들어 다음과 같이 코드를 작성 할 수 있다
  path("dinner/<str:menu>/<int:num>/", views.dinner)
  ```

- `views.py`: variable routing을 사용하는 url의 함수는 매개변수를 추가로 가진다. 이해를 돕기위해 위의 예시코드에 맞는 함수를 예로 든다.

  ```python
  def dinner(request, menu, num):
      context = {
          "menu": menu,
          "num": num,
      }
      return render(request, "dinner.html", context)
  ```

