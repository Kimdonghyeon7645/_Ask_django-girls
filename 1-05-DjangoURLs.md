# 1-5. 장고 urls
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431174#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_urls/

이제 관리자로 db의 값을 원하는데로 추가, 조회, 수정, 삭제할 수 있다. 

근데 그것은 관리자의 권한이고, 웹사이트를 방문한 일반 유저가 볼 수는 없다.  
우리가 다른 사이트에서 url<a href="#1"><sup>1</sup></a>를 이용해서 접속하면, 그 웹 사이트가 알맞은 페이지를 보여주듯이, 장고에서도 일반유저는 유저가 요청하는 url에 따라서 그에 맞는 페이지를 매핑 시켜주면 된다.  
(자세한 과정은 1-1의 장고의 요청 처리 방법에서 이미 설명했다.)


## 장고 URL 작동법

url에 대해서는, (프로젝트명)/urls.py 파일을 열어서 알아보자.

```python
# (프로젝트명)/urls.py
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```
그러면 위같이 되어있을 것이다.  

여기서 urlpatterns 리스트는 리스트 안에 있는 path의 url 패턴(첫번째 인자)들에 따라서, 패턴이 일치하면 실행할 함수(두번째인자)를 매칭 시켜준다. 

```python
# (프로젝트명)/urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
]
```
여기서 path('admin/', admin.site.urls) 를 보면, admin/ 으로 시작하는 url 패턴이 있는데,  
실제 클라이언트에서 http://(웹호스트)/(패턴) 으로 요청을 보낼때, 이 요청과 패턴이 일치하면 그에 맞는 매핑한 함수를 실행하는 것이다.  

예전에는 url 패턴을 지정하기 위해서, 정규표현식을 사용한 **django.conf.urls.url()** 함수를 사용했었는데,  
이제 장고 2.0 이상 부터는 보다 간결하고 단순한 **django.urls.path()** 함수를 사용할 수 있다.  
이함수로 일반적인 url의 패턴을 지정할 수 있는데, path()에서 지정못하는 복잡한 패턴일 경우엔,  
정규표현식을 사용하는, **django.urls.re_path()** 함수를 사용하면 된다.

(참고 : http://pythonstudy.xyz/python/article/311-URL-%EB%A7%A4%ED%95%91)


## 프로젝트에 url 만들기

참고로 urls.py 파일은, (프로젝트명)/urls.py 도 있고, (애플리케이션명)/urls.py 에서도 있으니, 둘을 헷갈리지 않게 구분해서 사용해야 한다. 

(프로젝트명)/urls.py 에서 클라이언트에서 요청받은 url를 패턴과 비교할 텐데, 이것을 view 의 함수로 바로 매칭시켜주는 것이 아니라, 애플리케이션의 urls.py 로 넘겨주도록 하자.

이렇게 urls.py 를 두번 사용하는 이유는, 코드도 깔끔해지고,  
코드가 적을땐 몰라도, 길다면  프로젝트의 urls.py 에서 모두 처리하는 것보다, 애플리케이션 별로 패턴을 구분해서, 해당 애플리케이션 안에서 패턴을 처리하게 하는 것이 더 보기도 좋다.


```python
# (프로젝트명)/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('(애플리케이션명).urls')),
]
```
프로젝트의 urls.py 에서 애플리케이션의 urls.py 로 넘겨주는 것은 **django.urls.include()** 함수를 이용하면 된다.  
이 함수를 원하는 패턴과 매핑해서 넣어주고, 이 함수의 인자로, 가져올 파일의 주소((애플리케이션명).urls) 를 적어주면 된다.  

그러면 원하는 패턴과 일치하는 url 요청이 들어오면, 이 include()함수가 호출되서, 애플리케이션의 urls.py 파일이 가져와 질 것이다.

그렇게 가져온 애플리케이션의 urls.py 파일에서는 이제 원하는 패턴과 view의 함수를 매핑 시켜주면 된다. 

```python
# (애플리케이션명)/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
바로 위와 같이 말이다.   
이제 그러면 장고의 URL resolver(URL 확인자)가 요청받은 url과 위 패턴('')을 비교해서 일치하면, view 파일에 있는 함수(views.post_list)를 실행하게 되는 것이다.

그리고 view 파일에 있는 함수는 화면에서 무엇이 보여지게 할지 정해준다.  
<br>

여기까지가 url 에서 패턴과 그에맞는 view 함수를 연결해주는 파트다.   
이정도까지 코드를 작성하고 서버를 돌리면 AttributeError 가 나는데, 왜냐면 지금 코드에서 views.post_list 함수를 사용했는데, 아직 실제로 함수를 정의해주지 않았기 때문이다.

이제 다음 장에서 view 의 함수를 정의해줘서, 웹 화면에 무엇이 보여지게 할지를 설정해주자.

---

<a name='1'>url : url는 웹 주소다. 정확히 말하자면 uniform resource locator, 자원의 고유 주소를 가리킨다는 뜻 이다.