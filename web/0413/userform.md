# 1. Userform

개인정보 (id, password)등을 다루는 DB의 경우 정보의 보관, 사용 측면에서 고려해야 할 점이 많다. 이러한 점을 고려하여 django에서는 사용자정보를 위한 form과 model을 제공한다.



## 1.1 라이브러리

제공되는 django라이브러리는 [github](https://github.com/django/django/tree/master/django)에서 확인 할 수 있다. 해당 git을 확인해보면 user의 정보를 저장할 model과 form을 정의 해놓았으며 이는 모두 기존의 `models.Model`**과** `forms.ModelForm`**을 상속**하고 있음을 확인 할 수 있다.

- model

	```python
class User(AbstractUser):
    """
    Users within the Django authentication system are represented by this
    model.
    Username and password are required. Other fields are optional.
    """
    class Meta(AbstractUser.Meta):
        swappable = 'AUTH_USER_MODEL'
	```

	```python
class AbstractUser(AbstractBaseUser, PermissionsMixin):
      """
      An abstract base class implementing a fully featured User model with
      admin-compliant permissions.
      Username and password are required. Other fields are optional.
      """
      username_validator = UnicodeUsernameValidator()
	
    username = models.CharField(
	        _('username'),
        max_length=150,
          unique=True,
          help_text=_('Required. 150 characters or fewer. Letters, digits and @/./+/-/_ only.'),
          validators=[username_validator],
          error_messages={
	            'unique': _("A user with that username already exists."),
	        },
	    )
	    first_name = models.CharField(_('first name'), max_length=150, blank=True)
	    last_name = models.CharField(_('last name'), max_length=150, blank=True)
	    email = models.EmailField(_('email address'), blank=True)
	    is_staff = models.BooleanField(
	        _('staff status'),
	        default=False,
	        help_text=_('Designates whether the user can log into this admin site.'),
	    )
	    is_active = models.BooleanField(
	        _('active'),
	        default=True,
	        help_text=_(
	            'Designates whether this user should be treated as active. '
	            'Unselect this instead of deleting accounts.'
	        ),
	    )
	    date_joined = models.DateTimeField(_('date joined'), default=timezone.now)
	
	    objects = UserManager()
	
	    EMAIL_FIELD = 'email'
	    USERNAME_FIELD = 'username'
	    REQUIRED_FIELDS = ['email']
	
	    class Meta:
	        verbose_name = _('user')
	        verbose_name_plural = _('users')
	        abstract = True
	
	    def clean(self):
	        super().clean()
	        self.email = self.__class__.objects.normalize_email(self.email)
	
	    def get_full_name(self):
	        """
	        Return the first_name plus the last_name, with a space in between.
	        """
	        full_name = '%s %s' % (self.first_name, self.last_name)
	        return full_name.strip()
	
	    def get_short_name(self):
	        """Return the short name for the user."""
	        return self.first_name
	
	    def email_user(self, subject, message, from_email=None, **kwargs):
	        """Send an email to this user."""
	        send_mail(subject, message, from_email, [self.email], **kwargs)
	
	```
	
	```python
	class PermissionsMixin(models.Model):
	  """
	  Add the fields and methods necessary to support the Group and Permission
	  models using the ModelBackend.
	  """
	  #...#
	```
	
- form

  ```python
  class UserCreationForm(forms.ModelForm):
      """
      A form that creates a user, with no privileges, from the given username and
      password.
      """
      #...#
  ```



## 1.2 사용 (회원가입)

model과 form을 모두 라이브러리에서 제공하기 때문에 **별도의 model 그리고 form을 정의 할 필요가 없다.**

간단한 회원가입 form을 가지는 페이지를 만들기 위해, 먼저 **migrate를 진행**해주어야 한다. 별도의 model을 만들지 않기 때문에 이를 놓칠 수 있음에 주의한다. model을 정의하지 않음에도 migrate를 진행하는 이유는 우리가 사용할 `auth`와 같은 migration을 사용하기 때문이다.

```bash
$ python manage.py migrate
```



userform을 사용할 때 가장 달라지는 부분은 `views.py`이다. 우리는 userform을 사용하기 위해 이와 관련된 모듈을 import해주어야 한다. `get_user_model`은 user정보를 가지고 있는 model 인스턴스를 생성하는 함수이며, `UserCreationForm`은 form 인스턴스를 생성한다.

```python
# views.py
from django.contrib.auth import get_user_model
from django.contrib.auth.forms import UserCreationForm
```



기본적으로 `ModelForm`과 `Model`을 상속한 인스턴스를 만들기 때문에 인스턴스를 만드는 두 함수를 사용하는 것 외엔 큰 차이가 없다.

- `form`인스턴스 생성

	```python
form = UserForm() -> form = UserCreationForm() # GET
	# or
	form = UserForm() -> form = UserCreationForm() # POST	
	```
	
- `User`인스턴스 생성

  ```python
  user = User() # User라는 모델 클래스를 사용할 경우
  ->
  user = get_user_model() # django 라이브러리에 정의된 auth_user사용
  ```



다음은 회원가입의 의미를 가지는 코드이다. 기존의 form과 사용방식은 동일하지만, `UserCreationForm`으로부터 반환된 form을 사용했기 때문에 `auth_user`에 사용자 정보를 저장한다.

```python
def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```



위의 `signup`함수를 이용하여 `admin`이라는 이름의 계정을 등록하면 다음과 같이 `auth_user`에 새로운 레코드가 생긴다. 여기서 **password**를 확인해보면 **해싱된 결과**이며 이 문서를 처음 작성할 때 언급했던 구현의 어려움을 해당 모듈이 해결해주고 있음을 알 수 있다.

![image-20200414194900921](C:/Users/11/AppData/Roaming/Typora/typora-user-images/image-20200414194900921.png)





