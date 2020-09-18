## Week2_방문길이



### 문제 이해

> - **결과**
>   : 명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.
>
> - **제한사항**
>   
> > - dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
>   > - dirs의 길이는 500 이하의 자연수입니다.
>   > - 좌표평면의 경계를 넘어가는 명령어는 무시합니다.
>   > - 캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 
>   > - 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.
>
> - **예시**
>
>   ```markdown
>   예를 들어, ULURRDLLU로 명령했다면
>   
>   위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)
>   ```
>



### 문제 해결

> \# 풀이 1 : Stack ? , set 자료형을 통한 시간복잡도 완화
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 이동할 때마다, 이동 좌표를 기록 
>      (start, end), (end, start) => 방향 상관없이 이동 추이를 기록한다.
>   2. 신규 이동 추이면 stack에 이동 추이를 쌓고 answer += 1 (길이 카운트 O)
>   3. 이동 추이가 Stack에 존재한다면 continue (중복길이라 카운트 X)

---

```python
def solution(dirs):
    answer = 0
    stack = set()
    directions = {'U':(0, 1), 'L':(-1, 0), 'R': (1, 0), 'D':(0, -1)}
    coord = (0, 0)
    
    for d in dirs:
        x, y = coord
        dx, dy = directions[d]
        sx, sy = x + dx, y + dy
        
        if -5 <= sx <= 5 and -5 <= sy <= 5:
            coord = (sx, sy)
            if ((x, y), (sx, sy)) in stack or ((sx, sy), (x, y)) in stack:
                continue
            stack.add(((x, y), (sx, sy)))
            stack.add(((sx, sy), (x, y)))
            answer += 1
            
    return answer
```

---



## 라이브 코딩



### Leader's Code

```python
# 이동한 경로를 O(1)로 탐색하는 것이 핵심
# O(1)만에 탐색을 하려면 무조건 Hash Table
# Hash Table => dict, set

def solution(dirs):
    position= (0, 0) # 시작좌표
    command_dict = {'U':(0, 1), 'L':(-1, 0), 'R': (1, 0), 'D':(0, -1)}
    check = set()
    
    for command in dirs:
        directions = command_dict[command]
        next_position = tuple(map(sum, zip(position, directions)))
        
        y, x = next_position
        if -5 <= y <= 5 and -5 <= x <= 5:
            check.add(tuple(sorted([position, next_position])))
            position = next_position
            
    return len(check)
```

- 굳이 탐색을 하지 않더라도 동일한 로직으로 계속 더해도 상관없다 (Hash Table)
  : set 은 어차피 중복이 제거된다.
- `tuple(map(sum, zip(position, directions)))`
  : 각 같은 자리의 요소를 더하거나 뺄 때 위와 같이 zip 을 이용하여 풀이를 할 수도 있다.

### 참고 자료

