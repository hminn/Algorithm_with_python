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