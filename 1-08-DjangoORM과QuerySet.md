# 1-8. 장고 ORM 과 쿼리셋(Query Set)
- 해당강좌: https://www.udemy.com/course/djangogirls-with-askdjango/learn/lecture/9431204#overview    
해당 튜토리얼: https://tutorial.djangogirls.org/ko/django_orm/

## ORM? 쿼리셋?

**ORM**이란, 원래 데이터베이스(DB)는 sql문, sql언어로써 읽고 쓰고 변경하고 삭제할 수 있는데, 그러지 않고 파이썬이라면 파이썬 코드로, 루비라면 루비 코드로 각 언어의 코드를 사용해서 sql문을 만들어내는 것이다.  
물론 이러기에는, 코드를 짜면 그 코드를 sql문으로 변경해주는 라이브러리가 필요할텐데, 그 라이브러리가 바로 ORM이다.

장고에서는 지금까지 배웠었던 장고 모델(Model)이 바로, 장고의 ORM이다.

그러면 **QuerySet**이란, 장고의 ORM인 장고 모델을 sql문으로 만들어주는 녀석(주체)이다.  
장고걸스 튜토리얼에서는 '전달받은 모델의 객체 목록'이라고 한다.

## 장고 쉘(shell)

이제 그러면 장고가 지원되는 파이썬 프롬프트, 바로 **장고 쉘**로 접속해보자.  
장고 쉘이 뭐냐면, 파이썬 프롬프트(cmd에서 python명령어 치면 나오는 거)와 비슷하다. 근데 파이썬의 모든 명령어 + 장고의 기능까지도 사용할 수 있는 곳이다.  
(장고걸스 튜토리얼에서는 '장고 인터랙티브(대화형) 콘솔'이라고 말한다.)

여기로 들어가본 이유는, 여기서 ORM, 쿼리셋을 다뤄보기 위해서다. 이제 장고 쉘에 접속해있으면,  
지금부터 장고의 명령어들로 한번 장고 모델(객체)를 입맛대로 조작해보도록 하겠다.

## (+) ipython 사용하기

파이썬 쉘을 쓴다면 ```pip install ipython```을 cmd에 입력해서, ipython을 설치해보는 것도 추천한다.   
ipython을 사용하면 기본 쉘보다 더 기능이 확장되고, 유용해진 쉘을 사용할 수 있다.  
(ipython을 깔았다면, 장고 쉘로 접속할때 원래 명령어로 접속해도 ipython이 알아서 적용된다.) 

ipython을 깔고 쉘을 다시 접속해보면, 쉘이 >>> 가 같은 꺽쇠3개의 프롬포트 모양이 아닌, 주피터 노트북에서 봤던 모습으로 바뀌어 있는 걸 볼 수 있다.   
여기서 활용할 수 있는 착한? 기능으로는, 기본 쉘에서는 tab키로 자동완성을 지원하지 않지만, ipython에선 자동완성을 지원한다. 그외에도 다른 기능은 알아서...

## 모든 객체 조회하기

이제 진짜로 장고 모델을 장고 쉘에서 조작해보도록 할텐데,
```python
from (애플리케이션명).models import Post
```
그전에 위 코드로, 예전에 장고 모델로 만들어줬던(models.py 파일 안의) Post객체를 임포트, 불러와준다. 



이제 그러면 장고 모델에서 만들어주고 관리자로 입력했던, DB의 값들을(객체들을) 모두 출력해보자.
```python
Post.objects.all()
```
(객체명).objects 에 .all()을 붙이면 해당 객체(DB)에 저장된 모든 데이터가 출력될 것이다. 

여기서 ```(객체명).objects```, 현재 예시에서는 ```Post.objects```가 쿼리셋이다. 실제로 이것을 type()함수로 타입을 찍어보면 QuerySet(쿼리셋)인것을 확인할 수 있다.

그래서 (객체명).objects 이라고 하면, 장고 ORM이 DB에 값을 요청에서 가져와주는 것이다.

## 객체 생성하기

