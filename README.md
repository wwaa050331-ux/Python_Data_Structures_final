# 🌲 [시험 대비] 이진 검색 트리(BST) & 이진 트리 소스 코드 완전 분석

본 문서는 이진 트리 및 이진 검색 트리(BST)의 핵심 요구 조건, 주요 메서드의 내부 파이썬 코드 작동 원리, 그리고 실습 과제 및 핵심 시험 출제 포인트를 완벽하게 정리한 서머리입니다.

---

## 1. 트리(Tree)와 이진 검색 트리(BST)의 핵심 요구 조건

### 📌 트리의 특징 (필수 이론)
* **계층적(Hierarchical) 구조**를 가지는 비선형 데이터 집합체입니다.
* 노드(Node)들과 노드를 연결하는 간선(Edge)으로 구성됩니다.
* 최상위 노드를 **루트(Root)**, 부모가 바로 연결된 상위 노드를 **부모(Parent)**, 하위 노드를 **자식(Child)**이라고 부릅니다.
* 자식이 없는 말단 노드를 **리프(Leaf)** 혹은 단말 노드(Terminal Node)라고 합니다.
* **사이클(Cycle)이 없다**는 것이 가장 큰 특징입니다 (자기 자신으로 돌아오는 경로가 없음).

### 📌 이진 검색 트리(BST)의 3대 성립 조건 (구현의 기초)
이진 트리의 일종으로 검색, 삽입, 삭제가 매우 효율적인 자료구조입니다. 균형이 잘 유지되면 평균적으로 **$O(\log N)$** 시간에 연산이 가능합니다.
1. 각 노드가 **최대 두 개의 자식**만을 가집니다.
2. **왼쪽 자식 노드**의 값은 부모 노드의 값보다 **작습니다**.
3. **오른쪽 자식 노드**의 값은 부모 노드의 값보다 **큽니다**.

---

## 2. 노드 구조와 BST 기본 메서드 코드 분석

### ① 노드 정의 클래스 (`TreeNode`)
노드는 데이터 값(`item`), 왼쪽 자식 가리킴(`left`), 오른쪽 자식 가리킴(`right`)으로 구성됩니다.

```python
class TreeNode:
    def __init__(self, newItem=None, left=None, right=None):
        self.item = newItem  # 노드에 저장할 데이터 (key)
        self.left = left     # 왼쪽 자식 노드 포인터 지시자
        self.right = right   # 오른쪽 자식 노드 포인터 지시자
```

② 노드 삽입 메서드 (insert, _insertItem)
새로운 데이터를 BST 크기 규칙에 맞는 위치로 재귀 탐색하여 단말(Leaf) 위치에 새로 연결해 주는 알고리즘입니다.

```Python
class BinarySearchTree:
    def __init__(self):
        self._root = None  # 트리가 비어있을 때는 root가 None으로 초기화

    def getRoot(self):
        return self._root  # 객체 외부에서 캡슐화된 내부 변수 _root를 반환

    def insert(self, newItem):
        # 루트 노드부터 재귀 함수를 호출하여 적절한 위치에 삽입 후 루트 갱신
        self._root = self._insertItem(self._root, newItem)

    def _insertItem(self, tNode: TreeNode, newItem) -> TreeNode:
        # [Case 1] 빈 자리에 도달(단말 노드 아래)했다면 새로운 노드를 생성하여 반환
        if tNode is None:
            tNode = TreeNode(newItem, None, None)
            
        # [Case 2] 넣으려는 값이 현재 노드보다 작으면 -> 왼쪽 서브트리로 재귀 호출
        elif newItem < tNode.item:
            tNode.left = self._insertItem(tNode.left, newItem)
            
        # [Case 3] 넣으려는 값이 현재 노드보다 크면 -> 오른쪽 서브트리로 재귀 호출
        else:
            tNode.right = self._insertItem(tNode.right, newItem)
            
        return tNode  # 부모 노드에 최종적으로 연결될 수 있도록 현재 노드 위치 반환
```

③ 노드 검색 메서드 (search, _searchItem)
찾고자 하는 key를 가지고 루트 노드부터 대소 비교를 수행하며 일치하는 노드를 리턴합니다.

```Python
    def search(self, key) -> TreeNode:
        return self._searchItem(self._root, key)  # 루트 노드부터 이진 검색 시작

    def _searchItem(self, tNode: TreeNode, key) -> TreeNode:
        if tNode is None:  
            return None  # 트리에 찾는 값이 존재하지 않는 경우
            
        if key == tNode.item:  
            return tNode  # 찾고자 하는 값을 발견한 경우 해당 노드 반환
            
        elif key < tNode.item:  
            return self._searchItem(tNode.left, key)  # 키가 작으면 왼쪽 서브트리로 이동
            
        else:  
            return self._searchItem(tNode.right, key)  # 키가 크면 오른쪽 서브트리로 이동
```

