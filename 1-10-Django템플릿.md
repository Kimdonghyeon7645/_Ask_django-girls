# 1-10. 장고 템플릿의 심화
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431212#overview     
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_templates/

장고의 템플릿은 'HTML시작하기'(1-7 참고)에서 배웠었다. 그때는 HTML의 양식을 사용해서 간단히 템플릿을 만들었는데,  
이번에는 심화적인 장고 템플릿에 대해 배워보겠다.

## 장고 템플릿으로 DB 값 활용하기

지난 장에서, 
```python
# (애플리케이션명).views.py
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```
와 같이 posts 쿼리셋을 posts 라는 이름으로 넘겨줬는데, 이것을 장고 템플릿에서 활용하는 것이다.

근데 장고 템플릿은 HTML 양식으로 짠다고 했었다. 그래서 html코드 안에 파이썬 코드를 무턱대고 넣을 수도 없다.  
브라우저는 html코드만을 이해하지, 파이썬 코드를 이해하지 않기 때문이다.  

그래서 필요한 것이 장고의 **템플릿 태그**다. 이 템플릿 태그는 파이썬코드를 HTML로 변환해서 빠르고 쉽게 파이썬의 동적인 데이터를 사용한 웹사이트를 만들 수 있게해준다.

## 템플릿 태그

장고 템플릿 태그는 {{, }} 로 중괄호 2개를 감싸서 처리한다.  
아까 views.py에서 posts쿼리셋을 posts라는 이름으로 넘겨주었는데, 그 변수 이름을 {{ }}안에 넣어주면 된다.

```python
{{ posts }}
```
이렇게 말이다. ({{과 변수이름 사이에 공백을 안 넣어줘도 되지만, 넣어주는 게 가독성도, 보기도 좋다.)

그렇게 하면, view에서 넘겨준 쿼리셋 객체목록이 HTML에서 그대로 출력된다.

## 템플릿을 더 보기좋게

근데, [<Post: ~>, ...]와 같은 리스트 형식으로 텍스트가 웹사이트에 보여지는데, 보기엔 별로다.  
그래서 약간의 템플릿 제어문, for문을 사용해서 출력해주자.
```python
{% for post in posts %}
    {{ post }}
{% endfor %}
```
위같이 해주면, posts가 파이썬처럼 for문을 돌면서 차례대로 뽑아져나와서 post라는 변수에 담겨, 출력될 것이다.

이렇게 하면, 텍스트만 보여지긴 하는데, 디자인이 하나도 없이 그냥 모두 일반 텍스트라서, 아직도 부족하다.

```python
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```
이와 같이, 그러면 {{}}의 형태인 템플릿 태그에 \<p>같은 태그를 감싸서 출력되게 해보자, 템플릿 태그는 변환되면 html에선 텍스트로 변환되기에,  
```<p>{{post.text}}</p>``` 는 ```<p>텍스트</p>```와 같다.

근데 여기서 보면, ```post.title``` 같은 것은 post 변수의 객체에 있는 속성(필드값)인 title을 출력하는 것을 알긴 하겠는데,  

```post.text|linebreaksbr``` 는 뭔지 난감할 것이다. 이것은, 장고 템플릿 태그에서 함께 사용하는 **필터**다.  
이 필터는, text 필드에 있는 텍스트에서 행이 바뀌면 그부분에 \<br>태그를 텍스트 사이에 넣어주는 필터로,  
이같이 변수에 필터를 넣어줄 때, 파이프문자인 |를 사이에 붙여서 넣어준다.

이 필터 말고도, linebreaks 필터를 대신 사용하면 \<br>을 추가하는 대신, 텍스트에 행이 바뀔때마다, \<p>태그로 묶어서 처리해준다.  
(강의하시는 분 말로는, linebreaksbr보다 linebreaks가 더 보기에 좋다고 한다.)

이외에도 필터는 다양하니 상황에 맞게 찾아쓰자.
