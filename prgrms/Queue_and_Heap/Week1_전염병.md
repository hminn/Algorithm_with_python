## Week1_전염병



### 문제 이해

> - **결과**
>   : 사무실의 크기 m, n과 병에 걸린 직원의 위치 infests, 백신을 맞은 직원의 위치 vaccinateds가 매개변수로 주어집니다. 이때 백신을 맞은 직원을 제외한 모든 직원이 병에 감염되기까지 며칠이 걸리는지 return 하는 solution 함수를 완성해주세요.
>
> - **예시**
>
>   
> 



### 문제 해결

> \# 풀이 1 : BFS
>
> - 예외처리
>   1. 병을 아무리 퍼트려도 백신을 맞은 직원을 제외한 모든 직원이 병에 감염될 수 없는 경우 -1을 리턴
> - 해결과정
>   1. 감염 좌표를 Queue에 담고 하나씩 뺀다.
>   2. 감염 가능한 좌표인지 Check 한다.
>      2-1. 범위를 벗어나지는 않았는지
>      2-2. 백신을 맞은 좌표는 아닌지
>   3. 감염 가능 좌표이면 new_infests에 담아준다.
>   4. 위 시행을 Queue에 아무것도 남아 있지 않을 때까지 반복한다.
>   5. (i) Queue에 아무것도 남아 있지 않고, (ii) 새로운 감염좌표(new_infests)가 비어있지 않을 경우, 1-4 과정을 반복한다.

---

```python
# 1차 풀이

from collections import deque

def solution(m, n, infests, vaccinateds):
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    new_infests = []
    answer = 0
    queue = deque(infests)
    
    while queue:
        infest = queue.popleft()
        x, y = infest[1], infest[0]
        
        for dx, dy in directions :
            px = x + dx
            py = y + dy
            if [py, px] in infests:
                continue
            if (0 < py <= m) and (0 < px <= n) and not ([py, px] in vaccinateds):
                new_infests.append([py, px]) # 감염 가능한 좌표를 추가
                
        if not queue and new_infests :
            answer += 1
            infests += new_infests
            queue = deque(new_infests)
            new_infests = []
            
    if len(infests) + len(vaccinateds) < (m * n) :
        answer = -1
        
    return answer
```

- BFS를 구현한 로직 자체는 맞았으나, 시간복잡도에서 이슈가 발생
- 코드 리뷰 결과, list에 in을 사용하면 O(n)의 시간복잡도를 갖기 때문에 시간 초과 문제가 발생함을 알게 되었음.
- list를 set 자료형으로 바꿔주어 시간복잡도를 조금이나마 줄여보고자 함.

---

```python
# 2차 풀이

from collections import deque

def solution(m, n, infests, vaccinateds):
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    infests = set([(i, j) for i, j in infests])
    vaccinateds = set([(i, j) for i, j in vaccinateds])
    new_infests = set()
    answer = 0
    queue = infests.copy()
    
    while queue:
        infest = queue.pop()
        x, y = infest[1], infest[0]
        
        for dx, dy in directions :
            px = x + dx
            py = y + dy
            if (py, px) in infests:
                continue
            if (0 < py <= m) and (0 < px <= n) and not ((py, px) in vaccinateds):
                new_infests.add((py, px)) # 감염 가능한 좌표를 추가
                
        if not queue and new_infests :
            answer += 1
            infests = infests.union(new_infests)
            queue = new_infests
            new_infests = set([])
            
    if len(infests) + len(vaccinateds) < (m * n) :
        answer = -1
        
    return answer
```

- 위와 같이 set 자료형으로 모두 변경해주고, 메소드도 그에 맞게 변경해주니 통과가 되었다.
- 위의 문제 풀이를 진행하면서 2가지에 대한 궁금증이 생겼다.
  1. 참조 (Call by ) 이슈?
  2. Set 이란?

---

### Check_Point