---

## 3. 이진 트리의 3가지 순회(Traversal) 구현 (★ 필수 암기)
트리의 모든 노드를 방문하는 연산으로, 루트(부모) 노드를 방문하는 타이밍에 따라 나뉩니다. 왼쪽 자식 노드는 항상 오른쪽보다 먼저 순회합니다.

```Python
    # 1. 전위 순회 (Preorder): Root -> Left -> Right
    def preorder(self, tNode: TreeNode):
        if tNode is not None:
            print(tNode.item, end=' ')  # [1] 부모 노드 먼저 출력
            self.preorder(tNode.left)   # [2] 왼쪽 자식 서브트리 방문
            self.preorder(tNode.right)  # [3] 오른쪽 자식 서브트리 방문

    # 2. 중위 순회 (Inorder): Left -> Root -> Right
    # ★ 중요 특징: BST에서 중위 순회를 하면 모든 값들이 '오름차순'으로 정렬되어 출력됨
    def inorder(self, tNode: TreeNode):
        if tNode is not None:
            self.inorder(tNode.left)    # [1] 왼쪽 자식 서브트리 먼저 방문
            print(tNode.item, end=' ')  # [2] 부모 노드 출력
            self.inorder(tNode.right)   # [3] 오른쪽 자식 서브트리 방문

    # 3. 후위 순회 (Postorder): Left -> Right -> Root
    def postorder(self, tNode: TreeNode):
        if tNode is not None:
            self.postorder(tNode.left)  # [1] 왼쪽 자식 서브트리 방문
            self.postorder(tNode.right) # [2] 오른쪽 자식 서브트리 방문
            print(tNode.item, end=' ')  # [3] 부모 노드 맨 마지막에 출력
```

---

## 4. [과제-1] 추가 메서드 빈칸 구현 소스 코드 (시험 출제 1순위)
강의 자료에서 공란으로 남겨진 실습용 과제 알고리즘의 전체 완성 코드입니다.

## (9) 리스트로 트리 구성: insert_list()
여러 데이터가 담긴 리스트를 인자로 받아 일괄적으로 트리에 삽입합니다.

```Python
    def insert_list(self, newItemList: list):
        for newItem in newItemList:  # 리스트 요소를 하나씩 반복문으로 순회
            self.insert(newItem)     # 기존에 정의된 insert 연산 호출
```

## (10) 최소값 찾기: min() - 일반(반복문) 버전
BST의 특성상 가장 작은 값은 트리의 최좌측 말단에 위치하므로, 왼쪽 자식이 없을 때까지 루프를 수행합니다.

```Python
    def min(self, tNode):
        if tNode is None:
            return None
        while tNode.left is not None:  # 왼쪽 자식이 연결되어 있는 동안 계속 이동
            tNode = tNode.left         # 왼쪽 포인터를 타고 내려감
        return tNode.item              # 가장 왼쪽 끝 노드의 데이터 값 리턴
```

## (11) 최대값 찾기: max() - 재귀(Recursive) 버전
가장 큰 값은 트리의 최우측 말단에 위치합니다. 재귀 호출 방식을 이용하여 우측 자식이 없을 때까지 타고 내려갑니다.

```Python
    def max(self, tNode):
        if tNode is None:
            return None
        if tNode.right is None:        # 더 이상 오른쪽 자식이 없으면 현재 노드가 최대값
            return tNode.item          # 현재 데이터 값 리턴
        return self.max(tNode.right)   # 오른쪽 자식이 있다면 더 우측으로 재귀 호출
```

## (12) 오름차순 출력: print_asc()
중위 순회(inorder) 알고리즘을 사용하면 자동으로 오름차순 조회가 이루어집니다.

```Python
    def print_asc(self):
        self.inorder(self._root)       # 루트 노드를 기점으로 중위 순회 수행
        print()
```
## (13) 내림차순 출력: print_desc()
방문 순서를 정반대로 정하여 Right -> Root -> Left 순서로 처리하는 역순 순회(_revorder)를 통해 큰 값부터 역순으로 출력합니다.

```Python
    def print_desc(self):
        self._revorder(self._root)     # 루트 노드를 기점으로 역순 순회 수행
        print()

    def _revorder(self, tNode):
        if tNode is not None:          # 노드가 null이 아닐 때만 수행
            self._revorder(tNode.right) # [1] 오른쪽 자식 먼저 방문 (큰 값)
            print(tNode.item, end=' ')  # [2] 부모 노드 출력
            self._revorder(tNode.left)  # [3] 왼쪽 자식 방문 (작은 값)
```

---

## 5. 교수님이 변별력을 위해 출제할 수 있는 심화 포인트  

