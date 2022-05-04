# Django_08

## HTTP

-   HyperText Transfer Protocol
-   웹 상에서 콘텐츠를 전송하기 위한 약속
-   HTML 문서와 같은 리소스들을 가져올 수 있도록 하는 프로토콜(규칙, 약속)
-   웹에서 이루어지는 모든 데이터 교환의 기초
    -   요청(request)
        -   클라이언트에 의해 전송되는 메시지
    -   응답(response)
        -   서버에서 응답으로 전송되는 메시지
-   기본 특성
    -   stateless
    -   connectionless
-   쿠키와 세션을 통해 서버 상태를 요청과 연결하도록 함



### Request methods

-   자원에 대한 행위(수행하고자 하는 동작)을 정의
-   주어진 리소스(자원)에 수행하길 원하는 행동을 나타냄
-   HTTP Method 예시
    -   GET(조회), POST(작성), PUT(수정), DELETE(삭제)



### Response status codes

-   특정 HTTP 요청이 성공적으로 완료되었는지 여부를 나타냄
-   응답은 5개 그룹으로 나뉘어짐
    -   Informational responses (1xx)
    -   Successful responses (2xx)
    -   Redirection messages (3xx)
    -   Client error responses (4xx)
    -   Server error responses (5xx)



### 웹에서의 리소스 식별

-   HTTP 요청의 대상을 resource라고 함
-   리소스는 문서, 사진 또는 기타 어떤 것이든 될 수 있음
-   각 리소스는 리소스 식별을 위해 HTTP 전체에서 사용되는 URI(Uniform Resource Identifier)로 식별됨

#### URI(Uniform Resource Identifier)

-   통합 자원 식별자
-   인터넷의 자원을 식별하는 유일한 주소 (정보의 자원을 표현)
-   인터넷에서 자원을 식별하거나 이름을 지정하는데 사용되는 간단한 문자열
-   하위 개념
    -   URL, URN
-   URN을 사용하는 비중이 매우 적기 때문에 일반적으로 URL은 URI와 같은 의미

##### URL(Uniform Resource Locator)

-   통합 자원 위치
-   네트워크 상에 자원이 어디 있는지 알려주기 위한 약속
-   과거에는 실제 자원의 위치를 나타냈지만 현재는 추상화된 의미론적 구성
-   '웹 주소', '링크'라고도 불림

##### URN(Uniform Resource Name)

-   통합 자원 이름
-   URL과 달리 자원의 위치에 영향을 받지않는 유일한 이름 역할을 함
-   e.g., ISBN(국제표준도서번호)



## RESTful API

-   REST 원리를 따라 설계한 API
-   RESTful services, 혹은 simply REST services라고도 부름
-   **프로그래밍을 통해 클라이언트의 요청에 JSON을 응답하는 서버를 구성**



### API

-   Application Programming Interface
-   프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
    -   애플리케이션과 프로그래밍으로 소통하는 방법
    -   CLI는 명령줄, GUI는 그래픽, **API는 프로그래밍을 통해 특정한 기능 수행**
-   Web API
    -   웹 어플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세
    -   현재 웹 개발은 모든 것을 직접 개발하기보다 여러 Open API를 활용하는 추세
-   **응답 데이터 타입**
    -   HTML, XML, JSON 등
    -   JSON의 데이터 용량이 XML보다 작기 때문에 표준으로 채택되었다.
-   대표적인 API 서비스 목록
    -   Youtube API, Naver Papago API, Kakao Map API, ...



### REST

-   **RE**presentational **S**tate **T**ransfer
-   API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론
    -   2000년 로이 필딩의 박사학위 논문에서 처음으로 소개된 후 네트워킹 문화에 널리 퍼짐
-   네트워크 구조(Network Architecture) 원리의 모음
    -   자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법
-   REST 원리를 따르는 시스템을 RESTful이란 용어로 지칭함
-   자원을 정의하는 방법에 대한 고민
    -   e.g., 정의된 자원을 어디에 위치시킬 것인가
-   **REST의 자원과 주소의 지정 방법**
    1.   자원
         -   URI
    2.   행위
         -   HTTP Method
    3.   표현
         -   자원과 행위를 통해 궁극적으로 표현되는 (추상화된) 결과물
         -   JSON으로 표현된 데이터를 제공
-   **설계 방법론은 지켰을 때 잃는 것보다 지켰을 때 얻는 것이 훨씬 많음**
    -   단, 설계 방법론을 지키지 않더라도 동작 여부에 큰 영향을 미치지는 않음



### JSON