(객체명).objects.create([인자='값'])  
과 같은 코드로 이제 객체를 생성해줄 수도 있다.  
```python
from django.contrib.auth.models import User
```
근데 그 이전에, User 모델을 불러와주자.  
왜나면, 장고 모델의 Post모델에서 사용자 명(author)필드를 만들어주어서, DB에 하나의 행을 입력할때, 행을 입력하는 사용자 명도 필드에 저장해주었기 때문이다.  
그래서 Post 모델에서 객체를 생성, DB에 행을 넣어줄려면, 유저명도 같이 입력해주기 위해서이다.

User 모델도 마찬가지로 장고 모델이기에, 위에서 배운 ```(객체명).objects.all()``` 로 User 모델의 모든 객체를 출력할 수 있다.

그렇게,
```python
User.objects.all()
```
로 User의 객체를 모두 출력하면, 사용자의 이름이 나올 것이다. 여기서 나오는 이름은 예전에 관리자(admin)페이지를 만들때 생성해준 슈퍼유저(superuser)다.

이 유저의 이름을 가져올려면, 
```python
(변수명) = User.objects.get(username='(슈퍼유저명')
```
과 같이 유저이름을 변수에 담아도 되고, 유저가 지금 하나만 만들어 주었으니까,
```python
(변수명) = User.objects.first()
```
으로 User 모델의 첫번째 객체를 가져오는 방법으로 유저이름을 가져와도 된다.

```python
me = User.objects.first()
```
튜토리얼에서는 위같이 me라는 변수이름으로 담아줬다.

그리고 여기서 가져온 유저에 대한 정보는, 유저이름 뿐만아니라 패스워드도 있다.
```python
me.username
me.password
```
근데 패스워드는 암호화가 되있어서 출력해도 제대로 볼 순 없다.

이제 Post 객체에 아래의 코드로 객체를 생성해주도록 하자.
```python
Post.objects.create(author=me, title='(제목텍스트)', text='(텍스트)')
```
이렇게 하고, ```Post.objects.all()```으로 다시 Post 모델의 모든 객체를 출력해보면, 추가한 객체도 같이 출력되는 것을 볼 수 있다.

위같은 객체 생성 코드로, 원하는만큼 객체를 추가해줄 수 있다.

## 객체 카운트하기

```python
Post.objects.count()
```
이렇게 하면, Post 모델의 객체 개수를 숫자로도 출력할 수 있다. 그냥 강의에서 나와서... 나중에 필요할때 써먹자.

## 필터링하기

(객체명).objects 에서 .all()로 모든 객체를 대상으로 하고, .first()로 첫번째 객체를 대상으로 한다면,  
.filter()로 인자값으로 필터할 내용을 넣어서 필터링한 객체를 대상으로 선택할 수 있다.

```python
Post.objects.filter(author=me)
```
예를들어 위같이 해준다면, author 필드가 me인 객체만 가져오게 된다.

```python
Post.objects.filter(title__contains='title')
```
```title__contains``` 같이 해서 필드에 연산자를 넣을 수 있다.  

    사실 'title__contains'는
    Post의 필드이름인 'title'에, 'contains'란 필터를 추가한 것이다. 
    
    첫 예시처럼(author=me)
    filter에 (필드)='값'으로 인자값을 간단히 넘겨줄 수 있지만,
    여기에 필터나 연산자를 추가해서 조건을 넣어주고 싶다면, (필드__(필터or연산자))='값'같이 해줘야 된다.
    이렇게 필드와 필터or연산자 둘을 구분할 때 장고 ORM은 __을 붙여준다. (_가 반드시 2개여야 된다.) 

이 ```title__contains```의 뜻은, title 필드에서, (contains=)~를 포함하고 있는 이라는 뜻이며,  
```title__contains='title'``` 은, title 필드의 값에서 'title'이란 텍스트를 포함하고 있는 값들만 가져오게 된다.

참고로 contains는 대소문자를 구분하는데,(물론 DB에 따라서 contains를 써도 DB가 대소문자를 구별안하면 걍 구별안함) 이 대신에 icontains를 사용하면 대소문자를 구분하지 않게 된다.

## 현재 시간 사용하기

```python
from django.utils import timezone
```
참고로 장고에서는 UTC 기준으로 시간을 제공한다.
이것은 유틸스(utils)의 timezone을 임포트, 가져와서 사용할 수 있는데, 

