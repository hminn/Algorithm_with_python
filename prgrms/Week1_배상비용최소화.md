## Week1_배상비용최소화



### 문제 이해

> - **결과**
>   : 조선소에서 작업할 수 있는 N 시간과 각 일에 대한 작업량이 담긴 배열(works)이 있을 때 배상 비용을 최소화한 결과를 반환
>
> - **예시**
>   1. N=4일 때, 선박별로 남은 일의 작업량이 works = [4, 3, 3]이라면 배상 비용을 최소화하기 위해 일을 한 결과는 [2, 2, 2]가 되고 배상 비용은 2^2 + 2^2 + 2^2 = 12가 되어 12를 반환해 줍니다.
>

### 문제 해결

> \# 풀이 1 최대 힙을 이용한 방법

---

```python
import heapq

def solution(no, works):
    works = list(map(lambda x : x * -1, works)) # 최대 힙을 얻기 위한 트릭
    heapq.heapify(works)
    while (no > 0):
        max_work = heapq.heappop(works)
        if max_work == 0: # works의 최댓값보다 no가 클 경우
            break
        heapq.heappush(works, (max_work.numerator + 1)) 
        # numerator 메소드를 이용해서 int와의 연산이 가능하게끔
        no -= 1
    result = sum(list(map(lambda x : x**2, works)))
    return result
```

- list() 함수 와 []의 차이

  > - list() : 리스트로 변환 가능한 다른 자료형을 리스트로 바꿔줄 수 있다. 내부(list 함수의 인자)에서 map을 이용한 연산을 했을 때, map 자료형이 list 자료형으로 변환.
  > - [] : 그냥 빈 리스트를 만들어 줄 때 사용된다. 내부에서 map을 이용한 연산을 했을 때, map 자료형이 리스트안에 그대로 들어오게됨.

- 최대 힙을 얻는 트릭
  : -1을 곱해주어 최소 힙을 뒤집어 준다. (heapq 모듈은 최소 힙만 지원한다.)

---

### 참고자료

- [힙 알고리즘 자세하게](https://www.fun-coding.org/Chapter11-heap.html) 

- [최대 힙을 얻는 방법](https://www.it-swarm.dev/ko/python/python%EC%97%90%EC%84%9C-%EC%B5%9C%EB%8C%80-%ED%9E%99%EC%9D%84-%EC%96%BB%EB%8A%94-%EB%B0%A9%EB%B2%95/836573061/)

---

## 코드리뷰_배상비용최소화

> ### QnA
>
> - Q. python에서 최대 힙을 얻는 다른 방법이 있는지 궁금합니다.
>   A. 아쉽게도 파이썬 내장 모듈로는 방법이 없습니다 ㅠㅠ 직접 구현하거나 트릭을 쓰는 것이 최선입니다.
>
> ### 코드 개선
>
> ```python
> import heapq
> 
> def solution(no, works):
> 
>     if no >= sum(works):
>         return 0
> 
>     works = [-x for x in works] # 최대 힙을 얻기 위한 트릭
>     heapq.heapify(works)
> 
>     while (no > 0):
>         max_work = heapq.heappop(works)
>         heapq.heappush(works, (max_work.numerator + 1)) 
>         # numerator 메소드를 이용해서 int와의 연산이 가능하게끔
>         no -= 1
>     result = [x**2 for x in works]
>     return result
> ```
>
> - 최상단에 예외조건을 추가하여 런타임을 최대한으로 줄여준다.
> - list(map(lambda)) 와 같은 복잡한 구문보다 List Comprehension을 통해 간단하게 표현 가능하다.
>
> ### Leader's Code
>
> ```python
> # Tim sort 를 이용한 풀이
> # Tim sort 덕분에 효율성 테스트를 통과한다.
> 
> def solution(no, works):
>     if no >= sum(works): # 가지치기
>         return 0
> 
>     for _ in range(no):
>         works = sorted(works) # 
>         works[-1] -= 1 # O(no * works log works)
>     return sum([work ** 2 for work in works])
> ```
>
> ```python
> # heapq를 이용한 풀이
> 
> import heapq
> 
> def solution(no, works):
>     if no > sum(works):
>         return 0
> 
>     works = [-i for i in works] # max heap
>     heapq.heapify(works) 
> 
>     for _ in range(no):
>         heapq.heapreplace(works, works[0] + 1)
>         # heappushpop 먼저 푸시하고 팝
>         # heapreplace 먼저 팝하고 푸시
> 
>     return sum([i**2 for i in works])
> ```
>
> - while의 동작방식을 for 문으로 줄일 때 쓸 수 있는 Skill (코드가 더 간단해짐)
> - heappop 과 heappush 과 동시에 작동할 수 있도록 하는 메소드
>   : heapreplace 를 이용!

