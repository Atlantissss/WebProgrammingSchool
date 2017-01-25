### day13. Python1
---
#### 1. 설치
- pyenv
프로젝트별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리

- virtualenv
파이썬 개발환경을 프로젝트별로 분리해서 관리할 수 있도록 도와주는 라이브러리
-- pyenv와 다른점은 pyenv는 파이썬의 버전을 관리, virtualenv는 파이썬 패키지 설치 환경을 관리
 
- pyenv-virtualenv
pyenv 제작자가 pyenv를 사용할 경우 쉽게 virtualenv를 사용할 수 있도록 만든 라이브러리

- 설치
    - pyenv
    curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
    
    - vim
    sudo apt-get install vim
    
    - zsh
	sudo apt-get install zsh
	curl -L http://install.ohmyz.sh | sh
	chsh -s `which zsh`

    - pyenv 설정: vim ~/.zshrc 접속 후,

			export PATH="$HOME/.pyenv/bin:$PATH"
			eval "$(pyenv init -)"
			eval "$(pyenv virtualenv-init -)"

    - 파이썬 설치 전 필요 패키지 설치
	https://github.com/yyuu/pyenv/wiki/Common-build-problems

    - pyenv를 사용한 파이썬 3.4.3버전 설치
	pyenv install 3.4.3

    - 가상환경 생성
    	pyenv virtualenv `<env name>`

     - 사용할 폴더로 이동
     	cd projects
     	mkdir python
     	cd python
     	
    - local에 가상환경 지정
     	pyenv local fc-python
<br>

- iPython
기본 파이썬 셸보다 다양한 기능을 사용할 수 있도록 해주는 셸을 제공

    - 설치
    pip 업데이트
    pip install ipython
    커맨드라인에서 ipython 실행

<br>

#### 2. 시작해보기

- 리터럴
변하지 않는 고정된 데이터 자체의 표현
	- 5 (정수형 데이터)
	- "Fastcampus" (문자열 데이터)
	- 1.4937 (부동소수점 데이터)

- 표현식(Expression)
값을 의미하는 표현 또는 값을 반환하는 표현
```
	>>> sec = 60
	>>> 365*24*sec # 표현식
	525600		   # 정수 525600의 리터럴 값
```

- 구문
값의 의미를 지니지 않으며, 어떠한 목적을 수행하는 코드
```
	>>> for char in '안녕하세요':	# 구문(제어문)
	...	print(char)
	...
```
 
 <br>
 
#### 3. 변수

- 파이썬은 모든 것(정수, 문자열, 함수 등)이 객체(Object)로 이루어짐

- 객체는 데이터의 형태를 결정해주는 타입으로, 파이썬에서 객체 타입 변경 불가

- 프로그래머는 변수를 선언하고 사용하는 형태로 컴퓨터의 메모리에 값을 할당하고 참조할 수 있음

- 파이썬에서는 값을 할당할 때 = 기호를 사용
** 일반적으로, 프로그래밍 언어에서 같다(Equal)의 의미는 =이 아닌 ==이 담당한다. *

- var1 변수에 100의 정수를 할당하고 print를 이용해 var1변수 값을 출력하는 코드
```
>>> var = 100
>>> print(var)
100
```
   - 변수는 단지 이름일 뿐, 그 자체가 어떤 값을 갖지 않음
	위의 경우 var1 변수는 100이란 데이터를 직접 가지는 것이 아니라, 100이라는 정수형 객체가 있고 var1은 단순히 해당 객체를 참조하는 역할

    - 예시
a = 35
b = a
a = a + 1
했을 때, a는 36이 나오지만, b는 여전히 35를 가리킴

    - Why?
	35라는 변수가 RAM의 어딘가에 저장되어 있는데 a가 이 35를 가리킴 그리고 b는 a가 가리키는 35를 가리킴. 이 때 a를 바꿔도 b는 여전히 35를 가리킴


    - id를 통해 확인 가능
