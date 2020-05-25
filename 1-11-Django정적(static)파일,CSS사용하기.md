# 1-11. 장고 정적(Static)파일, CSS로 웹페이지 예쁘게 만들기
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431214#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/css/

CSS는 HTML로 웹 문서를 만들었다면, CSS으로 웹문서의 스타일을 꾸며주는 언어다.  
아마 이번에 배울것은 CSS인데, HTML5-CSS3를 정리한 [저장소](https://github.com/Kimdonghyeon7645/HTML5-CSS3_Summary)와 연계 된다.

(추가로 JS로는 웹 문서의 로직, 동작을 정의할 수 있다. 이렇게 HTML, CSS, JS가 웹문서를 짤때 기본이 되는 것이다. JS에 대해서는 정리한 [저장소](https://github.com/Kimdonghyeon7645/JS_for_WebBrowser)를 참고하자.)

근데 CSS에 아무것도 없는 상태에서 시작하긴 어려울 것이다. (더군다나 여기는 장고 프로젝트를 하는거지, CSS를 심층있게 배우는 것이 아니다.)

그래서 미리 개발자들이 만들어둔, 오픈 소스 코드인, **부트스트랩(Bootstrap)**을 활용할 것이다.  

## 부트스트랩을 웹페이지에 적용

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
```
이 코드를 .html 파일안 \<header>태그 안에 추가해주면 된다.  
이것은 링크(link)태그를 이용해서 인터넷에 있는 부트스트랩 파일을 연결하는 것으로, 이것을 적용한 뒤에, 웹사이트를 새로고침하면, 스타일이 깔쌈해진 걸 볼 수 있다.

## 정적파일?

사실, 장고에서는 Media(미디어) 파일이 있고, Static(정적) 파일, 2가지가 있다.  
(미디어 파일은 장고에만 있는 용어고, 정적 파일은 일반적으로 사용하는 용어다.)

그중, **정적 파일(static files)**은 CSS와 이미지 파일등에 해당된다.  
그래서 CSS를 적용할때, .html 파일안에서 \<style>태그 안에 CSS 코드를 작성하지 않고 외부 .css 파일로 작성뒤에 .html 에서 불러온다면, 이때의 .css 파일이 정적 파일이 되는 것이다.

## 정적파일의 위치

이 정적 파일은 템플릿 폴더에 걍 저장해서 \<link>로 불러올 수가 없다. 장고에서 정적 파일이 있어야 할 위치가 정해져 있는데,  

(애플리케이션명)/ 폴더에서 templates/ 폴더를 만들었듯이, 애플리케이션 폴더 아래에 마찬가지로 static/ 폴더를 만들어주어야 한다.

    (프로젝트를 생성했던 폴더명)
        ├── (애플리케이션명)
        │   ├── migrations
        │   ├── static
        │   └── templates
        └── (프로젝트명)

위같이 말이다. (static 폴더 이름도, 철자 틀리면 안된다.)

이제 그리고, static/ 폴더 아래에, (애플리케이션명)/ 이나, CSS/ 같이 폴더를 추가로 만들어, (static/ 폴더 아래의 폴더이름이나 위치는 자유다.)   
정적파일을 구분할 수 있게 해준뒤, 그 생성한 폴더 아래에 이제 CSS파일을 생성하는 것이다.

    djangogirls
    └─── blog
         └─── static
              └─── css
                   └─── blog.css

그리고 CSS 파일을 html 파일에서 link로 불러와주자.
```html
<link rel="stylesheet" href="/static/blog/blog.css">
```

이제 새로고침해서 웹사이트의 변화를 확인하자. 이때도, 템플릿 파일을 만들때처럼, 웹서버가 돌아가는 중에 파일을 추가하면, 장고에서 파일이 생겼는지 이해를 못하니,   
서버를 재시작해서, 새로 생성한 css파일을 불러올 수 있도록 해주자.

## 장고에게 static 경로 처리 위임하기

```html
<link rel="stylesheet" href="/static/blog/blog.css">
```
과 같이 직접 link로 정적(Static)파일의 경로를 지정해줄 수 있지만,  

static/ 경로는 사실 ```settings.py```파일에서 관리하기에 여기서,   
```python
STATIC_URL = '/static/'
```  
이부분의 URL를 변경해주면 정적파일의 경로를 변경할 수 있다.  
그래서 link로 정적 파일의 위치를 직접 지정해주는 것은 셋팅에서 정적파일의 URL을 변경하면 html에서도 다시 지정해주어야 하는 번거로움이 있을 것이다.  

이것을 해결해주는 것이, 장고에게 static(정적)파일의 경로 처리를 맡겨버리는 것이다.

```html
<link rel="stylesheet" href="{% static '/static/blog/blog.css' %}">
```
바로 위와 같이 템플릿 태그를 이용해서 말이다. 대신에, 이렇게 템플릿 태그로 static 처리를 해줄려면,  
```html
{% load staticfiles %}
```
위와 같이 staticfiles 을 로드,(임포트)불러와 주어야한다.

이렇게 저장하고 웹사이트를 새로고침하면, 변함은 없다. 장고의 템플릿 태그는 그부분을 장고가 알맞은 텍스트(여기서는 정적 파일의 경로를 텍스트)로 변환하기에, 문제없이 돌아간다.

## CSS으로 웹사이트 메이크업하기

이제 어느정도 기본은 배웠으니, CSS파일과 html파일의 속을 채워주자.  

```css
.page-header {
    background-color: #ff9400;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content { margin-left: 40px; }

h1, h2, h3, h4 { font-family: 'Lobster', cursive; }

.date { color: #828282; }

.save { float: right; }

.post-form textarea, .post-form input { width: 100%; }

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post { margin-bottom: 70px; }

.post h1 a, .post h1 a:visited { color: #000000; }
```
이같이 CSS에 내용을 채워주고,

html코드의 body태그 안에는,
```html
<div class="content container">
    <div class="row">
        <div class="col-md-8">
            {% for post in posts %}
                <div class="post">
                    <div class="date">
                        <p>published: {{ post.published_date }}</p>
                    </div>
                    <h1><a href="">{{ post.title }}</a></h1>
                    <p>{{ post.text|linebreaksbr }}</p>
                </div>
            {% endfor %}
        </div>
    </div>
</div>
```
로 내용을 채워준다. 그리고 웹사이트를 새로고침하면,  
끝! 볼만한 웹사이트가 하나 완성되게 된다.
