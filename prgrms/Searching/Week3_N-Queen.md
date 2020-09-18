## Week3_N-Queen



### 문제 이해

> - **결과**
>   : 체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.
>
> - **제한사항**
>   
> > - 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
> > - n은 12이하의 자연수 입니다.
>
> - **예시**
>
>   ```markdown
>   
>   ```
>



### 문제 해결

> \# 풀이 1 : 백트래킹 알고리즘
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 

---

```python
# 가로, 세로, 대각에 퀸이 있는지 check
# 가로 : 1, 세로 : 2, 대각 : 3
# 아무것도 없다? 진출 가능 하면 0
def checker(i, j, chess):
    
    
def promise(i, j, chess, answer):
    
    if checker(i, j, chess):
        if 
        
        # promise(0, j + 1, chess, answer)
    else :
        chess[i][j] == 1
        promise(i, j + 1, chess, answer)
            
    
def solution(n):
    answer = 0
    chess = [[0] * n for _ in range(n)]
    
    
    return answer
```

---



### 참고 자료