-   JavaScript Object Notation
    -   JSON is a lightweight data-interchange format
    -   JavaScript의 표기법을 따른 단순 문자열
-   특징
    -   사람이 읽거나 쓰기 쉽고 기계가 파싱하고 만들어내기 쉬움
    -   **파이썬의 dictionary, 자바스크립트의 object처럼 C 계열의 언어가 갖고 있는 자료구조로 쉽게 변화할 수 있는 key-value 형태의 구조를 갖고 있음**



## Response

### JsonResponse

```python
# articles/views.py

from django.http.response import JsonResponse


def article_json(request):
    articles = Article.objects.all()
    articles_json = []
    
    for article in articles:
        articles_json.append(
        	{
            	'id': article.pk,
                'title': article.title,
                ...
        	}
        )
    return JsonResponse(articles_json, safe=False)
```



-   JSON-encoded response를 만드는 HttpResponse의 서브 클래스
-   `safe` parameter
    -   True (default)
    -   dict 이외의 객체를 직렬화(Serialization)하려면 False로 설정

#### Serialization

-   "직렬화"
-   데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고, 나중에 재구성할 수 있는 포맷으로 변환하는 과정
-   Serializers in Django
    -   Queryset 및 Model Instance와 같은 복잡한 데이터를 JSON, XML 등의 유형으로 쉽게 변환할 수 있는 Python 데이터 타입으로 만들어 줌



### Django Serializer

```python
# articles/views.py

from django.http.response import JsonResponse, HttpResponse
from django.core import serializers


def article_json(request):
    articles = Article.objects.all()
    data = serializers.serialize('json', articles)
    return HttpResponse(data, content_type='application/json')
```



-   Django의 내장 HttpResponse를 활용한 JSON 응답 객체
-   주어진 모델 정보를 활용하기 때문에 이전과 달리 필드를 개별적으로 직접 만들어 줄 필요가 없음



### Django REST Framework

```python
# articles/views.py


def article_json(request):
    articles = Article.objects.all()
    serializer = ArticleSerializaer(articles, many=True)
    return Response(serializer.data)
```



-   Django REST framework(DRF) 라이브러리를 사용한 JSON 응답
-   Web API 구축을 위한 강력한 Toolkit을 제공하는 라이브러리

|          |  Django   |    DRF     |
| :------: | :-------: | :--------: |
| Response |   HTML    |    JSON    |
|  Model   | ModelForm | Serializer |



## Single Model

-   단일 모델의 data를 직렬화하여 JSON으로 변환하는 방법에 대한 학습
-   단일 모델을 두고 CRUD 로직을 수행 가능하도록 설계
-   API 개발을 위한 핵심 기능을 제공하는 도구 활용
    -   DRF built-in form
    -   Postman



### ModelSerializer

-   모델 필드에 해당하는 필드가 있는 serializer 클래스를 자동으로 만들 수 있는 shortcut
-   아래 핵심 기능을 제공
    1.   모델 정보에 맞춰 자동으로 필드 생성
    2.   serializer에 대한 유효성 검사기를 자동으로 생성
    3.   `.create() & .update()`의 간단한 기본 구현이 포함됨

#### 'many' argument

-   `many=True`
    -   "Serializing multiple objects"
    -   단일 인스턴스 대신 QuerySet 등을 직렬화하기 위해서는 serializer를 인스턴스화할 때 `many=True`를 키워드 인자로 전달해야 한다.



### 1. GET - Article List

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article


class ArticleListSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title',)
```



-   `api_view` decorator
    -   기본적으로 GET 메서드만 허용되며 다른 메서드 요청에 대해서는 **405 Method Not Allowed로** 응답
    -   View 함수가 응답해야 하는 HTTP 메서드의 목록을 리스트의 인자로 받음
    -   DRF에서는 선택이 아닌 **필수적으로 작성해야** 해당 view 함수가 정상적으로 동작함



### 2. GET - Article Detail

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article


class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = '__all__'
```



-   Article List와 Article Detail을 구분하기 위해 추가 Serializer 정의
-   모든 필드를 직렬화하기 위해 fields 옵션을 '__all__'



### 3. POST - Create Article

-   201 Created 상태 코드 및 메시지 응답
-   RESTful 구조에 맞게 작성
    1.   URI는 자원을 표현
    2.   자원을 조작하는 행위는 HTTP Method
-   `article_list` 함수로 게시글을 조회하거나 생성하는 행위를 모두 처리 가능

#### 'raise_exception' argument

