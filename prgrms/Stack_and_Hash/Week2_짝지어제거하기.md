## Week2_짝지어제거하기



### 문제 이해

> - **결과**
>   : 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성
>
> - **예시**
>
>   ```markdown
>   예를 들어, 문자열 S = baabaa 라면
>   
>   b aa baa → bb aa → aa → 모두 제거
>   
>   의 순서로 문자열을 모두 제거할 수 있으므로 1을 반환합니다.
>   ```
>

### 문제 해결

> \# 풀이 1
>
> - 예외처리
>   1. ~~홀수 길이의 문자열이 Input으로 들어왔을 때 return 0~~
>      [굳이 넣을 필요가 없어서 제거]
> - Flow
>   1. 첫번째 문자부터 검사 시작
>   2. 문자를 하나씩 Stack 에 쌓는다.
>   3. 다음 문자를 쌓을 때, Stack의 마지막 문자와 비교
>      3-1. 같다면 둘 다 날리고 다음 시행으로
>      3-2. 같지 않다면 그냥 Stack에 쌓는다.

---

```python
def solution(s):
    stack = [s[0]]
    
    for i in s[1:]:
        if stack[-1:] == [i] :
            stack.pop()
        else :
            stack.append(i)
            
    return int(stack == [])
```

---

### 코드리뷰

> 리더 曰

- 조건을 `stack[-1] == i`로 할 수 있습니다. :)

- 조건과 `:` 사이는 붙이는 것이 좋습니다. :)

- 다음과 같이 정리 할 수 있습니다.

  ```python
  def solution(s):
      stack = []
  
      for i in s:
          if stack[-1] == i:
              stack.pop()
          else :
              stack.append(i)
  
      return int(stack == []) 
  ```

- 이 경우 코드의 깔끔함을 위해 다음과 같이 추가 조건을 넣어주는게 좋을 것 같습니다. :)

  ```python
  def solution(s):
      stack = []
  
      for i in s:
          if stack and stack[-1] == i:
              stack.pop()
          else :
              stack.append(i)
  
      return int(stac]k == []) 
  ```

> 自

- 리더님 코드에서, stack이 빈리스트일 경우 for 문의 첫 시행에서 IndexError 발생.

- 다음과 같이 try - except 구문을 이용하여 수정해보았음.

  ```python
  def solution(s):
      stack = []
      
      for i in s:
          try :
              if stack[-1] == i:
                  stack.pop()
              else :
                  stack.append(i)
          except IndexError:
              stack.append(i)
              
      return int(stack == [])
  ```

- 리더님께서 수정해주신 코드를 봤을 때, 훨씬 더 간결하고 깔끔한 느낌이 든다.

- 어거지로 끼워맞추는 식의 코드보다는, 그것들을 관통하는 조건들을 찾아 넣어주도록 하자!



### 참고 자료

