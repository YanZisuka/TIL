# Algorithms_04

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

### 재귀호출

-   자기 자신을 호출하여 순환 수행되는 것
-   함수에서 실행해야 하는 작업의 특성에 따라 일반적인 호출방식보다 재귀호출방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성

```python
def fibo(n):
    if n < 2:
        return n
    else:
        return fibo(n-1) + fibo(n-2)
```

### Memoization

-   메모이제이션은 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적 실행속도를 빠르게 하는 기술이며, **동적 계획법**의 핵심이 되는 기술이다.
-   `memoization`은 글자 그대로 해석하면 메모리에 넣기(to put in memory)라는 의미이며 기억되어야 할 것이라는 뜻의 라틴어 memorandum에서 파생되었다.

```python
def fibo(n):
    global memo
    if n >= 2 and n >= len(memo):
        memo.append(fibo(n-1) + fibo(n-2))
    return memo[n]

memo = [0, 1]
```

### DP(Dynamic Programming)

-   동적 계획 알고리즘은 그리디 알고리즘과 같이 **최적화 문제**를 해결하는 알고리즘이다.
-   동적 계획 알고리즘은 먼저 입력 크기가 작은 부분 문제들을 모두 해결한 후 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결, 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘이다.

```python
def fibo(n):
    f = [0, 1]
    
    for i in range(2, n+1):
        f.append(f[i-1] + f[i-2])
        
    return f[n]
```

-   **memoization**을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적이다.
-   재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문이다.

### DFS(Depth First Search)

-   비선형구조인 그래프는 표현된 모든 자료를 빠짐없이 검색하는 것이 중요함
-   **시작 정점의 한 방향으로** 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 **더 이상 갈 곳이 없게 되면**, 가장 **마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서** 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법
-   가장 마지막에 만났던 갈림길의 정점으로 되돌아가므로 스택과 재귀호출을 사용해 구현
-   반복을 이용한 구현
1.   시작 정점 `V`를 결정하여 방문한다.
    
2.   정점 `V`에 인접한 정점 중에서
    
     1. 방문하지 않은 정점 `W`가 있으면, 정점 `V`를 스택에 push하고 정점 `W`를 방문한다. 그리고 `W`를 `V`로 하여 다시 ii.를 반복한다.
    
     2. 방문하지 않은 정점이 없으면, 탐색 방향을 바꾸기 위해 스택을 pop하여 받은 가장 마지막 방문 정점을 `V`로 하여 다시 ii.를 반복한다.
    
3.   스택이 공백이 될 때까지 ii.를 반복한다.

