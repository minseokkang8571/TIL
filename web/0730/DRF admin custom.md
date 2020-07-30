# 1. DRF user custom

장고는 DRF(Django Rest Framework)라는 로그인, 회원가입과 같은 홈페이지의 기초가 되는 API를 제공하는 모듈이 있다. 실제로 DRF를 사용하다보면 DRF에서 제공하는 User모델을 수정해야하는 경우가 있는데 이 방법에 대해서 알아보도록 하자.



## 1.1 email기반 로그인, 회원가입

실제로 사용하면서 DRF는 상당히 쉽고 간편하며, 기능에 있어서 수정하고 싶은 부분은 크게 없었다. 문제가 하나있다면 username이라는 속성을 ID로 삼아 로그인, 회원가입을 하는 것이였는데, 이는 소셜 로그인을 할 때 문제가 되었다. 

예를 들어 구글 API를 통해 로그인을 하게되면 구글의 회원정보를 기반으로 DB에 회원정보를 저장하고 로그인을 하게된다. 이 때 구글의 회원정보의 경우, 이메일이 unique 속성이고 username은 중복을 허용하므로 이를 이용해 DB에 저장하는 것에 문제가 생기게 된다. 또한 이미 인증된 포털의 이메일을 ID로 사용하면 유저 인증에서도 이점이 있다. 실제로 카카오톡이나 여타 많은 앱들이 ID로 이메일을 사용하는 것을 보면 타당하다고 볼 수 있을 것이다.

먼저 User를 수정하므로 잊지말고 `settings.py`에 다음과 같은 코드를 추가해주자.

```python
# settings.py

...
AUTH_USER_MODEL = 'accounts.User'
```



ID로 이메일을 사용하기 위해, User모델을 수정해보자.

```python
# models.py

from django.db import models
from django.contrib.auth.models import AbstractUser
from .managers import CustomUserManager

class User(AbstractUser):
    # username = None
    email = models.EmailField(_('email address'), unique=True)

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['username']

    objects = CustomUserManager()
    
    def __str__(self):
        return self.email
```



위의 코드를 보면 email을 unique속성을 주어 선언하고, USERNAME_FIELD를 email로 대체했다. 또한 기존의 코드에서 보지 못한 개념이 존재하는데 바로 **managers**이다. managers는 모델과 데이터베이스가 상호작용을 할수있는 인터페이스로 기본적으로 app마다 `objects`라는 managers를 가지고 있다.

위의 `models.py`와 같은 디렉토리에서 `managers.py`를 생성하고 다음과 같은 코드를 추가해주자.

```python
# managers.py

from django.contrib.auth.base_user import BaseUserManager
from django.utils.translation import ugettext_lazy as _

class CustomUserManager(BaseUserManager):
    """
    Custom user model manager where email is the unique identifiers
    for authentication instead of usernames.
    """
    def create_user(self, email, password, **extra_fields):
        """
        Create and save a User with the given email and password.
        """
        if not email:
            raise ValueError(_('The Email must be set'))
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save()
        return user

    def create_superuser(self, email, password, **extra_fields):
        """
        Create and save a SuperUser with the given email and password.
        """
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)
        extra_fields.setdefault('is_active', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError(_('Superuser must have is_staff=True.'))
        if extra_fields.get('is_superuser') is not True:
            raise ValueError(_('Superuser must have is_superuser=True.'))
        return self.create_user(email, password, **extra_fields)
```



우리는 위의 custom managers를 통해 유저생성과정에서 email을 이용한 생성을 진행할 수 있게된다. `settings.py`에서도 추가로 유저생성에 필요한 것들을 정해주어야하는데, 다음의 코드에서 주석을 수정하며 원하는 결과를 찾도록 하자.

```python
# settings.py

...

# ACCOUNT_USER_MODEL_USERNAME_FIELD = None
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_UNIQUE_EMAIL = True
ACCOUNT_USERNAME_REQUIRED = True
ACCOUNT_AUTHENTICATION_METHOD = 'email'
# ACCOUNT_EMAIL_VERIFICATION = 'mandatory'
# ACCOUNT_CONFIRM_EMAIL_ON_GET = True
# ACCOUNT_EMAIL_CONFIRMATION_ANONYMOUS_REDIRECT_URL = '/?verification=1'
# ACCOUNT_EMAIL_CONFIRMATION_AUTHENTICATED_REDIRECT_URL = '/?verification=1'
```



위와 같이 세팅하면 유저 생성시 email을 필요로하며, username도 사용할 수 있다. 기본 ID가 email로 변경됨에 따라 `python manage.py createsuperuser`로 관리자 유저를 만들 때 문제가 발생할 수 있다. 이 문서를 그대로 따라왔을 경우 이러한 문제가 발생하지 않는데, 그 이유는 `User`모델에 작성했던 `REQUIRED_FIELDS = ['username']` 코드 때문이다.  실제로``REQUIRED_FIELDS`리스트에 들어가는 속성이 관리자 유저를 만들 때 요구되는 속성이 된다.





