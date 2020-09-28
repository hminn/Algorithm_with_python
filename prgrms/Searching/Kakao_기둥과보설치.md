## Week3_기둥과 보 설치



### 문제 이해

> - **결과**
>   : 
>
> - **제한사항**
> > - n은 5 이상 100 이하인 자연수입니다.
> > - build_frame의 세로(행) 길이는 1 이상 1,000 이하입니다.
> > - build_frame의 가로(열) 길이는 4입니다.
> > - build_frame의 원소는 [x, y, a, b]형태입니다.
> >   - x, y는 기둥, 보를 설치 또는 삭제할 교차점의 좌표이며, [가로 좌표, 세로 좌표] 형태입니다.
> >   - a는 설치 또는 삭제할 구조물의 종류를 나타내며, 0은 기둥, 1은 보를 나타냅니다.
> >   - b는 구조물을 설치할 지, 혹은 삭제할 지를 나타내며 0은 삭제, 1은 설치를 나타냅니다.
> >   - 벽면을 벗어나게 기둥, 보를 설치하는 경우는 없습니다.
> >   - 바닥에 보를 설치 하는 경우는 없습니다.
> > - 구조물은 교차점 좌표를 기준으로 보는 오른쪽, 기둥은 위쪽 방향으로 설치 또는 삭제합니다.
> > - 구조물이 겹치도록 설치하는 경우와, 없는 구조물을 삭제하는 경우는 입력으로 주어지지 않습니다.
> > - 최종 구조물의 상태는 아래 규칙에 맞춰 return 해주세요.
> >   - return 하는 배열은 가로(열) 길이가 3인 2차원 배열로, 각 구조물의 좌표를 담고있어야 합니다.
> >   - return 하는 배열의 원소는 [x, y, a] 형식입니다.
> >   - x, y는 기둥, 보의 교차점 좌표이며, [가로 좌표, 세로 좌표] 형태입니다.
> >   - 기둥, 보는 교차점 좌표를 기준으로 오른쪽, 또는 위쪽 방향으로 설치되어 있음을 나타냅니다.
> >   - a는 구조물의 종류를 나타내며, 0은 기둥, 1은 보를 나타냅니다.
> >   - return 하는 배열은 x좌표 기준으로 오름차순 정렬하며, x좌표가 같을 경우 y좌표 기준으로 오름차순 정렬해주세요.
> >   - x, y좌표가 모두 같은 경우 기둥이 보보다 앞에 오면 됩니다.
>
> - Check
>
>   1. [x, y, a, b] 에서/ a의 0은 기둥, 1은 보 / b의 0은 삭제, 1은 설치
>   2. 기둥은 바닥 위에 있거나 보의 한쪽 끝 부분 위에 있거나, 또는 다른 기둥 위에 있어야 합니다.
>   3. 보는 한쪽 끝 부분이 기둥 위에 있거나, 또는 양쪽 끝 부분이 다른 보와 동시에 연결되어 있어야 합니다.
>   4. 단, 바닥은 벽면의 맨 아래 지면을 말합니다.
>



### 문제 해결

> \# 풀이 1 : Hash, Searching
>
> - 예외처리
>   
>   1. 
> - 해결과정
>   
>   1. 조건에 맞는다면 result에 [x, y, a] 의 형태로 쌓는다.
>   
>   2. 설치 조건은 다음과 같다.
>      2-1. a가 기둥일 경우
>   
>      - 바닥위에 있거나 : (y == 0)
>      - 보의 한쪽 끝 부분 위에 있거나 :  [x - 1, y, 1] or [x, y, 1] 이 존재
>      - 기둥 위에 있거나 : [x, y-1, 0]이 존재
>   
>      2-2. a가 보일 경우
>   
>      - 한 쪽 끝 부분이 기둥 위에 있거나 : [x, y - 1, 0] or [x + 1, y - 1, 0] 이 존재
>      - 양쪽 끝 부분이 다른 보와 동시에 연결 : [x - 1, y, 1] and [x + 1, y, 1] 이 존재
>   
>   3. 삭제 조건은 다음과 같다.
>      3-1. a가 기둥일 경우
>   
>      - 기둥을 기준으로 양쪽 방향에 보-기둥 혹은 보-보 가 존재하는 경우
>      - [x - 1, y + 1, 1] and ([x - 2, y + 1, 1] or [x - 1, y, 0])
>      - [x , y + 1, 1] and ([x + 1, y + 1, 1] or [x + 1, y, 0])
>   
>      3-2. a가 보일 경우
>   
>      - 보의 양쪽에 기둥이 있는 경우 : [x, y - 1, 0] and [x + 1, y - 1, 0]
>      - 보의 한쪽에는 기둥이, 한쪽에는 아무것도 없는 경우
>        : [x, y - 1, 0] and (not in [x + 1, y, 1], [x + 1, y, 0])
>        : [x + 1, y - 1, 0] and (not in [x - 1, y, 1], [x - 1, y, 0])

---

```python
def solution(n, build_frame):
    answer = set()
    
    for build in build_frame :
        x, y, stuff, command = build
        
        pole_set_condition = (x != n and y != 0) and ((x, y - 1, 0) in answer or (x + 1, y - 1, 0) in answer or ((x - 1, y, 1) in answer and (x + 1, y, 1) in answer))
        pillar_del_condition = ((x - 1, y + 1, 1) in answer and ((x - 2, y + 1, 1) in answer or (x - 1, y, 0) in answer)) and ((x, y + 1, 1) in answer and ((x + 1, y + 1, 1) in answer or (x + 1, y, 0) in answer))
        pillar_set_condition = (y != n and y == 0 or (x - 1, y, 1) in answer or (x, y, 1) in answer or (x, y - 1, 0) in answer)
        pole_del_condition = ((x, y - 1, 0) in answer and (x + 1, y - 1, 0) in answer) or ((x, y - 1, 0) in answer and (x + 1, y, 1) not in answer and (x + 1, y, 0) not in answer) or ((x + 1, y - 1, 0) in answer and (x - 1, y, 1) not in answer and (x - 1, y, 0) not in answer)
        
        if command == 0:
            if (stuff == 0 and pillar_del_condition) or (stuff == 1 and pole_del_condition):
                answer.discard((x, y, stuff))
        else :
            if (stuff == 0 and pillar_set_condition) or (stuff == 1 and pole_set_condition):
                answer.add((x, y, stuff))
    return sorted([list(i) for i in answer])
```

- 딱히 로직이 떠오르지 않아 경우의 수에 대한 하드코딩을 진행함.
- 예시 테스트 케이스는 통과하나 나머지는 줄줄이 다 터짐
- 상당한 의욕 상실과 허무함이 나를 사무친다..

---



### 참고 자료

