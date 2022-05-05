# 자료구조(DataStructure)_01

## 배열(Array)

-   일정한 자료형의 변수들을 하나의 이름으로 열거하여 사용하는 자료구조
-   아래의 예는 6개의 변수를 사용해야 하는 경우, 이를 배열로 바꾸어 사용하는 것이다.

```python
num0, num1, num2, num3, num4, num5 = 0, 1, 2, 3, 4, 5

num_list = [0, 1, 2, 3, 4, 5]
```

-   프로그램 내에서 여러 개의 변수가 필요할 때, 일일이 다른 변수명을 이용하여 접근하는 것은 매우 비효율적
-   데이터들의 메모리 주소가 연속적으로 할당되어 있음
    -   때문에, 배열은 특정 인덱스에 접근하는 시간이 `O(1)`
        -   배열은 인덱스만 알면 해당 메모리주소를 바로 계산가능 (연속적으로 할당되어 있고, offset 시키면 되므로)
-   선언시 크기를 지정 (고정)
    -   제한된 갯수 이상의 데이터가 저장되는 것이 불가능

<details>
    <summary><strong>Java에서 리스트를 구현할 때,</strong> ArrayList와 LinkedList를 통해 구현하는 2가지 방법이 존재한다. 이 중 ArrayList로 구현된 리스트에 계속해서 엘리먼트를 추가하면 어떻게 될까?</summary>
    <div>
        <strong>ArrayList</strong>는 배열에 의해 동작하는데, <strong>배열은 동적으로 크기를 늘릴 수 없으므로</strong> ArrayList는 내부적으로 크기가 더 큰 배열을 생성해 가진 데이터를 모두 옮기고 새로운 데이터를 여기 추가한다.
    </div>
</details>






## 연결 리스트(Linked-List)

-   배열에 다음 노드의 주소를 참조시켜 연결시킨 자료형
    -   때문에, 연결리스트는 특정 인덱스에 접근하는 시간이 `O(N)`
    -   저장의 한계가 없다.
-   이전 자료의 데이터를 알 수 없음
-   `Doubly Linked-List`
    -   기존 연결 리스트의 단점을 개선하기 위해, 이전 데이터 주소를 함께 저장
    
    ```python
    class Node:
        def __init__(self, data):
            self.data = data
            self.next = None
            self.prev = None
       
    
    class DoublyLinkedList:
        def __init__(self):
            self.head = None
            
        def push(self, value):
            new = Node(value)
            new.next = self.head
            if self.head:
                self.head.prev = new
            self.head = new
            
        def print_list(self, node):
            lst = []
            while node:
                lst.append(node.data)
                node = node.next
            print(lst)
                
                
    dl = DoublyLinkedList()
    dl.push(12)
    dl.push(8)
    dl.push(62)
    dl.print_list(dl.head)  # [62, 8, 12]
    
    ```
    
    