## Week3_세소수의합



### 문제 이해

> - **결과**
>   : 자연수 n이 매개변수로 주어질 때, n을 서로 다른 소수 3개의 합으로 표현하는 경우의 수를 return 하는 solution 함수를 작성해주세요.
>
> - **제한사항**
> > - 에라토스테네스의 체 알고리즘을 이용해서 풀어주세요.
> > - n은 1,000 이하인 자연수입니다.
>
> - **예시**
>
>   ```markdown
>   어떤 수를 서로 다른 소수 3개의 합으로 표현하는 경우의 수를 구하려 합니다. 예를 들어 33은 총 4가지 방법으로 표현할 수 있습니다.
>   
>   3+7+23
>   3+11+19
>   3+13+17
>   5+11+17
>   ```
>



### 문제 해결

> \# 풀이 1
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 에라토스테네스의 체로 n 보다 작은 소수를 구한다.
>   2. 제일 큰 숫자부터 (sieve.pop()) 고정
>   3. 이중 루프 돌리면서 모든 경우의 수 탐색
>   4. total > n 이면 for 문을 중지시키는 가지치기 추가 
>      (없어도 통과는 함, 시간은 많이 단축)

---

```python
def eratosthenes(n):
    # n 보다 작은 소수 구하기
    sieve = [True for _ in range(n)]
    
    sqrt_n = int(n ** 0.5)
    for i in range(2, sqrt_n + 1):
        if sieve[i] == True:
            for j in range(i * 2, n, i):
                sieve[j] = False
                
    return [i for i in range(2, n) if sieve[i] == True]

def solution(n):
    sieve = eratosthenes(n)
    count = 0
    
    while len(sieve) >= 3:
        num1 = sieve.pop()
        i = 0
        for num2 in sieve:
            for num3 in sieve[i + 1:]:
                total = num1 + num2 + num3
                if total > n:
                    break
                if total == n:
                    count += 1
            i += 1
    return count
```

---



### 코드 리뷰

> - 사실상 이 문제는 완전 탐색 알고리즘입니다.
> - 파이썬에서는 순열과 조합을 쉽게 구할 수 있는 모듈을 제공하기 때문에 이런 문제의 경우 적극적으로 활용할 수 있습니다. : )
>
> - itertools 모듈의 조합 함수를 이용하면 다음과 같이 간결하게 작성가능하다.
>
> ```python
> from itertools import combinations
> 
> def eratosthenes(n):
>     # n 보다 작은 소수 구하기
>     sieve = [True for _ in range(n)]
> 
>     sqrt_n = int(n ** 0.5)
>     for i in range(2, sqrt_n + 1):
>         if sieve[i] == True:
>             for j in range(i * 2, n, i):
>                 sieve[j] = False
> 
>     return [i for i in range(2, n) if sieve[i] == True]
> 
> def solution(n):
>     return [sum(c) for c in combinations(eratosthenes(n), 3)].count(n)
> ```
>
> ### Check_point (스스로 점검)
>
> - 훨씬 더 간결하긴 하나, 원래의 풀이가 시간복잡도는 더 좋다.
> - 원래의 풀이처럼 루프를 돌려가며 하는 방법의 아이디어도 나중에 구현 문제를 풀어낼 때 많은 도움이 될 것이기에 너무 모듈에 의존하지 말자!
> - 하지만 효율성 테스트가 존재하지 않는 문제에 대해서는 위와 같이 깔끔하게 풀어 시간을 줄이자.



### 참고 자료

- [에라토스테네스의 체 - 확장 및 응용](https://velog.io/@joygoround/test-unsolved-%EC%86%8C%EC%88%98-%EC%B0%BE%EA%B8%B0-%ED%8C%8C%EC%9D%B4%EC%8D%AC)

- [에라토스테네스의 체 - 기본](https://leedakyeong.tistory.com/entry/python-%EC%86%8C%EC%88%98-%EC%B0%BE%EA%B8%B0-%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98-%EC%B2%B4)

