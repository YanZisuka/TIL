# Python_02

## 컴퓨터 프로그래밍 언어

-   선언적 지식
-   **명령적 지식(How-to)**

## 파이썬 개발환경

-   대화형 인터프리터
    -   기본 인터프리터
    -   **Jupyter Notebook**
-   스크립트 실행
    -   .py 파일을 작성, IDE or Text Editor 활용
-   코드 스타일 가이드
    -   PEP8
    -   Google Style Guide

## 문법

>   `변수(Variables)`

-   컴퓨터 메모리 어딘가에 저장되어 있는 객체(값)를 참조하기 위해 사용되는 이름
    -   객체(`object`): 숫자, 문자, 클래스 등 값을 가지고 있는 모든 것
-   동일 변수에 다른 객체를 언제든지 할당 가능
-   할당 연산자(=)를 통해 값을 할당(`assignment`)
-   `type()`: 변수의 자료형 확인
-   `id()`
    -   변수에 할당된 값의 고유한 아이덴티티(`identity`) 값이며, 메모리주소

>   `식별자(Identifiers)`

-   `CamelType`
-   `snake-type`
-   규칙
    -   식별자의 이름은 영문 알파벳, 언더스코어(`_`), 숫자로 구성
    -   첫 글자에 숫자가 올 수 없음
    -   길이제한이 없고, 대소문자 구별
    -   예약어(`reserved words`)는 사용 불가
-   내장함수, 모듈 등의 이름으로 만들면 안됨

## 자료형(Datatype)

>   `None`

-   값이 없음을 표현

>   `Boolean`

-   `True` / `False`
-   다음은 모두 `False`
    -   `0`, `0.0`, `()`, `[]`, `{}`, `''`, `None`

>   `Numeric Type`

-   정수(`int`)

-   실수(`float`)

    -   Floating point rounding error

    ```
    3.14 - 3.02
    
    0.1200000000000001
    ```

-   복소수(`complex`)

>   `String Type`

-   **PEP8**에서는 소스코드 내에서 하나의 문장부호를 선택하여 유지하도록 함
-   `Immutable`
-   `Iterable`
-   `Escape sequence`
    -   `\n`, `\t`, `\r`, `\0`, `\\`, `\'`, `\"`
-   `String Interpolation`
    -   `%-formatting`
    -   `str.format()`
    -   `f-strings`: python 3.6+

### 컨테이너(Container)

-   `순서가 있다 != 정렬되어 있다`

#### 시퀀스형 컨테이너(Sequence Container)

>   `List`

-   순서를 가지는 0개 이상의 객체를 참조하는 자료형
-   **가변자료형(mutable)**

>   `Tuple`

-   순서를 가지는 0개 이상의 객체를 참조하는 자료형
-   **불변자료형(immutable)**

>   `Range`

-   숫자의 시퀀스를 나타내기 위해 사용
-   **불변자료형(immutable)**

#### 비시퀀스형 컨테이너(Associative Container)

>   `Set`

-   순서없이 0개 이상의 해시가능한 객체(`immutable`)를 참조하는 자료형
-   빈 셋을 만들기 위해서는 반드시 `set()`활용
-   **가변자료형(mutable)**

>   `Dictionary`

-   순서 없이 키-값(`key-value`) 쌍으로 이루어진 객체를 참조하는 자료형
-   `key`에 `list` 할당 불가능
-   **가변자료형(mutable)**

### 자료형 변환(Typecasting)

-   파이썬에서 데이터 형태는 서로 변환할 수 있음
    -   암시적(`Implicit`)
        -   사용자가 의도하지 않고, 파이썬 내부적으로 자료형을 변환하는 경우
    -   명시적(`Explicit`)
        -   `int`, `float`

### 컨테이너형 변환(Container Typecasting)

-   `range`와 `dictionary`형으로 **변환 불가능**
-   `dictionary`를 다른 컨테이너형으로 변환하면 `key`만 변환

## 연산자(Operator)

>   `산술 연산자(Arithmetic)`