-- id는 어떠한 변수가 참조하고 있는 객체가 메모리 상에서 가지고 있는 고유의 주소(id)를 출력
-- a = 89273 , b = 89273 을 하고 id(a), id(b)를 하면 다른 id주소를 가리킴
-- 작은 숫자들은 같은 id를 가리키는데 그 이유는 자주 쓰는 숫자들이라서 id값이 고정되어 있음

- 변수의 타입 확인
	- 방법
	-- 내장함수 type 사용: `type(var1)` 또는 `type('안녕하세요')`
	-- str: 문자열

	- 변수의 사용가능한 문자
	-- 소문자
	-- 대문자
	-- 숫자
	-- 언더스코어(_)
	-- 한글(파이썬3.0이상)
	**숫자로 시작할 수 없으며, 언더스코어로 시작하는 변수명은 특별한 처리방법을 따르므로 일반적으로 사용하지 않음*

- 변수의 입력과 출력
	-입력
	`var = input('숫자를 입력해주세요 : ')`

	- 출력
	`print(var)`

<br>

#### 4. 숫자
- 수학 연산자

| 연산자 | 설명 | 예 | 결과 |
|:------:|:----------:|:------:|:----:|
| + | 더하기 | 32 + 7 | 39 |
| - | 빼기 | 82 - 2 | 80 |
| * | 곱하기 | 3 * 7 | 21 |
| / | 나누기 | 7 / 2 | 3.5 |
| // | 정수나누기 | 7 // 2 | 3 |
| % | 나머지 | 7 % 3 | 1 |
| ** | 지수 | 2**10 | 1024 |

- 산술연산 예시
```
>>> a = 100
>>> result = a - 3
>>> a = result
>>> print(a)
97
```
줄여서
```
>>> a = 100
>>> a = a - 3
>>> print(a)
97
```
더 줄여서
```
>>> a = 100
>>> a -= 3
>>> print(a)
97
```

- 진수(base)
	- 2진수(binary): 0b 또는 0B로 시작
	- 8진수(octal): 0o 또는 0O로 시작
	- 16진수(hex): 0x 또는 0X로 시작

	예)
```
>>> 10
10
>>> 0b10
2
>>> 0o10
8
>>> 0x10
16
```

- 형변환
	내장함수 int, float를 사용
```
>>> int("35")
35
```

#### 5. 문자열
- 문자열 표현
작은 따옴표 또는 큰 따옴표
```
>>> '패스트캠퍼스'
'패스트캠퍼스'
>>> "패스트캠퍼스"
'패스트캠퍼스'
```

사용하지 않은 인용 부호는 문자열 내부에서 사용 가능
```
>>> '패스트 캠퍼스 "웹 프로그래밍 스쿨"'
'패스트캠퍼스 "웹 프로그래밍 스쿨"'
```

- 장문 표현
세 개의 작은 따옴표 또는 큰 따옴표
```
>>> ''' 소환사 여러분.
...
... 7.1 패치를 소개합니다.
...
... 앞으로 있을 여러 번의 패치에 대해서는 차차...
... 하지만 그렇다고 이번 패치가 하향...
... 정의의 전장에서 승리를 기원합니다. '''
```

- 문자열 더하기
```
>>> notice = ''
>>> notice += '공지사항'
>>> notice += '(패치노트)'
>>> notice += ' : 7.1 패치노트'
>>> notice
'공지사항(패치노트): 7.1 패치노트'
```

- 형 변환
내장함수 str을 사용
```
>>> str(147)
'147'
```
문자열을 제외한 객체를 `print` 함수로 호출하면, 내부적으로 str 함수를 사용한 결과를 나타냄

- 이스케이프 문자
특수문자, 또는 특별한 역할을 하는 의미를 나타내는 문자

| 이스케이프 문자 | 설명 |
|-----------------|--------------------|
| \a | 비프음 발생 |
| \t | 탭(tab) |
| \n | 줄바꿈 |
| \ | \(역슬래시) 입력 |
| \ | 작은따옴표(') 입력 |
| \" | 큰따옴표(") 입력 |

