## Week2_자물쇠와 열쇠



### 문제 이해

> - **결과**
>   : 열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.
>
> - **제한사항**
>   
> > - key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
>   > - lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
>   > - M은 항상 N 이하입니다.
>   > - key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
>   >   - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.
> 
> - **예시**
>
>   ```markdown
>   
>   ```
>

## 라이브 코딩

### 서론

> - 핵심을 파악하며 읽어 내려가자. (필요없는 부분은 읽지말고 넘어가자)
> - 카카오 문제의 경우, 초반에는 완전 탐색 문제들이 자주 나온다.
> - 특정 알고리즘을 알아야 풀 수 있는 문제들은 잘 나오지 않는다.
> - 오로지 '코딩', '구현력' 등을 잘하는 사람들을 위한 문제들이 나온다.
> - 문제의 Key Point 는 다 해보는 것, 즉 완전탐색을 어떻게 구현할 것인가.
> - 카카오 문제의 경우, 로직 상 해야하는 문제들을 Step by Step으로 정리가 가능하다.



### 문제 이해

> - 맞춰볼 수 있는 모든 경우의 수를 하나씩 다 Check 해본다.
> - 회전, 이동, 홈과 돌기 부분이 맞는지 Check 등 문제에 로직이 구분되어있다.
> - 키를 확장하여 맞춰볼 수 있다. 그림을 그려 확인해보면 확 와닿는다.



### Leader's code

```python
import copy

# 자물쇠 확장 함수
def expand_lock(lock, N, M, size):
    expanded_lock = [[0] * size for  _ in range(size)]
    for y in range(N):
        for x in range(N):
            expanded_lock[y + M - 1][x + M - 1] = lock[y][x]
            
    return expanded_lock

# 시계 방향으로 90도 회전
def rotate(key):
    # *key에서 *은 껍집을 벗겨내는 Unpacking 역할을 한다.
    # (0, 0, 0), (1, 0, 0), (0, 1, 1)]: *key
    # [(0, 1, 0), (0, 0, 1), (0, 0, 1)] : list(zip(*key))
    # [(0, 1, 0), (1, 0, 0), (1, 0, 0)] : reversed
    return [list(reversed(i)) for i in zip(*key)]

def is_unlock(y, x, key, lock, N, M):
    _lock = copy.deepcopy(lock) # 똑같은 값, 다른 주소 형태를 복사    
    for _y in range(M):
        for _x in range(M):
            _lock[_y + y][_x + x] += key[_y][_x]
            # 돌기가 나와있는 부분은 2
            # 돌기가 없는 부분은 1
    for _y in range(N):
        for _x in range(N):
            if _lock[_y + M - 1][_x + M - 1] != 1:
                return False
            
    return True

def solution(key, lock):
    M = len(key) # key의 길이
    N = len(lock) # 자물쇠의 길이
    size = N + (M - 1)
    expanded_lock = expand_lock(lock, N, M, size)
    
    for _ in range(4):
        for y in range(size - M + 1):
            for x in range(size  - M + 1):
                if is_unlock(y, x, key, expanded_lock, N, M):
                    return True
        key = rotate(key)
    return False
```

### Check_Point

> 1. 문제를 잘 읽고 로직을 구분하여 함수로 구현하자. (Devide & Conquer)
>    - 어떤 함수가 필요한지 밑그림을 그린 다음,
>      구체적인 로직을 설계 및 작성하자.
> 2. 그림을 그려 메인 로직의 흐름을 시뮬레이션 해보자.
>    ![image-20200918151302280](C:\Users\143011\AppData\Roaming\Typora\typora-user-images\image-20200918151302280.png)
>    - 어떤 작업을 해야하는지 체크
>    - Loop의 크기, 범위 등을 체크
> 3. 효율성의 문제보다는 구현력에 초점을 맞춰서 풀자.
> 4. 시계방향 90˚  회전 트릭 기억하자.
>    `[list(reversed(i)) for i in zip(*key)]`
>    - python에서의 * : Unpacking의 역할을 해준다.
> 5. 변수의 값을 다른 변수에 할당하고자 할 때 : copy.deepcopy() 메소드를 사용하자.
>    - 변수의 주소값을 넘겨주는 실수를 하지 말자.



### Tips for 완전 탐색 문제

> - 문제를 읽었을 때, 바로 떠오르는 알고리즘이 없고 DP 문제도 아닐 경우는 
>   완전 탐색 문제일 확률이 높다.
> - 완전 탐색 문제는 제한사항의 범위가 작다.
> - 처리해야 될 사항이 구구절절 많은 경우