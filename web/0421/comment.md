# 1. comment

게시글에 댓글을 추가하기 위해 `Comment` 모델과 form을 만들어 사용한다.



## 1.1 외래키

`Comment` 모델의 경우 게시글 모델에게 종속되어 있기 때문에 외래키라는 개념이 사용된다. 



### 외래키

다른 테이블의 기본키 필드를 참조 할 수 있는 필드를 외래키라고 한다. 외래키를 이용하면 두 테이블을 연결하고 관계를 가지게 할 수 있다.

예를들어, 고객들의 정보를 나타내는 `costomers`와 주문 데이터를 저장하는 테이블인 `orders`가 있다고 가정해보자. 

- costomers

| customer_id | name  | phone | adress |
| :---------: | :---: | :---: | :----: |
|   1364546   |  tom  |  ...  |  ...   |
|   1175456   | jason |  ...  |  ...   |

- orders

| order_id | customer_id | order_list |
| :------: | :---------: | :--------: |
|   104    |   1175456   |    ...     |
|   105    |   1364546   |    ...     |
|   106    |   1175456   |    ...     |



위와 같이 테이블의 구조가 있을 때 `orders`의 `customer_id`는 `customers`의 기본키를 외래키로 가져온 필드이다. 우리는 위와 같이 두 테이블을 외래키로 연결해 다른 테이블을 참조 할 수 있다. 예를 들어 105번 주문을 한 고객님이 누구인가? 라는 질의에 응답할 수 있는 것이다.



### 외래키를 써야하는가?

외래키는 꼭 써야하는가? 라는 의문이 들 수 있다. 외래키를 쓰지 않고 단일 테이블에 위의 정보들을 담으면 어떻게 되는지 생각해보자.

다음 테이블은 주문이 들어왔을 때 주문 내역과 손님의 정보를 모두 저장하는 테이블이다.

| order_id | order_list | customer_id |   name    |  phone  | adress  |
| :------: | :--------: | :---------: | :-------: | :-----: | :-----: |
| **104**  |  **...**   | **1175456** | **jason** | **...** | **...** |
|   105    |    ...     |   1364546   |    tom    |   ...   |   ...   |
| **106**  |  **...**   | **1175456** | **jason** | **...** | **...** |



위의 테이블을 보면 정보의 저장에는 크게 문제가 없어보이지만 효율성이 떨어지는 것을 알 수 있다. 데이터가 중복 되기 때문이다. 중복된 데이터가 존재할 경우 메모리를 많이 사용 할 뿐만 아니라 데이터의 크기가 커진만큼 데이터의 조작성능도 떨어지게 된다.

이러한 특성 때문에 외래키를 사용할 경우 1:1이 아닌 1:N의 관계가 이루어 질 수 있다. (한 고객이 여러 주문을 하는 경우)



## 1.2 comment 모델

게시글에 댓글을 달 수 있도록 구현을 하려면 게시글을 나타내는 모델의 기본키를 외래키로 가져와야 한다.

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class Comment(models.Model):
    content = models.TextField(max_length=100)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    article = models.ForeignKey(Article,
                                on_delete=models.CASCADE)
```



위의 코드를 보면 `Article` 모델의 기본키인 `article_id`를 외래키로 가져오는 것을 확인 할 수 있다. 이 때 변수의 이름을 `article`이라고 명명하는 이유는 실제 사용시 `_id`가 꼬리에 붙어 사용되기 때문이다.

`ForeignKey`함수를 보면 `on_delete`라는 속성을 인자로 받는 것을 볼 수 있는데, 해당 인자는 외래키가 가르키는 값이 삭제될 경우 어떻게 처리할지를 정한다. 속성은 다음과 같다

- models.CASCADE: 외래키를 포함하는 인스턴스를 삭제
- models.PROTECT: 외래키가 가르키는 값이 삭제되지 못하도록 에러가 발생
- models.SET_NULL: 외래키값을 null로 변경



## 1.3 comment 폼

폼의 경우 상당히 간단한데 댓글에서 유저로부터 받을 정보만 fields로 추가해주면 된다. 다음과 같이 폼을 정의 할 때 생각해야 할 점이 있는데 **""해당 폼을 통해 Comment모델의 레코드를 생성 할 수 있는가?"** 이다. `created_at`과 `updated_at`은 자동으로 추가되고, `content`는 유저로부터 제공 받지만 article_id의 경우 폼을 통해 받을 수 없다는 것을 인지해야한다.

```python
class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ['content']
```



## 1.4 객체 사용

`1.3 Comment 폼`에서 언급한 바와 같이 `Comment` 폼의 필드만으로는 `Comment`모델의 레코드를 생성 할 수 없다. 이를 고려하지 않을 경우 form을 저장할 때 장애를 겪게 된다.

우리는 모델 인스턴스를 만들어 값을 넣고, 이를 저장하므로서 문제를 해결 할 수 있다. `form.save`함수의 반환값으로 `Comment` 인스턴스를 얻고, form이 저장되는 것을 막기위해 `commit`을 False한다.

```python
article = get_object_or_404(Article, pk=article_pk)
form = CommentForm(request.POST)
if form.is_valid():
    comment = form.save(commit=False)
    comment.article = article
    comment.save()
```



`Article`과 `Comments`는 1:N의 관계이므로 `Article`이 `Comment`의 집합 `comment_set`을 가지고, `Comment`는 `article`속성을 가지게 된다. 각 속성들을 통해 두 테이블은 연결되고 접근할 수 있게 된다.

```python
comments = article.comment_set.all()
```

