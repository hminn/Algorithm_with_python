## Week3_주사위게임



### 문제 이해

> - **결과**
>   : 몬스터의 위치를 담고 있는 배열 monster와 각 주사위에서 나올 수 있는 최대 숫자 S1, S2, S3가 매개변수로 주어질 때, 1번 칸에 위치한 캐릭터가 주사위를 한 번만 굴려 나온 눈금의 합만큼 이동해서 도착한 칸에 몬스터가 없을 확률을 return 하도록 solution 함수를 완성해 주세요. 단, return 값은 몬스터를 만나지 않을 확률에 1000을 곱한 후 소수부는 버리고 정수 부분만 return 하세요.
>
> - **제한사항**
> > - monster는 몬스터의 위치를 담은 배열이며 길이(몬스터의 개수)는 1 이상 99 이하입니다.
> > - monster의 각 원소는 현재 몬스터의 위치를 나타내며, 모든 몬스터의 위치는 2 이상 100 이하의 자연수입니다.
> > - 같은 위치의 몬스터가 여러 번 중복해서 주어지지 않으며, 한 칸에는 한 마리의 몬스터만 있습니다.
> > - 각 주사위를 던져 나올 수 있는 수의 최댓값 S1, S2, S3는 다음과 같습니다.
> >   - 2 ≤ S1 ≤ 30, 2 ≤ S2 ≤ 30, 2 ≤ S3 ≤ 40, S1, S2, S3는 자연수
> > - 몬스터를 만나지 않을 확률에 1000을 곱한 후 소수부는 버리고 정수 부분만 int형으로 return 해주세요.
>
> - **예시**
>
>   ```markdown
>   다음은 S1 = 2, S2 = 3, S3 = 4인 경우의 예시입니다.
>   
>   위 그림에서 별은 캐릭터이며, 붉은 사각형은 몬스터입니다. 캐릭터는 1번 칸에 있습니다. 주사위를 던져 나온 숫자가 1, 1, 2라면 캐릭터는 총 4칸을 이동하여 5번째 칸에 도착해 몬스터를 만납니다. 반면에 주사위를 던져 나온 숫자가 2, 2, 1이라면 총 5칸을 이동한 캐릭터는 6번째 칸에 도착해 몬스터를 만나지 않습니다. 위 예시에서 주사위를 한 번만 굴렸을 때, 주사위 눈의 합만큼 이동해 도착한 칸에서 몬스터를 만나지 않을 확률은 0.5입니다.
>   ```
>



### 문제 해결

> \# 풀이 1 : 완전탐색
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 단순하게 루프 3개 돌려서 조건에 맞는 경우 Counting
>   2. set 자료형을 이용하여 in 연산의 시간복잡도를 줄여주었다.

---

```python
def solution(monster, S1, S2, S3):
    dice = []
    monster = set(monster)
    case = [0, 0] #몬스터를 안 만날 경우, 만날 경우
    
    for i in [S1, S2, S3]:
        dice.append([j for j in range(1, i + 1)])
    
    for i in dice[0]:
        for j in dice[1]:
            for k in dice[2]:
                if (i + j + k + 1) in monster:
                    case[1] += 1
                else :
                    case[0] += 1
                    
    return int((case[0] / sum(case)) * 1000)
```

---



### 코드리뷰

> ### Leader's said
>
> - 조합, 순열은 파이썬에서 유리한 부분이기 때문에 최대한 모듈을 잘 활용하는 것이 좋습니다. 한 번 `product'를 사용해서 이 문제를 풀어보세요.
>
> ### Check_point (스스로 점검)
>
> ```python
> from itertools import product
> 
> def solution(monster, S1, S2, S3):
>     dice = []
>     monster = set(monster)
>     
>     for i in [S1, S2, S3]:
>         dice.append([j for j in range(1, i + 1)])
>     
>     sum_dice = list(map(sum, product(dice[0], dice[1], dice[2])))
>     case = sum([1 for i in sum_dice if i + 1 not in monster])
>                     
>     return int((case / len(sum_dice)) * 1000)
> ```
>
> - product는 리스트들의 곱집합을 구해주는 메소드다.
>   (from itertools)
>   곱집합이란 다음의 예시를 보면 알 수 있다.
>
>   ````python
>   a = [1, 2]
>   b = [3, 4, 5]
>   
>   list(product(a, b))
>   
>   # [(1,3), (1,4), (1,5), (2,3), (2,4), (2,5)]
>   ````
>
> - 순열, 조합 문제는 되도록이면 python 모듈을 이용하도록 하자!

### 참고 자료

- [곱집합(Cartesian product) 구하기 : product](https://programmers.co.kr/learn/courses/4008/lessons/12835)