- 인덱스 연산
문자열에서 문자를 추출
가장 왼쪽은 0, 가장 오른쪽은 -1로 시작
```
>>> lux = '빛으로 강타해요!'
>>> lux[0]
'빛'
>>> lux[-1]
'!'
```
문자열은 불변이므로 인덱싱한 부분에 새 값 대입 불가
```
>>> lux[0] = '손'
TypeError
```

- 슬라이스 연산
[start:end:step] 형식 사용

	- [:]
	처음부터 끝까지
	
	- [start:]
	start오프셋부터 마지막까지
	
	- [:end]
	처음부터 end오프셋까지
	
	- [start:end]
	start오프셋부터 end오프셋까지
	
	- [start:end:step]
	start오프셋부터 end오프셋까지, step만큼씩 뛰어넘은 부분

- 길이
내장함수 **len** 사용

- 문자열 나누기(split)
내장함수 **split** 사용
**split**함수에 인자로 주어진 **구분자**를 기준으로 하나의 문자열을 리스트 형태로 반환
```
>>> girlsday = "민아,유라,소진,혜리"
>>> girlsday.split(',')
['민아', '유라', '소진', '혜리']
```
**split**함수에 인자를 주지 않을 겨우, 공백문자를 구분자로 사용

- 문자열 결합(join)
**split**함수와 반대의 역할
문자열 리스트를 하나의 문자열로 결합
각 문자열을 결합해 줄 문자열을 지정한 후 **join**함수로 결합
```
>>> girlsday_list = girlsday.split(',')
>>> girlsday_str = ','.join(girlsday_list)
>>> print(girlsday_str)
민아, 유라, 소진, 혜리
```

- 대소문자 다루기
```
>>> lux = 'lux, the Lady of Luminosity'
>>> lux.capitalize()					#문장의 첫 단어만 대문자
'Lux, the lady of luminosity'
>>> lux.title()							# 각 단어의 첫 글자만 대문자
'Lux, The Lady Of Luminosity'
>>> lux.upper()							# 모든 글자 대문자
'LUX, THE LADY OF LUMINOSITY'
>>> lux.lower()							# 모든 글자 소문자
'lux, the lady of luminosity'
>>> lux. swapcase()						# 모든 글자 역변환
'LUX, THE lADY OF lUMINOSITY'
```

- 문자열 포맷: 옛 스타일(%)

	`string % data`
	
| 변환타입 | 설명 |
|:--------:|:-------------------------------------------------:|
| %s | 문자열 |
| %d | 10진수 |
| %x | 16진수 |
| %o | 8진수 |
| %f | 10진 부동소수점수 |
| %e | 지수로 나타낸 부동소수점수 |
| %g | 10진 부동소수점수 혹은 지수로 나타낸 부동소수점수 |
| %% | 리터럴 % |

```
>>> '%s' % 42
'42'
>>> '%d x %d : %d' %(3, 4, 12)
'3 x 4 : 12'
```

- 문자열 포맷: 정렬
%[정렬기준(-,없음)][전체글자수].[문자길이 또는 소수점 이후 문자길이][변환타입]
```
>>> d = 37
>>> f = 3.14
>>> s = 'Fastcampus'
>>> '%d %f %s' % (d, f, s)
'37 3.140000 Fastcampus'
>>> '%10d %10f %10s' % (d, f, s)
'        37   3.140000 Fastcampus'
>>> '%14d %14f %14s' % (d ,f ,s)
'            37       3.140000     Fastcampus'
>>> '%-14d %-14f %-14s' % (d, f, s)
'37             3.140000       Fastcampus    '
>>> '%-14.3d %-14.3f %-14.3s' % (d, f, s)
'037            3.140          Fas           '
```

- 문자열 포맷: 새 스타일( {}. format)

`{}.format(변수)`

