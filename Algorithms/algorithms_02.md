# Algorithms_02

## 2차원 배열(2-Dimensional Array)

-   1차원 리스트를 묶은 리스트
-   2차원 이상의 다차원 리스트는 차원에 따라 `Index`를 선언
-   2차원 리스트의 선언: 행과 열을 필요로 함
-   Python에서는 데이터 초기화를 통해 변수선언과 초기화가 가능함

>    지그재그 순회

```python
for i in range(n):
    for j in range(m):
        Array[i][j + (m-1-2*j) * (i%2)]
```

>   델타를 이용한 2차 배열 탐색

-   2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법

```
arr[0...N-1][0...N-1]  # N x N 배열
di[] <- [-1, 1, 0, 0]  # 상하좌우
dj[] <- [0, 0, -1, 1]
for i : 1 -> N-1:
	for j : 1 -> N-1:
    	for k in range(4):
        	ni <- i + di[k]
        	nj <- j + dj[k]
        	if 0 <= ni < N and 0 <= nj < N  # 유효한 인덱스
        		test(arr[ni][nj])
```

>   전치 행렬

```python
arr = [[1, 2, 3],[4, 5, 6],[7, 8, 9]]

for i in range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```

## 부분집합(Subset)

-   유한 개의 정수로 이루어진 집합이 있을 때, 이 집합의 부분집합 중에서 그 집합의 원소를 모두 더한 값이 0이 되는 경우가 있는지를 알아내는 문제

-   예를 들어, [-7, -3, -2, 5, 8]이라는 집합이 있을 때, [-3, -2, 5]는 이 집합의 부분집합이면서 합이 0이므로 이 경우의 답은 `참`

-   집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수는 `2^n`개

    ```python
    bit = [0, 0, 0, 0]
    for i in range(2):
        bit[0] = i
        for j in range(2):
            bit[1] = j
            for k in range(2):
                bit[2] = k
                for l in range(2):
                    bit[3] = l
                    print_subset(bit)
    ```

### 비트 연산자

-   `&`: 비트 단위로 AND 연산

    -   `i & (1<<j)`: i의 j번째 비트가 1인지 아닌지 검사

-   `|`: 비트 단위로 OR 연산

-   `<<`: 피연산자의 비트 열을 왼쪽으로 이동시킨다

    -   `1 << n`: 2^n 즉, 원소가 n개일 경우의 모든 부분집합의 수

-   `>>`: 피연산자의 비트 열을 오른쪽으로 이동시킨다

-   보다 간결하게 부분집합을 생성하는 방법

    ```python
    arr = [3, 6, 7, 1, 5, 4]
    
    n = len(arr)
    
    for i in range(1<<n):				 # 1<<n: 부분 집합의 개수
        for j in range(n):				 # 원소의 수만큼 비트를 비교
            if i & (1<<j):				 # i의 j번 비트가 1인 경우
                print(arr[j], end=', ')	 # j번 원소 출력
        print()
    ```

## 검색(Search)

-   저장되어 있는 자료 중에서 원하는 항목을 찾는 작업
-   목적하는 탐색 키를 가진 항목을 찾는 것
    -   탐색 키(search key): 자료를 구별하여 인식할 수 있는 키
-   검색의 종류
    -   순차 검색(sequential search)
    -   이진 검색(binary search)
    -   해시(hash)

### Sequential Search

-   가장 간단하고 직관적인 검색 방법
-   배열이나 연결 리스트 등 순차구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용
-   알고리즘이 단순하여 구현이 쉽지만, 검색 대상의 수가 많은 경우 수행시간이 급격히 증가

>   정렬되어 있지 않은 경우

-   평균 비교 회수: (1/n)*(1+2+...+n) = (n+1)/2

-   시간 복잡도: `O(n)`

```python
def sequentialSearch(a, n, key):
    i = 0
    while i<n and a[i]!=key:  # 인덱스 조건이 항상 먼저 와야 함
        i += 1
    if i<n: return i
    else: return
```

>   정렬되어 있는 경우

-   정렬되어있으므로, 검색 실패를 반환하는 경우 평균 비교 회수가 반으로 줄어든다.
-   시간 복잡도: `O(n)`

```python
def sequentialSearch2(a, n, key):
    i = 0
    while i<n and a[i]<key:
        i += 1
    if i<n and a[i]==key:
        return i
    else:
        return
```

### Binary Search

-   자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
-   이진 검색을 하기 위해서는 자료가 정렬된 상태여야 한다.

```python
def binarySearch(a, n, key):
    start = 0
    end = n-1
    while start <= end:
        middle = (start+end) // 2
        if a [middle] == key:
            return middle
        elif a[middle] > key:
            end = middle - 1
        else:
            start = middle + 1
    return
```

-   재귀함수를 이용한 구현

```python
def binarySearch2(a, low, high, key):
    if low > high:
        return
    else:
        middle = (low+high) // 2
        if key == a[middle]:
            return middle
        elif key < a[middle]:
            return binarySearch2(a, low, middle-1, key)
        elif a [middle] < key:
            return binarySearch2(a, middle+1, high, key)
```

## 선택 정렬(Selection Sort)

-   주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식
-   시간 복잡도: `O(n^2)`

```python
def selectionSort(a, n):
    for i in range(n-1):
        minIdx = i
        for j in range(i+1, n):
            if a[minIdx] > a[j]:
                minIdx = j
        a[i], a[minIdx] = a[minIdx], a[i]
```

-   k번째로 작은 원소를 찾는 알고리즘

    -   1번부터 k번째까지 작은 원소들을 찾아 배열의 앞쪽으로 이동시키고, 배열의 k번째를 반환
    -   k가 비교적 작을 때 유용하며 `O(kn)`의 수행시간을 필요로 한다.

    ```python
    def select(arr, k):
        for i in range(0, k):
            minIdx = i
            for j in range(i+1, len(arr)):
                if arr[minIdx] > arr[j]:
                    minIdx = j
            arr[i], arr[minIdx] = arr[minIdx], arr[i]
        return arr[k-1]
    ```

    