# Data Structure_02

## 스택(Stack)

### 스택의 특성

-   물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
-   스택에 저장된 자료는 선형 구조를 갖는다.
    -   선형구조: 자료 간의 관계가 1대1의 관계
    -   비선형구조: 자료 간의 관계가 1대N의 관계 `ex. 트리`
-   스택에 자료를 삽입하거나 꺼낼 수 있다.
-   마지막에 삽입한 자료를 가장 먼저 꺼낸다. **후입선출**(**Last-In-First-Out**)

### 스택의 구현

-   자료구조: 자료를 선형으로 저장할 저장소
    -   배열을 사용할 수 있다.
    -   저장소 자체를 스택이라 부르기도 한다.
    -   스택에서 마지막 삽입된 원소의 위치를 `top`이라 부른다.
-   연산
    -   `push`: 저장소에 자료를 저장
    -   `pop`: 저장소에서 자료를 꺼냄
    -   `isEmpty`: 스택이 공백인지 아닌지 확인
    -   `peek`: 스택의 top에 있는 item을 반환

```python
class Stack:
    def __init__(self, size):
        self.size = size
        self.items = [None] * self.size
        self.top = -1
        
    def isEmpty(self):
        return self.top == -1
    
    def isFull(self):
        return self.top == self.size - 1
    
    def push(self, item):
        if self.isFull():
            raise ValueError('Stack Overflow!')
        else:
            self.top += 1
            self.items[self.top] = item
        
    def pop(self):
        if self.isEmpty():
            raise ValueError('Empty Stack. Nothing to pop.')
        else:
            item = self.items[self.top]
            self.items[self.top] = None
            self.top -= 1
            return item
    
    def peek(self):
        if self.isEmpty():
            raise ValueError('Empty Stack. Nothing to peek.')
        else:
            return self.items[self.top]
    
    def __str__(self):
        result = '\n-----'
        for item in self.items:
            if item is None:
                result = f'\n|   |' + result
            else:
                result = f'\n| {item} |' + result
        return result
```

### 스택의 응용

>   괄호검사

-   문자열에 있는 괄호를 차례대로 조사하면서 왼쪽 괄호를 만나면 스택에 삽입하고, 오른쪽 괄호를 만나면 스택에서 top 괄호를 삭제한 후 오른쪽 괄호와 짝이 맞는지 검사한다.

>    함수호출

-   프로그램에서의 함수 호출과 복귀에 따른 수행 순서를 관리
    -   가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조이므로, 후입선출 구조의 스택을 이용
    -   함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임에 저장하여 시스템 스택에 삽입
    -   함수의 실행이 끝나면 시스템 스택의 top 원소(스택 프레임)를 삭제하면서 프레임에 저장되어 있던 복귀주소를 확인하고 복귀
    -   함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스택이 된다.



## 큐(Queue)

-   큐(Queue)의 특성
    -   스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조
        -   큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
    -   **선입선출(First-In-First-Out)**
    -   큐의 기본 연산
        -   삽입: `enQueue`
        -   삭제: `deQueue`

```python
class Queue:
    def __init__(self, *args):
        self.items = list(args)
    
    def peek(self):
        return self.items[0]
    
    def enqueue(self, item):
        self.items.append(item)
        
    def dequeue(self):
        if self.isEmpty():
            raise ValueError('Queue is empty')
        else:
            item = self.items[0]
            self.items.pop(0)
            return item
    
    def isEmpty(self):
        return not self.items
    
    def __str__(self):
        return f'Front -> {self.items} <- Rear'
```

### 선형 큐

-   1차원 배열을 이용한 큐
    -   큐의 크기 = 배열의 크기
    -   front: 저장된 첫 번째 원소의 인덱스, 가장 최근 꺼내진 위치
    -   rear: 저장된 마지막 원소의 인덱스, 가장 최근 저장된 위치
-   상태 표현
    -   초기 상태: front = rear = -1
    -   공백 상태: front == rear
    -   포화 상태: rear == n-1
-   선형 큐 이용시의 문제점
    -   잘못된 포화상태 인식
        -   선형 큐를 이용하여 원소의 삽입과 삭제를 계속할 경우, **배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구**하고, `rear = n-1`인 상태, 즉, **포화상태로 인식하여 더 이상의 삽입을 수행하지 않게 됨**
    -   해결방법 1
        -   매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
        -   원소 이동에 많은 시간이 소요되어 큐의 효율성이 급격히 떨어짐
    -   해결방법 2
        -   1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용

### 원형 큐

-   초기 공백 상태
    -   front = rear = 0
-   Index의 순환
    -   front와 rear의 위치가 배열의 마지막 인덱스인 n-1를 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함
    -   이를 위해 나머지 연산자 mod를 사용
-   front 변수
    -   공백 상태와 포화 상태 구분을 쉽게 하기 위해 front가 있는 자리는 사용하지 않고 항상 빈자리로 둠

|         | 삽입 위치               | 삭제 위치                 |
| ------- | ----------------------- | ------------------------- |
| 선형 큐 | rear = rear + 1         | front = front + 1         |
| 원형 큐 | rear = (rear + 1) mod n | front = (front + 1) mod n |

### 우선순위 큐

-   우선순위를 가진 항목들을 저장하는 큐
-   FIFO 순서가 아니라 우선순위가 높은 순서대로 먼저 나가게 된다.
-   적용 분야
    -   시뮬레이션 시스템
    -   네트워크 트래픽 제어
    -   운영체제의 태스크 스케줄링

### 큐의 활용

#### 버퍼

-   데이터를 한 곳에서 다른 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역
-   버퍼링: 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미
-   버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에서 이용된다.
-   순서대로 입력/출력/전달되어야 하므로 FIFO 방식의 자료구조인 큐가 활용된다.