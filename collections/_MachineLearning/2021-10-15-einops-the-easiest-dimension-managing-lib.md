---
layout: post
title: 이제 차원 관리는 einops
subtitle: stupidly easy
tags: [STUDY, DEVELOP, MACHINE_LEARNING]
cover-img: /assets/img/einops.png
comments: true
---

Deep learning model implementation에서 제일 재미있고 어려운 부분이 차원 조작 및 관리입니다. 텐서의 차원을 요리조리 변경하면서 레이어를 쌓는 것이 모델 개발의 핵심이자 정수이지 않을까 생각합니다.

`numpy`로 처음 차원 조작 개념을 배워서 `torch`로 실제 적용하다가, 최근에는 [einops](https://github.com/arogozhnikov/einops)를 발견하고, 즐겨 사용하고 있습니다. 굉장히 직관적이며 빠르게 작동합니다. 머리가 복잡해질 일 없이 내가 원하는 차원 배열만 입력하면 알아서 동작하는 방식으로 차원을 바꿔줍니다. 가끔은 이런 유용한 툴들이 계속해서 만들어질때마다 "이러다가 모델 개발이 너무 쉬워지면 어떡하지?" 싶습니다. 결국 답은 계속해서 공부하는 것이네요😅

이 분야의 셀럽들도 einops를 좋아하는지 tweeter에 언급한 내용들이 있습니다. 그리고 einops의 프로젝트 매니저는 README에서 이걸 자랑하고 있습니다🤣

<div style="display: table">
  <div style="float: left; width: 50%;">
  <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Writing better code with PyTorch and einops <a href="https://t.co/GTIWdttOhu">https://t.co/GTIWdttOhu</a> 👌</p>&mdash; Andrej Karpathy (@karpathy) <a href="https://twitter.com/karpathy/status/1290826075916779520?ref_src=twsrc%5Etfw">August 5, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
  </div>
  <div style="float: left; width: 50%;">
  <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Slowly but surely, einops is seeping in to every nook and cranny of my code. If you find yourself shuffling around bazillion dimensional tensors, this might change your life: <a href="https://t.co/xYgSrbqoy9">https://t.co/xYgSrbqoy9</a></p>&mdash; Nasim Rahaman (@nasim_rahaman) <a href="https://twitter.com/nasim_rahaman/status/1216022614755463169?ref_src=twsrc%5Etfw">January 11, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
  </div>
</div>

Tesla에서 활약하고 있는 Andrej Karpathy도 언급한 적이 있다니 신기하네요! 얼마전 Tesla의 AI-day 컨퍼런스 생중계에서 발표하는 걸 봤었는데 말이죠😍
각설하고, einops의 사용법에 대해 간단히 알아봅니다.

---
<br/>


# ✍einops API  
## Operation  
```python
from einops import rearrange, reduce, repeat
# rearrange elements according to the pattern
output_tensor = rearrange(input_tensor, 't b c -> b c t')
# combine rearrangement and reduction
output_tensor = reduce(input_tensor, 'b c (h h2) (w w2) -> b h w c', 'mean', h2=2, w2=2)
# copy along a new axis 
output_tensor = repeat(input_tensor, 'h w -> h w c', c=3)
```

einops 자체가 굉장히 직관적이라 README에 설명되어 있는 간단한 예시만 보아도 어느정도 감이 잡힙니다. input tensor의 각 차원이 가지는 의미를 space seprated하게 입력하고 `->` 이후에 원하는 방식으로 수정을 요청하는 방법입니다. 각 차원 순서의 크기를 상세히 기록하여 가독성을 높일 수도 있습니다. `str`로 작성하는 `pattern`이 자연어에 가깝기 때문에 기존의 방법보다 사용하기도 쉽고, 해석하기도 쉽습니다.

input_tensor의 각 차원을 순서대로 naming하고 이를 재배치합니다. 소괄호`(``)`로 묶으면 해당 차원 크기가 곱해집니다.

## Layer   

Deep learning framework의 일반 layer module과 같이 쓸 수 있는 `Rearrange`와 `Reduce` 두 가지 layer가 제공됩니다. 다양한 Deep Learning framework에 대응하기 위해 같은 인터페이스의 API를 별도의 버전으로 제공합니다.
아래처럼 사용하는 framework에 따라 instance를 만들고 `forward`를 통해 tensor나 variable을 조작할 수 있습니다.

```python
from einops.layers.chainer import Rearrange, Reduce
from einops.layers.gluon import Rearrange, Reduce
from einops.layers.keras import Rearrange, Reduce
from einops.layers.torch import Rearrange, Reduce
from einops.layers.tensorflow import Rearrange, Reduce

layer = Rearrange(pattern, **axes_lengths)
layer = Reduce(pattern, reduction, **axes_lengths)

# apply created layer to a tensor / variable
x = layer(x)
```

보통은 아래처럼 일반적인 layer module들과 동일한 매커니즘으로 이용합니다. 아래는 Pytorch 예시입니다.

```python
from torch.nn import Sequential, Conv2d, MaxPool2d, Linear, ReLU
from einops.layers.torch import Rearrange

model = Sequential(
    Conv2d(3, 6, kernel_size=5),
    MaxPool2d(kernel_size=2),
    Conv2d(6, 16, kernel_size=5),
    MaxPool2d(kernel_size=2),
    # flattening
    Rearrange('b c h w -> b (c h w)'),  
    Linear(16*5*5, 120), 
    ReLU(),
    Linear(120, 10), 
)
```

굳이 억지로 모든 operator를 대체할 필요는 없지만, 적재적소에 잘 사용하면 여러모로 효율적일 것 같습니다🤟  
