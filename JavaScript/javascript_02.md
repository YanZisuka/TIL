# JavaScript_02

## 함수 (Function)

### 1급 객체

-   **변수에 할당할** 수 있어야 한다.
-   **함수의 인자값으로** 넘길 수 있어야 한다.
-   **함수의 리턴값이** 될 수 있어야 한다.



### Rest Operator

-   **rest operator(...)**를 사용하면 함수가 정해지지 않은 수의 매개변수를 배열로 받음

```javascript
const restOpr = function (...restArgs) {
    
}
```



### Spread Operator

-   **spread operator(...)**를 사용하면 배열을 전개하여 인자로 넘겨줄 수 있음
-   얕은 복사에 활용 가능

```javascript
const arr = [1, 2, 3]
const newArr = [0, ...arr, 4]

const sprdOpr = function (x1, x2, x3) {
    return x1 + x2 + x3
}

sprdOpr(...arr)  // 6
```



### Arrow Function

```javascript
const f = function (name) {
    return `Hello, ${name}!`
}

const f = (name) => { return `Hello, ${name}!` }

// 인자가 1개라면 소괄호 생략 가능
const f = name => { return `Hello, ${name}!` }

// 함수 body return을 포함해 표현식 1개일 경우 중괄호와 return 생략 가능
const f = name => `Hello, ${name}!`
```



### 문자열 관련 메서드

|                      메서드                       |                             설명                             |
| :-----------------------------------------------: | :----------------------------------------------------------: |
|                `.includes(value)`                 |     문자열에 value가 존재하는지 판별 후 참 or 거짓 반환      |
|                  `.split(value)`                  | value가 없을 경우, 기존 문자열을 배열에 담아 반환<br />value가 빈 문자열일 경우 각 문자로 나눈 배열을 반환<br />value가 기타 문자열일 경우, 해당 문자열로 나눈 배열을 반환 |
| `.replace(from, to)`<br />`.replaceAll(from, to)` | 문자열에 from값이 존재할 경우, 1개만 to값으로 교체하여 반환<br />문자열에 from 값이 존재할 경우, 모두 to값으로 교체하여 반환 |
|  `.trim()`<br />`.trimStart()`<br />`.trimEnd()`  | 문자열 시작과 끝의 모든 공백문자(스페이스, 탭, 엔터 등)를 제거한 문자열 반환<br />문자열 시작의 공백문자를 제거한 문자열 반환<br />문자열 끝의 공백문자를 제거한 문자열 반환 |



### 배열 관련 메서드

|     메서드      |                             설명                             |
| :-------------: | :----------------------------------------------------------: |
|     reverse     |         **원본 배열의** 요소들의 순서를 반대로 정렬          |
|   push & pop    |          배열의 **가장 뒤에** 요소를 추가 또는 제거          |
| unshift & shift |          배열의 **가장 앞에** 요소를 추가 또는 제거          |
|    includes     |     배열에 특정 값이 존재하는지 판별 후 **참/거짓 반환**     |
|     indexOf     | 배열에 특정 값이 존재하는지 판별 후 **인덱스 반환**<br />요소가 없을 경우 -1 반환 |
|      join       | 배열의 **모든 요소를 구분자를 이용하여 연결**<br />구분자 생략 시 쉼표 기준 |



#### forEach

```javascript
array.forEach(callback(element[, index[, array]]))
```

-   배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
-   콜백 함수는 3가지 매개변수로 구성
    -   element: 배열의 요소
    -   index: 배열 요소의 인덱스
    -   array: 배열 자체
-   **return이 없는 메서드**

#### map

```javascript
array.map(callback(element[, index[, array]]))
```

-   배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
-   콜백 **함수의 반환 값을 요소로 하는** 새로운 배열 반환
-   기존 배열 전체를 다른 형태로 바꿀 때 유용

#### filter

```javascript
array.filter(callback(element[, index[, array]]))
```

-   배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
-   콜백 **함수의 반환 값이 참인 요소들만** 모아서 새로운 배열을 반환
-   기존 배열의 요소들을 필터링 할 때 유용

#### find

```javascript
array.find(callback(element[, index[, array]]))
```

-   배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
-   콜백 함수의 **반환 값이 참이면, 조건을 만족하는 첫번째 요소를 반환**
-   찾는 값이 배열에 없으면 `undefined` 반환

#### some

```javascript
array.some(callback(element[, index[, array]]))
```

-   배열의 요소 중 **하나라도** 주어진 판별 함수를 통과하면 참을 반환
-   하나의 요소라도 통과하지 못하면 거짓 반환
-   (참고) 빈 배열은 항상 거짓 반환

#### every

```javascript
array.every(callback(element[, index[, array]]))
```

-   배열의 **모든 요소가** 주어진 판별 함수를 통과하면 참을 반환
-   하나의 요소라도 통과하지 못하면 거짓 반환
-   (참고) 빈 배열은 항상 참 반환

#### reduce

```javascript
array.reduce(callback(acc, element[, index[, array]])[, initialValue])
```

-   배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
-   콜백 함수의 반환 값들을 **하나의 값(acc)에 누적 후 반환**
-   reduce 메서드의 주요 매개변수
    -   acc
        -   이전 callback 함수의 반환 값이 누적되는 변수
    -   `initialValue(optional)`
        -   최초 callback 함수 호출 시 acc에 할당되는 값, default 값은 배열의 첫 번째 값
