# HTML/CSS_01

## HTML(Hyper Text Markup Language)

-   웹 페이지를 작성(구조화)하기 위한 언어

-   Hyper Text: 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
-   Markup Language: 태그 등을 이용하여 문서나 데이터의 **구조를 명시**하는 언어

-   **WHATWG**(Web Hypertext Application Technology Working Group)

### HTML 기본 구조

-   `html`: 문서의 최상위(root) 요소
-   `head`: 문서 메타데이터 요소
    -   문서 제목, 인코딩, 스타일, 외부 파일 로딩 등
    -   일반적으로 브라우저에 나타나지 않는 내용
-   `body`: 문서 본문 요소
    -   실제 화면 구성과 관련된 내용

>   Open Graph Protocol

-   메타 데이터를 표현하는 새로운 규약
    -   HTML 문서의 메타 데이터를 통해 문서의 정보를 전달
    -   메타정보에 해당하는 제목, 설명 등을 쓸 수 있도록 정의

>   DOM(Document Object Model) 트리

-   텍스트 파일인 HTML 문서를 브라우저에서 렌더링 하기 위한 구조
    -   HTML 문서에 대한 모델을 구성함
    -   HTML 문서 내의 각 요소에 접근 / 수정에 필요한 프로퍼티와 메서드를 제공함

>   요소(element)

-   HTML 요소는 시작 태그와 종료 태그, 그리고 태그 사이에 위치한 내용으로 구성
    -   태그는 컨텐츠를 감싸는 것으로 그 정보의 성격과 의미를 정의
-   요소는 중첩될 수 있음
    -   요소의 중첩을 통해 하나의 문서를 구조화

>   속성(attribute)

-   속성을 통해 태그의 부가적인 정보를 설정할 수 있음
-   요소는 속성을 가질 수 있으며, 경로나 크기와 같은 추가적인 정보를 제공
-   요소의 시작 태그에 작성하며 보통 이름과 값이 하나의 쌍으로 존재
-   태그와 상관없이 사용 가능한 속성(HTML Global Attribute)들도 있음
    -   `id, class, data-*, style, title, tabindex` etc.,

>   시맨틱 태그(semantic tag)

-   HTML5에서 의미론적 요소를 담은 태그의 등장
    -   `header, nav, aside, section, article, footer` etc.,
-   Non semantic 요소는 div, span 등이 있으며 h1, table 등도 시맨틱 태그로 볼 수 있음
-   검색엔진최적화(SEO)를 위해서 메타태그, 시맨틱 태그 등을 통한 마크업을 효과적으로 활용해야함

### HTML 문서 구조화

>   텍스트 요소

-   `<a>, <b>, <strong>, <i>, <em>, <br>, <img>, <span>`

>   그룹 컨텐츠

-   `<p>, <hr>, <ol>, <ul>, <pre>, <blockquote>, <div>`

>   table

-   `<thead>, <tbody>, <tfoot>, <caption>`

>   form

-   form은 정보(데이터)를 서버에 제출하기 위한 영역
-   form 기본 속성
    -   action: form을 처리할 서버의 URL
    -   method: form을 제출할 때 사용할 HTTP 메서드 (GET 혹은 POST)
    -   enctype: method가 post인 경우 데이터의 유형
        -   application/x-www-form-urlencoded: 기본값
        -   multipart/form-data: 파일 전송시 (input type이 file인 경우)
        -   text/plain: HTML5 디버깅용 (잘 사용되지 않음)

>   input

-   다양한 타입을 가지는 입력 데이터 유형과 위젯이 제공됨
-   대표적 속성
    -   name: form control에 적용되는 이름
    -   value: form control에 적용되는 값
    -   `required, readonly, autofocus, autocomplete, disabled` etc.,
-   input 유형 - 일반
    -   `text, password, email, number, file, checkbox, radio, color, date, hidden` etc.,

>   input label