> #### 1. 참조 (Call by) In Python
>
> - 파이썬 call-by-value 일까 call-by-reference 일까?
>
>   ```markdown
>   결론부터 말하자면 passed by assignment 라고 한다.
>   
>   즉, 어떤 값을 전달하느냐에 따라 달라지는 것이다.
>   
>   파이썬의 자료형엔 크게 immutable(불변) 과 mutable(가변) 이 있다.
>   
>   int, str 같은 타입이 불변이고 list, dictionary 같은 타입이 mutable 이다.
>   
>   불변 타입의 객체를 넘기면 call by value 가 되고 가변 타입의 객체를 넘기면 call by reference 가 된다. 즉 할당 되는 것에 따라 전달 방식이 달라지는 것이다.
>   
>   어떻게 이것이 가능할까?
>   
>   바로 파이썬에선 모든 것이 객체이기 때문이다.
>   
>   그래서 int 타입의 변수(객체) 를 함수의 인자 값으로 넘기면 이 객체는 불변 이기 때문에 함수 안에서는 새로운 값을 생성한다.
>   
>   이는 마치 call-by-value 처럼 보이게 한다! 호출할 때 쓰인 변수 A와 함수 내의 A'는 서로 다른 값을 갖게 되니까!
>   
>   하지만 가변 객체는 말이 달라진다.
>   
>   새로 값을 만들 필요가 없기 때문에 레퍼런스만 유지 되서 call-by-reference 처럼 보이는 것이다.
>   
>   그렇기에 파이썬은 passed by assignment 라고 한다.
>   
>   ```
>
> - 파이썬의 함정
>
>   ```markdown
>   ​``` 중략 ```
>   
>   여기서 문제는 각 기본값이 함수가 정의 될 때 (즉,일반적으로 모듈이 로딩 될 때) 평가되고 기본값은 함수 객체의 속성이 된다는 것이다. 따라서 기본값이 가변 객체고, 이 객체를 변경하면 변경 내용 이 향후에 이 함수의 호출에 영향을 미친다. 
>   
>   가변 기본값에 대한 이러한 문제 때문에, 가변 값을 받는 매개변수의 기본값으로 None 을 주로 사용하며,  __init__ 메서드는 passengers 인수가 None 인지 확인하고 새로 만든 빈 리스트를 할당한다. 
>   
>   ```
>
>   ```markdown
>   파이썬에서 값이 같은지 비교하는것은 == 이고, is 의 경우는 같은 객체인지를 검토한다.
>   ```
>
> 
>
> #### 2. Set 자료형
>
> - Set 자료형이란?
>
>   ```markdown
>   set 은 순서가 없는 중복이 불가능한 collection 자료형이다. 주로 item 테스트, 중복제거 등에 사용되고 교집합, 합집합, 차집합 등을 수학적인 계산이 가능하다. 다른 collection 자료형 처럼 item 검사, 길이, 루프가 가능하다. set 은 삽입된 item 의 위치를 저장하지 않기 때문에 item 간의 순서가 없다. 따라서 indexing 이 불가능하고, 자르기가 안되고, 그외의 sequential 한 작업이 불가하다. set 은 dictionary 를 구현한 클래스인데, dictionary 의 key 가 set 의 item 이 된다. 그렇기 때문에 set 은 dictionary 나 list 처럼 중복되는 요소를 담을 수 없다.
>   ```
>
> - Set 자료형의 Looping 속도
>
>   ```markdown
>    set 의 루핑 속도에 대한 것이었습니다. 파이썬에서 set 역시 list 와 같이 iterable 한 자료구조이긴 하지만, list 에 비해서 iteration 성능은 떨어지는 것으로 익히 알려져 있습니다. 대신 hash 기반으로 만들어지기 때문에 검색 속도는 list 에 비해 훨씬 뛰어나죠.
>   ```
>
>   - Iteration 성능 ? list > set
>   - 검색 성능 ? list < set

### 참고 자료

- [Call by what?](https://www.pymoon.com/entry/Python-%EC%9D%80-callbyvalue-%EC%9D%BC%EA%B9%8C-callbyreference-%EC%9D%BC%EA%B9%8C) 

- [파이썬의 함정](https://hamait.tistory.com/844)

- [Set 자료형](https://blueshw.github.io/2016/02/28/python-about-set/)