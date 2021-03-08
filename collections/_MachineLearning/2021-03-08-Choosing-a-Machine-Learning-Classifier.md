---
layout: post
title: Machine Learning 분류 모델 선정하기
subtitle: 상황별
tags: [MACHINE_LEARNING]
cover-img: assets/img/Machine-Learning-Cover.jpg
comments: true
mathjax: true
---

Machine Learning Project를 갓 시작하게 되었다면, 문제 해결을 위해 어떤 모델을 정해야할지 고민하는 단계가 가장 먼저 시작됩니다. 여기서 Machine Learning은 Deep Learning을 포함하는 더 큰 범주의 학습형 인공지능을 이야기합니다. [http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/](http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/)에 좋은 글이 있어 정리하려고 합니다.

![](https://assets-global.website-files.com/5fb24a974499e90dae242d98/5fb24a974499e96f7b2431db_AI%20venn%20diagram.png)


## Occam's Razor  
> "더 많은 것들을 필요없이 가정해서는 안된다"
> "더 적은 수의 논리로 설명이 가능한 경우, 많은 수의 논리를 세우지 말라"

![](https://www.dropbox.com/s/afl1be96ryn3yng/occam%27s_Razor.png?raw=1)

이미지의 가로축은 모델의 coverage (=depth, complexity)이며 세로축은 모델의 결과 확률 분포입니다. 만약, 해결해야할 문제의 범위가 $$C_{1}$$이라면 $$H_{1}$$보다는 $$H_{2}$$가 더 효과적인 모델일 것입니다. 

모델 선정에는 Occam's Razor 가설을 항상 염두에 두는 것이 좋습니다.

## Training set의 크기와 모양
Task의 complexity를 정확히 계산하는 것은 어렵습니다. 대신, 우리가 가진 Training set의 크기와 모양을 가지고 complexity를 추정해볼 수 있습니다. 1차원적으로는 크기(개수)만 볼 수 있습니다. 만약 training set이 작다면 high bias/low variance 모델이 low bias/high variance 모델보다 나은 선택이 될 수 있습니다. 후자는 과적합(overfitting)문제가 발생할 수 있기 때문입니다. 하지만 training set의 크기가 점점 커진다면 어느 순간부터는 low bias / high variance 모델이 더 좋은 결과를 보일 것입니다. high bias 모델은 높은 정확도를 얻기에는 부적합하기 때문입니다.  

또한 Generative model과 Discriminative model을 각각 생각해볼 수 있습니다. Generative Model은 데이터 $$X$$가 생성되는 과정을 두 개의 확률 모형, 즉 $$p ( Y )$$ , $$p(X \mid Y)$$으로 정의하고, 베이즈룰을 사용해 $$p ( Y \mid X )$$를 간접적으로 도출하는 모델을 가리킵니다. Generative Model의 학습 목표는 확률에 대한 분포(distribution)이 되며 Supervised(지도), Unsupervised(비지도) learning에 속합니다. Discriminative model은 데이터 $$X$$가 주어졌을 때 레이블 $$Y$$가 나타날 조건부확률 $$p ( Y \mid X )$$를 직접적으로 반환하는 모델을 가리킵니다. Supervised learning 범주에 속합니다.  

## high bias / low variance  
**Naive Bayes** : 확률적 독립을 가정하고 count 기반으로 확률을 추정합니다.  

$$P ( c \mid x ) = \frac { P ( x \mid c ) P ( c ) } { P ( x ) }$$  

거의 가장 간단한 Machine learning 모델입니다. 매우 빠른 연산속도가 장점이며, 낮은 complexity의 task를 처리할 때 꽤 괜찮은 결과를 보여줍니다.  

**Logistic Regression** : 데이터의 feature들 사이에 연관성을 고려하면서, 확률적 해석이 가능합니다. 계속해서 새로운 데이터가 수급되는 경우, 쉽게 모델을 업데이트할 수 있다는 장점이 있습니다. 분류 threshold 값을 수정해나가면서 확률적 framework을 만들고 싶을 때 활용하기 좋습니다. Neural Network과 깊은 연관성이 있습니다.  


## low bias / high variance  
**KNN(k-Nearest Neighbor)** : Dataset의 모든 데이터와 비교하여 가장 가까운 K개의 데이터를 참고하여 추정을 수행하는 방법입니다. Dataset이 클수록, feature가 많을수록 계산량이 많아집니다. 일관성이 있는 __적당한__ 크기의 데이터가 확보되었을 때 사용하기 좋습니다. 

**Decision Trees** : 결과를 쉽게 해석 및 설명이 가능하며, feature들 간의 상호작용을 제어하기 용이합니다. KNN와 같이 non-parametric 모델이기 때문에 data에 대한 사전 지식이 전혀 없을 때 유용합니다. 하지만 overfitting되기 쉽다는 단점이 있어, 이를 보완한 Random Forest를 이용하는 것이 좋습니다.

**SVM(Support Vector Machine), Deep Leaning Models** : Neural Network를 이용한 모델입니다. Deep Learning이 대두되기 전에는 SVM은 아주 많은 분야에서 Machine Learning 파트를 주도했습니다. 높은 정확도에 overfitting에 대해서도 나쁘지 않은 특징이 있지만, 컴퓨팅 자원 소모가 높고, 해석이 힘든 것은 단점이 됩니다. 그럼에도 불구하고 하드웨어와 Deep Learning 기술의 발전으로 __매우 높은 정확도__ 를 얻는 것이 가능해지면서, 이제는 아주 많은 Task에서 Deep Learning은 대세가 되었습니다.


### 참고
[https://ratsgo.github.io/generative%20model/2017/12/17/compare/](https://ratsgo.github.io/generative%20model/2017/12/17/compare/)  
[http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/](http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/)  
[https://process-mining.tistory.com/131](https://process-mining.tistory.com/131)