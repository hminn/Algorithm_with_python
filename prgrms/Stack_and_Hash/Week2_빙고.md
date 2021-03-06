## Week2_빙고



### 문제 이해

> - **결과**
>   : 빙고 게임 보드에 적힌 숫자가 담겨있는 배열 board, 게임 보드에서 순서대로 지운 숫자가 들어있는 배열 nums가 매개변수로 주어질 때, board에서 nums에 들어있는 숫자를 모두 지우면 몇 개의 빙고가 만들어지는지 return
>
> - **예시**
>

| board                                            | nums                       | result |
| ------------------------------------------------ | -------------------------- | ------ |
| [[11,13,15,16],[12,1,4,3],[10,2,7,8],[5,14,6,9]] | [14,3,2,4,13,1,16,11,5,15] | 3      |

### 문제 해결

> \# 풀이 1
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 일단 떠오르는대로 때려박은 풀이.
>   2. 빙고를 카운트 할 수 있는 리스트를 만든다.
>   3. 이중 루프 돌리며 빙고 규칙에 맞게 카운트 진행
>   4. 달성된 빙고 수 카운트

---

```python
# 1차 풀이

def solution(board, nums):
    N = len(board)
    h = [0] * N
    v = [0] * N
    c = [0] * 2

    for i in range(N):
        for j in range(N):
            if board[i][j] in nums:
                h[i] += 1
                v[j] += 1
                if i == j:
                    c[0] += 1
                if i + j == N - 1:
                    c[1] += 1
    
    result = sum([1 if i == N else 0 for i in (h + v + c)])
    return result
```

- 시간 복잡도 이슈로 효율성 테스트를 통과하지 못함.
- List 탐색이 아닌 Set 탐색으로 변경해보려고 함.

```python
# 2차 풀이

from itertools import permutations

def solution(board, nums):
    N = len(board)
    nums = set(nums)
    h = [0] * N
    v = [0] * N
    c = [0] * 2

    for i in range(N):
        for j in range(N):
            if board[i][j] in nums:
                h[i] += 1
                v[j] += 1
                if i == j:
                    c[0] += 1
                if i + j == N - 1:
                    c[1] += 1
    
    result = sum([1 if i == N else 0 for i in (h + v + c)])
    return result
```

- 역시 탐색의 성능은 set이 훨씬 빠름을 다시 한 번 느끼게 되었다.
- 이제부터 in 연산을 진행할 때는 set 자료형으로 변경하여 진행하도록 하자.
- h, v, c 변수에 대한 코드 개선이 가능할 것으로 보임.

---

### Check Point

- In 연산(탐색)은 주저없이 Set 자료형으로 합시다.

---



## 라이브 코딩

### 문제 핵심 파악

> 1. 요소가 선택되었는지 확인하는 것을 O(1)로 푸는 것이 핵심
>
>    : set, dict 와 같은 해시 형태 탐색

### Leader's Code

```python
def solution(board, nums):
    N = len(board)
    nums = set(nums)
    row_check = [0] * N
    col_check = [0] * N
    left_diagonal = 0
    right_diagonal = 0
    
    
    for i in range(N):
        for j in range(N):
            if board[i][j] in nums: # nums가 set 자료형이므로 in 연산이 O(1)
                board[i][j] = 0
                row_check[i] += 1
                col_check[j] += 1
                
                if i == j:
                    left_diagonal += 1
                if i + j == N - 1:
                    right_diagonal += 1
                    
	answer = 0
    answer += len([1 for x in row_check if x == N]) 
    answer += len([1 for x in col_check if x == N])
```



### 참고 자료

