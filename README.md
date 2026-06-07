# 🌲 [시험 대비] 이진 검색 트리(BST) & 이진 트리 소스 코드 완전 분석
> 파이썬 기초 개념과 9주~14주차의 내용 요약(코드,개념)☺️

[Chapter 09 바로가기](#-chapter-09-정렬-알고리즘-심화-분석-week9)  
[Chapter 10 바로가기](#-chapter-10트리tree와-이진-검색-트리bst의-핵심-요구-조건-week10)  
[Chapter 11 바로가기](#-chapter-11-균형-색인검색트리-week11)  
[Chapter 12 바로가기](#-chapter-12-해시테이블-week12)  

---
##  파이썬 기초 개념
**  __: 특정 상황에서만 실행되는 함수 **  

**  _: 접근권한이 private **  

** self: 이 클래스에 만든 객체 인스턴스 고유의 저장공간에 접근 가능 **

---

# 📖 Chapter 09. 정렬 알고리즘 심화 분석 (week9) 
> 1. 기본 정렬 알고리즘 (Selection, Bubble, Insertion)

---

## 1) 선택 정렬 (Selection Sort)💡 
> 시험 적중 포인트:데이터가 이미 정렬되어 있든, 역순이든 상관없이 항상 일정한 비교 횟수($\Theta(n^2)$)를 가진다는 점이 참/거짓 문제로 단골 출제됩니다.
 
```Python
def selectionSort(A, reverse=False):
    n = len(A)
    # len(A)-1 만큼 반복하는 이유는 마지막에 남은 1개는 자동으로 정렬되기 때문
    for i in range(n - 1):
        ext_idx = i  # 현재 라운드에서 확정할 위치를 일단 최소/최대 인덱스로 설정
        
        # 아직 정렬되지 않은 나머지 배열을 순회하며 진짜 최소/최대값 탐색
        for j in range(i + 1, n):
            if not reverse:
                # 오름차순(기본값): 가장 작은 값을 찾아서 인덱스 갱신
                if A[j] < A[ext_idx]:
                    ext_idx = j
            else:
                # 내림차순(reverse=True): 가장 큰 값을 찾아서 인덱스 갱신
                if A[j] > A[ext_idx]:
                    ext_idx = j
                    
        # 탐색이 끝나면 확정 위치(i)의 요소와 찾은 요소(ext_idx)를 서로 교환
        A[i], A[ext_idx] = A[ext_idx], A[i]
```

## 2) 버블 정렬 (Bubble Sort)💡 
>시험 적중 포인트:매 라운드가 끝날 때마다 맨 끝자리부터 정렬이 확정되어 나갑니다. (뒤에서부터 채워짐)인접한 요소끼리 계속 자리를 바꾸기 때문에 비교 기반 정렬 중 연산 오버헤드(스왑 횟수)가 가장 큰 편에 속합니다.
```Python
def bubbleSort(A, reverse=False):
    n = len(A)
    # stop은 정렬 작업을 중단할 맨 끝 지점 (라운드가 돌 때마다 앞으로 한 칸씩 당겨짐)
    for stop in range(n - 1, 0, -1):
        # 처음부터 stop 직전 항목까지 옆 자리 원소와 하나하나 비교
        for i in range(stop):
            if not reverse:
                # 오름차순: 앞의 항목이 뒤의 항목보다 크면 서로 자리 바꿈
                if A[i] > A[i + 1]:
                    A[i], A[i + 1] = A[i + 1], A[i]
            else:
                # 내림차순: 앞의 항목이 뒤의 항목보다 작으면 서로 자리 바꿈
                if A[i] < A[i + 1]:
                    A[i], A[i + 1] = A[i + 1], A[i]
```

## 3) 삽입 정렬 (Insertion Sort)💡 
>시험 적중 포인트:"최선 시간 복잡도 $O(n)$" 조건이 서술형/객관식 불문 1순위 타깃입니다. 데이터가 이미 정렬되어 있는 상태라면 while문 조건이 바로 깨지면서 단 한 번씩만 비교하고 끝나기 때문입니다.
```Python

def insertionSort(A, reverse=False):
    # 0번 인덱스는 이미 정렬된 상태로 보고, 1번 인덱스(두 번째)부터 시작
    for i in range(1, len(A)):
        newItem = A[i]  # 이번 라운드에 정렬 위치를 찾아 끼워 넣을 타깃 데이터
        loc = i - 1     # 이미 정렬된 앞쪽 영역의 가장 맨 끝 경계점
        
        if not reverse:
            # 오름차순: 정렬 영역 안에서 내 자리(newItem보다 작은 값의 뒤)를 찾을 때까지 순회
            while loc >= 0 and newItem < A[loc]:
                A[loc + 1] = A[loc]  # 나보다 큰 값들은 한 칸씩 뒤로 밀어내어 빈자리 생성
                loc -= 1             # 더 왼쪽 자리와 비교하기 위해 이동
        else:
            # 내림차순: 정렬 영역 안에서 newItem보다 큰 값의 뒤를 찾을 때까지 순회
            while loc >= 0 and newItem > A[loc]:
                A[loc + 1] = A[loc]  # 작은 값들을 한 칸씩 뒤로 밀어냄
                loc -= 1
                
        # while문 조건 탈출 시 loc은 끼워 넣을 위치보다 한 칸 왼쪽에 가 있으므로 loc + 1에 삽입
        A[loc + 1] = newItem
```

---

## 🚀 2. 고급 정렬 알고리즘 (Merge, Quick, Heap, Shell)

## 1) 병합 정렬 (Merge Sort)💡
> 시험 적중 포인트:"제자리 정렬(In-place Sort)이 아니다"라는 문장의 진위를 고르는 문제가 무조건 나옵니다. 쪼개진 배열들을 합칠 때 원본 배열이 아닌 새로운 리스트(추가 메모리 공간) 공간이 반드시 필요하다는 치명적인 특징을 가집니다.
```Python
def mergeSort(A):
    # 원소 개수가 1개 이하가 될 때까지 쪼개는 기저 조건
    if len(A) > 1:
        mid = len(A) // 2           # 리스트를 반으로 나누기 위한 중간 인덱스 계산
        left = mergeSort(A[:mid])   # 전반부 영역을 슬라이싱하여 재귀적으로 계속 분할 및 정렬
        right = mergeSort(A[mid:])  # 후반부 영역을 슬라이싱하여 재귀적으로 계속 분할 및 정렬
        return merge(left, right)   # 쪼개져서 각각 정렬되어 올라온 두 그룹을 최종 병합
    return A                        # 원소가 1개일 때는 쪼갤 수 없으므로 자기 자신 그대로 반환

def merge(left, right):
    result = []      # 두 그룹을 순서대로 정렬해서 합쳐 담을 새로운 임시 공간 공간 (In-place 아님)
    i = j = 0        # i는 left 배열 탐색용 인덱스, j는 right 배열 탐색용 인덱스
    
    # 두 리스트 모두에 비교할 원소가 남아있는 동안 반복
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])  # 왼쪽 원소가 더 작거나 같으면 결과에 담고 왼쪽 인덱스 증가
            i += 1
        else:
            result.append(right[j])  # 오른쪽 원소가 더 작으면 결과에 담고 오른쪽 인덱스 증가
            j += 1
            
    # 한쪽 배열이 먼저 다 비워졌을 경우, 다른 한쪽에 남아있는 나머지 자투리를 통째로 병합
    result.extend(left[i:])   # 왼쪽 리스트에 남은 요소들 털어 넣기 (없으면 공백 추가됨)
    result.extend(right[j:])  # 오른쪽 리스트에 남은 요소들 털어 넣기
    return result             # 정렬 완료된 병합 리스트 반환
```

## 2) 퀵 정렬 (Quick Sort) - 제자리 정렬(In-place) 버전💡 
> 시험 적중 포인트:최악의 시간 복잡도 $O(n^2)$ 조건: 이미 정렬되어 있거나 역순인 상태에서 맨 앞이나 맨 뒤 원소를 피봇으로 고르면, 트리가 한쪽으로 완전히 치우치는 편향(Skewed) 트리가 되어 성능이 $O(n^2)$으로 대폭락합니다.partition 함수 내에서 이뤄지는 교환 논리 구조 메커니즘을 빈칸 채우기로 물어볼 수 있습니다.
```Python
def quickSortInPlace(A, p, r):
    # p(시작 인덱스)가 r(끝 인덱스)보다 작을 때만 실행 (원소가 2개 이상일 때)
    if p < r:
        q = partition(A, p, r)        # 피봇(기준)을 중심으로 좌우 분할 후 피봇이 확정된 위치 인덱스(q) 반환
        quickSortInPlace(A, p, q - 1)  # 피봇 왼쪽 영역(피봇보다 작은 그룹)에 대해 재귀적으로 퀵 정렬
        quickSortInPlace(A, q + 1, r)  # 피봇 오른쪽 영역(피봇보다 큰 그룹)에 대해 재귀적으로 퀵 정렬

def partition(A, p, r):
    x = A[r]     # 맨 오른쪽 끝 원소를 기준 원소(Pivot)로 채택
    i = p - 1    # i는 피봇보다 작은 값들이 쌓일 구역의 오른쪽 경계선 인덱스
    
    # 맨 왼쪽부터 피봇 직전 원소까지 j를 이동시키며 비교 스캔 시작
    for j in range(p, r):
        if A[j] < x:  # 현재 확인 중인 원소가 피봇보다 작다면
            i += 1    # 작은 원소 구역의 경계선을 한 칸 오른쪽으로 넓히고
            A[i], A[j] = A[j], A[i]  # 경계선 새로 들어온 자리에 작은 원소를 스왑해서 배치
            
    # 전체 순회가 끝나면 큰 원소 구역이 시작되는 지점인 (i + 1) 자리와 맨 끝의 피봇(r)을 교환
    A[i + 1], A[r] = A[r], A[i + 1]  # 이로써 피봇의 좌우 분할 완성 및 위치가 최종 확정됨
    return i + 1  # 최종 확정된 피봇의 위치 인덱스를 반환
```

## 3) 힙 정렬 (Heap Sort)💡 
> 시험 적중 포인트:트리 인덱스 연산 공식 암기는 필수입니다. 0번 인덱스가 루트 노드일 때 자식 노드의 위치는 아래와 같습니다.$\text{왼쪽 자식} = 2 \times \text{부모} + 1$$\text{오른쪽 자식} = 2 \times \text{부모} + 2$배열을 Max Heap으로 빌드한 직후의 초기 힙 형태를 묻는 서술형을 주의 깊게 보셔야 합니다.
```Python
def heapSort(A):
    n = len(A)
    # 1단계: 무작위 배열을 최대 힙(Max Heap) 구조로 빌드
    # 완전 이진 트리의 마지막 노드의 부모 노드 인덱스부터 시작해서 역순으로 루트 노드까지 연산 수행
    for i in range((n - 2) // 2, -1, -1):
        percolateDown(A, i, n - 1)    # 아래로 내려가며 자식들과 크기를 비교해 힙 속성 만족시키기
        
    # 2단계: 최대값(루트 노드인 A[0])을 추출하여 맨 뒤로 보내고 힙 범위를 축소하며 정렬 마무리
    for last in range(n - 1, 0, -1):
        A[last], A[0] = A[0], A[last]  # 맨 위에 있는 최대값(A[0])과 맨 끝 노드(A[last])를 교환
        percolateDown(A, 0, last - 1)  # 교환 후 흐트러진 루트 노드를 중심으로 나머지(0 ~ last-1) 구역 최대 힙 재조정

def percolateDown(A, parent, end):
    left = 2 * parent + 1   # 왼쪽 자식 노드의 배열 인덱스 계산
    right = 2 * parent + 2  # 오른쪽 자식 노드의 배열 인덱스 계산
    larger_child = left     # 자식 노드 중 더 큰 값을 가진 쪽을 고르기 위해 일단 왼쪽으로 임시 설정
    
    if left <= end:  # 왼쪽 자식이 존재할 때만 비교 수행 (트리 범위 체크)
        # 오른쪽 자식 노드도 존재하고, 심지어 오른쪽 자식이 왼쪽 자식보다 값이 더 큰 경우
        if right <= end and A[left] < A[right]:
            larger_child = right  # 비교 대상을 오른쪽 자식으로 변경
            
        # 자식 노드 중 더 큰 값과 현재 부모 노드의 값을 비교
        if A[parent] < A[larger_child]:
            A[parent], A[larger_child] = A[larger_child], A[parent]  # 자식이 더 크면 부모와 자리를 스왑
            percolateDown(A, larger_child, end)  # 자리가 바뀐 자식 노드 위치에서 다시 재귀적으로 아래쪽 힙 조정
```
## 4) 셸 정렬 (Shell Sort)💡 
> 시험 적중 포인트:일반적인 삽입 정렬의 비효율성을 극복하고자 만들어졌습니다. 멀리 떨어진 요소들끼리 먼저 대략 정렬하여 원소의 이동 거리를 혁신적으로 줄이는 원리입니다.갭이 줄어들다가 최종적으로 갭 수열의 마지막 값이 1이 되었을 때는 완전히 일반 삽입 정렬과 동일하게 작동합니다.
```Python
def gapSequence(n):
    H = [1]   # 갭 수열의 최종 마무리 간격은 무조건 1이어야 하므로 1로 시작
    gap = 1
    # Knuth 갭 수열 공식 활용: gap = 3 * gap + 1
    while gap < n // 5:  # 원소 수의 1/5 수준이 적절한 초기 최대 갭의 상한선
        gap = 3 * gap + 1
        H.append(gap)
    H.reverse()  # 큰 간격부터 시작해야 하므로 내림차순으로 수열 뒤집기
    return H

def shellSort(A):
    H = gapSequence(len(A))  # 갭 수열 리스트 생성 (예: [4, 1])
    for gap in H:
        # 갭 크기만큼 나뉜 독립된 각 그룹의 시작점(start)들을 기준으로 순회
        for start in range(gap):
            stepInsertionSort(A, start, gap)  # 갭만큼 떨어진 원소들끼리만 묶어 삽입 정렬 수행

def stepInsertionSort(A, start, gap):
    # 일반 삽입 정렬에서 '1'씩 이동하던 자리를 모두 'gap' 간격으로 대체한 구조
    for i in range(start + gap, len(A), gap):
        newItem = A[i]
        loc = i - gap  # 직전에 정렬된 내 그룹 안의 경계 요소
        
        # 내 그룹 안에서 나보다 큰 원소들을 발견하면 gap만큼 뒤로 밀어내며 내 자리 탐색
        while loc >= 0 and newItem < A[loc]:
            A[loc + gap] = A[loc]
            loc -= gap  # 그룹 내 다음 왼쪽 원소와 비교하기 위해 gap만큼 앞으로 이동
            
        A[loc + gap] = newItem  # 탐색 완료된 빈 자리에 타깃 데이터 삽입
```

---

## 🔢 3. 특수 정렬 알고리즘 (Counting, Radix, Bucket)1) 계수 정렬 (Counting Sort)💡 
> 시험 적중 포인트:"비교 연산을 전혀 하지 않는 정렬"치명적인 단점 한계점: 양수/정수 데이터만 원활하게 다룰 수 있으며 (인덱스 활용 목적), 데이터의 개수($n$)가 적어도 데이터의 최댓값($k$)이 너무 크면 카운팅 배열 $C$를 만드느라 극심한 공간 낭비가 생깁니다.
```Python
def countingSort(A):
    k = max(A)                          # 원본 배열에서 가장 큰 최대값을 찾아 데이터 범위 지정
    C = [0 for _ in range(k + 1)]       # 0부터 최대값(k)까지의 인덱스를 담을 수 있는 빈도 카운팅 배열 생성
    
    # 1단계: 원본 데이터를 훑으며 각 숫자가 등장할 때마다 인덱스 카운터 누적증가
    for j in range(len(A)):
        C[A[j]] += 1
        
    # 2단계: 카운팅 배열을 앞 노드와 결합하여 '누적합 배열'로 변환
    # 이 과정을 거치면 C[i] 값은 i라는 숫자가 정렬 배열에서 들어갈 마지막 인덱스 맵 정보를 지님
    for i in range(1, k + 1):
        C[i] = C[i] + C[i - 1]
        
    B = [0 for _ in range(len(A))]      # 정렬된 최종 결과물들을 배치해 담을 동일 크기의 빈 리스트 생성
    
    # 3단계: 원본 데이터의 '맨 끝 원소부터 앞으로 역순 순회'하며 정렬 위치 배치
    # ⚠️ 중요: 역순 순회를 해야만 동일한 키 값을 가졌을 때 원래 순서가 유지되는 '안정 정렬(Stable Sort)' 조건 만족!
    for j in range(len(A) - 1, -1, -1):
        B[C[A[j]] - 1] = A[j]           # 누적합 값(C[A[j]])에서 1을 뺀 자리가 실제 정렬 결과 배열(B)의 절대 주소
        C[A[j]] -= 1                    # 중복된 숫자가 나올 때를 대비하여 사용한 누적 카운트 값을 한 칸 내려줌
        
    return B                            # 최종 정렬이 완료된 새 배열 B 반환
```

## 2) 기수 정렬 (Radix Sort)💡 
> 시험 적중 포인트:가장 낮은 자릿수(1의 자리)부터 높은 자릿수 방향으로 차근차근 계수 정렬을 수행하는 방식을 LSD(Least Significant Digit)라고 부릅니다. 자릿수 단위 기반 정렬의 개념적 절차를 이해하고 있어야 합니다.
```Python
import math

def radixSort(A):
    maxValue = max(A)                             # 배열 내 최대값을 찾아 몇 자릿수까지 루프를 돌지 계산 준비
    numDigits = math.ceil(math.log10(maxValue))   # 자릿수 구하기 공식 (예: 123 -> log10 결과값 올림 시 3자릿수)
    bucket = [[] for _ in range(10)]              # 0부터 9까지의 자릿수 매칭을 위한 10개의 2차원 버킷 바구니 리스트 생성
    
    # 1의 자리(i=0), 10의 자리(i=1), 100의 자리(i=2) 순으로 낮은 자릿수부터 반복 실행
    for i in range(numDigits):
        for x in A:
            y = (x // 10**i) % 10                 # 특정 자릿수에 위치한 정수값 1자리 분리 추출 연산
            bucket[y].append(x)                   # 추출한 자릿값(y)과 일치하는 버킷 바구니 위치에 원소 임시 적재
            
        A.clear()                                 # 원본 자리를 비우고 자릿수 정렬 기준으로 재생성 준비
        for j in range(10):
            A.extend(bucket[j])                   # 0번 버킷부터 9번 버킷까지 차례대로 꺼내 원본 리스트 뒤에 이어 붙이기
            bucket[j].clear()                     # 다음 상위 자릿수 연산을 위해 사용했던 버킷 깨끗하게 청소
```

## 3) 버킷 정렬 (Bucket Sort)💡 
> 시험 적중 포인트:실수(Float) 데이터를 다룰 때 효율성이 극대화됩니다.최악의 시간 복잡도 $O(n^2)$ 조건: 모든 실수 데이터가 분포도 고르지 못해 단 하나의 특정 버킷에만 전부 쏠려 들어간 경우, 그 안에서 삽입 정렬을 돌리느라 성능이 $O(n^2)$으로 변질됩니다.
```Python
import math

def bucketSort(A):
    if len(A) == 0: return A                     # 빈 배열 입력 시 예외 처리 및 조기 종료
    n = len(A)
    B = [[] for _ in range(n)]                   # 정렬할 원소 개수(n)만큼 구역별 슬롯 버킷 리스트 생성
    
    # 1단계: 원소 분배 단계
    for i in range(n):
        # 0 ~ 1 사이 영역의 실수를 버킷 범위 정수형 인덱스로 형변환 계산
        idx = math.floor(n * A[i])
        B[idx].append(A[i])                      # 계산된 소속 버킷 위치에 소수점 데이터 분류 적재
        
    A.clear()                                    # 버킷 병합 취합을 위해 원본 리스트 클리어
    
    # 2단계: 버킷별 개별 정렬 및 병합 단계
    for i in range(n):
        insertionSort(B[i])                      # 개별 버킷 안에 담긴 소수 소집단 데이터를 삽입 정렬로 개별 정렬
        A.extend(B[i])                           # 정렬 완료된 버킷 요소를 최종 리스트에 차례대로 누적 결합

def insertionSort(A):                            # 버킷 정렬 내부 보조를 위한 표준 삽입 정렬 구현
    for i in range(1, len(A)):
        newItem = A[i]
        loc = i - 1
        while loc >= 0 and newItem < A[loc]:
            A[loc + 1] = A[loc]
            loc -= 1
        A[loc + 1] = newItem
```

---

## ⏱️ 4. 모든 정렬 알고리즘 종합 성능 측정 스크립트과제 제출용 스크립트 소스코드입니다.
> 파일로 저장해서 터미널에서 구동하거나 주피터 노트북 환경에 붙여넣어 실행하면 성능 측정 데이터 보고서 형태로 출력됩니다.

```Python
import random
import time
import math
import sys

### 퀵 정렬의 최악 조건 처리 시 파이썬 기본 재귀 제한 방지선 해제
sys.setrecursionlimit(20000)

### ==========================================
### 모든 정렬 함수의 매개변수 구조 통일 구현부
### ==========================================


def selectionSort(A):
    n = len(A)
    for i in range(n - 1):
        min_idx = i
        for j in range(i + 1, n):
            if A[j] < A[min_idx]:
                min_idx = j
        A[i], A[min_idx] = A[min_idx], A[i]

def insertionSort(A):
    for i in range(1, len(A)):
        newItem = A[i]
        loc = i - 1
        while loc >= 0 and newItem < A[loc]:
            A[loc + 1] = A[loc]
            loc -= 1
        A[loc + 1] = newItem

def bubbleSort(A):
    n = len(A)
    for stop in range(n - 1, 0, -1):
        for i in range(stop):
            if A[i] > A[i + 1]:
                A[i], A[i + 1] = A[i + 1], A[i]

def mergeSort(A):
    if len(A) > 1:
        mid = len(A) // 2
        left = mergeSort(A[:mid])
        right = mergeSort(A[mid:])
        
        result = []
        i = j = 0
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
        result.extend(left[i:])
        result.extend(right[j:])
        return result
    return A

def quickSort(A):
    if len(A) > 1:
        pivot = A[0]
        tail = A[1:]
        left = [x for x in tail if x <= pivot]
        right = [x for x in tail if x > pivot]
        return quickSort(left) + [pivot] + quickSort(right)
    return A

def heapSort(A):
    n = len(A)
    for i in range((n - 2) // 2, -1, -1):
        _percolateDown(A, i, n - 1)
    for last in range(n - 1, 0, -1):
        A[last], A[0] = A[0], A[last]
        _percolateDown(A, 0, last - 1)

def _percolateDown(A, parent, end):
    left = 2 * parent + 1
    right = 2 * parent + 2
    larger = left
    if left <= end:
        if right <= end and A[left] < A[right]:
            larger = right
        if A[parent] < A[larger]:
            A[parent], A[larger] = A[larger], A[parent]
            _percolateDown(A, larger, end)

def shellSort(A):
    H = [1]
    gap = 1
    while gap < len(A) // 5:
        gap = 3 * gap + 1
        H.append(gap)
    H.reverse()
    
    for gap in H:
        for start in range(gap):
            for i in range(start + gap, len(A), gap):
                newItem = A[i]
                loc = i - gap
                while loc >= 0 and newItem < A[loc]:
                    A[loc + gap] = A[loc]
                    loc -= gap
                A[loc + gap] = newItem

def countingSort(A):
    k = max(A)
    C = [0 for _ in range(k + 1)]
    for j in range(len(A)): C[A[j]] += 1
    for i in range(1, k + 1): C[i] = C[i] + C[i - 1]
    B = [0 for _ in range(len(A))]
    for j in range(len(A) - 1, -1, -1):
        B[C[A[j]] - 1] = A[j]
        C[A[j]] -= 1
    return B

def radixSort(A):
    maxValue = max(A)
    numDigits = len(str(abs(maxValue)))
    bucket = [[] for _ in range(10)]
    for i in range(numDigits):
        for x in A:
            y = (x // 10**i) % 10
            bucket[y].append(x)
        A.clear()
        for j in range(10):
            A.extend(bucket[j])
            bucket[j].clear()
```
---

# 📖 Chapter 10.트리(Tree)와 이진 검색 트리(BST)의 핵심 요구 조건 (week10)

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
> 💡 일상생활로 비유
마트에 무인 보관함 키오스크(insert)가 있습니다.

사용자가 키오스크 화면에 물건(newItem)을 올리고 "보관" 버튼을 누릅니다.

키오스크 내부 시스템(insert)은 관리자 전용 프로그램(_insertItem)을 켭니다. 그러고는 "현재 보관함들의 총괄 데이터(self._root)"와 "손님 물건"을 넘겨줍니다.

프로그램이 빈칸을 찾아서 물건을 쏙 넣고, "영수증(업데이트된 보관함 상태)"을 새로 뽑아서 키오스크 메인 화면(self._root)에 갱신합니다.  

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
