## Week1_게임아이템



### 문제 이해

> - **결과**
>   : 캐릭터들의 체력을 담은 배열 healths와 아이템별 효과를 담은 이차원 배열 items가 solution 함수의 매개변수로 주어질 때, 팀의 공격력을 최고로 끌어올리려면 어떤 아이템을 사용해야 하는지 return 
>
> - **제한 조건**
>
>   > - healths의 길이는 1 이상 10,000 이하입니다.
>   > - healths의 원소(캐릭터의 체력)는 1 이상 1,000,000 이하인 자연수입니다.
>   > - items의 길이는 1 이상 5,000 이하입니다.
>   > - items에는 아이템이 [올려줄 공격력, 낮출 체력]이 번호 순서대로 들어있습니다.
>   >   - 아이템 번호는 1번 부터 시작합니다.
>   >   - items[i]에는 i + 1번 아이템이 [올려줄 공격력, 낮출 체력]이 들어있습니다.
>   >   - 아이템이 올리는 공격력은 1 이상 500,000 이하인 자연수입니다.
>   >   - 아이템이 내리는 체력은 1 이상 500,000 이하인 자연수입니다.
>   > - 아이템 번호는 오름차순으로 정렬해 return 해주세요.
>   > - **올려주는 공격력이 같은 아이템은 없습니다.**
>   > - **아이템을 사용하는 방법이 여러 가지라면, 그러한 방법 중 아무거나 하나를 return 해주세요. 단, 아이템 번호는 오름차순으로 정렬되어 있어야 합니다.**
>
> - **예시**
>

| healths       | items                                 | return |
| ------------- | ------------------------------------- | ------ |
| [300,200,500] | [[1000, 600], [400, 500], [300, 100]] | [3]    |

---

### 문제 해결

> \# 풀이 1
>
> 1. healths를 최소 힙 자료구조로 변환
> 2. 가장 낮은 체력을 뽑아, 
>    items의 낮출 체력을 뺐을 때 100 이상이면서 공격력을 가장 높이는 요소의 인덱스 추출
> 3. 추출한 인덱스 자리 요소의 낮출 체력을 큰 수로 변경 (float('inf'))
> 4. 동일한 과정 진행
>
> \# 관건
>
> - 2번 탐색 과정의 시간 복잡도를 어떻게 해결할 것인가?

---



### 참고 자료

---



## 라이브 코딩_게임아이템

``` python
from heapq import heapify, heappush, heappop
from collections import deque

def solution(healths, items):
    healths.sort() # 체력을 오름차순으로 정렬
    # 깎는 체력이 낮은 순서대로 체크를 하는 것이 효율적이라고 판단
    # 아이템을 깎는 체력순으로 정렬
    # 아이템은 최초로 한 번 정렬하기 때문에 heap을 사용할 필요가 없다.
    items = deque(sorted([(item[1], item[0], index + 1) 
                          for index, item in enumerate(items)]))
    answer = []
    heap = []
    
    for health in healths: # O(healths)
        if health <= 100:
            continue # 가지치기

        while items: # O(items log items)
            # 깎는 체력, 올려주는 공격력, 아이템 번호
            debuff, buff, index = items[0] 
            if health - debuff < 100:
                break
            heappush(heap, (-buff, index))
            # deque를 사용하지 않고 pop(0)을 쓸 경우 O(n)
            items.popleft() # O(1)
            
        if heap: # 힙이 비어있지 않으면
            _, index = heappop(heap)
            answer.append(index) # 답 리스트에 추가
            
      return sorted(answer)
```



## Q & A

> Q. 아이템을 공격력순으로 내림차순으로 정렬후에 그리디 방법으로 푸는건  별로인가요?
> Health는 똑같이 오름차순으로 정렬후에 아이템 리스트와 비교하는 것이요!
>
> A. 체크할 부분이 하나 더 생겨서 아마 시간초과가 날 것임.
> 말로 설명하기 애매하여 반례를 슬랙에 올려드리겠다.

> Q. 시험문제를 맞딱뜨렸을 때, 어떻게 Queue와 Heap과 같은 개념을 어떻게 떠올리는지?
>
> A.  문제 지문을 보면 어느정도 힌트를 얻을 수 있음. 
> 예를 들어 게임아이템 문제 같은 경우,
> 리스트가 주어지고, 그 중 무언가를 선택하고 그 중에서 가장 효율적인 것들을 찾는 문제 
> -> Heap or Greedy
> 효율적인 것들이 들어갔다 나갔다 할 수 있다. -> Heap
> 최대한 다양한 문제 유형을 풀어보며 익히는 것이 가장 좋은 방법.

>Q. 파이썬 내장 hash 함수
>A. 스트링을 암호화해주는 Method



### Tips)

문제를 푸실 때, 오퍼레이터 사이와 콤마 사이에 띄어쓰기를 넣어주는 것이 더 좋은 코드.
한글에서의 띄어쓰기와 같은 중요성
큰 블럭 단위 당 개행을 넣어주어 구분해주는 것이 가독성이 더 좋아짐



- 배상 비용 최소화
  : 단순 sort를 이용한 풀이, Heap을 이용한 풀이, 페라메트릭 서치를 이용한 풀이, O(n) 시간복잡도를 이용한 풀이가 존재. 프로그래머스 스쿨에 따로 올려드릴 예정.



### 해시 & 스택 미리보기

- Stack

  ```python
  # Stack은 FILO : First In Last Out, 먼저 들어온 것이 나중에 나간다.
  
  answer.append(1)
  answer.append(2)
  answer.append(3)
  answer.pop() # 3
  answer.pop() # 2
  answer.pop() # 1
  
  # 위처럼, First In Last Out 의 구조를 갖는 것이 바로 스택이다.
  # 올바른 괄호 문제 같은 경우가 스택 문제의 대표적인 예
  ```

- Hash 

  ``` python
  # Hash 같은 경우는 두가지 의미가 있다.
  # 1. Hash는 암호화된 string을 의미
  # 2. Hash Table을 의미
  
  # 파이썬의 경우 해시 사용이 매우 간단하다.
  
  answer['a'] = 1
  answer['b'] = 2
  
  # 이와 같이 Key : value 의 형태의 자료구조를 hash Table 이라고 한다.
  ```

  ```python
  from collections import defaultdict
  
  # defaultdict()는 딕셔너리를 만드는 dict 클래스의 서브클래스.
  # defaultdict의 인자로 주어진 객체의 기본값을
  # 딕셔너리의 초기값으로 지정한다.
  
  answer = defaultdict(list) 
  
  for i in range(10):
      # defaultdict()의 경우,
      # 키에 해당 하는 Value를 초기화 시켜주지 않아도 잘 돌아간다.
      answer[i].append(i)
  ```

  ```python
  answer = set()
  
  for i in range(10):
      answer.add(i)
      
  for i in range(10) :
      if i in answer : # answer가 set 인 경우 시간복잡도는 O(1)
      	print(i)	 # 만약 answer가 리스트 형태라면 시간복잡도는 O(n)
          
      
  # key 값으로 무엇을 찾아야 할 때, 시간을 줄여야 하는 상황에서 많이 사용된다.
  ```

  

