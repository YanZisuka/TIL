# 자료구조(DataStructure)_05

## Trie

<details>
    <summary>우리가 여러 개의 문자열을 가진 리스트를 가지고 있을 때, 어떤 문자열이 그 리스트에 속해있는 지 알아내는 방법은 무엇이 있을까?</summary>
    <div>
        <br>
        단순하게 일일이 비교할 경우, 최대 길이가 m인 문자열 n개의 집합에서 최대길이가 m인 문자열이 포함되는지 확인하려면 최악의 경우 O(nm)의 비교가 필요하다.<br><br>
            문자열을 정렬시킨 뒤, 이진 탐색을 사용하면 O(m log n)으로 단축시킬 수 있으나, 정렬 과정(사전처리) 자체에 O(nm log n)의 시간이 걸린다.<br><br>
    <strong>지금부터 소개할 Trie 자료구조가 문자열 검색에 가장 효율적인 자료구조이다.</strong>
	</div>
</details>



<img src="data_structure_05.assets/images%2Fkimdukbae%2Fpost%2F50497c5d-1598-48ad-b7cd-e60b2df366da%2Fimage.png" alt="img" style="zoom: 50%;" />

-   K진 트리의 구조를 띠고 있는 자료구조
-   우리가 사전에서 단어를 찾는 과정을 생각해보자
    -   'word'라는 단어를 찾을 때, 'w'의 색인을 찾고, 그 다음 'o', ... 순서대로 찾아간다.
    -   이를 컴퓨터에 적용한 자료구조가 바로 트라이
-   'tea'라는 문자열이 입력되었다면 순서대로 't', 'e', 'a' 노드를 추가하고 마지막 `char`에 문자열이 있다는 정보를 포함시키면 된다.
-   이러한 시작 문자열을 **접두어(prefix)** 라고 부른다.
-   트라이 자료구조는 공간을 많이 사용하는 대신, 문자열 길이의 속도만큼 초고속 탐색이 가능하다.
-   **문자열의 길이가 m일 때, 시간복잡도는 `O(m)`**
    -   배열로 구현했을 때, 현재 노드의 위치를 i번째 문자라고 한다면 `Trie[i]`를 탐색하는 것은 `O(1)`에 수행하므로

```python
class Node(object):
    def __init__(self, key, data=None):
        self.key = key
        self.data = data
        self.children = {}


class Trie:
    def __init__(self):
        self.head = Node(None)

    def insert(self, word):
        now = self.head

        for char in word:
            if char not in now.children:
                now.children[char] = Node(char)
            now = now.children[char]
        now.data = word

    def search(self, word):
        now = self.head

        for char in word:
            if char in now.children:
                now = now.children[char]
            else:
                return False
        if now.data:
            return True

    def starts_with(self, prefix):
        now = self.head
        words = []

        for p in prefix:
            if p in now.children:
                now = now.children[p]
            else:
                return None

        now = [now]
        next = []

        while True:
            for v in now:
                if v.data:
                    words.append(v.data)
                next.extend(list(v.children.values()))
            if next:
                now = next
                next = []
            else:
                break

        return words

```

