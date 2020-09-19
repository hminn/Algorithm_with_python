## Week3_카펫



### 문제 이해

> - **결과**
>   : Leo가 본 카펫에서 갈색 격자의 수 brown, 빨간색 격자의 수 red가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.
>
> - **제한사항**
> > - 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
> > - 빨간색 격자의 수 red는 1 이상 2,000,000 이하인 자연수입니다.
> > - 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.
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
>   
>   1. 가로의 길이는 세로의 길이보다 길거나 같다.
> - 해결과정
>   
>   1. brown + red = 가로 x 세로 꼴이므로, 총 격자 수의 약수를 구한다.
>   
>   2. 세로의 범위는 3부터 (brown + red)의 제곱근까지의 자연수 이므로 이에 해당하는 후보 수를 리스트로 만든다.
>   
>   3. 후보 수들을 loop를 돌리며 다음 규칙을 만족하는지 check
>   
>      3-1. ((세로 - 2) + 가로 ) * 2 == brown (브라운이 둘러싸고 있음을 나타내는 규칙)

---

```python
def solution(brown, red):
    n = brown + red
    answer = [0, 0]
    sqrt_n = int(n**0.5)
    cand = [i for i in range(3, sqrt_n + 1) if n % i == 0]
    for c in cand :
        v = n // c # 세로 v, 가로 c
        if brown == ((v - 2) + c) * 2:
            answer[0], answer[1] = v, c
    return answer
```

---



### 코드 리뷰

>- 미리 후보군에 해당하는 수들의 List를 만들지 않고,
>  '검사 + 숫자 조정' 의 역할을 모두 하는 Loop를 만들면 훨씬 더 깔끔하다.
>- 다음의 코드를 보며 느껴보자.
>
>```python
>def solution(brown, red):
>    # width, height의 초기값을 설정해준다.
>    width = (brown + red) // 3 # width의 최댓값
>    height = 3 # height 의 최솟값
>
>    # 후보가 될 수 있는 값들을 가지고 Loop를 돌린다.
>    # 조건을 만족하면 바로 빠져나온다.
>    while (width - 2) * (height - 2) != red:
>        width -= 1 # width를 하나씩 줄여가며 검사한다.
>        height = (brown + red) // width # width가 줄면 height은 증가
>	return [width, height]
>```
>
>- 위의 코드가 훨씬 깔끔하고 직관적으로도 더 와닿는다.