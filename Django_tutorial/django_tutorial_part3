### 첫 번째 장고 앱 작성하기, part3
---

<br>

#### 1. 개요
- view는 django 어플리케이션이 일반적으로 특정 기능과 템플릿을 제공하는 웹페이지의 한 종류이다.

- 각 view는 간단한 python 함수(혹은 클래스를 사용할 경우엔 메소드)를 사용하여 작성

- view를 URL에서 얻기 위해, django는 URLconfs를 사용하는데, URLconfs는 URL 패턴(정규 표현식)과 view를 연결한 것

<br>

#### 2. 조금 더 view 작성하기

- polls/views.py에 detail, results, vote 함수 추가
```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

<br>

- polls/urls.py에 url()함수 호출을 추가하여 새로 작성된 view들을 연결
```
from django.conf.urls import url

from . import views

urlpatterns = [
    # ex: /polls/
    url(r'^$', views.index, name='index'),
    # ex: /polls/5/
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

<br>

- http://127.0.0.1:8000/poll/34 및 http://127.0.0.1:8000/poll/34/results , http://127.0.0.1:8000/poll/34/vote 를 통해 결과 확인

<br>

- 동작 과정
	- 사용자가 웹사이트의 페이지를 요청 (예. /polls/34)
	- django는 mysite.urls 모듈을 불러와서 urlpatterns 변수를 찾아, 정규표현식을 순서대로 따라감
	- '^polls/'를 찾은 후, 일치하는 문자열("polls/")를 버리고 이후 남은 문자인 "34"를 polls.urls URLconf에게 전달하여 남은 처리를 진행
	- 이제 r'^(?P<question_id>[0-9]+)$'에 패턴이 일치하고, 그 결과로 detatil()함수가 호출

<br>

#### 3. view가 실제로 뭔가 하도록 만들기

- view가 하는 역할
	- 데이터베이스의 레코드를 읽는다.
	- django나 python에서 서드파티로 제공되는 템플릿 시스템을 사용할 수 있다.
	- PDF를 생성하거나 XML 출력을 하거나 ZIP 파일을 만든다.
	- view는 무엇이든 python의 어떤 라이브러리라도 사용할 수 있다.

- project의 TEMPLATES 설정에는 django가 어떻게 template를 불러오고 렌더링 할 것인지 서술되어 있다.
- 기본 설정 파일은 APP_DIRS 옵션이 True로 설정된 DjangoTemplates 백엔드를 구성
- 관례에 따라, Django Templates는 각 INSTALLED_APPS 띠렉토리의 "template' 하위 디렉토리를 탐색

- templates > polls 생성 > index.html 생성: polls > templates > polls > index.html
	템플릿 네임스페이싱: polls/templates/polls가 아닌 polls/templates라고 만들 경우, Django는 이름이 일치하는 첫 번째 templates를 선택하므로 동일한 template이름이 다른 어플리케이션에 있을 경우 Django는 이 둘 간의 차이를 구분하지 못하므로 정확하게 polls/templates/polls 라고 하는 것이 좋다.

- 새로운 index() view를 호출했을 때, 시스템에 저장된 최소한 5개의 투표 질문이 콤마로 분리되어 발행일에 따라 출력되도록 하기

-- polls/templates/polls/index.html에 다음 입력
```
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```

-- polls/views.py에 다음 추가
```
from django.http import HttpResponse
from django.template import loader

from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```
** 이 코드는 polls/index.html template를 불러온 후, context를 전달한다.**
** context는 template에서 쓰이는 변수명과 python의 객체를 연결하는 사전형 값이다.**

<br>

#### 3-1. 지름길 render()
- template에 context를 채워넣어 표현한 결과를 HttpResponse객체와 함께 돌려주는 구문은 자주 사용하는 용법이므로 Django는 이런 표현을 쉽게 표현할 수 있도록 다음과 같은 단축기능을 제공한다.
-- polls/views.py 변경
```
from django.shortcuts import render
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```
-- 모든 views에 적용한다면, 더 이상 loader와 HttpResponse를 import 하지 않아도 된다. (만약 detail, results,vote에서 stub 메소드를 가지고 있다면, HttpREsponse를 유지해야 할 것이다.)

-- render() 함수는 request 객체를 첫 번째 인수로 받고, template 이름을 두 번째 인수로 받으며 context 사전형 객체를 선택적(Optional) 인수로 받는다. 인수로 지정된 context로 표현된 template의 HttpResponse 객체가 반환된다.

<br>

#### 4. 404에러 일으키기
- view에 요청된 질문의 ID가 없을 경우 Http404 예외를 발생시킴
-- polls/views.py
```
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```

-- polls/templates/polls/detail.html에 다음 작성
```
{{ question }}
```

<br>

#### 4-1. 지름길 get_object_or_404()
-detail() view 단축 기능으로 Http404 예외 발생시키기
-- polls/views.py에 다음으로 수정
```
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```
-- get_object_or_404() 함수는 Django 모델을 첫 번째 인자로 받고, 몇 개의 키워드 인수를 모델 관리자의 get() 함수에 넘긴다. 만약 객체가 존재하지 않을 경우, Http404 예외를 발생시킨다.

<br>

#### 5. Template 시스템 사용하기
- polls/templates/polls/detail.html
```
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```
- template 시스템은 변수의 속성에 접근하기 위해 점-탐색(dot-lookup) 문법을 사용한다.
- 예제의 {{ question.question_text }} 구문을 보면, Django는 먼저 question 객체에 대해 사전형으로 탐색한다. 탐색에 실패하기 되면 속성값으로 탐색한다. 만약 속성 탐색도 실패한다면 리스트의 인덱스 탐색을 시도한다.

- {% for} 반복 구문에서 메소드 호출이 일어난다. question.choice_set.all은 python에서 question.choice_set.all() 코드로 해석되는데, 이 때 반환된 Choice 객체의 반복자는 {% for %} 에서 사용하기 적당하다.

<br>

#### 6. Template에서 하드코딩된 URL 제거하기
- polls/index.html template에 링크를 적으면, 이 링크는 다음과 같이 부분적으로 하드코딩된다.
`<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>`
- 이러한 하드코딩된 강한 결합 접근법의 문제는 수많은 template을 가진 project의 URL을 바꾸는게 어려운 일이 된다는 점이다.
- 그러나, 이미 polls.urls 모듈의 url() 함수에서 인수의 이름을 정의했으므로 이들 {% url %} template태그에서 사용하여 특정 URL 경로의 의존성을 제거할 수 있다.

** 무슨 소린지 몰라서 일단 패스. 공부하고 다시 볼 것 **

<br>

#### 7. URL의 이름공간(namespace) 나누기
- 한 Project에서 여러개의 app을 사용할 경우, 이 app들의 URL을 구별하는 방법
	: URLconf에 이름공간(namespace)을 추가한다. polls/urls.py 파일에 app_name을 추가하여 어플리케이션의 이름공간을 설정할 수 있다.

-- polls/urls.py
```
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
    url(r'^$', views.index, name='index'),
    url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

이제 polls/templates/polls/index.html을 살펴보면 다음과 같이 바뀐다.
```
<li><a href="{% url **'detail' question.id %}">{{ question.question_text }}</a></li>
```
```
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```