## ① 노드 삭제 연산(_deleteNode)의 3가지 분기 케이스 (★ 서술형 유력)
노드를 삭제할 때는 트리의 규칙(BST 조건)이 유지되어야 하므로 아래의 세 가지 예외 처리가 필요합니다.

Case 1 (자식이 없는 리프 노드 삭제): 대상 노드를 끊어내고 부모에게 None을 리턴하면 종료됩니다.

Case 2 (자식이 하나만 있는 노드 삭제): 나를 지우고, 내 하나뿐인 자식 노드를 위로 끌어올려서 부모와 연결해 줍니다.

Case 3 (자식이 둘 다 있는 노드 삭제): 삭제할 노드의 오른쪽 서브트리에서 최소값 노드(deleteMinItem)를 찾아와 그 값을 지우고자 하는 노드 자리에 덮어쓴 뒤, 아래쪽에 원래 복사했던 최소값 노드를 제거해 줍니다.  


## ② 정렬된 데이터 삽입 시 발생하는 편향 트리(Skewed Tree) 현상
성적 관리 실습 결과처럼 이미 순서대로 정렬된 데이터(예: 2022001, 2022002...)를 그대로 트리에 삽입하는 경우, 트리의 균형이 깨지고 오른쪽으로만 일렬로 길게 늘어지는 편향 트리가 형성됩니다.

문제점: 트리의 높이가 전체 데이터 수 N과 동일해져(Tree depth: 74), 탐색 시 장점이었던 O(logN) 성능을 완전히 잃고 선형 탐색과 다름없는 O(N)으로 속도가 저하됩니다.  


## ③ 수식 표기법과 수식 트리 연산 구조
수식을 이진 트리로 변환한 뒤 후위 순회(Postorder Traversal)를 적용하면 스택 구조를 통해 효율적인 컴퓨터 수식 연산이 가능해집니다.

실습에 등장하는 evaluate_binary_tree 메서드는 좌우 서브트리를 재귀적으로 파고든 후 피연산자(숫자)를 마주치면 스택에 추가(append)하고,

연산자를 만나면 스택에서 피연산자 2개를 꺼내(pop) 연산을 수행한 후 결과를 다시 스택에 집어넣는 메커니즘을 가집니다.  

---

## 7. 교수님이 변별력을 위해 출제할 수 있는 심화 포인트
## ① 노드 삭제 연산(_deleteNode)의 3가지 분기 케이스 (★ 서술형 유력)노드를 삭제할 때는 트리의 규칙(BST 조건)이 유지되어야 하므로 아래의 세 가지 예외 처리가 필요합니다.

Case 1 (자식이 없는 리프 노드 삭제): 대상 노드를 끊어내고 부모에게 None을 리턴하면 종료됩니다.  

Case 2 (자식이 하나만 있는 노드 삭제): 나를 지우고, 내 하나뿐인 자식 노드를 위로 끌어올려서 부모와 연결해 줍니다.  

Case 3 (자식이 둘 다 있는 노드 삭제): 삭제할 노드의 오른쪽 서브트리에서 최소값 노드(deleteMinItem)를 찾아와 그 값을 지우고자 하는 노드 자리에 덮어쓴 뒤, 아래쪽에 원래 복사했던 최소값 노드를 제거해 줍니다.  

## ② 정렬된 데이터 삽입 시 발생하는 편향 트리(Skewed Tree) 현상성적 관리 실습 결과처럼 이미 순서대로 정렬된 데이터(예: 2022001, 2022002...)를 그대로 트리에 삽입하는 경우,  

 트리의 균형이 깨지고 오른쪽으로만 일렬로 길게 늘어지는 편향 트리가 형성됩니다.  문제점: 트리의 높이가 전체 데이터 수 $N$과 동일해져(Tree depth: 74),  
 
탐색 시 장점이었던 $O(\log N)$ 성능을 완전히 잃고 선형 탐색과 다름없는 $O(N)$으로 속도가 저하됩니다.  

## ③ 수식 표기법과 수식 트리 연산 구조수식을 이진 트리로 변환한 뒤 후위 순회(Postorder Traversal)를 적용하면 스택 구조를 통해 효율적인 컴퓨터 수식 연산이 가능해집니다.  

실습에 등장하는 evaluate_binary_tree 메서드는 좌우 서브트리를 재귀적으로 파고든 후 피연산자(숫자)를 마주치면 스택에 추가(append)하고,  

연산자를 만나면 스택에서 피연산자 2개를 꺼내(pop) 연산을 수행한 후 결과를 다시 스택에 집어넣는 메커니즘을 가집니다.  

RULE 1에 따라 완결된 형태로 작성되었으며, 별도의 안내 멘트나 추가 질문 없이 종료됩니다.  

---

