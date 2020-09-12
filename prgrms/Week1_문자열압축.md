## Week1_문자열압축



### 문제 이해

> - **결과**
>
>   문자를 앞에서부터 n개 단위로 잘라 압축했을 때 가장 짧은 것의 길이를 Return
>
> - **예시**
>
> 1. input : 문자열 "aabbaccc" 
> 2. 문자를 1개 단위로 잘라 압축했을 때 
>    : a / a / b / b / a / c / c / c 
>    : 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현
>    : 압축 결과 => 2a2ba3c
> 3. 문자를 2개 단위로 잘라 압축했을 때는 압축되는 것이 없음.
>    : 압축되는 것이 없으므로 문자열의 길이는 그대로 8
> 4. 가능한 단위까지 잘라 압축해본 결과, 1개 단위로 잘라 압축했을 때 가장 짧다.



### 문제 해결

> #1 Hash(?) 를 이용한 방법
> : 압축 가능한 모든 단위에 대해 압축 과정을 진행하여 길이를 산출한다.
> => 단위(key) : 길이(value) 형태의 Hash를 만들어 가장 짧은 길이 (min values)를 리턴
>
> ※ 가능한 모든 단위는 input 문자열 길이의 반에 해당하는 값까지로 규정 (반 이상의 값은 의미x)

1. 먼저, 압축 가능한 모든 단위로 문자열을 잘라 2차원 리스트를 만든다.
   ***Input : "aabbaccc"***

   : 단위1 => [a, a, b, b, a, c, c, c]
   : 단위2 => [aa, bb, ac, cc]
   : ~ 단위4 까지

2. Checker 함수를 통해 압축 길이를 산출한다.
   1) 해당 원소의 문자가 뒤의 문자와 같다면 flag에 1을 넣어준다.
   2) 해당 원소의 문자가 뒤의 문자와 같지 않다면?
   2-1) [flag가 1일 경우] result에 ( 해당 원소의 길이 + len(str(압축수)) )을 더해주고 flag에 0을 대입
   2-2) [flag가 0일 경우] result에 해당 원소의 길이를 더해준다.
   3) 반복 시행
   4) 마지막 원소에 대해서는 2-1), 2-2) 과정만 진행한다.

3. 이를 통해 만들어진 Hash Table의 Values 중 최소값을 반환한다.

---

\# 풀이 1

```python
def checker(lst_element):
    result = 0
    flag = 0
    i = 0
    while (1):
        try:
            if lst_element[i] == lst_element[i+1]:
                flag = 1
            else :
                if flag == 1:
                    result += (len(lst_element[i]) + 1)
                    flag = 0
                else :
                    result += len(lst_element[i])
        except:
            result += len(lst_element[i])
            if flag == 1:
                result += 1
            break
        i = i+1
    return result


def solution(s):
    len_s = len(s)
    if len_s == 1:
        return 1
    answer_dict = {}
    for i in range(len_s // 2) :
        until = len_s // (i + 1) if len_s % (i + 1) == 0 else (len_s // (i + 1)) + 1
        check_result = checker((list(s[j*(i+1):(j+1)*(i+1)] for j in range(until))))
        answer_dict[i+1] = check_result
    return min(answer_dict.values())
```

결과 : 72.0 / 100.0

---

\# 풀이 2

> 1. \+ 약간의 가지치기 추가 

```python

# 각 단위 별로 잘라 압축했을 때의 총 길이를 재는 함수
def checker(lst_element, promise):
    result = 0
    flag = 0
    i = 0
    while (1):
        try:
            if lst_element[i] == lst_element[i+1]:
                flag = 1
            else :
                if flag == 1:
                    result += (len(lst_element[i]) + 1)
                    flag = 0
                else :
                    result += len(lst_element[i])
        except:
            result += len(lst_element[i])
            if flag == 1:
                result += 1
            break
        if result > promise:
            return result
        i = i+1
    return result


def solution(s):
    len_s = len(s)
    answer_dict = {}
    promise = float("inf")
    for i in range(len_s // 2) :
        until = len_s // (i + 1) if len_s % (i + 1) == 0 else (len_s // (i + 1)) + 1
        check_result = checker((list(s[j*(i+1):(j+1)*(i+1)] for j in range(until))), promise)
        if promise > check_result:
            promise = check_result
            answer_dict[i+1] = check_result
    return min(answer_dict.values())
```

