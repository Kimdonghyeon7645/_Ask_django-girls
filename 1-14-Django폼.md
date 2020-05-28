# 1-14. 장고 폼(form)
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431222#content  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_forms/

지금까지 장고로는 글(db의 객체)를 추가하거나 삭제할 때, 어드민 사이트에서 직접 작성을 해주었는데, 그보다 더 좋은 폼(form)을 사용하는 방법을 배워볼 것이다.

장고에서 솔직히 폼을 모든다면, 장고의 절반만 쓸줄 안다고 한덴다. 그정도로, 장고 폼은 잘쓰면 잘 쓸수록, 효율적인 웹사이트 개발이 가능하다.

## forms.py 만들기

우선 애플리케이션 폴더 아래에, ```forms.py```를 추가해준다.   
(참고로 다른 파일 이름(models)은 고정돼 있는데, 폼 파일을 만들땐 굳이 forms.py 라는 이름이 아니여도 된다. 대신에 가급적이면 이 이름으로 해주자.)

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ['title', 'text']
```
그리고 forms.py 파일에 이 코드를 추가해준다.  
폼(form)을 사용할 것이기에, django 의 forms와, 폼(form)과 연결될, db, 장고 모델인 Post를 같이 임포트(가져와)준다.

그리고 클래스로, PostForm 이라는 폼의 이름과, 그 클래스는 forms.ModelForm을 상속받게 해준다.

그리고 그 클래스안에 Meta(meta아님, 대문자 유의) 클래스를 추가해서, 그 클래스에는 ```model = Post``` 와 같이 어떤 모델(model)이 사용되는지와, ```fields = ['title', 'text']``` 와 같이 폼에 어떤 필드가 들어가는 지를 적어준다. (fields 리스트를 만들때 튜플로도 만들 수 있는데, 실수가 있을 수 있어서 리스트[]를 권장한다.)

이제 그러면, 이 폼을 url와 뷰를 이용해서 템플릿에 보여주게 해보자.

## 폼을 링크로 접속하게 연결해주기

```py3
# (애플리케이션명)/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    path('post/new/' views.post_new, name='post_new') # 이 한줄을 원래 urls.py파일에 추가
]
```
와 같이, ```post/new```라는 새 url 패턴을 추가해서, 이것을 view의 post_new라는 view함수에 연결해준다.


```py3
# (애플리케이션명)/views.py
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.all()
    return render(request, 'blog/post_list.html', {'posts': posts})

def post_detail(request, pk):
    posts = Post.objects.get(pk=pk)
    return render(request, 'blog/post_detail.html', {'posts': posts})
```
이제 그리고, view에서는, url 패턴에서 연결해준 새 post_list 함수를 만들어준다.

참고로, 이 view함수에선, 폼을 사용할 것이기에, 
```py3
from .forms import PostForm
```
과 같이, 아까 forms.py 에서 만든 PostForm이란 폼을 임포트(불러와)준다.

```py3
def post_new(request):
    form = PostForm()
    return render(request, 'blog/post_edit.html', { 'form': form})
```
그후, 이같이 post_new 라는 view함수를 작성해준다. 여기서는 PostForm()객체를 생성해서, 그걸 담은 변수인 form을 render() 함수에, 새로운 템플릿인 ```post_edit.html```과 함께 리턴해준 것이다.

이제 마지막으로, 템플릿을 작성해준다.

템플릿이 저장되는 위치에, 새로운 템플릿, ```post_edit.html```을 생성하고, 거기에 아래의 코드를 붙여넣어준다.

```py3
{% extends 'blog/base.html' %}

{% block content %}
    <h1>New post</h1>
    <form method="POST" class="post-form">{% csrf_token %}
        {{ form.as_p }}
        <button type="submit" class="save btn btn-default">Save</button>
    </form>
{% endblock %}
```