-   기본적인 사칙연산 및 수식 계산

>   `비교 연산자(Comparison)`

-   값을 비교하며, `True / False` 값을 리턴

>   `논리 연산자(Logical)`

-   `and`, `or`, `not`
-   단축평가
    -   결과가 확실한 경우 두번째 값을 확인하지 않고 첫번째 값 반환
        -   `and` 연산에서 첫번째 값이 `False`인 경우 무조건 `False`
        -   `or` 연산에서 첫번째 값이 `True`인 경우 무조건 `True`

>   `복합 연산자(In-Place)`

-   복합 연산자는 연산과 대입이 함께 이루어짐
    -   `+=`, `-=`, etc.,

>   `식별 연산자(Identify)`

-   `is`연산자를 통해 동일한 객체(`object`)인지 확인
-   `-5 ~ 256`까지는 고정된 `메모리주소`, `257`이상은 다른 `id`를 가짐

>   `멤버십 연산자(Membership)`

-   포함 여부 확인
    -   `in`, `not in`

>   `시퀀스형 연산자(Sequence Type)`

-   시퀀스를 연결, 반복
    -   `+`, `*`

>   `인덱싱(Indexing)`

-   시퀀스의 특정 인덱스 값에 접근
    -   해당 인덱스가 없는 경우 `IndexError`

>   `슬라이싱(Slicing)`

-   시퀀스를 특정 단위로 슬라이싱

-   문자열 슬라이싱
    -   `s[::] == s[0:len(s):1]`
    -   `s[::-1]`

>   `Set 연산자`

-   `|`: 합집합
-   `&`: 교집합
-   `-`: 여집합
-   `^`: 대칭차

## 제어문(Control Statement)

-   제어문은 순서도(flow chart)로 표현이 가능

### 조건문(Conditional Statement)

-   조건문은 참/거짓을 판단할 수 있는 조건식과 함께 사용

>   `if/else`

-   `else`문은 선택적으로 사용가능

```python
num = int(input())
type(num)
print(num)

if num % 2:
	print(f'{num}is odd number')
else:
	print(f'{num}is even number')
```

#### 복수조건문

```python
if <expression>:
	# Code block
elif <expression>:
	# Code block
elif <expression>:
	# Code block
else:
	# Code block
```

#### 조건표현식

```python
truecase if <expression> else falsecase
```

### 반복문(Loop Statement)

>   `while`

-   `while`문은 조건식이 참인 경우 반복적으로 코드를 실행

```python
while <expression>:
	# Code block
```

>   `for`

-   `for`문은 시퀀스(`string, tuple, list, range` )를 포함한 순회가능한 객체(`iterable`)요소를 모두 순회함

```python
for <variable> in <iterable>:
	# Code block
```

-   `단순 순회`

```python
chars = 'apple'
for char in chars:
    print(char)
```

-   `인덱스로 접근`

```python
chars = 'apple'
for idx in range(len(chars)):
    print(chars[idx])
```

-   `dictionary 순회`

```python
grades = {'kim' : 80, 'lee' : 90}

for key in grades:
	print(key)

for key in grades.key():
	print(key)
	
for value in grades.values():
	print(value)
	
for key, value in grades.items():
	print(key, value)
```

-   `enumerate 순회`

```python
members = ['minsu', 'young', 'cheol']

for idx, member in enumerate(members):
	print(idx, member)
```

-   `comprehension 식`

```python
# List comprehension
[<expression> for <variable> in <iterable> if <condition>]

# Dictionary comprehension
{<key:value> for <variable> in <iterable> if <condition>}
```

#### 반복문 제어

>   `break`

-   `break`문을 만나면 반복문은 종료됨

>   `continue`

-   `continue`이후의 코드 블록은 수행하지 않고, 다음 반복을 수행

>   `pass`

-   아무것도 하지 않음

>   `for-else`

-   끝까지 반복문을 실행한 이후에 else문 실행

```python
for char in 'banana':
	if char == 'b':
		print('b!')
		break
else:
	print('b가 없습니다.')
```

