### 첫 번째 장고 앱 작성하기, part2
---
<br>
models.py의 내용이 변경될 경우 "python manage.py makemigrations"로 변경내용을 migrations폴더에 파이썬 파일로 저장
"python manage.py migrate"로 변경내용을 데이터베이스에 적용

#### 1. 데이터베이스 설치
- 기본적으로는 SQLite를 사용하도록 구성되어 있다. 별도 설치 필요 없음
- 다른 데이터베이스를 사용하고 싶다면, http://django-document-korean.readthedocs.io/ko/latest/topics/install.html#database-installation 참고

<br>

#### 2. 모델 만들기
- 모델: 저장하는 데이터의 필수적인 필드들과 동작들을 포함
- 설문조사앱은 Question과 Choice라는 두 개의 모델을 가짐
- Question 모델은 quesiont과 publication date 두 개의 필드를 가짐
- Choice 모델은 choice와 vote 두 개의 필드를 가짐

polls/models.py 파일
```
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

- 각 모델은 django.db.model.Model이라는 클래스의 서브클래스로 표현
- 각 모델은 몇 개의 클래스 변수를 가지고 있으며, 각각의 클래스 변수들은 모델의 데이터베이스 필드를 나타냄
- 데이터베이스의 각 필드는 Field 클래스의 인스턴스로 표현
	- CharField는 문자(character)필드를 표현하고
	- DateTimeField는 날짜와 시간(datetime) 필드를 표현
	- 이것은 각 필드가 어떤 자료형을 가질 수 있는지를 django에 전달
- 몇몇 Field 클래스들은 필수 인수가 필요
	- 예를 들어, CharField의 경우 max_length 인수가 필요
- 또한 Field는 다양한 선택적 인수를 가질 수 있음
	- 예를 들어, default로 하여금 vote의 기본값을 0으로 설정
	
- app을 현재 project에 포함시키기 위해서는 app의 구성 클래스에 대한 참조를 INSTALLED_APPS 설정에 추가해야 한다.
mysite/settings.py의 INSTALLED_APPS에 `polls.apps.PollsConfig' 추가

- polls app 추가 확인
```
➜  mysite python manage.py makemigrations polls
Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
```
makemigrations를 실행시킴으로서, 모델을 변경(또는 생성) 시킨 사실과 이 변경사항을 migration으로 저장하고 싶다는 것을 django에 알려줌

- migration은 django가 모델의 변경사항을 저장하는 방법으로 디스크상의 파일로 존재함
`python manage.py sqlmigrate polls 0001`을 통해 migration 확인
```
BEGIN;
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
    "id" serial NOT NULL PRIMARY KEY,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL
);
--
-- Create model Question
--
CREATE TABLE "polls_question" (
    "id" serial NOT NULL PRIMARY KEY,
    "question_text" varchar(200) NOT NULL,
    "pub_date" timestamp with time zone NOT NULL
);
--
-- Add field question to choice
--
ALTER TABLE "polls_choice" ADD COLUMN "question_id" integer NOT NULL;
ALTER TABLE "polls_choice" ALTER COLUMN "question_id" DROP DEFAULT;
CREATE INDEX "polls_choice_7aa0f6ee" ON "polls_choice" ("question_id");
ALTER TABLE "polls_choice"
  ADD CONSTRAINT "polls_choice_question_id_246c99a640fbbd72_fk_polls_question_id"
    FOREIGN KEY ("question_id")
    REFERENCES "polls_question" ("id")
    DEFERRABLE INITIALLY DEFERRED;

COMMIT;
```
-- 테이블 이름은 app이름과 모델의 이름(소문자) 조합으로 자동 생성(변경 가능)
	polls_choice 와 polls_question

 -- 기본키(Primary, ID)는 자동으로 추가(변경 가능)
 
 -- 관례에 따라, django는 외래키(foreign key) 필드명에 "_id" 이름을 자동으로 추가(변경 가능)
 
 -- 외래키 관계는 FOREIGN KEY 제약이 명시적으로 생성
 
-- 사용하는 데이터베이스에 따라, 데이터베이스 고유의 필드타입이 조정. 따라서, 자동 증가 필드를 생성할 경우 auto_increment(MySQL), serial(PostgreSQL), integer primary key qutoincrement(SQLite)와 같이 사용하는 데이터베이스에 따라 적절한 필드타입이 자동으로 선택
 
-- sqlmigrate 명령은 실제로 데이터베이스의 migration을 실행하지 않음. 이 명령은 단순히 결과만 출력할 뿐이며 django가 필요로 하는 SQL이 무엇인지 확인 가능
 
 - 데이터베이스에 모델과 관련된 테이블 생성
 	`python manage.py migrate`
 	migrate 명령은 아직 적용되지 않은 모든 migration들을 수집하여 이를 실행
 
 - 모델만들기 정리
 	- models.py 에서 모델을 변경
 	- python manage.py makemigrations 을 통해 이 변경사항에 대한 migration 생성
 	- python manage.py migrate 명령을 통해 변경사항을 데이터베이스에 적용

<br>

#### 3. API 가지고 놀기
- python 쉘 접속 `$ python manage.py shell`
	manage.py에 설정된 DJANGO_SETTING_MODULE 환경변수 적용. 즉, django에서 동작하는 모든 명령을 대화식 python쉘에서 그대로 시험

```
>>> from polls.models import Question, Choice
>>> Question.objects.all()
<QuerySet []>
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2017, 2, 5, 9, 32, 19, 687323, tzinfo=<UTC>)
>>> q.question_text="What's up?"
>>> q.save()
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
>>> q.question_text
"What's up?"
```

- polls/models.py 변경
```
import datetime

from django.db import models
from django.utils import timezone

# Create your models here.

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    def __str__(self):
        return self.question_text
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __str__(self):
        return self.choice_text
```
** 하단 실패 **

<br>

####4. Admin 모듈
- 관리자 생성
```
python manage.py createsuperuser
Username: admin
Email address: 이메일 입력
Password: 비번 입력
Password (again): 비번 다시 입력

python manage.py runserver
```

http://127.0.0.1:8000/admin/ 접속하면 관리자 페이지 접속 가능

- 관리 사이트에서 poll app 변경가능하도록 만들기
- polls/admin.py 에서 다음 코드 입력
```
from django.contrib import admin
from .models import Question
admin.site.register(Question)
```
