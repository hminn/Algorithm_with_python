## Week1_더맵게 : Heap & Queue



### 문제 이해

> - **결과**
>   :  스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return
>
> - 조건
>
>   > - scoville의 길이는 1 이상 1,000,000 이하입니다.
>   > - K는 0 이상 1,000,000,000 이하입니다.
>   > - scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
>   > - 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.
>
> - **예시**
>
>   > ex) scoville : [1, 2, 3, 9, 10, 12], K : 7
>   >
>   > 1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
>   >    새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
>   >    가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]
>   > 2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
>   >    새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
>   >    가진 음식의 스코빌 지수 = [13, 9, 10, 12]
>   > 3. 모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.
>



### 문제 해결

> \# 풀이 1 : 단순 구현

```python
def mixed(scoville):
    most = scoville.pop(scoville.index(min(scoville)))
    second = scoville.pop(scoville.index(min(scoville)))
    scoville.append(most + (second * 2))

def solution(scoville, K):
    answer = 0
    while (min(scoville) < K):
        if len(scoville) == 1 :
            if scoville[0] < K:
                answer = -1
            break
        mixed(scoville)
        answer += 1
    return answer
```

: 답은 나오나, 효율성 문제(시간복잡도)에서 조건을 만족하지 못함.

---

> \# 풀이2 : heap 자료구조를 이용한 풀이 (heapq 모듈)

```python
import heapq

def mixed(scoville):
    most = heapq.heappop(scoville)
    second = heapq.heappop(scoville)
    heapq.heappush(scoville, most + (2 * second))
    
def solution(scoville, K):
    heapq.heapify(scoville)
    answer = 0
    while (scoville[0] < K):
        if len(scoville) == 1:
            if scoville[0] < K:
                answer = -1
            break
        mixed(scoville)
        answer += 1
    return answer
```

---



### 참고 자료

1. [파이썬 힙 큐 알고리즘](https://python.flowdas.com/library/heapq.html)
2. [파이썬 힙 큐 묘듈 사용방법](https://www.daleseo.com/python-heapq/)

3. [힙 : 자료구조 형태 중 하나로, 우선순위 큐를 위해 만들어진 자료구조](https://programming119.tistory.com/85)
4. [종합설명 : 힙 자료구조 설명 / 힙큐 / 파이썬 힙큐 사용하기](https://littlefoxdiary.tistory.com/3)

---

## 코드리뷰_더맵게

> ### QnA
>
> - Q. pop, push 과정을 하나의 함수로 만들어서 구분해주었는데 이런 방식의 코딩스타일을 지향해도 괜찮은가요?!
>   A. 보통 애플리케이션을 만들 때에는 가독성이 더 좋기 때문에 문제는 없습니다. :) 하지만 너무 간단한 코드인 경우 함수로 만들 경우 더 복잡할 수도 있기 때문에 잘 판단해야 합니다. 저 같은 경우 10줄이 넘어갈 경우 함수로 분리할지 고민합니다.
>
> - Q. while문 내에 if 중첩을 사용하였는데 개선할 수 있는 방법이 있을까요??
>   A. 예외 처리는 `if`를 이용한 분기보다 `try ~ except`를 사용하는 것이 좀 더 파이썬스럽습니다. :)
>   `if`를 이용한 예외 처리를 LBYL이라 부르고 `try ~ except`를 이용한 예외 처리를 EAFP라고 부릅니다. 파이썬은 EAFP 방식을 권장하고 있습니다. :)
>   [https://suwoni-codelab.com/python%20%EA%B8%B0%EB%B3%B8/2018/03/06/Python-Basic-EAFP/](https://suwoni-codelab.com/python 기본/2018/03/06/Python-Basic-EAFP/)
>   관련 블로그 글 입니다.
>
>   

> ### 코드 개선
>
> ```python
> import heapq
> 
> def solution(scoville, K):
>     heapq.heapify(scoville)
>     answer = 0
>     while (scoville[0] < K):
>         try :
>             heapq.heappush(scoville, heapq.heappop(scoville) + (2 * heappop(scoville)))
>         except IndexError: # 에러 종류도 명시적으로 표시해 주는 게 좋다.
>             return -1
>         answer += 1
>     return answer
> ```
>
> 

