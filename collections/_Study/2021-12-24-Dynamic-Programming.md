---
layout: post
title: Dynamic Programming
subtitle: 동적 프로그래밍
tags: [STUDY, DEVELOP]
mathjax: true
comments: true
---

학부 알고리즘 수업 때 처음 배웠던 동적 프로그래밍. 수업을 들으면서도 왜 이름이 동적 프로그래밍인지 점점 더 헷갈리게 되는 개념이었습니다. 결론적으로, **반복적으로 계산하게 되는 부분들을 저장해놓고, 다음 스텝의 계산에 이용하는 방법**을 뜻하는 동적 프로그래밍을 오랜만에 다시 정리해봅니다. 기초적인 내용이라 쉽게 읽고 떠올릴 수 있도록 작성했습니다.

## 피보나치 수열

동적 프로그래밍의 가장 쉬운 예가 바로 피보나치 수열입니다.
잘 알려진 대로, 피보나치 수열은 앞의 두 숫자의 합을 다음 숫자로 하는 수열을 말합니다. 수식으로 보면 다음과 같습니다.

$$\begin{cases}
    F_1 = F_2 = 1 \\
    F_n = F_{n-1} + F_{n-2}  (n \in \{3, 4, 5, \dots \})
\end{cases}$$

이 공식대로 계산하면 우리는 다음과 같은 수열을 얻을 수 있습니다.

| 순서 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 값 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 |

$$F_{10}$$ 값을 얻기 위해서 우리는 $$F_{9}$$, $$F_{8}$$ 값을 계산해야합니다. 그런데, 우리가 위의 표를 그릴 때도 그랬듯, 우리는 $$F_{9}$$, $$F_{8}$$를 먼저 계산하지 않습니다. $$F_{3}$$과 $$F_{4}$$부터 계산해나가는게 훨씬 쉽다는걸 직관적으로 알 수 있습니다. 중복된 계산을 피하기 위해서 이전에 이미 해놓은 계산을 참조하는 것이죠.

위에 언급한 피보나치 수열의 수식을 _극단적으로_ 그대로 구현하면 아래와 같습니다.

```python
def fibo_bad(x):
    assert isinstance(x, int) and x > 0
    if x <= 2:
        return 1
    else:
        return fibo_bad(x-1) + fibo_bad(x-2)
```

`fibo_bad(x)`를 정석적으로 계산하기 위해 `fibo_bad(x-1)`과 `fibo_bad(x-2)`를 구해야 하는데, 이렇게 구현하면 중복된 계산을 _매우_ 많이 하게 됩니다.

![](https://www.dropbox.com/s/dxsy0y5177vuisb/fibonacci.png?raw=1)

그림과 같이, x가 커질수록 layer가 하나씩 더 쌓이면서, 계산량이 기하급수적으로 늘어납니다. 계산량은 등비수열의 합으로 늘어나겠네요.

$$S_n = a + ar + ar^2 + ... + ar^n-1  
     =\frac{a(r^n-1)}{r-1}$$

위 등비 수열에서 $$a=1$$, $$r=2$$니까 총 계산량은 $$2^x-1$$이 되겠습니다.

동적 프로그래밍을 이용해서 구현하면 아래와 같습니다.

```python
def fibo_good(x):
    assert isinstance(x, int) and x > 0
    memory = [1, 1]
    for i in range(2, x):
        memory.append(memory[i-1] + memory[i-2])
    return memory[x-1]
```

결과는 당연히 `fibo_good`이 훨씬 빠릅니다. 피보나치 수열에서 적용할 때 직관적으로 그랬듯, 다수의 경우에서 Bottom-up 방식이 더 효과적입니다. 해결해야할 문제의 성격을 먼저 파악해보고 문제를 작은 단위로 나누어 Divide and Conquer로 공략할 때, 중복되는 계산이 많이 발견된다면 Dynamic Programming 적용을 고려해볼만 할 것 같습니다.