: 속도는 조금 빨라졌으나 정확성 결과는 그대로.

---

\# 풀이 3

> 1. `(len(lst_element[i]) + 1)` 에서 압축된 문자의 수를 한자리(1)라고 고정해버렸음.
>    이를 `(len(lst_element[i]) + len(str(total)))`로 수정(자릿수 반영)
>    *[total : 압축된 문자의 수, 만약  total이 10이면 len(str(total))은 2가 된다.]*
> 2.  checker 함수의 while - try - else 문에 있는 `if flag == 1 :` 내부에서 total을 초기화 시켜주지 않아 그 부분을 수정하였음.

```python
from math import ceil

def checker(lst_element, batch_size):
    result = 0 # 압축된 문자열의 총 길이를 담는 변수
    total = 1 # 압축된 문자의 수 (default : 1)
    flag = 0 # while 문을 돌 때, 현재 요소가 압축되고 있는 상태인지를 판별하기 위한 flag
    i = 0 # index 
    while (1):
        try: # 마지막 요소에 대한 검사를 진행할 때 오류가 발생.
            if lst_element[i] == lst_element[i+1]: # 현재 문자와 다음 문자가 같다면
                total += 1 # 같다면 압축 수 1 증가
                flag = 1 # flag에 1을 주어 압축상태 표시
            else : # 현재 문자와 다음 문자가 같지 않다면
                if flag == 1: # 압축된 상태라면 
                    result += (batch_size + len(str(total))) 
                    # 단위 길이 + 압축 횟수의 자리수를 result 에 더해준다.
                    total = 1 # 압축 수 초기화
                    flag = 0 # flag 해제
                else : # 압축된 상태가 아니면
                    result += batch_size # result에 단위 길이를 더해준다.
        except: # 마지막 요소에 대한 검사
            result += len(lst_element[i]) # 마지막 문자 단위의 길이는 가변적이므로 len 함수로 직접 재서 더해준다.
            if flag == 1: # 압축 상태라면
                result += len(str(total)) # 압축 횟수의 자리수도 더해준다.
            break # 마지막까지 검사완료하여 while문 종료
        i = i+1
    return result # 압축 문자열 총길이 반환

def solution(s):
    len_s = len(s)
    if len_s == 1: # 한자리 문자열이 Input으로 들어왔을 때 [예외처리]
        return 1
    answer_dict = {} # hash table로 쓸 빈 dict 변수 선언
    for i in range(len_s // 2) : # 압축 단위는 문자열 총 길이의 반까지만 검사하면 되므로
        until = ceil(len_s / (i + 1)) # 문자열 인덱싱 루프 수 설정
        check_result = checker((list(s[j*(i+1):(j+1)*(i+1)] for j in range(until))), (i + 1)) # s[0:1], s[1:2], s[2:3] / s[0:2], s[2:4], s[4:6] / .... 
        # 문자열을 압축 단위로 나누는 작업
        answer_dict[i+1] = check_result # hashing 작업
    
    return min(answer_dict.values())
```

- 가지치기 작업 추가?
  -> for문을 돌 때, 가장 짧게 나온 압축 문자열 길이를 promise에 저장.
  -> checker 함수를 돌 때, result 값이 promise 보다 더 크게 나오면 바로 종료.