-   "Raising an exception on invalid data"
-   `is_valid()`는 유효성 검사 오류가 있는 경우 `serializers.ValidationError` 예외를 발생시키는 선택적 `raise_exception` 인자를 사용할 수 있음
-   DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며, **기본적으로 HTTP status code 400을 응답으로 반환함**



### Status Codes in DRF

```python
from rest_framework import status


def example_list(request):
    return Response(serializer.data, status=status.HTTP_201_CREATED)
```



-   DRF에는 status code를 보다 명확하고 읽기 쉽게 만드는 데 사용할 수 있는 정의된 상수 집합을 제공
-   `status` 모듈에 HTTP status code 집합이 모두 포함되어 있음



### 4. DELETE - Delete Article

-   204 No Content 상태 코드 및 메시지 응답
-   `article_detail` 함수로 상세 게시글을 조회하거나 삭제하는 행위 모두 처리 가능



### 5. PUT - Update Article

-   `article_detail` 함수로 상세 게시글을 조회하거나 삭제, 수정하는 행위 모두 처리 가능



## 1:N Relation

-   1:N 관계에서의 모델 data를 직렬화(serialization)하여 JSON으로 변환하는 방법에 대한 학습
-   2개 이상의 1:N 관계를 맺는 모델을 두고 CRUD 로직을 수행 가능하도록 설계하기



### 3. POST - Create Comment

-   Article 생성과 달리 Comment 생성은 생성 시에 참조하는 모델의 객체 정보가 필요
    -   1:N 관계에서 N은 어떤 1을 참조하는지에 대한 정보가 필요하기 때문 (외래키)

#### Passing Additional attributes to .save()

-   `.save()` 메서드는 특정 Serializer 인스턴스를 저장하는 과정에서 추가적인 데이터를 받을 수 있음
    -   인스턴스를 저장하는 시점에 추가 데이터 삽입이 필요한 경우

#### Read Only Field (읽기 전용 필드)

-   어떤 게시글에 작성하는 댓글인지에 대한 정보를 form-data로 넘겨주지 않았기 때문에 직렬화하는 과정에서 article 필드가 유효성 검사(`is_valid`)를 통과하지 못함
    -   `CommentSerializer`에서 article field에 해당하는 데이터 또한 요청으로부터 받아서 직렬화하는 것으로 설정되었기 때문
-   이때는 읽기 전용 필드(`read_only_fields`) 설정을 통해 직렬화하지 않고 반환 값에만 해당 필드가 포함되도록 설정할 수 있음



### 특정 게시글에 작성된 댓글 목록 출력하기

-   Serializer는 기존 필드를 override하거나 추가 필드를 구성할 수 있음
-   우리가 작성한 로직에서는 크게 2가지 형태로 구성할 수 있음
    1.   `PrimaryKeyRelatedField`
         -   pk를 사용하여 관계된 대상을 나타내는 데 사용할 수 있음
         -   필드가 to many relationships(N)를 나타내는데 사용되는 경우 `many=True` 속성 필요
         -   `comment_set` 필드 값을 form-data로 받지 않으므로 `read_only=True` 설정 필요
         -   [참고] 역참조 시 생성되는 `comment_set`을 다른 매니저 이름으로 override 할 수 있음
             -   단, 다음과 같이 수정할 경우 이전 serializers.py 에서의 클래스 변수명도 일치하도록 수정해야함
    2.   `Nested relationships`
         -   모델 관계상으로 참조된 대상은 참조하는 대상의 표현(응답)에 포함되거나 중첩(nested) 될 수 있음
         -   이러한 중첩된 관계는 serializers를 필드로 사용하여 표현할 수 있음
         -   두 클래스의 상하위치 변경



### 특정 게시글에 작성된 댓글의 개수 구하기

-   `comment_set` 매니저는 모델 관계로 인해 자동으로 구성되기 때문에 커스텀 필드를 구성하지 않아도 `comment_set`이라는 필드명을 fields 옵션에 작성만 해도 사용할 수 있었음
-   하지만 지금처럼 별도의 값을 위한 필드를 사용하려는 경우 자동으로 구성되는 매니저가 아니기 때문에 직접 필드를 작성해야 함
-   [주의사항] `read_only_fields` shortcut issue
    -   특정 필드를 override 혹은 추가한 경우 `read_only_fields` shortcut으로 사용할 수 없음

#### 'source' arguments

-   필드를 채우는 데 사용할 속성의 이름
-   점 표기법(dot notation)을 사용하여 속성을 탐색할 수 있음
-   `comment_set`이라는 필드에 .(dot)을 통해 전체 댓글의 개수 확인 가능
-   `.count()`는 built-in Queryset API 중 하나

