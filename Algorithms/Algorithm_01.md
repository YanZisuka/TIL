# Algorithms_01

## 배열(Array)_01

### 알고리즘

-   유한한 단계를 통해 문제를 해결하기 위한 절차나 방법
-   컴퓨터 분야에서 알고리즘을 표현하는 방법은 크게 두가지
    -   **의사코드(Pseudocode)**와 **순서도**
-   많은 문제에서 성능 분석의 기준으로 알고리즘의 작업량을 비교
-   **시간 복잡도(Time Complexity)**
    -   실제 걸리는 시간을 측정
    -   실행되는 명령문의 개수를 계산
    -   **Big-O Notation**
        -   시간 복잡도 함수 중 가장 큰 영향을 미치는 n에 대한 항만을 표시
        -   계수는 생략

![Big O(시간 복잡도)란?](https://blog.kakaocdn.net/dn/dqt3XN/btqxKAwgT7d/KrLD2p4Ea1VeKzj8jkuLN0/img.jpg)



### 배열(Array)

-   일정한 자료형의 변수들을 하나의 이름으로 열거하여 사용하는 자료구조
-   아래의 예는 6개의 변수를 사용해야 하는 경우, 이를 배열로 바꾸어 사용하는 것이다.

```python
num0, num1, num2, num3, num4, num5 = 0, 1, 2, 3, 4, 5

num_list = [0, 1, 2, 3, 4, 5]
```

-   프로그램 내에서 여러 개의 변수가 필요할 때, 일일이 다른 변수명을 이용하여 접근하는 것은 매우 비효율적
-   데이터들이 RAM 위에 물리적으로 인접해 있음
-   선언시 크기를 지정 (고정)



### 연결 리스트(Linked-List)

-   배열에 다음 데이터의 주소를 참조시켜 연결시킨 자료형
-   이전 자료의 데이터를 알 수 없음
-   `Double Linked-List`
    -   기존 연결 리스트의 단점을 개선하기 위해, 이전 데이터 주소를 함께 저장



### 정렬(Sort)

-   2개 이상의 자료를 특정 기준에 의해 작은 값부터 큰 값, 혹은 그 반대의 순서대로 재배열하는 것

-   키

    -   자료를 정렬하는 기준이 되는 특정 값

-   대표적인 정렬 방식의 종류

    ![img](https://d2.naver.com/content/images/2020/01/img.png)

    -   버블 정렬 (Bubble Sort)
    -   카운팅 정렬 (Counting Sort)
    -   선택 정렬 (Selection Sort)
    -   퀵 정렬 (Quick Sort)
    -   삽입 정렬 (Insertion Sort)
    -   힙 정렬 (Heap Sort)
    -   병합 정렬 (Merge Sort)

#### 버블 정렬(Bubble Sort)

-   인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식
-   정렬 과정
    -   첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막자리까지 이동한다.
    -   한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬된다.
    -   교환하며 자리를 이동하는 모습이 물 위에 올라오는 거품 모양과 같다고 하여 버블 정렬이라고 한다.
-   시간 복잡도
    -   `O(n^2)`: 이중 `for`문
-   Pseudo Code

```
for i:N-1 > 1
	for j:0 > i-1
		if arr[j] > arr[j+1]
			arr[j] <-> arr[j+1]
```

-   안정적(동일한 값을 갖는 요소의 상대적 순서가 유지됨)
-   내부 정렬 알고리즘

#### 카운팅 정렬(Counting Sort)

-   항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬하는 효율적인 알고리즘

-   제한 사항

    -   정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능: 각 항목의 발생 회수를 기록하기 위해, 정수 항목으로 인덱스 되는 카운트들의 배열을 사용하기 때문
    -   카운트를 위한 충분한 공간을 할당하려면 집합 내의 가장 큰 정수를 알아야 한다.

-   과정

    -   1단계: Data에서 각 항목들의 발생 회수를 세고, 정수 항목들로 직접 인덱스 되는 카운트 배열 countArray에 저장한다.

    ![img](https://ichi.pro/assets/images/max/724/1*KU8AtbmRrYCp6Yhvq3T1DQ.png)

    ![img](https://ichi.pro/assets/images/max/724/1*NJx2YVjxBNuhnshg8KtZMQ.png)

    -   정렬된 집합에서 각 항목의 앞에 위치할 항목의 개수를 반영하기 위해 countArray의 원소를 조정한다(누적합).

    ![img](https://ichi.pro/assets/images/max/724/1*Ol87vz2H-GMCb4wRr_pZAw.png)

    -   출력 배열의 `outputArray[countArray[inputArray[value]]-1]`위치에 요소를 배치하고 count 값을 1씩 줄인다.

    ![img](https://ichi.pro/assets/images/max/724/1*V2QfYN8ML_SmOXx4PVZP-w.png)

-   시간 복잡도

    -   `O(n)`

-   안정적

-   내부 정렬 알고리즘이 아님

```python
def Counting_Sort(A, B, k):
# A [] -- 입력 배열 (1 to k)
# B [] -- 정렬된 배열
# C [] -- 카운트 배열

	C = [0] * (k+1)
	
	for i in range(0, len(A)):
		C[A[i]] += 1
		
	for i in range(1, len(C)):
		C[i] += C[i-1]
		
	for i in range(len(B)-1, -1, -1):
		C[A[i]] -= 1
		B[C[A[i]]] = A[i]
```

### 완전탐색(Exhaustive Search)

-   완전 탐색은 문제의 해법으로 생각할 수 있는 모든 경우의 수를 나열해보고 확인하는 기법
-   `Brute-force` 혹은 `Generate-and-test` 라고도 불린다.
-   **경우의 수가 상대적으로 작을 때 유용**하다.
-   모든 경우의 수를 생성하고 테스트하기 때문에 수행 속도는 느리지만, 해답을 찾아내지 못할 확률이 적다.
-   주어진 문제를 풀 때, 우선 완전탐색으로 접근하여 해답을 도출한 후, 성능 개선을 위해 다른 알고리즘을 사용하고 해답을 확인하는 것이 바람직하다.

>   순열 (Permutation)

-   동일한 숫자가 포함되지 않았을 때, `ex. {1, 2, 3}을 포함하는 모든 순열`

```python
for i1 in range(1, 4):
	for i2 in range(1, 4):
		if i2 != i1:
			for i3 in range(1, 4):
				if i3 != i1 and i3 != i2:
					print(i1, i2, i3)
```

### 그리디(Greedy)

-   그리디 알고리즘은 최적해를 구하는 데 사용되는 근시안적 방법
-   여러 경우 중 하나를 결정해야 할 때마다 그 순간 최적이라고 생각되는 것을 선택해 나가는 방식으로 진행하여 최종적인 해답에 도달
-   각 선택의 시점에서 이루어지는 결정은 지역적으로는 최적이지만, 그 선택들을 계속 수집하여 최종적인 해답을 만들었다고 하여 그것이 최적이라는 보장은 없다.
-   일반적으로, 머릿속에 떠오르는 생각을 검증 없이 바로 구현하면 Greedy 접근이 된다.

```python
num = 456789 # Baby-gin 확인할 수
c = [0] * 12

for i in range(6):
    c[num%10] += 1
    num //= 10
    
i = 0
tri = run = 0
while i < 10:
    if c[i] >= 3:  # triplet 조사 후 데이터 삭제
        c[i] -= 3
        tri += 1
        continue
    if c[i] >= 1 and c[i+1] >= 1 and c[i+2] >= 1:  # run 조사 후 데이터 삭제
        c[i] -= 1
        c[i+1] -= 1
        c[i+2] -= 1
        run += 1
        continue
    i += 1
    
if run + tri == 2:
    print("Baby Gin")
else:
    print("Lose")
```

