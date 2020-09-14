## Week2_사전순부분문자열



### 문제 이해

> - **결과**
>   : 문자열 s가 주어졌을 때 s로부터 만들 수 있는 부분 문자열 중 사전 순으로 가장 뒤에 나오는 문자열을 리턴
>
> - **예시**
>
>   ```markdown
>   예를 들어 문자열 xyb로 만들 수 있는 부분 문자열은 다음과 같습니다.
>   
>   x
>   y
>   b
>   xy
>   xb
>   yb
>   xyb
>   
>   이 중 사전 순으로 가장 뒤에 있는 문자열은 yb입니다.
>   ```
>



### 문제 해결

> \# 풀이 1
>
> - 예외처리
>   1. 
> - 해결과정
>   1. Combinations 함수를 이용하여 만들 수 있는 문자열 생성
>   2. Sort()
>   3. 마지막 문자열 반환

---

```python
from itertools import combinations

def solution(s):
    answer = []
    for i in range(len(s)):
        answer += list(map(''.join, combinations(s, i + 1)))
    answer.sort()
    return answer[-1]
```

- 답이 나오는 로직이긴 하나, 시간초과 문제로 다 터짐

---

> \# 풀이 2 : Stack 을 이용
>
> - 예외처리
>   1. 2-2 루프 돌 때, stack == [] 이면 break
> - 해결과정
>   1. 일단 첫번째 문자를 Stack에 담는다.
>   2. 다음 문자와의 대소비교(순서)를 진행한다.
>      2-1. 다음 문자가 더 작거나 같다면, Stack에 쌓는다.
>      2-2. 다음 문자가 더 크다면, 
>      stack의 마지막 문자가 더 크거나 같을 때까지 Stack.pop()

---

```python
def solution(s):
    stack = [s[0]]
    for i in s[1:]:
        while (stack[-1:] < [i]):
            stack.pop()
            if stack == []:
                break
        stack.append(i)
    return ''.join(stack)
```

---

### Check Point

> 1. combinations 는 itertools 모듈의 함수이다.
> 2. 문자열 연산 vs 리스트 pop, append 시간복잡도?
> 3. 조금이라도 더 시간을 줄일 수는 없는지 계속 Check!

---



### 참고 자료

