## Week3_사탕담기



### 문제 이해

> - **결과**
>   : 가방이 감당할 수 있는 무게 m, 사탕별 무게가 담긴 배열 weights가 매개변수로 주어질 때, 가방을 정확히 m 그램으로 채우는 경우의 수를 return 하는 solution 함수를 작성해주세요.
>
> - **제한사항**
> > - 단, 같은 사탕은 또 넣을 수 없습니다.
> > - m은 1,000 이상 100,000 이하인 자연수입니다.
> > - 모든 사탕의 무게는 10 이상 100,000 이하인 자연수입니다.
> > - weights의 길이는 3 이상 15 이하입니다.
>
> - **예시**
>
>   ```markdown
>   m = 3000, weights = [500, 1500, 2500, 1000, 2000]
>   
>   사탕을 하나씩 선택해 3000 그램으로 만드는 방법은 [500, 1000, 1500], [1000, 2000], [500, 2500] 으로 3가지입니다.
>   ```
>



### 문제 해결

> \# 풀이 1 : combinations를 이용한 완전탐색, Hash table을 이용하여 Counting
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 나올 수 있는 모든 경우의 수를 조합(combinations)을 통해 뽑아낸다.
>   2. 조합들의 합을 Hash table 형태의 Key로 저장하고 Counting 한다.

---

```python
from itertools import combinations
from collections import defaultdict

def solution(m, weights):
    
    weights_dict = defaultdict(int)

    for i in range(1, len(weights) + 1):
        lst = [sum(nums) for nums in combinations(weights, i)]
        for j in lst:
            weights_dict[j] += 1
            
    return weights_dict[m]
```

- 9번째 줄 
  1. `lst = list(map(sum, combinations(weights, i)))` 로 호환 가능.
     But, 시간이 좀 더 걸림을 확인.

---



### 참고 자료

