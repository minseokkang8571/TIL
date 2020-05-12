# 1. serializer

serializer는 쿼리셋, 모델 인스턴스와 같은 데이터를 JSON, XML 같은 컨텐츠 유형으로 변환한다. django에서 core serializer를 제공하지만 편의성이 좋지 않기 때문에 해당 문서에선 `django rest framework`를 사용한다. django rest framework (이하 drf)는 기본 ui를 제공하며 이를 토해 템플릿에서 벗어 난 환경에서 코딩이 가능하다.



## 1.1 기본 설정

- drf 설치

```dash
$ pip install djangorestframework
```

- INSTALLED APP에 등록

```python
# settings.py

INSTALLED_APPS = [
	# ... #
    'rest_framework',
]
```

- app디렉토리내에 `serializers.py` 생성



## 1.2 사용

serializer는 `serializers.py`에 정의를 하고 `views.py`에서 사용한다.



### serializers.py

우리는 drf를 이용해, 모델의 필드 중 원하는 데이터만을 JSON으로 변환 할 수 있다. serializer는 `serializers.py`에서 form과 유사한 형태로 정의하면 된다.

예를들어, Artist의 이름과 pk만을 JSON으로 반응받으려면 다음과 같이 코딩하면 된다.

```python
from rest_framework import serializers
from .model import Artist

class ArtistSerializer(serializers.ModelSerializer):
    class Meta:
        model = Artist
        fields = ['name']
```



만약 serializer의 필드에 다른 모델의 필드를 넣고 싶다면 다음과 같이 구현 할 수 있다. 여기서 Music은 Artist의 음악을 가지고 있는 모델이며 ArtistDetailSerializer를 통해 Artist와 그에 관련된 음악의 정보를 얻기 위한 예시이다.

serializers의 `source`는 직렬화를 위해 데이터를 가져 올 인스턴스를 의미하는데 default 값으로 변수의 이름이 들어가게 된다. 변수의 이름을 변경하고 싶은 경우 값을 넣어 넘겨주면 된다.

```python
from rest_framework import serializers
from .model import Artist, Music

class ArtistSerializer(serializers.ModelSerializer):
    class Meta:
        model = Artist
        fields = ['id', 'name']
        
class MusicSerializer(serializers.ModelSerializer):
    class Meta:
        model = Music
        fields = ['id', 'title', 'artist_id']
        
class ArtistDetailSerializer(serializers.ModelSerializer):
    music_list = MusicSerializer(source='music_set', many=True)
    music_count = serializers.IntegerField(source='music_set.count')
    
    class Meta:
        model =  Artist
        field = ['id', 'name', 'music_list', 'music_count']
```



### views.py

drf를 사용하면 기존의 템플릿을 이용한 render가 아닌 Response라는 함수를 사용해야 한다. 우린 이를 통해 템플릿에서 탈피하고 프레임워크를 사용 할 수 있다.

해당 함수에서 어떤 request 메서드를 사용 할 수 있게 하는지를 정해 줄 필요가 있는데, `api_view` 데코레이터를 사용하여 이를 정한다.

serializer를 이용해 모델의 값을 JSON으로 나타내려면 이를 위해 정의한 serializer 인스턴스를 생성하고 Response를 해주면 된다.

아래 코드는 Artist의 모든 값을 가지고 있는 쿼리셋을 직렬화하는 코드이다. 이 때 직렬화하는 복수의 오브젝트이므로 serializer의 many키워드 인자를 True로 넘겨준다.

```python
from django.shortcuts import get_object_or_404
from rest_framework.response import Response
from rest_framework.decorator import api_view
from .models import Artist
from .serializers import ArtistSerializer

@api_view(['GET'])
def artist_list(request):
    artists = Artist.objects.all()
    serializer = ArtistSerializer(artists, many=True)
    return Response(serializer.data)
```