-   label을 클릭하여 input 자체의 초점을 맞추거나 활성화 시킬 수 있음
    -   사용자는 선택할 수 있는 영역이 늘어나 웹 / 모바일 환경에서 편하게 사용할 수 있음
    -   label과 input 입력의 관계가 시각적 뿐만 아니라 화면리더기에서도 label을 읽어 쉽게 내용을 확인할 수 있도록 함
-   `<input>`에 id 속성을, `<label>`에는 for 속성을 활용해 연관



## CSS(Cascading Style Sheets)

-   스타일을 지정하기 위한 언어
-   CSS 구문은 선택자를 통해 스타일을 지정할 HTML 요소를 선택
-   중괄호 안에서는 속성과 값, 하나의 쌍으로 이루어진 선언을 진행

>   CSS 정의 방법

-   인라인(inline)
-   내부 참조(embedding) - `<style>`
-   외부 참조(link file) - 분리된 CSS 파일

### CSS Selectors

-   기본 선택자
    -   전체 선택자, 요소 선택자
    -   클래스 선택자, 아이디 선택자, 속성 선택자
-   결합자(Combinators)
    -   자손 결합자
        -   selectorA 하위의 모든 selectorB 요소
    -   자식 결합자
        -   selectorA 바로 아래의 selectorB 요소
    -   일반 형제 결합자
        -   selectorA의 형제 요소 중 뒤에 위치하는 selectorB 요소를 모두 선택
    -   인접 형제 결합자
        -   selectorA의 형제 요소 중 바로 뒤에 위치하는 selectorB 요소를 선택
-   의사 클래스/요소(Pseudo Class)
    -   링크, 동적 의사 클래스
    -   구조적 의사 클래스, 기타 의사 클래스, 의사 엘리먼트, 속성 선택자

### CSS 상속

-   CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속한다.
    -   속성 중에는 상속이 되는것과 되지 않는 것들이 있다.
    -   상속 되는 것
        -   **text 관련**(font, color, text-align), opacity, visibility
    -   상속 되지 않는 것
        -   **box model 관련 요소**(width, height, margin, padding, border, box-sizing, display), **position 관련 요소**(position, top/right/bottom/left,z-index) etc.,

### Box model

-   모든 요소는 박스이고, 위에서부터 아래로, 왼쪽에서 오른쪽으로 쌓인다.
-   하나의 박스는 네 영역으로 이루어짐
    -   content
    -   padding
    -   border
    -   margin

>   box-sizing

-   기본적으로 모든 요소의 box-sizing은 content-box
-   다만, 우리가 일반적으로 영역을 볼 때는 border까지의 너비를 보는 것을 원함
    -   그 경우 box-sizing을 border-box로 설정

### Display

-   `display: block`
    -   줄 바꿈이 일어나는 요소
    -   화면 크기 전체의 가로 폭을 차지한다.
    -   블록 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있음
-   `display: inline`
    -   줄 바꿈이 일어나지 않는 행의 일부 요소
    -   content 너비만큼 가로 폭을 차지한다.
    -   width, height, margin-top, margin-bottom을 지정할 수 없다.
    -   상하 여백은 line-height로 지정한다.

### Position

-   문서 상에서 요소 위치를 지정
-   `static`: 모든 태그의 기본 값(기준 위치)
    -   일반적인 요소의 배치 순서에 따름(좌측 상단)
    -   부모 요소 내에서 배치될 때는 부모 요소의 위치를 기준으로 배치됨
-   아래는 좌표 프로퍼티를 사용하여 이동 가능
    -   `relative`
        -   자기 자신의 static 위치를 기준으로 이동(normal flow 유지)
        -   레이아웃에서 요소가 차지하는 공간은 static일 때와 같음(normal position 대비 offset)
    -   `absolute`
        -   요소를 일반적인 문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음(normal flow에서 벗어남)
        -   static이 아닌 가장 가까이 있는 부모/조상 요소를 기준으로 이동(없는 경우 body)
    -   `fixed`
        -   요소를 일반적인 문서 흐름에서 제거 후 레이아웃에 공간을 차지하지 않음(normal flow에서 벗어남)
        -   부모 요소와 관계없이 viewport를 기준으로 이동
            -   스크롤 시에도 항상 같은 곳에 위치

