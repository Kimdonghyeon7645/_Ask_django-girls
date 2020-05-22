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

```python
# (애플리케이션명).views.py
from django.shortcuts import render

def post_list(request):
    return render(request, '(애플리케이션명)/post_list.html', {})
```
뷰는 위같이 views.py 파일에 이러한 함수형태로 만들어 주면 되는 것이다. 

이 뷰 함수는 request를 인자로 받아서, (이 request는 요청으로, 요청받은 정보가 여기에 들어있다.) **django.shortcuts.render() 함수**를 반환한다.   

이 render() 함수는 인자로 3개를 받는데, 첫번째 인자는 request, 두번째 인자로, 웹페이지에 보여질 html소스다. 세번째 인자는 db(모델)의 값이 온다. (여기서는 생략) 

물론 view는 항상 render 함수로만 반환하는 것이 아니고, HttpResponse 함수로 html코드를 문자열형태로 인자값으로 전달해줘도 되고, 다양하다.  
여기선, html 소스를 파이썬에서 제어하긴 어려우니까, 따로 소스파일로 분리해서 그것을 render 함수로 참조해주는 것이다.

---

사실 설명이 많았지, 뷰에서는 현재 장고걸스 튜토리얼상 위의 4줄정도의 코드만 작성해주면 끝이다.

이제 다음에는 render 함수에서 참조한 post_list.html 파일을 만들어주자.
