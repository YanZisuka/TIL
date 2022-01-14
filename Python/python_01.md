# Python_01
## Version

-   `Release 버전`: 사용자에게 배포할 때의 버전
-   `개발 버전`: 개발자가 정의한 버전
-   대다수의 SW version은 `Semantic Version`으로 이루어져 있음
-   `SemVer`: `MAJOR.Minor.patch`
-   `patch`는 **사소한 버그 패치 등**
-   `Minor`는 **patch보다 더 많은 변화**
-   `MAJOR`는 **기존 버전과 호환되지 않을 정도로 큰 변화**



## What

-   컴퓨터 프로그래밍 언어
    -   `컴파일러`를 통해 `컴퓨터`에게 `기능을 구현`시키는 언어
    
    

## Why

-   프로그래밍이 쉬워진 이유
    1.   언어가 **쉬워졌다**(`천공카드` > `어셈블리어` > `C` > `Python`)
    2.   **환경**이 좋아졌다(`Open Source`)
    3.   좋은 강사가 있다
    
    

## How: 문법

### 저장

#### 무엇을 저장할까?

-   숫자: `0, 1, 2`

-   글자: `'먼지'`

-   참/거짓: `True/False`

#### 어떻게 저장할까?
-   변수(variable)

    ```
    # dust 변수에 70을 할당
    dust = 70
    
    # dust는 70이다
    dust == 70
    ```

-   리스트(list)

    ```
    # 3일치의 미세먼지 양
    dust = [58, 40, 60,]
    ```

-   딕셔너리(dictionary)

    ```
    dust = ['서대문구' : 58, '마포구' : 40, '도봉구' : 60,]
    ```

    

### 제어

#### if/elif/else

```
if dust > 150:
	print('매우나쁨')
elif dust > 80:
	print('나쁨')
elif dust > 30:
	print('보통')
elif dust >= 0:
	print('좋음')
else:
	print('지구멸망')
```



### 반복

#### while

```
# while에 해당하는 조건인 동안 계속 반복
dust = [59, 24, 102, 45, 64,]

n = 0
while n < 3:
	print(dust[n])
	n = n + 1
```

#### for

```
# 정해진 박스 내에서의 반복
dust = [59, 24, 102,]

for i in dust:
	print(i)
```



### 함수

-   특정한 용도의 동작하는 코드를 한 곳에 모아 놓은 것
-   함수는 `일의 단위`이다
- 내장함수(bulit-in function)
- 외장함수(external function)
