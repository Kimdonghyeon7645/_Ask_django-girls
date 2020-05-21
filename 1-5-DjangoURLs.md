# 1-5. 장고 urls
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431174#overview  
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_urls/

이제 관리자로 db의 값을 원하는데로 추가, 조회, 수정, 삭제할 수 있다. 

근데 그것은 관리자의 권한이고, 웹사이트를 방문한 일반 유저가 볼 수는 없다.  
우리가 다른 사이트에서 url<a href="#1"><sup>1</sup></a>를 이용해서 접속하면, 그 웹 사이트가 알맞은 페이지를 보여주듯이, 장고에서도 일반유저는 유저가 요청하는 url에 따라서 그에 맞는 페이지를 매핑 시켜주면 된다.  
(자세한 과정은 1-1의 장고의 요청 처리 방법에서 이미 설명했다.)


## 장고 URL 작동법

url에 대해서는, (프로젝트)

---

<a name='1'>url : url는 웹 주소다. 정확히 말하자면 uniform resource locator, 자원의 고유 주소를 가리킨다는 뜻 이다.