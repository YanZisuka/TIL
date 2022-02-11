# HTML/CSS_03

## 반응형 웹 & Media Query

### 웹 페이지 디자인 형태

-   고정폭 레이아웃
-   유동적 레이아웃
    -   이미지 및 글씨 등 영역이 유동적으로 변화함
-   별도의 사이트
    -   디바이스에 따른 별도의 사이트(도메인)로 구분됨
-   반응형 레이아웃
    -   하나의 웹사이트에서 PC, 스마트폰, 태블릿 등 접속하는 디스플레이의 종류에 따라 화면의 크기가 자동으로 변하도록 만든 웹페이지 접근 기법
    -   미디어 쿼리를 활용하여 CSS를 작성함

### 미디어 쿼리(Media Query)

-   미디어 쿼리는 CSS에서 `@media` 키워드를 활용하여 브라우저 및 디바이스 등 환경에 따라 CSS를 적용할 수 있는 방법

    ```css
    /* 가로 모드 (너비 > 높이) */
    @media (orientation: landscape) {
        h1 {
            color: green;
        }
    }
    
    /* 세로 모드 (높이 > 너비) */
    @media (orientation: portrait) {
        h1 {
            color: red;
        }
    }
    
    /* Media type : all, print, screen, speech */
    @media only print {
        * {
            color: black !important;
        }
    }
    ```

### 웹 페이지 부가 요소

>   Favicon (favorite icon)

-   사이트를 대표하는 아이콘으로 브라우저 주소창, 탭, 북마크 바 등에 표시됨
-   사용자가 웹 사이트를 쉽게 시각적으로 식별하는 데 사용되며, 일반적으로 브랜드 로고와 동일한 것으로 사용하여 브랜드 인지도를 높임

-   Favicon 생성기
    -   https://favicon.io/
    -   이미지 혹은 텍스트 기반의 favicon을 생성하여 쉽게 활용 가능

>   Icon

-   일반적인 웹 사이트에서 활용되는 아이콘은 이미지가 아닌 `i `태그로 구성되어 있음
-   대표적인 서비스 - Font Awesome, Material Icons (Google), Bootstrap Icons

-   Font Awesome
    -   Kit 다운로드
        -   이메일을 입력하고 가입을 통해 Kit 활용
    -   CDN 활용
        -   https://cdnjs.com/libraries/font-awesome/

>   Fonts

-   웹 폰트
    -   일반적으로 사용자 컴퓨터에 폰트가 없는 경우 원하는 폰트가 보여지지 않아 웹 폰트를 다운받아 적용할 수 있도록 함
    -   대표적인 서비스 - Google Fonts

### 반응형 웹 페이지 구성

>   SCSS

-   CSS를 만들기 위한 도구로 변수, 상속, mixin 등의 개념을 통해 가독성과 재사용성을 높여 유지보수가 쉽도록 함
-   일반적으로 SCSS로 개발하고 CSS로 변환하여 활용 가능하게 함

>   코드 경량화 (minify)

-   웹 사이트 접속시 모든 소스코드는 네트워크를 통해 전송되므로 더욱 빠른 다운로드를 위해 파일의 공백 및 개행 문자를 제거하여 경량화 함

>   Reboot.css & Reset.css & Normalize.css

-   브라우저 고유의 설정을 초기화해 어떤 브라우저에서도 잘 작동하도록 하기 위해 사용