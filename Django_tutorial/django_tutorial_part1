###첫 번째 장고 앱 작성하기, part1
---
<br>

#### 1. mysite 디렉토리 생성
```
➜  tutorial django-admin startproject mysite
➜  tutorial ls
mysite
```
<br>

#### 2. mysite 디렉토리 내 정보
```
➜  mysite tree
.							# 디렉토리 바깥의 디렉토리는 단순히 프로젝트를 담는 공간
├── manage.py			# Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드 라인의 유틸리티
└── mysite				# 디렉토리 내부에는 project를 위한 실제 python 패키지들이 저장
    ├── __init__.py		# python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 용도의 단순한 빈 파일
    ├── settings.py		# 현재 django project의 환경/구성을 저장
    ├── urls.py			# 현재 django project의 URL선언을 저장
    └── wsgi.py			# 현재 progject를 서비스하기 위한 WSGI 호환 웹 서버의 진입점

1 directory, 5 files
```

<br>

#### 3. 개발 서버 동작 확인
```
➜  mysite **python manage.py runserver**
Performing system checks...

System check identified no issues (0 silenced).

You have 13 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

February 05, 2017 - 07:20:05
Django version 1.10.5, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
[05/Feb/2017 07:21:59] "GET / HTTP/1.1" 200 1767
Not Found: /favicon.ico
[05/Feb/2017 07:22:00] "GET /favicon.ico HTTP/1.1" 404 1936
```

** 절대로 개발 서버를 운영 환경에서 사용하면 안된다. 개발 서버는 오직 개발 목적으로만 사용해야 한다. **

** 웹 브라우져에서 http://127.0.0.1:8000/ 을 통해 접속 확인 **

<br>

#### 4. 포트변경
```
$ python manage.py runserver 8080
```

<br>

#### 5. 서버IP 변경
```
$ python manage.py runserver 0.0.0.0:8000

```

<br>

#### 6. 설문조사 앱 만들기
- 앱 vs 프로젝트
	앱: 블로그나 공공 기록물을 위한 데이터베이스나 간단한 설문조사 앱과 같이 특정한 기능을 수행하는 웹 어플리케이션
	프로젝트: 이런 특정 웹 사이트를 위한 앱들과 각 설정들을 한데 묶어 놓은 것
	So, 프로젝트는 다수의 앱을 포함할 수 있고, 앱은 다수의 프로젝트에 포함될 수 있다.

**순서**
1. mysite/polls app 생성 `python manage.py startapp polls`
2. mysite/polls/view.py에 파이썬 코드 입력
3. mysite/polls/urls.py 생성: view를 호출하기 위해 이와 연결된 url 생성
4. mysite/urls.py 생성: polls.urls 모듈을 바라보게 설정
5. 동작 확인 `python manage.py runserver`
6. 브라우져 확인 http://localhost:8000/polls/

1. polls app 생성
- app 생성
```
➜  mysite python manage.py startapp polls		# python manage.py startapp polls를 통해 polls 디렉토리 생성
➜  mysite ls
db.sqlite3  manage.py  mysite  polls
➜  mysite tree polls
polls											# poll 어플리케이션의 집
├── __init__.py
├── admin.py
├── apps.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py

1 directory, 7 files
```

2. polls/views.py에 다음 입력
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

3. polls/urls.py에 다음 입력
```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

4. mysite/urls.py에 다음 입력
```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```
- include() 함수는 다른 URLconf를 참조할 수 있도록 도와준다.
- include() 함수를 위한 정규 표현식에서 끝을 의미하는 기호로 $ 대신 슬래시(/)를 사용
- django가 include()를 만나게 되면, 그 시점까지 일치하는 URL의 부분을 잘라내고, 남은 부분을 후속처리를 위해 include된 URLconf로 전달

5~6. 확인
- url()에는 4개의 인수가 전달된다. 두 개의 필수 인자로 regex와 view가 있고, 두 개의 옵션 인자로 kwargs와 name이 있다.
- regex 인수
	"regex"는 보통 정규 표현식("Regular Expression"을 짧게 줄여 쓰는 표현이다. 문자열의 패턴을 일치시키는 문법을 말하며, 이 경우에는 url의 패턴을 찾아내는데 사용되었다.

- view 인수
	django에서 일치하는 정규 표현식을 찾아내면, HttpRequest객체를 첫 번째 인수로 하고, 정규표현식에서 "잡힌" 값들을 나머지 인수로 하여 특정한 view 함수를 호출
	
- kwargs 인수
	임의의 키워드 인수들은 목표한 view에 사전형으로 전달된다.

- name 인수
	URL에 이름을 지으면, 템플릿을 포함한 django 어디에서나 명확하게 참조한다. 이 기능을 활용하여 단 하나의 파일만 수정해도 project 내의 모든 URL 패턴을 바꿀 수 있다.