```
# 기본형태
>>> '{} {} {}'.format(d, f, s)
'37 3.14 Fastcampus'

# 각 인자의 순서를 지정
>>> '{1} {2} {0}'.format(d, f, s)
'3.14 Fastcampus 37'

# 각 인자에 이름을 지정
>>> '{d} {f} {s}'.format(d=50, f=1.432, s='WPS')
'50 1.432 WPS'

# 딕셔너리로부터 변수 할당
>>> dict = {'d': d, 'f': f, 's': s}
>>> '{0[d]} {0[f]} {0[s]} {1}'.format(dict, 'WPS')

# 타입 지정자 입력
>>> '{:d} {:f} {:s}'.format(d, f, s)
'37 3.140000 Fastcampus'
	
# 이름과 타입지정자를 모두 사용
>>> '{digit:d} {float:f} {string:s}'.format(digit=700, float=1.4323, string='Welcome')
'700 1.432300 Welcome'	

# 필드길이 10, 우측정렬
>>> '{:10d}'.format(d)
'        37'
>>> '{:>10d}'.format(d)
'        37'

# 필드길이 10, 좌측정렬
>>> '{:<10d}'.format(d)
'37        '

# 필드길이 10, 가운데 정렬
>>> '{:^10d}'.format(d)
'    37    '

# 필드길이 10, 가운데 정렬, 빈 공간은 ~로 채움
>>> '{:~^10d}'.format(d)
'~~~~37~~~~'	
```

<br>

#### 6. 시퀀스
- 설명
	-- 파이썬에 내장된 시퀀스 타입: 문자열, 리스트, 튜플
	-- 문자열은 인용부호(작은따옴표, 큰따옴표) 사용
	-- 리스트는 대괄호[] 사용
	-- 튜플은 괄호() 사용

- 리스트
리스트는 순차적인 데이터를 나타내는데 유용하며, 문자열과는 달리 내부 항목 변경 가능

	- 리스트 생성
```
>>> empty_list1 = []
>>> empty_list2 = list()
>>> sample_list = ['a', 'b', 'c', 'd']
>>> sample_list2 = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

- 	- 다른 데이터를 리스트로 변환
list 함수 사용
```
>>> list('League of legends')
['L', 'e', 'a', 'g', 'u', 'e', ' ', 'o', 'f', ' ', 'l', 'e', 'g', 'e', 'n', 'd', 's']
```

- - 인덱스 연산
```
>>> sample_list2[4]
'May'
>>> sample_list[6]
'Jul'
```
- - 내부항목 변경
```
>>> sample_list[2] = 'C'
['a', 'b', 'C', 'd']
```

- - 슬라이스 연산
```
>>> quarters = sample_list2[::3]
['Jan', 'Apr', 'Jul', 'Oct']
>>> last_three = sample_list2[9:12]
['Jan', 'Apr', 'Jul', 'Oct']
>>> reverse_two_steps = sample_list2[::-2]
['Dec', 'Oct', 'Aug', 'Jun', 'Apr', 'Feb']
```

- - 리스트 항목 추가(append)
```
>>> sample_list.append('e')
>>> sample_list
['a', 'b', 'c', 'd', 'e']
```

- - 리스트 병합(extend, +=)
```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
또는
>>> fruits += colors
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

- extend 대신 append를 쓰면?
```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.append(colors)
['apple', 'banana', 'melon', ['red', 'green', 'blue']]
```
- -특정 위치에 리스트 항목 추가(insert)
```
>>> fruits.insert(0, 'mango')
>>> fruits
['mango', 'apple', 'banana', 'melon']
>>>
>>> fruits.insert(99. 'pineapple')
>>> fruits
['mango', 'apple', 'banana', 'melon', 'pineapple']
```
-- 5개 항목에 99번째 넣으면 자동으로 6번째에 들어감
-- 또한 5개 항목에 -99번째 넣으면 자동으로 맨 앞에 들어감

- -특정 위치 리스트 항목 삭제(del)
```
>>> del fruits[0]
```
-- del은 리스트 함수가 아닌, 파이썬 구문이므로 `del <리스트>[오프셋] 형식을 사용`

