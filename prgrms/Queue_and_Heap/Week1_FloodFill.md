## Week1_FloodFill



### 문제 이해

> - **결과**
> : n x m 크기 도화지에 그려진 그림의 색깔이 2차원 리스트로 주어집니다. 같은 색깔은 같은 숫자로 나타난다고 할 때, 그림에 있는 영역은 총 몇 개인지 알아내려 합니다.
>
> - **예시**
>   ![image-20200904130152404](C:\Users\143011\AppData\Roaming\Typora\typora-user-images\image-20200904130152404.png)
>   : [[1,2,3], [3,2,1]] 와 같은 2차원 리스트는 위의 이미지처럼 표현할 수 있다.
>   : 총 5개 영역으로 나뉘어져 있으므로 result 는 5이다.
>



### 문제 해결

> #1 
>
> 1. (0,0) 부터 시작하여 양 옆과 위, 아래가 같은 숫자로 이루어져있는지 확인
> 2. adjoin_checker 함수에서 확인 과정 진행
>    2-1. 다른 숫자로 이루어져 있다면? result += 1
>    2-2. 같은 숫자로 이루어져 있다면? 해당 좌표가 counted_coord 에 존재하는지 check
>      2-2-1) 존재하지 않다면result +=1 하고, 같은 숫자로 이루어져있는 좌표를 탐색하며 모두 저장
>      2-2-2) 존재한다면 다음 좌표로 넘어간다.
>    (좌표를 담을 리스트 필요 : 좌표 형태의 튜플을 담고 있는 리스트)
> 3. (n,m) 까지 동일한 과정을 반복한다.

---

\# 풀이 1

```python
# 인접해 있는 좌표 중 동일한 숫자를 가진 좌표가 있는지 확인하는 함수

# coord[0] : x좌표
# coord[1] : y좌표

def adjoin_checker(coord, image, counted_coord, result):
    x, y = coord[0], coord[1]
    m, n = len(image[0]), len(image)
    cd_list = [(i, j) for (i, j) in [(x - 1, y), (x, y - 1), (x + 1, y), (x, y + 1)] if (0 <= i < m) & (0 <= j < n)]
    print("x, y :",x,y)
    print("cd_list :", cd_list)
    for (i, j) in cd_list:
        if image[y][x] == image[j][i]:
            counted_coord.append((i, j))
            #result = adjoin_checker([i, j], image, counted_coord, result)
    
    if (x, y) not in counted_coord:
        return (result + 1)
    return (result)
    
def solution(n, m, image):
    counted_coord = []
    result = 0
    for i in range(m):
        for j in range(n):
            result = adjoin_checker([i, j], image, counted_coord, result)
            print("result :", result)
            print("-------------------")
    return result
```



---

## 라이브코딩

- 채워나가거나 뻗어나가는 유형은 DFS, BFS
- 해당 그림과 같은 유형은 대부분 BFS.

\# 풀이1

```python
# BFS 혹은 DFS를 이용하는 문제.

# DFS = 깊이 우선 탐색 -> 재귀 호출을 많이 사용 
# 파이썬 같은 경우 재귀 호출이 상당히 느리기 때문에 가급적 BFS를 쓰는 것이 좋다.

def solution (n, m, image):
    answer = 0
    # BFS는 Queue를 이용한 알고리즘
    
    for sy in range(n):
        for sx in range(m):
            if image[sy][sx] == -1:
                continue
            target_color = image[sy][sx]
            image[sy][sx] = -1
            queue = [(sy, sx)] # 처음 선택된 좌표
            
            while queue:
                y, x = queue.pop() # list의 pop은 O(n)
                
                # if문은 왼쪽에서 부터 읽기 때문에
                # 인덱스 관련 조건문을 사용할 때는
                # 인덱스 체크 조건을 왼쪽에 넣어준다.
                if y + 1 < n and image[y + 1][x] == target_color :
                    image[y + 1][x] = -1
                    queue.append((y + 1, x))
                if y - 1 >= 0 and image[y - 1][x] == target_color:
                    image[y - 1][x] = -1
                    queue.append((y - 1, x))          
                if x + 1 < m and image[y][x + 1] == target_color :
                    image[y][x + 1] = -1
                    queue.append((y, x + 1))
                if x - 1 >= 0 and image[y][x - 1] == target_color :
                    image[y][x - 1] = -1
                    queue.append((y, x - 1))
            answer += 1
	return answer
```

\# 풀이2 : 코드 개선 및 중복 제거

```python
from collections import deque

def solution (n, m, image):
    answer = 0
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    # BFS는 Queue를 이용한 알고리즘
    
    for sy in range(n):
        for sx in range(m):
            if image[sy][sx] == float('inf'):
                continue
            target_color = image[sy][sx]
            queue = deque([(sy, sx)]) # 처음 선택된 좌표
            
            while queue:
                # deque를 이용한 popleft()는 O(1)
                y, x = queue.popleft() # list의 pop은 O(n)
                
                for dy, dx in directions:
                    py = y + dy
                    px = x + dx
                    if px >= m or px < 0 or py >= n or py < 0:
                        continue
                    if image[py][px] == target_color:
                        image[py][px] = float('inf')
                        queue.append((py, px))
                        
            answer += 1
	return answer
```

- float("inf"), 파이썬에서 가장 큰 수.

- product 함수를 이용하여 2중 루프를 줄일 수는 있으나 추천 x (itertools 모듈 내장함수)
  `for sy, sx in product(range(n), range(m))`

- 지도가 있고 옆으로 퍼져나가며 탐색을 진행해야한다? -> DFS & BFS