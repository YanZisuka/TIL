# Django_03

## Model

-   단일한 데이터에 대한 정보를 가짐
    -   사용자가 저장하는 데이터들의 필수적인 필드들과 동작들을 포함
-   저장된 데이터베이스의 구조(layout)
-   웹 애플리케이션의 데이터를 **구조화하고 조작**하기 위한 도구



## Database

-   체계화된 데이터의 모임
-   쿼리(Query)
    -   데이터를 조회 및 조작하기 위한 명령어
    -   조건에 맞는 데이터를 추출하거나 조작하는 명령어



### Database의 기본 구조

#### 스키마(Schema)

-   데이터베이스의 구조와 제약 조건(자료의 구조, 표현 방법, 관계)에 관련한 전반적인 명세를 기술한 것

#### 테이블(Table)

-   열과 행의 모델을 사용해 조직된 데이터 요소들의 집합
    -   열(column): 필드(field) or 속성(attribute)
    -   행(row): 레코드(record) or 튜플(tuple)

#### 기본키(Primary Key)

-   각 행의 고유값으로, 반드시 설정하여야 하며, 데이터베이스 관리 및 관계 설정시 주요하게 활용된다



## ORM

-   **Object-Relational-Mapping**
-   **객체 지향 프로그래밍 언어**를 사용하여 호환되지 않는 유형의 시스템 간에(django - **SQL**)데이터를 **변환**하는 **프로그래밍** 기술
-   OOP 프로그래밍에서 **RDBMS**을 연동할 때, 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법
-   django는 내장 django ORM을 사용함
-   장점
    -   SQL을 잘 알지 못해도 DB 조작이 가능
    -   SQL의 절차적 접근이 아닌 객체 지향적 접근으로 인한 높은 생산성
-   단점
    -   ORM만으로 완전한 서비스를 구현하기 어려운 경우가 있을지도 모른다
-   현대 웹 프레임워크의 요점은 웹 개발의 속도를 높이는 것 (**생산성**)



## Django Model

```
$ pip install django_extensions  // settings.py에 'django_extensions' 추가
$ pip install django_extensions ipython
```



### Table 생성/수정 반영

```
$ python manage.py makemigrations [app_name]
$ python manage.py migrate [app_name]
$ python manage.py sqlmigrate practice 0001
```

```
$ python manage.py shell_plus --print-sql
```



### Data C/R/U/D/

#### 1. Create

```python
In [1]: s2 = Student()

In [2]: s2.name = '김철수'

In [3]: s2.age = 30

In [4]: s2.major = '경영학'

In [5]: s2.save()
```

```python
In [6]: s3 = Student(name='이영희', age=30, major='컴퓨터공학')      

In [7]: s3.name
Out[7]: '이영희'

In [8]: s3.save()
INSERT INTO "practice_student" ("name", "age", "major")
VALUES ('이영희', 30, '컴퓨터공학')

Execution time: 0.008992s [Database: default]
```

```python
In [10]: Student.objects.create(name='박짱구', age=20, major='수학') 
    ...: 
INSERT INTO "practice_student" ("name", "age", "major")
VALUES ('박짱구', 20, '수학')

Execution time: 0.007995s [Database: default]
```

#### 2. Read

```
1. 전체 목록
Student.objects.all()

2. 단일 조회
Student.objects.get(id=1)
```

#### 3. Update

```
s = Student.objects.get(pk=3)
s.is_married = True
s.save()
```

#### 4. Delete

```
s = Student.objects.get(pk=3)
s.delete()
```



### Admin

```
$ python manage.py createsuperuser
```

#### [app_name]/admin.py

```python
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```



### 오름차순 정렬

```python
articles = Article.objects.all()[::-1]		# Python이 추가적 메모리를 사용해 정렬
articles = Article.objects.order_by('-pk')  # DB 엔진이 정렬
articles = Article.objects.order_by('-updated_at')
```

-   DB는 **데이터를 다루는 데 특화**되어있으므로, **최대한 DB가 데이터를 조작하도록 명령하는 것이 성능상 이점을 가져갈 수 있다.**



## CSRF(Cross Site Request Forgery)

-   **CSRF(Cross Site Request Forgery)**는 특정 사용자를 대상으로 하지 않고, 불특정 다수를 대상으로 로그인된 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록, 송금 등)를 하게 만드는 공격이다.
-   즉, `CSRF token`을 통해 올바른 사이트에서 들어온 요청인지 판단한다.