```timezone.now()```라고 하면 현재 시간을 가져올 수 있다.

그리고 이것을 이용해서, 필터에 현재 시간을 사용할 수 있다. Post 모델을 만들때, published_date 필드에 객체를 만든 시간을 넣어주도록 짰었기 때문이다.

그리고 이제 published_date에 담았던 시간을 기준으로 필터링도 해보자.
```python
Post.objects.filter(published_date__lte=timezone.now())
```
이와 같이 해줄 수 있는데, ```published_date__lte```는 마찬가지로 published_date라는 필드에 lte라는 연산자를 추가한 것이다.  
여기서 'lte'는, 'less than equal'의 약자로, 작거나 같다(<=)라는 연산자를 의미한다.  
(반대로 'gte'는 'greater than equal'로, 크거나 같다(>=)란 연산자다.)

그래서 ```published_date__lte=timezone.now()```은 ```published_date <= timezone.now()```와 같으며, ((published필드에 있는 시간값) <= 현재 시간값)이란 소리다.  
그래서 이 필터는 현재시간보다 이른, 그니까 과거의 시간의 정보를 가져올 것이다.

## 장고모듈, 객체의 메서드 호출하기

그래서 원래라면 현재시간보다 이른 시간값인 경우만 가져오게 될텐데, 사실은 그렇지 않다. 이걸 출력해보면 빈 상태가 출력된다.

왜냐면 published_date 가 비여있기 때문이다..  하지만, 지난번의 Post 모델에서 대충 넘어간 코드중에, 
```python
# models.py
    def publish(self):
        self.published_date = timezone.now()
        self.save()
```
와 같이, Post 모델안에 메서드 publish()로 이 메서드를 호출하면, 현재 시간(timezone.now())을 published_date 필드에 넣고, save()까지 해줘서 필드에 적용까지 해주었다.  
그러면 이 메서드를 호출해주면 현재 비여있는 published_date 필드에 시간값을 넣어줄 수 있을 것이다.

그러면 실제로 객체에 메서드를 호출해주도록 하자.
```python
post = Post.objects.get(title="Sample title")
```
라고 하면, 이제 Post 모델중에서 title이 'Sample title'인 객체(DB로 말하면 행)을 가져와서 post 변수에 저장해 줄 수 있을 것이다. 그러면 post엔 해당 객체가 담기겠고, 이대로
```python
post.publish()
```
와 같이 객체의 메서드를 실행해주면, published_date 필드에 시간값이 들어가게 된다. 잘 들어갔는지 궁금하면,
```python
post.published_date
```
로 published_date필드도 장고 모델에선 객체로 구현했으니, 필드도 객체의 속성으로 접근해서 출력해줄 수 있다.  
그렇게 published_date필드를 출력해주면 정상적으로 객체에 시간값이 저장된 걸 확인할 수 있다.

그리고 마지막으로,
```python
Post.objects.filter(published_date__lte=timezone.now())
```
다시 위 코드를 실행한다면, post에서 published_date로 시간값을 넣어줬었으니까, 그 시간값을 넣어준 객체만 출력이 되는 것을 볼 수 있다.

## 객체 정렬하기

쿼리셋(=(객체명).objects)에서는 또한 .order_by() 함수를 사용해서 객체목록을 정렬할 수도 있다.

```python
Post.objects.order_by('created_date')
```
이렇게 ```.order_by('(필드명)')```과 같이 해당 필드명을 기준으로 객체 목록을 오름차순으로 정렬할 수 있다.

반대로 내림차순으로 정렬하고 싶다면, 필드명 앞에 -를 붙여주면 된다. 아래와 같이 말이다.
```python
Post.objects.order_by('-created_date')
```

## 쿼리셋 체이닝(연결)

사실 이런 함수들을 여러개를 연결해서(파이썬에서 흔히 체이닝이라고 했던 것) 사용할 수도 있다.  
그러면, and 연산을 한 것처럼 여러 조건이나 동작을 묶어서 복잡한 구조도 쉽게 작성할 수 있다.

예를들어,
```python
Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```
과 같은 코드라면, .filter(published_date__lte=timezone.now())로 필터가 된 객체들을 다시, .order_by('published_date')로 정렬하게 되는 것이다.
