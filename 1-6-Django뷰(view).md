# 1-6. 장고 뷰(view)
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431188#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_views/

장고의 뷰(view)는 어려운 말로, 애플리케이션의 로직을 넣는 곳이라고 한다. 

뭔 소린지 풀어서 알아보자, 이전 장들을 기억해보면 url에 따라서 view의 함수가 연결되서, 해당 url 이라면, 그에 맞는 함수가 불러와, 가져와진다고 했었다. 

다시말해서, url과 연결되는 함수가 view(뷰)인데, 함수다 보니까 로직이 들어가며, 이 뷰라는 함수를 사용한다면,  
파이썬에서의 함수이기에 다양한 동작을 수행할 수 있으며, 동시에 애플리케이션에 대한 것이기 때문에,  
모델(데이터베이스, db)에서 필요한 정보를 받아와서 템플릿(웹페이지)에 전달하는 역할을 해줄 수 있다.

이 뷰(view)는 이제 views.py 파일에서 함수로 정의한다.

## 뷰(view) 만들기

