# 1. form

`models.py`에 정의한 모델에 맞는 form을 객체로 사용 할 수 있다. 



## 1.1 form 정의

 app디렉토리 내에 `forms.py`를 만들고 정의 모델에 맞는 클래스를 정의해야 한다. 또한 forms 클래스에 정의 모델의 정보를 넘겨 줄 필요가 있으므로 `models.py`내 정의모델을 import해야 한다.

다음 예시에서는 **Article**이라는 이름의 모델클래스를 사용한다. 이 때 form 클래스의 이름은 관습적으로 `클래스명`+`Form`으로 작성하며 `Meta`라는 내부 클래스 안에 model의 이름과 form의 요소가 될 필드를 설정 할 수 있다.

```python
# forms.py

from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        field = ['title', 'content']
```



위와 같이 개별적으로 필드를 적어주는 것 외에 모델의 전체 필드를 지정해 줄 수도 있다. 이 때 제외하고 싶은 요소가 있을 경우 `exclude`리스트에 작성하여 제외할 수 있다.

```python
class Meta:
    model = Article
    field = '__all__'
    exclude = ['created_at', 'updated_at']
```



필드를 커스터마이징하는 것 또한 가능하다.

```python
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
    	max_length=100,
        label='제목',
    )
    content = forms.CharField(
    	label='내용'
        widget=forms.Textarea(
        	attrs={
                'row': 5,
                'col': 50,
            }
        )
    )
```



## 1.2 form의 사용

`forms.py`에 정의한 form을 사용자에게 제공하기 위해서, 먼저 views.py의 render단계에서 form을 넘겨줄 필요가 있다. form의 객체를 받는 경우 `GET`, `POST` 두 개의 방식을 구분해야 한다.

```python
# views.py

from django.shortcuts import render
from .forms import ArticleForm

def create(request):
    form = ArticleForm() # GET의 경우
    form = ArticleForm(request.POST) # POST의 경우
    # 위의 두개의 방식중에 맞는 것을 선택해서 사용
    context = {
        'form': form
    }
    return render(request, 'create.html', context)
```



위와 같이 form 객체를 생성해 html에 넘겨준 후, html에서 기존의 form을 대체하면 된다. form의 사용은 굉장히 간단하다. 위의 경우 form을 `<p>`의 형태로 사용하는 경우이며,  as_p외에도 테이블의 행을 출력하는 as_table, 목록 아이템을 출력하는 as_ul이 있다.

```python
# create.html

<form action="" method='POST'>
	{% csrf_token %}
	{ form.as_p }
</form>
```



사용자로부터 요청받은 데이터를 모델에 적용하는 것 또한 form의 객체로 가능하다. 이 때 유효성 체크까지 할 수 있으므로 form객체를 사용하여 db를 저장하는 것이 좋다. 위의 `create.html`이 url을 통해 save함수를 불러 온 다고 가정하여 보자. 

`form.save()`함수를 사용하는 시점에서 db의 저장이 끝나며 해당 함수의 반환값으로 모델의 객체를 반환 받을 수 있어 추가적인 조작이 가능하다. 

```python
# views.py

from django.shortcuts import render, redirect
from .forms import ReviewForm

def save(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
      	# ...
```



##  1.3 bootstrap form

form의 형태를 간단하게 잡을 수 있도록 bootstrap4를 사용할 수 있다. 먼저 bootstrap4를 설치한다.

```bash
$ pip install bootstrap4
```



**사용할 html, 혹은 base가 되는 html에 bootstrap CDN을 추가한다.**



이후 `setting.py`의 `INSTALLED_APPS`리스트에 추가한다.

```python
# settings,py

INSTALLED_APPS = [
    # local, basic app,
    'bootstrap4'
]
```



이후 form을 render하는 html에서 사용한다. **load 태그를 추가할 때 extends 태그가 항상 최상위에 있어야 함을 주의한다.**

```html
<!--form.html-->

{% extends 'base.html' %}
{% load bootstrap4 %}

{% block content %}
    <form action="" method='POST'>
        {% csrf_token %}
        {% bootstrap_form form %}
        <input class="btn btn-primary" type="submit" value='submit'>
    </form>
{% endblock %}
```





 

