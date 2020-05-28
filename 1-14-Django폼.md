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
그렇게 하면, 웹사이트에서 해당 url로 접속했을때, 웹사이트에서 폼을 볼 수 있다.

참고로 여기서 ```<button type="submit" class="save btn btn-default">Save</button>``` 에서 class 속성값을 ```save btn btn-default``` 가 아닌, ```save btn btn-primary```로 변경하면,  
submit 버튼이 흰색이 아닌, 파란색으로 포인트를 설정할 수 있다.

대신에 이 폼에다 값을 입력하고, 전송버튼을 누르면, 값이 증발해버린다.  
view에서 폼을 입력받았을 때, 그 값을 db에 저장하는 코드를 추가하지 않아서 이다. 이제 그부분을 만들어주자.

## 폼을 저장하기 (포스팅 추가)

폼을 입력할때, ```post/new/``` 링크로 들어가서, ```post_new``` 라는 view함수를 불러왔는데, 폼을 저장하고 전송할 때도 같은 view함수를 불러온다.

대신에 같은 view함수를 부른다고 해도, request에서 담긴 데이터에서 폼(form)에 입력한 값들이 전달되서 함수가 불러와진다.

이때, 요청의 메서드(method)에 따라서 그 요청의 값이, request.GET, request.POST 에 담겨서 view함수의 매개변수로도 전달받게 된다. (파일이라면 request.FILES 에 담긴다.)  
그리고 요청의 메서드 형식도, request.method 에 담겨서, 요청 형식(method)이 뭔지도 구할 수 있다. 

이것들을 이용해서 이제 폼을 db에 저장하는 코드를 view에 작성해주자. (폼을 입력하기 전이나, 폼을 전송할때 모두 같은 post_new란 view함수를 불러오기에 그 함수에서 코드를 추가해주면된다.)

```py3
# (애플리케이션명)/views.py
# request.POST (POST요청으로 전달한 폼의 값들이 저장)
# request.method (요청 메서드의 형식이 무엇인지 저장)
def post_new(request):
    form = PostForm(request.POST, request.FILES)
    if form.is_valid(): # 폼에 입력한 값이 비여있지(or 이상하지)않은지 참, 거짓으로 검출
        post = form.save(commit=False)  # 폼을 db에 저장하고, 그걸 쿼리셋으로 변수에 저장
        post.author = request.user  # 그 객체에 유저 필드 채우기
        post.published_date = timezone.now() # 객체에 작성시간 필드 채우기
        post.save() # 필드 변경한 것을 저장
        return redirect('post_detail', pk=post.pk)  # post_detail 함수로 넘어가고, pk 변수엔 지금 추가한 최신 객체의 번호를 인자값으로 전송
    else:
        form = PostForm()
    return render(request, 'blog/post_edit.html', { 'form': form, })
```
이렇게 저장후에 웹사이트에서, 폼을 제출할 수 있는 url에 접속하면 폼으로 값을 추가하고 전송해서 포스팅을 추가할 수 있다.

마지막으로, 루트 url의 웹페이지(post_list.html)에서 이 폼으로 포스팅을 추가하는 링크(post_edit.html)로 연결되는 a태그를 템플릿에 만들어주면 끝이다.
```html
<a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
```

## 포스팅 수정하기

이제 포스팅을 추가하는 폼을 만들었으니, 포스팅을 수정하는 폼도 만들어주자.
```python
# (애플리케이션명)/urls.py
path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
```
url 패턴에 위같은 패턴을 추가해주고, 

```py3
def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_edit.html', {'form': form})
```
위같이 view함수를 추가해준다.   
전체적인 구조는 post_new view함수와 같은데,  
포스트를 불러와서 미리 수정할 폼에 저장하는 코드를 수정했다.
```py3
form = PostForm(request.POST, instance=post)
```
위같이 해주면, 불러온 포스트(객체)를 담은 변수 post를 instance로 넘겨주면서, 빈 폼이 뜨는 것이 아니라,  
기존의 포스트내용으로 폼의 필드를 불러와서 다시 저장할 수 있도록 코드를 수정했다.

그리고 이렇게 저장했으면, 각각의 포스트를 보여주는 페이지(post_detail.html)에서도 손쉽게 그 포스트를 수정하는 페이지(post_edit.html)로 넘어갈 수 있도록, 템플릿에서 a태그를 추가해주면 끝이다.

## 보안 : 관리자만 포스트를 변경하도록 하기

