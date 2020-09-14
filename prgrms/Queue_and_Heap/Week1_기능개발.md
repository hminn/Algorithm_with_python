## Week1_기능개발

### 문제 이해

> - **결과**
>
>   : 먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return.
>
> - **예시**
>
> 1. Progresses = [93, 30, 55], speeds = [1, 30, 5]
> 2. 첫번째 기능은 93% 완료되어 있는 상태. 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포 가능
> 3. 두번째 기능은 30% 완료되어 있는 상태. 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포 가능
> 4. 하지만 첫번째 기능이 아직 완성이 되지 않았기에 첫번째 기능이 배포되는 7일째에 같이 배포된다.
> 5. 세번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능.
> 6. 따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포.
> 7. return : [2, 1]

---



### 문제 해결

> \# 1
>
> 1) 각각의 기능이 배포되는데 걸리는 일 수로 구성된 리스트를 생성
>
> 2) 문제이해 예시를 참고하면 [7, 3, 9] 와 같은 리스트를 만든다. ((100 - Progress) // speed) + 1
>
> 3) 첫번째 요소부터 다음과 같은 과정을 진행한다.
> 	3-1) 
> 		3-1-1) total 이라는 정수 1을 담고 있는 변수 생성 (total = 1) [배포 기능의 총 개수]
> 		3-1-2) max_day 라는 첫번째 days 요소를 담고 있는 변수 생성 [max_day = day(index:0)] 
> 																													[비교 요소 (배포 기준) ]
>
> ​	3-2) max_day 와 현재 요소의 대소 비교. 더 큰 값을 max에 대입한다.
> ​		3-2-1) 뒤의 요소와 비교 했을 때, max_day가 값이 더 크다면 total += 1
> ​		3-2-2) 뒤의 요소와 비교 했을 때, max_day가 값이 더 작다면 
> ​					max_day = day, answer.append(total) , total = 1
>
> ​	3-3) 마지막 요소에 대한 예외 처리

```python
from math import ceil

def solution(progresses, speeds):
    days = list(ceil((100 - progress) / speed) for progress, speed in zip(progresses, speeds))
    answer = []
    total = 1
    for ind, day in enumerate(days):
        if ind == 0 :
            max_day = day
        else :
            if max_day >= day:
                total += 1
            else :
                max_day = day
                answer.append(total)
                total = 1
        if (ind + 1) == len(days):
            answer.append(total)
    return answer
```

- 결과 100 / 100
- 적용되는 알고리즘에 대한 지식?
- 파이써닉한 코드?

---

## 코드 리뷰_기능개발

> ### QnA
>
> - Q. 알고리즘에 대한 지식이 부족하여 일단 주먹 구구식으로 짜봤는데 혹시 적용 가능한 알고리즘이 존재하는지 궁금합니다.
>   A . 기능 개발 문제같은 경우 Queue의 특징을 이용하여 푸는 문제입니다. :) 값을 계산하고 맨 앞 요소를 순서대로 제거하는 방식으로 로직을 작성할 수 있습니다. 그렇지만 조금 더 효율적으로 작성한다면 @hminn 님이 작성하신 로직이 더 빠르고 효율적입니다.
> - Q. for문 내의 개선 방법
>   A. 기능 개발 문제의 `for` 내부 로직은 배치를 조금 다르게 하는 방법으로 쉽게 개선이 가능합니다. 코드 리뷰를 확인해주세요. :) 
> - 전체적으로 보자면 Queue의 형태를 사용하여 해결이 가능합니다. 조건이 맞으면 맨 앞의 값부터 빠져나오기 때문에 FIFO의 형태를 가지게됩니다.

> ### 코드리뷰 내용
>
> - List() -> [] 로 줄이자.
>
>   ```python
>   days = list(ceil((100 - progress) / speed) for progress, speed in zip(progresses, speeds))
>   
>   # 위 부분에서 list 함수를 사용하지 않고 다음과 같이 줄일 수 있다.
>   
>   days = [ceil((100 - progress) / speed) for progress, speed in zip(progresses, speeds)]
>   
>   ```
>
> - for 문의 개선
>
>   ```python
>   from math import ceil
>   
>   def solution(progresses, speeds):
>       days = list(ceil((100 - progress) / speed) for progress, speed in zip(progresses, speeds))
>       answer = []
>       
>       max_day = days[0]
>       total = 0
>       
>       for day in days: 
>           if max_day < day :
>               max_day = day
>               answer.append(total)
>               total = 0
>           total += 1
>   
>       if total > 0: # 마지막 요소에 대한 처리
>           answer.append(total)
>           
>       return answer
>   ```
>
>   - for 문 내에서 조건을 줄이기 위해서는 일관된 로직이 중요하다.
>   - 여러 개의 조건을 넣는 것보다 하나의 방법으로 전체 조건을  어우를 수 있는 조건을 생각해볼 것.

> ### Leader's Code
>
> ```python
> # O(n) 풀이
> 
> from math import ceil
> 
> def solution(progresses, speeds):
>     answer = []
>     max_duration = ceil((100 - progresses[0]) / speeds[0])
>     count = 0
> 
>     for progress, speed in zip(progresses, speeds):
>         duration = ceil((100 - progress) / speed)
>         # duration = -(-(100 - progress) // speed) # 이런식으로도 가능합니다.
>         if max_duration < duration:
>             answer.append(count)
>             count = 0
>             max_duration = duration
>         count += 1
> 
>     if count > 0:
>         answer.append(count)
> 
>     return answer
> ```
>
> ```python
> # 리스트의 특성을 이용한 풀이
> 
> from math import ceil
> 
> def solution(progresses, speeds):
>     answer = [[ceil((100 - progresses[0]) / speeds[0]), 0]]
> 
>     for progress, speed in zip(progresses, speeds):
>         duration = ceil((100 - progress) / speed)
>         if answer[-1][0] < duration:
>             answer.append([duration, 0])
>         answer[-1][1] += 1
> 
>     return [count for _, count in answer]
> ```
>
> 