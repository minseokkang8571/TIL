# 1. Django ORM

ORM은 Object-relational mapping의 줄임말로 관계형 데이터베이스를 객체와 매핑(mapping)시켜 개발환경에서 편리성을 더한 방식을 말한다. ORM은 기존의 관계형 데이터베이스를 OOP와 함께 사용함에 있어, 필요한 데이터를 따로 저장할 클래스를 만들고 이렇게 생성한 객체를 데이터베이스에 다시 입력해야하는 불편함을 개선하기 위해 개발되었다.

ORM을 사용하면 데이터의 재사용이 편리해지고 객체지향적인 코드로 직관성 또한 좋아진다. 해당 문서에서는 Django에서 ORM을 사용하기 위한 환경구축과 sql문에 대응되는 ORM코드에 대해서 알아본다.



## 1.1 기본 환경

Django는 기본적으로 url에 접근함에 따라 함수를 실행하기 때문에 파이썬 코드나 ORM을 실험하는데 부적합하다. 우리는 코딩의 편리성을 위해 `shell-plus`를 설치하고 해당 환경에서 여러 코드들을 실행할 것이다.



먼저 `shell-plus`를 설치하고 사용을 위해 `settings.py`의 `INSTALLED_APPS`에 추가해준다.

```bash
$ pip install django_extensions
```

```python
# settings.py

INSTALLED_APPS = [
  ...
  'django_extensions', # _에 주의하자.
  ...
]
```



다음으로 `shell-plus`를 실행해주면 IDE처럼 파이썬 코드를 작성할 수 있는 환경을 얻게된다.

```bash
$ python manage.py shell_plus
```



## 1.2 SQL에 대응하는 ORM들

ORM을 사용하기 전에 `QuerySet`이라는 타입에 대해 알아보자. 우리는 데이터베이스로 부터 `QuerySet`타입으로 데이터를 가져오고 이를 추가, 수정 그리고 삭제하는 등 조작할 수 있다. 쿼리셋의 특징에 대해서는 1.3에서 알아보고 해당 절에서는 SQL문에 대응하는 ORM코드들에 대해서 알아보도록 하자.



### SELECT * FROM *table*

> 메서드 `all()`을 사용하면 모든 레코드를 쿼리셋으로 가져온다.

```python
In [5]: movies = Movie.objects.all()

In [6]: movies
Out[6]: <QuerySet [<Movie: Movie object (5261)>, <Movie: Movie object (5262)>, <Movie: Movie object (5263)>, <Movie: Movie object (5264)>, '...(remaining elements truncated)...']>
```

### SELECT *attribute* From *table*

> 메서드 `values()`를 사용하면 필드 값을 모두 가져온다.

```python
In [18]: movie_titles = Movie.objects.values('title')

In [19]: movie_titles
Out[19]: <QuerySet [{'title': ' 골든 에이지'}, {'title': ' 어떤이의 꿈'}, {'title': ' 보루토 - 나루토 더 무비'}, {'title': ' 프레셔'}, {'title': ' 란제리 살인사건'}, {'title': 
' 개구리왕국'}, '...(remaining elements truncated)...']>
```

> 복수의 필드를 인자로 넘겨도 결과로 받을 수 있다.

```python
In [28]: movies = Movie.objects.values('title', 'nation')

In [29]: movies
Out[29]: <QuerySet [{'nation': '영국', 'title': ' 골든 에이지'}, {'nation': '대한민국', 
'title': ' 어떤이의 꿈'}, {'nation': '일본', 'title': ' 보루토 - 나루토 더 무비'}, {'nation': '영국', 'title': ' 프레셔'}, '...(remaining elements truncated)...']>
```

### SELECT *attribute* From *table* Where *condition*

> 메서드 `get()`을 사용하면 조건에 따른 하나의 결과값을 가져온다(`QuerySet`이 아닌 필드 자료형으로 가져옴).

```python
In [11]: movie_title = Movie.objects.get(id=5261).title

In [12]: movie_title
Out[12]: ' 골든 에이지'

In [13]: type(movie_title)
Out[13]: str
```

