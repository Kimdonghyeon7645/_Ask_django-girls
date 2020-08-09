# 1-3. 장고의 모델
- 해당강좌 : https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431156#overview  
해당 튜토리얼 : https://tutorial.djangogirls.org/ko/django_models/


이제는 블로그의 포스트를 저장하는 부분을 만들텐데,  
그전에 알아둘 내용은 객체(Object)다. (+ 객체지향 프로그래밍, OOP)  
혹시 모른다면, 배워놓고 오자,  
(개인적으로, 내가 쓴거긴 하지만, [이곳에서,](https://github.com/Kimdonghyeon7645/Python_Summary/tree/master/_summary_python_code/6) 파이썬 객체의 모든 문법을 배울 수 있다.)


## 모델?

장고 안의 모델은, 객체의 특별한 종류로써, 이 모듈을 저장하면 데이터베이스에 저장이 된다.  

물론 장고에서는 여러가지 데이터베이스를 사용할 수 있는데, 기본적으로는 SQLite db가 기본 장고 데이터 베이스의 어댑터이다. (어댑더는 설명하기엔 너무 길어지니 생략!)

쉽게 생각해서, 데이터베이스의 모델을 엑셀 스프레드시트와 같다. 모델도 걔처럼 열과 행이 있다.

## 어플리케이션 만들기

일단 프로젝트 내부에 별도의 어플리케이션을 만들어보자.  
앞으로 개발할 어플리케이션에 대해 준비하는 것이다.

```python manage.py startapp (애플리케이션명)```
###### 앞으로 생략하겠지만, 장고에 대한 명령어들은 manage.py가 있는 폴더 위치에서 가상환경을 사용한다면, 가상환경이 활성화된 상태에서 명령어를 콘솔창에 쳐야된다.

그러면 해당 폴더 위치에 애플리케이션 명의 폴더가 생기고, 그안에 아래와 같은 파일들이 알아서 기본적으로 생성되게 된다.  

    └── (애플리케이션명)
            ├── migrations
            ├── __init__.py
            ├── admin.py
            ├── models.py
            ├── tests.py
            └── views.py

이렇게 애플리케이션을 만들었으면, 장고에게도 이게 만들어진 것에 대해 알려줘야 된다.  
(프로젝트명)/settings.py 에서 지난시간에 웹사이트의 모든 설정을 적는다 했는데, 여기서 애플리케이션이 추가된것을 설정에 적어주자.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    '(애플리케이션명)',
]
```
과 같이 애플리케이션을 INSTALLED_APPS 리스트에 추가해준다.

## 애플리케이션의 모델 만들기

장고의 모델은 애플리케이션 폴더의 models.py 에서 만들어 줄 수 있다.  
(애플리케이션명)/models.py 파일을 열어서 그안의 모든 내용을 삭제한 후, 아래 코드를 추가하자.  

```python
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
갑자기 생뚱맞은 코드가 한번에 추가됬는데,  
하나하나 천천히 알아보자. (웬만하면 파이썬 문법을 미리떼고 오자)

from import로 특정 파일들을 현재 파일로 가져온 후에,  

class 로 Post 라는 클래스를 만들어 주었다. 이것이 구현되면 인스턴스, 객체가 되는 것이다.  

장고의 모델은 객체로 만든다고 앞에서 말했었는데, 이렇게 클래스로 객체의 틀을 만들어주는 것이, 다시말해 모델의 틀을 만들어 주는 것이다.  
근데 이렇게 ```class Post```로 클래스를 만들면, 파이썬의 일반 클래스니까, 장고의 모델을 상속받는다.  
 ```class Post(models.Model)``` 와 같이, 괄호안에 상속받을 클래스를 넣어주면, 그 클래스를 상속받을 수 있다.

이제 장고 모델을 클래스로 선언해 줬으니, 장고 모델에서 들어가는 속성들이 있듯이, 클래스에서도 장고 모델의 속성을 정의해주자.

걍 클래스에 (속성명) = (데이터타입) 으로 속성을 정의해주면 되는데,  
여기서 데이터타입은, 필드마다에 어떤 종류의 데이터 형식(숫자든 문자든)을 가지는지 정해줘야 된다. 이 데이터타입을 어떻게 대입하냐면, 
이와 일치하는 종류의 데이터타입 객체를 (이용)참조하면 된다.  

- ```models.CharField``` 는 글자수가 제한된 텍스트를 정의(db에서는 같은 텍스트도 다른 데이터 타입으로 각각 정의할 수 있다.)
- ```models.TextField``` 는 글자수의 제한이 없는 긴 텍스트를 정의
- ```models.DateTimeField``` 는 날짜와 시간 데이터를 정의
- ```models.ForeignKey``` 는 다른 모델에 대한 링크를 정의

이외에도 필드는 매우 다양하니, [장고공식문서](https://docs.djangoproject.com/en/2.0/ref/models/fields/#field-types) 에서 필요한 것은 찾아서 사용하자.

```def publish(self):``` 는 클래스안에 있고, def 가 앞에 있으며, (self)를 인자로 받으니, 빼박 메서드인 것을 알 수 있다. (publish 는 이 메서드의 이름인 건 말하지 않아도 알거다. 어려워 죽겠으면 파이썬 문법의 클래스를 다시보고 오라.)

```def __str__(self):``` 같이 __(더블스코어)가 붙은 스페셜 메서드도 있다. 이 메서드 처럼 return 으로 함수를 호출 했을 때의 반환값을 지정해 줄 수 있다.   
(스페셜 메서드? 생소할 텐데, 구글링 해보라. [내가 정리한 것](https://github.com/Kimdonghyeon7645/Python_Summary/blob/master/_summary_python_code/6/04-9-1-%EC%8A%A4%ED%8E%98%EC%85%9C%EB%A9%94%EC%8F%98%EB%93%9C%2C%EB%A7%A4%EC%A7%81%EB%A9%94%EC%8F%98%EB%93%9C.py)을 참고해도 좋다.)  
참고로 이 메서드는 닉값 그대로, str(문자열)형식으로 객체를 출력하는 것 같이 사용할 때 호출된다.

이렇게 models.py 에서 코드 몇줄 넣었을 뿐인데, 장고의 새 모델, Post 모델을 만들었다.  
이제 그러면 데이터베이스에 장고의 모델을 추가하는 작업만 남았다.

## 데이터베이스에 모델을 추가

콘솔로 가자. 명령어를 쳐주러 가면된다.  

```python manage.py makemigrations``` (```python manage.py makemigrations (애플리케이션명)```으로 해도 괜찮다.)   
이와 같이, models.py 에서 추가한(새로 변경된) 모델을 migrations 파일로 저장하라고 장고에게 시키면,  
장고는 코드에 문제가 있음 오류를 주는 것이 아니면, ```...OK``` 같은 것을 보여주면서 migrations 파일들을 만들어준다.   
참고로 migrations 파일들은 실제 (애플리케이션명)/migrations/ 폴더 안에 저장된다. (첫 migrations 파일이면 0001_initial.py 란 이름일 것이다.)  (이 migrations 파일을 읽거나, 그 파일을 직접 변경할 수도 있다.)

```python manage.py migrate``` (```python manage.py migrate (애플리케이션명)```으로 해도 괜찮다.)  
그리고 위 명령어를 입력해서, migrations 파일을 실제 데이터베이스에 반영해주면 된다.  
그러면 makemigrations 명령어로 만들어준 migrations 파일들이 sql(db의 그 sql 맞다)문을 실행시켜서 실제 데이터베이스에 테이블을 만들어 준다.  

(혹시 migrations 파일들이 어떤 sql 문을 실행하는 지 궁금하다면, sqlmigrate 명령어를 이용하자.)
(이 외의 다양한 migrate 명령어에 대해서는, [이곳](https://brownbears.tistory.com/443)을 참고하자.)

## 짜잔!

그러면 만든 장고의 모델이 데이터베이스에 저장됬다. 문제없다면 후딱 다음 장으로!