```python
from math import ceil

def checker(lst_element, batch_size, promise):
    result = 0 # 압축된 문자열의 총 길이를 담는 변수
    total = 1 # 압축된 문자의 수 (default : 1)
    flag = 0 # while 문을 돌 때, 현재 요소가 압축되고 있는 상태인지를 판별하기 위한 flag
    i = 0 # index 
    while (1):
        try: # 마지막 요소에 대한 검사를 진행할 때 오류가 발생.
            if lst_element[i] == lst_element[i+1]: # 현재 문자와 다음 문자가 같다면
                total += 1 # 같다면 압축 수 1 증가
                flag = 1 # flag에 1을 주어 압축상태 표시
            else : # 현재 문자와 다음 문자가 같지 않다면
                if flag == 1: # 압축된 상태라면 
                    result += (batch_size + len(str(total))) 
                    # 단위 길이 + 압축 횟수의 자리수를 result 에 더해준다.
                    total = 1 # 압축 수 초기화
                    flag = 0 # flag 해제
                else : # 압축된 상태가 아니면
                    result += batch_size # result에 단위 길이를 더해준다.
        except: # 마지막 요소에 대한 검사
            result += len(lst_element[i]) # 마지막 문자 단위의 길이는 가변적이므로 len 함수로 직접 재서 더해준다.
            if flag == 1: # 압축 상태라면
                result += len(str(total)) # 압축 횟수의 자리수도 더해준다.
            break # 마지막까지 검사완료하여 while문 종료
        if result > promise:
            return result
        i += 1
    return result # 압축 문자열 총길이 반환

def solution(s):
    len_s = len(s)
    if len_s == 1: # 한자리 문자열이 Input으로 들어왔을 때 [예외처리]
        return 1
    answer_dict = {} # hash table로 쓸 빈 dict 변수 선언
    promise = float("inf")
    for i in range(len_s // 2) : # 압축 단위는 문자열 총 길이의 반까지만 검사하면 되므로
        until = ceil(len_s / (i + 1)) # 문자열 인덱싱 루프 수 설정
        check_result = checker((list(s[j*(i+1):(j+1)*(i+1)] for j in range(until))), (i + 1), promise) # s[0:1], s[1:2], s[2:3] / s[0:2], s[2:4], s[4:6] / .... 
        # 문자열을 압축 단위로 나누는 작업
        if promise > check_result:
            promise = check_result
            answer_dict[i+1] = check_result # hashing 작업
            
    return min(answer_dict.values())
```

---

## 코드리뷰_문자열압축

> ### QnA
>
> - Q. checker 함수를 어떻게 하면 더 개선시킬 수 있는지 궁금합니다.
>   A. checker 함수에서 무한 루프를 쓰는 부분, try를 쓰는 부분을 제거한 후 로직을 수정하면 자연스럽게 개선될 것이라고 생각됩니다. :)
> - Q. 출제의도에 해당하는 알고리즘이 어떤 식으로 문제에 적용되는지 궁금합니다. 
>   (CS 기초가 거의 없다시피해서, 어떤 알고리즘이 이 문제에서 사용될 수 있는지 크게 감이 오지 않습니다..ㅜㅜ)
>   A. 문자열압축의 경우 카카오 블라인드 테스트 문제인데 카카오 문제의 경우 대부분 특정 알고리즘을 사용하지 않아도 풀 수 있습니다. 이 문제의 경우 문제 유형 중에서는 **탐색**에 속하고 알고리즘으로는 **완전 탐색**이라고 부를 수는 있습니다. :) 완전 탐색의 경우 모든 경우의 수를 찾아보는 로직을 의미합니다.
> - Q. 두번째 풀이에서 가지치기 작업을 추가했는데, 이 작업이 의미가 있는지 궁금합니다. (시간 측정 결과 좀 더 빨라지긴 했습니다)
>   A. 가지치기 작업은 의미가 없는 수준이거나 크게 코드 미관을 해치는 것이 아니라면 항상 의미가 있습니다. 가능하다면 하는 것이 좋습니다.
>
> - 전체적으로 로직의 방향은 좋습니다. 코드만 조금 정리하면 완벽할 것 같습니다. 😄 

> ### Tips
>
> - 가급적 무한 루프를 사용하지 않는 것이 좋다.
>   (사용 하더라도 while True: 로 사용하는 것이 좋다.)
> - 예외가 없는 로직을 작성하도록 노력하자.
>   (불가피한 경우는 어쩔 수 없지만, 하나의 로직에 대해 여러 예외 케이스가 존재하면 지저분하게 되고 시간도 더 걸린다.)

> ### Leader's Code
>
> ```python
> def solution(s):
>     answer = len(s)
> 
>     for size in range(1, len(s) // 2 + 1):
>         count = 1
>         compress = 0
> 
>         prev = s[:size]
>         for i in range(size, len(s) + size, size):
>             curr = s[i:i + size]
>             if prev == curr:
>                 count += 1
>             else:
>                 compress += size + len(str(count)) if 1 < count else len(prev)
>                 prev = curr
>                 count = 1
>         answer = min(answer, compress)
> 
>     return answer
> ```
>
> - `answer = min(answer, compress)`
>   모든 케이스를 다 저장하고 그 다음에 비교하려고 하지말고,
>   비교를 해나가면서 최적해만 저장을 해나가자.

