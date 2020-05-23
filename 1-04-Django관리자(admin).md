# 1-4. 장고 관리자, admin(어드민)
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431162#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_admin/

참고로 장고에서는 db 등을 쉽게 관리 할 수 있게, 장고 관리자 페이지를 지원한다.   
이 장고 관리자에서 아까 모델링했던 장고의 모델을 장고관리자에서 추가, 조회, 수정, 삭제(db의 값을 crud할 수 있는 것이다.)할 수 있다.

그러면 장고 관리자 페이지을 만들어보자.

## 장고 관리자 페이지 만들기

(애플리케이션명)/admin.py 파일을 열어서, 
```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
그 안의 내용을 이걸로 바꿔준다.

그리고 ```python manage.py runserver``` 로 현재 장고 프로젝트를 로컬 서버로 실행하면,  

http://127.0.0.1:8000/ 에서는 전과 같이 로컬 서버가 잘 열린 것을 볼 수 있고,   
http://127.0.0.1:8000/admin/ 에서는 장고의 관리자 페이지를 볼 수 있다. 

![image](https://user-images.githubusercontent.com/48408417/82524318-957ba080-9b69-11ea-8085-1f6a9373926c.png)

참고로 관리자 화면을 한국어로 변경하길 원하면, settings.py 파일에서 
```python
LANGUAGE_CODE = 'en-us'
```
를 ```ko-kr``` 로 바꿔서 대입하고 저장하면 된다.

## 슈퍼유저(superuser) 만들기

근데 로그인을 어떻게 할까..?  
장고 관리자 페이지로 로그인하는 화면은 띄었는데, 생각했는데 로그인할 수 있는 계정이 없다!  
(나는 구글계정이 있어서 로그인할 수 있다는 사람은 없길 바란다.)

근데 계정이 없는 문제를 알았다면, 문제를 해결하기만 하면 될 것이다. 

```python manage.py createsuperuser``` 로 장고 관리자 페이지에 로그인 할 수 있는 유저(사용자)를 만들어주자.  
근데 createuser 는 말그대로 유저를 만들어 주겠다는 걸 알겠는데,  
super가 왜 붙었는 가 하면, 우리가 만들어 줄 유저(사용자)가 모든 권한을 가지는 슈퍼 유저(superuser)이기 때문이다.

```
Username: 
Email address: 
Password:
Password (again):
```
그러면 위같이 유저의 이름, 메일과 비밀번호를 입력할 수 있는데, 알아서 잘 빈칸을 채워주자.  
참고로 Password 에서는 보안때문에 입력하는 내용이 화면에 보이지 않는다. 

다 입력한 후에 엔터(Enter)를 치면, ```Superuser created successfully.``` 를 볼 수 있을 것이다.  
성공적으로 사용자가 만들어 졌으니, 드디어 장고 관리자 페이지에 로그인 해보자.  
(서버가 닫혔으면, ```python manage.py runserver```으로 다시 열자.)

이제 그리고 나오는 화면에서, 원하는 데로 db를 추가, 조회, 수정, 삭제 해주도록 하자!