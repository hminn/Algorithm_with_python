## Week4_최솟값만들기



### 문제 이해

> - **결과**
>   : 
>
> - **제한사항**
>   
> > - 
>
> - **예시**
>
>   ```markdown
>   
>   ```
>



### 문제 해결

> \# 풀이 1
>
> - 예외처리
>   1. 
> - 해결과정
>   1. 

---

```python
def solution(A,B):
    return sum([i * j for i, j in zip(sorted(A), sorted(B, reverse = True))])
```

- sorted 함수의 reverse 옵션을 True로 넣어주면 내림차순으로 정렬이 가능하다.

---



### 참고 자료

