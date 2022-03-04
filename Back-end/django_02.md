# Django_02

## Namespace

-   이름공간 또는 네임스페이스는 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체만을 가리키게 된다.
-   프로그래밍을 하다 보면 모든 변수명과 함수명 등 이들 모두를 겹치지 않게 정의하는 것은 매우 어려운 일
-   그래서 `django`에서는
    -   서로 다른 `app`의 같은 이름을 가진 `url name`은 이름공간을 설정해서 구분
    -   `templates, static` 등 `django`는 정해진 경로 하나로 모아서 보기 때문에 중간에 폴더를 임의로 만들어 줌으로써 이름공간을 설정

### URL namespace

-   **URL namespace**를 사용하면 서로 다른 앱에서 동일한 URL 이름을 사용하는 경우에도 이름이 지정된 URL을 고유하게 사용할 수 있음
-   **urls.py**에 `app_name` attribute 작성
-   참조
    -   `:` 연산자를 사용하여 지정

### Template namespace

-   **django**는 기본적으로 `app_name/templates/` 경로에 있는 templates 파일들만 찾을 수 있으며, `INSTALLED_APPS`에 작성한 app 순서로 templates을 검색 후 렌더링 함
-   그래서 임의로 templates의 폴더 구조를 `app_name/templates/app_name/`형태로 변경해 임의로 **이름 공간을 생성** 후 변경된 추가 경로로 수정

## Static files

-   웹 서버와 정적 파일
    -   웹 서버는 특정 위치(URL)에 있는 자원(resource)을 요청(HTTP request)받아서 제공(serving)하는 응답(HTTP response)을 처리하는 것을 기본 동작으로 함
    -   이는 자원과 접근 가능한 주소가 정적으로 연결된 관계
        -   예를 들어, 사진 파일은 자원이고 파일 경로는 웹 주소라 함
    -   즉, 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 자원(static resource)을 제공

### Static file

-   정적 파일
-   응답할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
    -   사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일
-   예를 들어, 웹 서버는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가 파일(움직이지 않는)을 제공해야 함
-   파일 자체가 고정되어 있고, 서비스 중에도 추가되거나 변경되지 않고 고정되어 있음
-   django에서는 이러한 파일들을 `Static file`이라 함
    -   django는 `staticfiles` 앱을 통해 정적 파일과 관련된 기능을 제공

#### Static file 구성

1.   `django.contrib.staticfiles`가 `INSTALLED_APPS`에 포함되어 있는지 확인
2.   **settings.py**에서 `STATIC_URL`을 정의
3.   템플릿에서 static 템플릿 태그를 사용하여 지정된 상대경로에 대한 URL을 빌드
4.   앱의 static 디렉토리에 정적 파일을 저장
     -   `ex) my_app/static/my_app/example.jpg`

#### The staticfiles app (1/3)

-   `STATICFILES_DIRS`
    -   `app/static/` 디렉토리 경로(기본 경로)를 사용하는 것 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트
    -   추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록으로 작성되어야 함

#### The staticfiles app (2/3)

-   `STATIC_URL`
    -   `STATIC_ROOT`에 있는 정적 파일을 참조할 때 사용할 URL
        -   개발 단계에서는 실제 정적 파일들이 저장되어 있는 `app/static/` 경로(기본 경로) 및 `STATICFILES_DIRS`에 정의된 추가 경로들을 탐색함
    -   실제 파일이나 디렉토리가 아니며, URL로만 존재
    -   비어 있지 않은 값으로 설정 한다면 반드시 **slash(/)**로 끝나야 함

#### The staticfiles app (3/3)

-   `STATIC_ROOT`
    -   *collectstatic*이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
    -   django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아놓는 경로
    -   개발 과정에서 **settings.py**의 `DEBUG` 값이 **True**로 설정되어 있으면 해당 값은 작용되지 않음
        -   직접 작성하지 않으면 django 프로젝트에서는 **settings.py**에 작성되어 있지 않음
    -   실 서비스 환경(배포 환경)에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위함

### Django template tag

-   **load**
    -   사용자 정의 템플릿 태그 세트를 로드(load)
    -   로드하는 라이브러리, 패키지에 등록된 모든 태그와 필터를 불러옴
-   **static**
    -   `STATIC_ROOT`에 저장된 정적 파일에 연결