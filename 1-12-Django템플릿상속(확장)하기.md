# 1-12. 장고 템플릿(templates) 확장하기(상속받기)
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431216#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/template_extending/

장고에서는 **템플릿 확장(=상속)(template extending)**이라는 기능을 제공해준다.  
웹페이지를 보면, 페이지마다 중복되는 부분, 레이아웃이 있다.  
근데 이 템플릿 확장 없이는, 중복되는 부분, 레이아웃도 일절없이 html코드를 복사해서든지 배치해야된다.  
어떻게보면, 이미짰던 html코드인데, 중복해서 다시 작성해야되니까 번거롭다.

여기서 템플릿 확장(=템플릿 상속)을 쓰면, html코드에서 다른 html코드를 받아와서 사용할 수 있다.  

## 템플릿 상속하기

이제 ```base.html``` 이란 부모 html코드를 만들어서, (이름이 뭐든 상관은 없다.) 거기에 ```post_list.html```에서 써주었던 코드를 모두 복사하고 붙여넣자.

어떻게 해줄것이나면, 부모 html파일(base.html)에서 기본적인 코드를 넣어주고, 자식 html파일(post_list.html)에서 부모html파일을 상속받아 사용할 것이다.

```html
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href='//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                {% for post in posts %}
                    <div class="post">
                        <div class="date">
                            {{ post.published_date }}
                        </div>
                        <h1><a href="">{{ post.title }}</a></h1>
                        <p>{{ post.text|linebreaksbr }}</p>
                    </div>
                {% endfor %}
                </div>
            </div>
        </div>
    </body>
</html>
```

이와 같은 전체 코드를 base.html(부모) 파일에 저장했으면, post_list.html(자식) 파일의 내용을 모두 지운다.

다시 base.html(부모)파일로 가서, 자식 html(post_list.html) 파일에 정의할 내용을 잘라내서 자식 html 파일에 붙여넣는다. 
```html
{% for post in posts %}
    <div class="post">
        <div class="date">
            {{ post.published_date }}
        </div>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```
 이제 템플릿 상속을 다른 자식 html파일에서 할 수 있게, 부모 html파일(base.html)에 필요한 템플릿 태그를 추가해주자.

아까 부모 html파일(base.html_에서 자식태그 부분으로 잘라내기한 부분의 빈공간을, 
```html
{% block (영역명) %}
{% endblock %}
```
으로 대체해준다.  
여기서 튜토리얼의 영역명은 'content'라고 해주었다.  
그렇다면 'content'라는 block 영역이 만들어지는 것이다.   

이 block 템플릿 태그는, 자식이 어떠한 부모 html파일을 상속받았을 때, 자식이 부모의 어떤 부분에서 컨텐츠를 추가로 표현하기 위해서, 부모가 정의한 block 공간에만, 자식이 어떤 내용을 표시할 수 있다.

그래서 아무리 자식 html파일에서 코드가 있고, 부모 html파일을 상속받는다고 해도, 부모html파일이 정의한 block공간이 없으면 자식의 컨텐츠는 웹페이지에 보여질 수 없다.  
(부모가 정의한 block 공간에 자식의 컨텐츠를 연결시켜줘야 되는 것이다.)

암튼,  
이렇게 부모 html 파일에서 block 영역을 정의했으니, 자식 html 파일도 수정해주자.  

```html 
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```
이렇게 맨처음에 ```{% extends '(부모html파일경로)' %}```와 같이해서, 부모 html파일(blog/base.html)을 상속(extends)받고,  
부모에서 정의한 영역인 content 블럭에 컨텐츠를 넣어주는 것이다. 
```html
{% block content %}

{% endblock %}
```
이 사이에 말이다. 이때, 이 block 템플릿 태그안에 들어가지 않은 코드는 웹페이지에 보여지지 않는다.  
```{% block (영역명) %}```으로써, 부모가 정의한 block 영역에 자식의 코드를 넣어줘야지만, 그부분의 컨텐츠를 웹사이트에서 보여줄 수 있는 것이다.  

여기서 템플릿 태그의 영역명을 틀리게 하면 부모가 정의해준 영역명과 자식이 작성한 영역명이 다르기 때문에 서로 연결되지 않아서, 웹사이트에도 보여지지 않는다.  
이점 유의하자.

## 끝!

이렇게 코드의 수정을 모두 끝내고 저장까지 해주었으면, 성공적으로 템플릿의 상속이 이루어 진 것이다.  

기억할만한 핵심은, 여러 html파일에서 중복되는 코드를 부모파일로 떼어놓아서, 자식파일들이 부모파일을 상속받을 수 있으며,  
상속받을 때는, ```{% extends '(부모html파일경로)' %}``` 를 자식html파일의 맨 윗부분에 작성해서 상속받는다.  

이때, 상속받은 부모html파일에서 자식html파일만의 컨텐츠를 덧붙이고 싶으면, 부모html파일에서 ```{% block (영역명) %} {% endblock %}```으로 영역을 정의한후,  
자식html파일에선 ```{% block (영역명) %} 코드 {% endblock %}```과 같이, block 템플릿 태그 안에 덧붙일 컨텐츠에 대한 코드를 작성해서, 부모파일과 자식파일의 영역을 연결하면 된다.