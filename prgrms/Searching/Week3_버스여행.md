## Week3_버스여행



### 문제 이해

> - **결과**
>   : 정류장 표지판(signs)이 매개변수로 주어질 때, 특정 정류장 A에서 특정 정류장 B로 도달할 수 있는지를 표시하여 return 하는 solution 함수를 완성해 주세요.
>
> - **제한사항**
> > - N : 100 이하의 자연수
> > - 정류장 표지판(signs)는 2차원 배열이며, 1 또는 0으로만 이루어져 있습니다. 단, i번째 줄의 i번째 숫자는 항상 0입니다.
>
> - **예시**
>
>   ```markdown
>   
>   ```
>



### 문제 해결

> \# 풀이 1 : 재귀 호출을 이용한 DFS 이용
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 

---

```python
def recursive(start, end, bus_course, signs):
    cand = [[i, j] for i, j in bus_course if i == end and signs[start - 1][j - 1] == 0]
    for coord in cand:
        end = coord[1]
        signs[start - 1][end - 1] = 1
        recursive(start, end, bus_course, signs)
    return 0

def solution(n,signs):
    bus_course = []
    for i in range(n):
        bus_course += [[i + 1, j + 1] for j in range(n) if signs[i][j] == 1]
    for start, end in bus_course:
        recursive(start, end, bus_course, signs)
    return signs
```

- 정확성 테스트는 all pass, 테스트 케이스에서 터짐
- 처음에 생각했던 재귀호출을 이용한 DFS 방법
- bus_course를 생성하는 부분에서 시간복잡도가 터지는 것 같다.
- O(n^3) 이상의 시간복잡도를 가진다는 것을 캐치하지 못함.
- 로직을 짜나감에 있어서 좀 더 편한 방식으로 데이터를 재가공할때, 시간복잡도를 잘 계산하는 습관을 갖자.
- bus_course를 구성하는 부분의 시간복잡도는 최약의 경우 O(n^4) [모든 좌표가 1인 경우]

---

```python
def recursive(start, route, signs):
    temp = [end for end, sign in enumerate(signs[route]) if sign and not signs[start][end]]
    for end in temp:
        signs[start][end] = 1
        recursive(start, end, signs)
    return 0

def solution(n,signs):
    for start in range(n):
        stack = [end for end, sign in enumerate(signs[start]) if sign]
        for route in stack:
            recursive(start, route, signs)
    return signs
```

- bus_course를 사용하지 않는 로직으로 변경했을 때는 통과하였음.
- 굳이 자료를 새롭게 가공하지 않더라도 풀 수 있는 방법이 없는지 생각해보자.
- 최대한 간단하게.
- 필요한 자료가 어떤 것인지 명확하게 캐치하는 연습을 하자.



## 라이브 코딩

> #### Check_point
>
> - 전형적인 그래프 문제
> - 문제를 읽는 것보다 제한 사항을 먼저 보는 것이 중요하다.
> - n, m 값들을 확인하여 어떤 시간복잡도로 풀어야 하는지 캐치를 하자.
> - 이 문제는 N이 100이하의 자연수면, O(n^3)으로 풀어도 풀 수 있다는 뜻.
> - 만약 n이 1,000,000이면 최대 O(n log n)안에 풀어야 한다 라고 알 수 있어야 함.
> - 이런식으로 쓰지 말아야 할, 써야할 알고리즘을 필터링 하도록 한다.

> #### BFS : Queue를 이용한 풀이

```python
# BFS, DFS로 풀 수 있는 문제
# BFS를 이용하여 풀어보도록 함.
# BFS는 Queue를 이용한다.
# DFS는 Stack을 이용. (마지막에 방문한 곳부터 깊게 나아간다.)

from collections import deque

def solution(n, signs):
    for start in range(n): # 시작 정류장 번호
        # 갈 수 있는 정류장들을 queue에 담는다.
        queue = deque([end for end, sign in enumerate(signs[start]) if sign])
        while queue:
            route = queue.popleft() # 중간 정류장
            
            for end, sign in enumerate(signs[route]):
                if sign and not signs[start][end]: # 이미 방문한 곳은 또 가지 않도록 체크
                    signs[start][end] = 1 # 방문한 곳이라고 체크한다
                    queue.append(end)
            
    return signs
```

> #### DFS : Stack을 이용한 풀이

```python
# BFS, DFS로 풀 수 있는 문제
# BFS를 이용하여 풀어보도록 함.
# BFS는 Queue를 이용한다.
# DFS는 Stack을 이용. (마지막에 방문한 곳부터 깊게 나아간다.)

def solution(n, signs):
    for start in range(n): # 시작 정류장 번호
        # 갈 수 있는 정류장들을 queue에 담는다.
        stack = [end for end, sign in enumerate(signs[start]) if sign]

        while stack:
            route = stack.pop() # 중간 정류장
            
            for end, sign in enumerate(signs[route]):
                if sign and not signs[start][end]: # 이미 방문한 곳은 또 가지 않도록 체크
                    signs[start][end] = 1 # 방문한 곳이라고 체크한다
                    stack.append(end)
            
    return signs
```

### Check_point

> 1. DFS는 스택을, BFS는 Queue를 사용하며, 그 원리에 대해 이해를 하자.
> 2. 루프를 돌릴 때 시간 복잡도를 계산하는 연습을 하자.
> 3. 최대한 주어진 자료를 이용하도록 하자. (주어진 자료 2차 가공하는 습관 x)
> 4. 로직에 필요한 자료가 어떤 것인지 캐치하는 연습을 하자.
> 5. 제한사항을 보고 어떤 시간복잡도의 알고리즘을 사용해야하는지 캐치하는 연습을 하자.



### 참고 자료