- - 값으로 리스트 항목 삭제(remove)
```
>>> fruits.remove('mango')
```

- - 리스트 항목 추출 후 삭제(pop)
```
>>> fruits.pop(오프셋)
```

- - 값으로 리스트 항목 오프셋 찾기(index)
```
>>> fruits.index('red')
```

- -존재여부 확인(in)
```
>>> 'red' in fruits
True
```

- - 값 세기(count)
```
>>> fruits.count('red')
```

- - 정렬하기
	-- sort: 리스트 자체를 정렬
	-- sorted: 원본은 두고 복사본을 정렬하여 반환
	
- 튜플
튜플은 리스트와 비슷하나, 정의 후 내부 항목의 삭제나 수정이 불가능
```
>>> empty_tuple = ()
>>> colors = 'red'
>>> fruits = 'apple', 'banana'
```
튜플을 정의 할 때 괄호가 없어도 무관하나, 괄호로 묶는것이 좀 더 튜플임을 구분하기 좋음
튜플의 요소가 1개일 때는 요소의 뒤에 쉼표(,)를 붙임

<br>

#### 7. 딕셔너리, 셋

#### 딕셔너리
Key-Value 형태로 항목을 가지는 자료구조

- 딕셔너리 생성
```
>>> empty_dict1 = {}
>>> empty_dict2 = dict()
>>> champion_dict = {
... 'Lux': 'the Lady of Luminosity',
... 'Ahri': 'the Nine-Tailed Fox',
... 'Ezreal': 'the Prodigal Explorer',
... 'Teemo': 'the Swift Scout',
... }
```

- 형 변환
dict 함수를 사용하여 두 값의 시퀀스(리스트 또는 튜플)을 딕셔너리로 변환
```
>>> sample = [[1,2], [3,4], [5,6]]
>>> dict(sample)
{1: 2, 3: 4, 5: 6}
```

- 항목 찾기/추가/변경 [key]
```
>>> champion_dict['Lux']								# 찾기
'the Lady of Luminosity'
>>> champion_dict['Sona'] = 'Maven of the Strings'		# 추가
>>> champion_dict['Lux'] = 'Demacia'					# 변경
```

- 결합(update)
```
>>> item_dict = {
... 'Doran\'s Ring' : 400,
... 'Doran\'s Blade': 450,
... 'Doran\'s Shield': 450m
... }
>>> com_dict = {}
>>> com_dict.update(champion_dict)
>>> com_dict.update(item_dict)
>>> com_dict
```

- 삭제(del)
```
>>> del com_dict['Doran\'s Blade']
>>> del com_dict['Doran\'s Ring']
>>> del com_dict['Doran\'s Shield']
```

- 전체 삭제(clear)
전체 항목을 삭제

- in으로 키 검색
True/False를 반환
`'Lux' in com_dict`

- keys()
모든 키 얻기

- values()
모든 값 얻기

- items()
모든 키-값 얻기(튜플로 반환)

<br>

#### 셋(set)
셋은 키만 있는 딕셔너리와 같으며, 중복된 값 존재 불가

 - 셋 생성
```
>>> empty_set = set()
>>> champions = {'lux', 'ahri', 'ezreal'}
```
 
 - 형 변환
 문자열, 리스트, 튜플, 딕셔너리를 셋으로 변환할 수 있으며, 중복된 값은 사라짐
```
>>> set('ezreal')
{'e', 'z', 'a', 'l', 'r'}
>>> set(champion_dict)
{'Ahri', 'Lux', 'Ezreal', 'Sona', 'Teemo'}
```
 딕셔너리는 키만 남는다.
 
 - 집합 연산

| 연산자 | 설명 |
|:------:|:---------------------------:|
| Shift + \ | 합집합(Union) |
| & | 교집합(Intersection) |
| - | 차집합(Difference) |
| ^ | 대칭차집합(Exclusive) |
| <= | 부분집합(Subset) |
| < | 진부분집합(Proper subset) |
| >= | 상위집합(Superset) |
| > | 진상위집합(Proper superset) |
 
