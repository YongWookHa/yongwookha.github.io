---
layout: post
title: Cosine Annealing Warm Up Restarts
subtitle: with code
tags: [STUDY, DEVELOP, MACHINE_LEARNING]
cover-img: /assets/img/cosine-annealing-warm-up-restart.png
comments: true
mathjax: true
---

Optimzer의 Learning Rate을 관리하는 Scheduler를 이용하면 똑같은 환경에서도 조금 더 나은 학습 결과를 얻을 수 있습니다. [pytorch](https://pytorch.org/)에서 여러가지 종류의 Scheduler를 제공하니, 종류와 활용 방법을 체크하는 것이 좋습니다. [https://sanghyu.tistory.com/113](https://sanghyu.tistory.com/113) 블로그에서는 각 Scheduler의 Learning Rate 변화를 시각적으로 표현해주셔서 직관적으로 파악하는 것을 도와줍니다. 참고하시길 추천합니다.

다만, Pytorch가 제공하는 Scheduler 중 일부는 그대로 이용하기에는 몇가지 아쉬움이 있습니다. 특히 이번 포스팅에서 다룰 Cosine Annealing Warm Up Restarts의 경우 특히 그렇습니다.  

## Pytorch Original  
`torch.optim.lr_scheduler.CosineAnnealingWarmRestarts`는 아래와 같은 parameter를 가집니다.  
_[https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.CosineAnnealingWarmRestarts.html](https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.CosineAnnealingWarmRestarts.html)
- `optimizer` _(Optimizer)_ – Wrapped optimizer.
- `T_0` _(int)_ – Number of iterations for the first restart.
- `T_mult` _(int, optional)_ – A factor increases $$T_{i}$$ after a restart. Default: 1.
- `eta_min` _(float, optional)_ – Minimum learning rate. Default: 0.
- `last_epoch` _(int, optional)_ – The index of last epoch. Default: -1.
- `verbose` _(bool)_ – If `True`, prints a message to stdout for each update. Default: False.

위의 parameter로 조작할 수 있는 요소는 아래의 그림과 같이 제한적입니다.

![](https://www.dropbox.com/s/gwvl9heiv7kvp54/cosine-annealing-warm-up-restart-pytorch.png?raw=1)

## Custom Cosine Annealing Warm Up Restarts  

위의 Pytorch 제공 class에서 scheduler 주기에 따라 `eta_max`를 변경하거나, `warmup_steps`를 바꾸는 등의 더 많은 조작 요소를 추가하기 위해 나름대로의 custom 코드를 만들어 이용해왔습니다. 그런데 제가 만들었던 코드 보다 더 깔끔하고 직관적으로 작성된 repository가 있어 소개하고자 합니다.

[katsura-jp/pytorch-cosine-annealing-with-warmup](https://github.com/katsura-jp/pytorch-cosine-annealing-with-warmup/blob/master/cosine_annealing_warmup/scheduler.py)에서는 `pip`을 통해 install해서 간편하게 import 할 수 있는 pytorch `lr_scheduler` class를 제공합니다.

이 라이브러리에서는 아래의 parameter를 제공합니다.
- `optimizer` _(Optimizer)_: Wrapped optimizer.
- `first_cycle_steps` _(int)_: First cycle step size.
- `cycle_mult` _(float)_: Cycle steps magnification. Default: 1.
- `max_lr` _(float)_: First cycle's max learning rate. Default: 0.1.
- `min_lr` _(float)_: Min learning rate. Default: 0.001.
- `warmup_steps` _(int)_: Linear warmup step size. Default: 0.
- `gamma` _(float)_: Decrease rate of max learning rate by cycle. Default: 1.
- `last_epoch` _(int)_: The index of last epoch. Default: -1.

조금 더 많은 parameter를 이용하여 아래의 그림과 같이 내가 원하는 대로 특정 학습 단계에서 Learning rate을 조절할 수 있습니다.

![](https://www.dropbox.com/s/ue5lf570o8z3m4b/cosine-annealing-warm-up-restart-custom.png?raw=1)

