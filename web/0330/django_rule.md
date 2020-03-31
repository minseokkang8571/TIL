# 1. Django_rule

장고를 이용할 때 코드 작성, 디렉토리 위치 등 지켜주어야 하는 요소가 있다.



## 1.1 base.html

여러가지 앱을 만들 때, 예를 들어 홈과 커뮤니티를 만드는 경우 nav는 기본 틀이 되는 영역이다. 이런 영역은 두개의 html파일에 중복으로 넣는 것이 아니라 `base`라는 이름의 html에 기술하고 이를 `extends`해서 사용한다.

`base.html`의 경우 프로젝트 단위에서 사용하므로 각 앱의 `templates`에 위치하는 것이 아니라 `settings.py`등이 위치하는 프로젝트 디렉토리에 새로운 `templates`를 만들어 작성하도록 한다. 

하지만 초기 설정에서는 각 앱들의 `templates`폴더만 탐색하므로 이를 수정해주어야 한다. 이를 위해 `settings.py`를 열어 아래와 같이 수정해준다.

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, "{프로젝트명}", "templates")],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```



위와 같이 수정하게 되면 장고에서 템플릿을 찾을 경우 `프로젝트명/프로젝트명/`에 위치한 `templates`폴더를 탐색하게 된다.



## 1.2 app templates 디렉토리 구체화

장고에선 템플릿을 찾을 때 `settings.py`에 등록된 모든 DIR을 확인하고 제일 먼저 찾는 html을 반환하게 된다. 때문에 같은 이름을 가진 html파일이 여러개라면 의도와 다른 결과가 발생 할 수 있다. 이를 방지하기위해 app내의 templates 디렉토리를 변형할 필요가 있다.

우리는 `{app}/templates/{html}`이였던 양식을 `{app}/templates/{app}/{html}`과 같이 디렉토리를 변경하므로서 원하는 앱의 templates에서 html파일을 가져올 수 있다. 다만 디렉토리를 위와 같이 변경하는 경우 `views.py`에서 html파일을 render할 때 경로를 주의해야한다. 

| 변경된 templates 경로                                        | 변경된 render                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20200331213523131](../../../visualcode/online-lecture/0330/images/image-20200331213523131.png) | ![image-20200331213535928](../../../visualcode/online-lecture/0330/images/image-20200331213535928.png) |



## 1.3 urls include

프로젝트를 만든 후 `urls.py`에선 url 요청에 대한 함수 호출을 정의 할 수 있다.  url요청의 경우 app을 통해서 이루어지는 경우가 많기 때문에 프로젝트 단의 url만을 사용할 경우 불편할 수 있다. 우리는 url정의를 `include`를 통해 app단에서 할 수 있다.

먼저, 프로젝트 디렉토리에 있는 `urls.py` 에서 앞으로 사용할 **app의** `urls.py`를 등록한다. 이 때 `include`를 사용하는데 `django.urls`모듈에서 추가로 import해주어야 한다.