> 메서드 `filter()`를 사용하면 조건에 따른 복수의 결과값을 가져온다.

```python
In [26]: movie_titles = Movie.objects.filter(nation='미국').values('title')

In [27]: movie_titles
Out[27]: <QuerySet [{'title': ' 굿 다이노'}, {'title': ' 헤이트풀8'}, {'title': ' 마이펫의 이중생활'},'...(remaining elements truncated)...']>
```

필터의 경우 다양한 `Lookup`을 사용해 다양한 접근을 가능하게 한다. 

> `>=`: `gte`, `>`: `gt`, `<=`: `lte`, `<`: `lt` 을 사용해 범위를 정하고 결과값을 가져올 수 있다.

```python
In [31]: movies = Movie.objects.filter(id__gt=5000)

In [32]: movies
Out[32]: <QuerySet [<Movie: Movie object (5261)>, <Movie: Movie object (5262)>, <Movie: 
Movie object (5263)>, '...(remaining elements truncated)...']>
```

> `contains`을 사용해 해당 str을 포함하는 경우의 결과값을 가져올 수 있다.

```python
In [39]: movies = Movie.objects.filter(title__contains='타임').values('title')

In [40]: movies
Out[40]: <QuerySet [{'title': ' 스노우타임'}, {'title': ' 타임 체인저'}, {'title': ' 쓰 리 타임즈'}, {'title': ' 아웃 오브 타임'}, {'title': ' 원스 어폰 어 타임 인 멕시코'}, {'title': ' 아노말리: 타임 게이트'}, {'title': ' 엣지 오브 타임'}, {'title': ' 타임 패러독'}]>
```

> `startswith`, `endswith`를 사용해 시작과 끝이 조건에 맞는 결과값을 가져올 수 있다.

```python
In [43]: movies = Movie.objects.filter(title__endswith='타임').values('title')

In [44]: movies
Out[44]: <QuerySet [{'title': ' 스노우타임'}, {'title': ' 아웃 오브 타임'}, {'title': ' 엣지 오브 타임'}, {'title': ' 다이노 타임'}]>
```

> `filter()`에 다수의 인자를 넘기면 `AND`의 의미를 가진다.

```python
In [45]: movies = Movie.objects.filter(id__gt=7000, nation='미국')
```

> `Q`모듈을 사용해 `OR`의 결과값을 가져올 수 있다.

```python
from django.db.models import Q
In [46]: movies = Movie.objects.filter(Q(id__gt=7000) | Q(nation='미국'))
```

### UPDATE *table* SET *attribute* WHERE *condition*

> 메서드 `update()`를 사용해 데이터를 수정할 수 있다.

```python
In [47]: Movie.objects.get(id=5261).title
Out[47]: ' 골든 에이지'
  
In [50]: Movie.objects.filter(id=5261).update(title='수정')
Out[50]: 1 # 반환값
  
In [51]: Movie.objects.get(id=5261).title
Out[51]: '수정'
```

### DELETE From *table*

> 메서드 `delete()`를 사용해 테이블 데이터를 삭제할 수 있다.

```python
Movie.objects.all().delete()
```



## 1.3 QuerySet의 특징

Django ORM에서 사용되는 `QuerySet`은 **객체지향적인 접근**이 가능하는 장점을 가지고 있다. 객체지향적인 접근이 무엇을 의미하는 지 알아보자.



### 슬라이싱, 인덱싱이 가능하다

파이썬의 기본문법을 사용해 마치 리스트처럼 슬라이싱, 인덱싱을 할 수 있다. 이 외에도 `QuerySet`을 합치는 것 또한 가능하다.

```python
In [55]: len(movies)
Out[55]: 2572

In [56]: len(movies[:10])
Out[56]: 10
  
In [57]: movies[1]
Out[57]: <Movie: Movie object (5262)>
```

### chaining이 가능하다

chaining은 앞선 실습들에서 사용한 것처럼 함수를 꼬리물어서 사용하는 것을 의미한다. 우리는 chaining을 사용해 조건에 맞는 데이터를 가져올 수 있다.