-   (참고) 빈 배열의 경우 initialValue를 제공하지 않으면 에러 발생



## 객체 (Objects)

```javascript
const me = {
    name: 'jack',
    phoneNumber: '01012345678',
    'samsung products': {
        buds: 'Galaxy Buds Pro',
        galaxy: 'Galaxy S20',
    },
}

me.name
me['samsung products'].buds
```

-   객체는 property의 집합이며, 중괄호 내부에 key-value 쌍으로 표현
-   key는 문자열 타입*만 가능
    -   (참고) key 이름에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현
-   value는 모든 타입(함수포함) 가능
-   객체 요소 접근은 점 또는 대괄호로 가능
    -   (참고) key 이름에 띄어쓰기같은 구분자가 있으면 대괄호 접근만 가능



### 메서드

-   메서드는 어떤 객체의 속성이 참조하는 함수
-   객체.메서드명()으로 호출 가능
-   메서드 내부에서는 `this` 키워드가 객체를 의미함



### 객체 관련 ES6 문법

#### 속성명 축약 (shorthand)

```javascript
const books = ['Learning JS', 'Learning Python']
const magazines = ['Vogue', 'Science']

const bookShop = {
    books,
    magazines,
}
```

-   객체를 정의할 때 **key와 할당하는 변수의 이름이 같으면** 예시와 같이 축약 가능

#### 메서드명 축약 (shorthand)

```javascript
const obj = {
    greeting() {
        console.log('Hi!')
    }
}
```

-   메서드 선언 시 **function 키워드 생략 가능**

#### 계산된 속성 (computed property name)

```javascript
const key = 'regions'
const value = ['서울', '판교']

const location = {
    [key]: value,
}
```

-   객체를 정의할 때 key의 이름을 표현식을 이용하여 동적으로 생성 가능

#### 구조 분해 할당 (destructing assignment)

```javascript
const user = {
    name: 'yan',
    email: '1234@gmail.com',
}

const { name, email } = user
```

-   배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

#### JSON (JavaScript Object Notation)

-   key-value 쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷
-   자바스크립트의 객체와 유사하게 생겼으나 실제로는 문자열 타입
    -   따라서 JS의 객체로써 조작하기 위해서는 구문 분석(parsing)이 필수
-   자바스크립트에서는 JSON을 조작하기 위한 두 가지 내장 메서드를 제공
    -   `JSON.parse()`
        -   JSON => 객체
    -   `JSON.stringify()`
        -   객체 => JSON



## this

-   JS의 `this`는 실행 문맥(execution context)에 따라 다른 대상을 가리킨다.
-   class 내부의 생성자(constructor) 함수
    -   `this`는 생성되는 객체를 가리킴(Python의 `self`)
-   메서드
    -   `this`는 해당 메서드가 소속된 객체를 가리킴
-   위의 두가지 경우를 제외하면 모두 최상위 객체(window)를 가리킴



### 함수 선언문과 Arrow function의 차이

```javascript
const obj = {
    PI: 3.14,
    radiuses: [1, 2, 3, 4, 5],
    printArea: function() {
        this.radiuses.forEach(function(r) {
            console.log(this.PI * r ** 2)
        }.bind(this))
    },
}
```

```javascript
const obj = {
    PI: 3.14,
    radiuses: [1, 2, 3, 4, 5],
    printArea: function() {
        this.radiuses.forEach((r) => {
            console.log(this.PI * r ** 2)
        })
    },
}
```

-   `this.radiuses`는 메서드이기에 정상적으로 접근 가능
-   `forEach`의 콜백함수의 경우 메서드가 아님
    -   때문에 콜백함수 내부의 `this`는 `window`가 되어 `this.PI`는 의도한 접근이 불가능
-   이 콜백함수 내부에서 `this.PI`에 접근하기 위해서 `함수객체.bind(this)` 메서드를 사용
-   번거로운 `bind` 과정을 없앤 것이 화살표 함수



## 프로토타입 (Prototype)

-   JS는 객체지향 프로그래밍 언어

-   JS의 객체는 key-value 자료구조

-   클래스 기반의 객체지향 프로그래밍 언어의 객체와 다름

-   JS는 클래스 개념이 존재하지 않고, 프로토타입이라는 개념을 활용해 상속을 구현

    -   JavaScript는 흔히 **프로토타입 기반 언어(prototype-based language)**라 불린다.

        -   모든 객체들이 메소드와 속성들을 상속 받기 위한 템플릿으로써 **프로토타입 객체(prototype object)**를 가진다는 의미
        -   프로토타입 객체도 또 다시 상위 프로토타입 객체로부터 메소드와 속성을 상속 받을 수도 있고 그 상위 프로토타입 객체도 마찬가지
        -   이를 **프로토타입 체인(prototype chain)**이라 부르며 다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있도록 하는 근간

        -   정확히 말하자면 상속되는 속성과 메소드들은 각 객체가 아니라 객체의 생성자의 `prototype`이라는 속성에 정의됨

        -   프로토타입 체인에서 한 객체의 메소드와 속성들이 다른 객체로 **복사되는 것이 아님**

-   ES6+부터 직관적인 상속 구현을 위해 `class` 키워드가 추가됨
