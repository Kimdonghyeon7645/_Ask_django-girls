# 1-3. 장고의 모델

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

    └── blog
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

(작성중...)
