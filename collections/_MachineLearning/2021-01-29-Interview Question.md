---
layout: post
title: Interview Question & Answer
subtitle: 출근 루틴, 하루 3문제
tags: [MACHINE_LEARNING, IT, DEVELOP]
cover-img: https://assets.rappler.com/C39701B689414EE2858D1ED6A10DF1A0/img/D067EDAB25364E8CA6AFBB7537387778/1-job-interview-tips-impress-20150308.jpg
comments: true
mathjax: true
---

항상 양질의 글을 읽을 수 있어 즐겨찾는 [zzsza(변성윤)](https://github.com/zzsza)님의 블로그에서 [Datascience-Interview-Questions](https://zzsza.github.io/data/2018/02/17/datascience-interivew-questions/) 포스트를 발견했습니다. 공유되어 있는 양질의 문제들을 보며 출근 루틴으로 ~~2~3문제씩~~(현실은 1문제씩..) 답안을 만들어야겠다는 생각이 들었습니다. 원문에는 다양한 도메인에 대한 질문들이 있는데 그 중, 관심을 가지고 있는 몇 가지 주제에 대해서 공부하고 나름대로 답안을 작성하여 기록하고자 합니다.

_항상 좋은 글로 영감을 주시는 변성윤님 감사합니다._

## Intro

Question by [Seongyun Byeon](https://github.com/zzsza)  
Answer by [YongWook Ha](https://github.com/YongWookHa)

답안은 `펼치기/접기` 형식으로 문제 아래 작성하고 있습니다. 각 문제의 컨텐츠를 매일 꾸준히 공부하고 답안을 채워나갈 예정이라 컨텐츠 완성까지는 꽤 시간이 소요될 것 같습니다.   
답안에 오류가 있거나 부족한 점이 발견된다면, 댓글 남겨주시기 바랍니다.

## Contents

- [통계 및 수학](#통계-및-수학)
- [분석 일반](#분석-일반)
- [머신러닝](#머신러닝)
- [딥러닝](#딥러닝)	 
	- [딥러닝 일반](#딥러닝-일반)
	- [컴퓨터 비전](#컴퓨터-비전) 
	- [자연어 처리](#자연어-처리)
	- [강화학습](#강화학습)
	- [GAN](#GAN) 
- [추천 시스템](#추천-시스템)


## 통계 및 수학
- 고유값(eigen value)와 고유벡터(eigen vector)에 대해 설명해주세요. 그리고 왜 중요할까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 어떤 선형 변환 A에 대해, 해당 선형 변환의 결과가 원래의 상수배가 되는 0이 아닌 벡터를 고유벡터라고 하며, 이 상수 값을 고유값이라고 한다.  
  > ![EigenValue-EigenVector](https://www.dropbox.com/s/eethy4l4b7oqfxd/EigenValue-EigenVector.jpeg?raw=1)  
  > 고유값과 고유벡터는 정방행렬의 대각화와 밀접한 관련이 있다. 행렬을 고유벡터와 고유값으로 이루어진 행렬들로 대각화 분해하면 (eigen decomposition) 이를 이용하여 해당 행렬의 거듭제곱, 역행렬, 대각합, 행렬의 다항식 등을 매우 효율적으로 계산할 수 있기 때문에 중요하다.  
  >
  > 참조 : [https://darkpgmr.tistory.com/105](https://darkpgmr.tistory.com/105)

  </details>
- 샘플링(Sampling)과 리샘플링(Resampling)에 대해 설명해주세요. 리샘플링은 무슨 장점이 있을까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 샘플링은 모집단에서 부분집합을 추출하는 행위를 말하며, 리샘플링은 부분집합으로 모집단을 추론하는 행위이다. 전체 집단에 대한 완전한 조사 없이 부분집합만으로 해당 집단에 대한 추론을 '경제적으로' 수행할 수 있다는 장점이 있다.  

  </details>
- 확률 모형과 확률 변수는 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > ![](https://t1.daumcdn.net/cfile/tistory/99D24E335A223F560D)  
  > 확률 변수는 표본(사건)의 특성을 숫자로 변환하는 단계를 의미하며, 실제 공간에서 일어나는 표본(사건)을 discrete 혹은 continuous한 숫자로 할당한다.   
  > 확률 모형은 확률 변수를 이용하여 데이터의 분포를 수학적으로 정의하는 방법을 말하며, 확률 함수들의 조합으로 구성될 수 있다.  
  >  
  > _그림 출처 : https://drhongdatanote.tistory.com/49_

  </details>
- 누적 분포 함수와 확률 밀도 함수는 무엇일까요? 수식과 함께 표현해주세요
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 확률 : 사건(event)이라는 표본의 집합에 대해 할당된 숫자
  > 확률 분포 : 어떤 사건에 어느 정도의 확률이 할당되었는지를 묘사한 것
  >  
  > ![](https://www.dropbox.com/s/payloh7uqv3zxfp/%ED%99%95%EB%A5%A0%EB%B0%80%EB%8F%84%ED%95%A8%EC%88%98.jpeg?raw=1)
  > 누적 분포 함수는 (−∞ )부터 특정 지점까지 확률 변수가 존재할 확률을 나타낸다.   
  > 확률 밀도 함수는 특정 구간(\[a,b]) 내에서 확률 변수의 분포를 나타내는 함수이다.   
  >  
  > _출처 및 참조 : [https://datascienceschool.net/view-notebook/fa24ed1f520f4691b8bce979860ebc0a/](https://datascienceschool.net/view-notebook/fa24ed1f520f4691b8bce979860ebc0a/)_

  </details>
- 베르누이 분포 / 이항 분포 / 카테고리 분포 / 다항 분포 / 가우시안 정규 분포 / t 분포 / 카이제곱 분포 / F 분포 / 베타 분포 / 감마 분포 / 디리클레 분포에 대해 설명해주세요. 혹시 연관된 분포가 있다면 연관 관계를 설명해주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > **베르누이 분포**  
      동전 던지기처럼 0혹은 1의 값만 가지는 확률변수 $$X$$에 대해 $$P(X=0)=p, P(X=1)=q, 0 \leq p \leq 1, q=1-p$$ 를 만족하는 확률분포. 일종의 이항분포다.  
    >  
    > **이항 분포**  
      **연속**된 $$n$$번의 독립 시행에서 각 시행의 확률이 $$p$$일때, $$n \geq 0, 0 \leq p \leq 1$$ 를 만족하는 확률분포.  
    >  
    > **카테고리 분포**  
      확률변수 $$X$$의 시행 결과가 $$k$$개의 카테고리이고, 각 카테고리가 선택될 확률이 $$\theta = ( \theta _ { 1 } , \cdots , \theta _ { k } )$$ 라고 할 때, 확률변수 $$X$$는 모수가 $$\theta$$이고 카테고리가 $$k$$개인 카테고리 분포를 따른다고 한다.  
    >  
    > **다항 분포**
      확률이 $$\theta = ( \theta _ { 1 } , \cdots , \theta _ { k } )$$인 카테고리 시행을 n번 **반복**했을 때, 각 카테고리가 선택되는 횟수는 다항분포를 따른다.
      확률변수 $$Y = ( Y _ { 1 }, \cdots , Y _ { k } ) \in \mathbb { R } ^ { k }$$ 가 모수 $$( n , \theta )$$의 다항분포를 따르고, 카테고리 시행으로 얻은 $$n$$개의 샘플을 $$X = ( x _ { 1 } , \cdots , x _ { n } ) \in \mathbb { R }$$이라 할 때, 다음과 같이 나타낼 수 있다.
      $$\sum_{i=1}^n \mathbf{x}_i = 
        \begin{bmatrix} x_{11} \\ \vdots \\ x_{1k} \end{bmatrix} + 
        \cdots + 
        \begin{bmatrix} x_{n1} \\ \vdots \\ x_{nk} \end{bmatrix} 
        = \begin{bmatrix} \sum_i^n x_{i1} \\ \vdots \\ \sum_i^n x_{ik} \end{bmatrix} 
        = \begin{bmatrix} y_{1} \\ \vdots \\ y_{k} \end{bmatrix} 
        = \mathbf{y}$$
  </details>  
- 조건부 확률은 무엇일까요?
  <details markdown="1">
    <summary>[답안]</summary>
    
    > 조건부 확률(Conditional probability)는 주어진 사건이 일어났다는 가정 하에, 다른 한 사건이 일어날 확률이다.  
    > A에 대한 B의 조건부 확률. 즉, A가 일어났을 때 B도 일어날 확률은 다음과 같다.  
    > $$Pr(B|A)=\frac {Pr(A \cap B)} {Pr(A)}$$  

  </details>
- 공분산과 상관계수는 무엇일까요? 수식과 함께 표현해주세요
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 어떤 확률변수 X가 있을 때, 이 변수의 분포를 나타내기 위해 평균과 분산을 이용한다. 그런데 만약, 확률변수가 2가지라면 그 분포를 어떻게 나타내야할지 생각해보자.  
  > 이때 X와 Y 두 확률변수의 상관관계를 반영하기 위해 이용하는 개념이 공분산(Covariance)이다.  
  > 확률변수 X, Y에 대한 분포를 나타내려할 때, X의 평균과 Y의 평균을 각각 $$E(X) = \mu, E(Y) = v$$ 라고 하자.  
  > 이때, 공분산은 X의 편차와 Y의 편차를 곱한것의 평균을 의미한다. 수식은 다음과 같다.  
  > $$ Cov(X, Y) = E((X-\mu)(Y-v))$$  
  > 공분산은 두 변수 간에 양의 상관관계가 있는지, 음의 상관관계가 있는지 알려준다. 만약 두 변수가 서로 독립이면 공분산 값은 0 이다.
  > 
  > 상관계수는 공분산을 구할 때, X, Y의 단위에 의한 영향을 제거하기 위해 이용된다. 정의는 아래와 같이 공분산을 X와 Y의 분산으로 나누는 개념이다.
  > $$\rho = \frac { Cov(X, Y) } { \sqrt { Var(X) Var(Y)} } , -1 \leq \rho \leq 1$$
  >  
  > 참조 : [https://destrudo.tistory.com/15](https://destrudo.tistory.com/15)

  </details>

- 신뢰 구간의 정의는 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 통계에서는 보통 모집단에서 임의로 선택한 표본으로 확률 분포를 구하게 된다. 이때 모집단에서 선택할 수 있는 수많은 표본들을 대표하기 위해 해당 표본 평균에 대한 표준 편차를 "표준 오차"라고 하며 그 식은 아래와 같다.  
  > $$ SEM = \frac { \sigma } { \sqrt { n } } $$
  > **신뢰 구간**은 표본의 평균을 기준으로 -2SEM에서부터 +2SEM까지를 말하며, 모집단에서 선택한 어떤 표본이 해당 구간 안에 들어올 확률인 **신뢰 수준**과 함께 쓰인다. ex) 신뢰수준 95%, 표본오차 ±3% 
  > 
  > 참조 : [https://angeloyeo.github.io/2021/01/05/confidence_interval.html](https://angeloyeo.github.io/2021/01/05/confidence_interval.html)

  </details>
- p-value를 고객에게는 뭐라고 설명하는게 이해하기 편할까요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > ![](https://www.dropbox.com/s/fslndtyehdy0z2k/p-value.png?raw=1)   
  >  
  > p-value는 확률 분포 그래프의 양쪽 극단의 범위를 나타낸다.  
  > 모집단에서 추출한 표본으로 생성한 가설을 검증하는 단계에서, 추가로 뽑은 샘플이 매우 희박한 발생확률을 가지고 있을 때, 이 샘플이 우연히 발생한 것인지 가설이 틀린 것인지를 판단할 때 사용된다.  
  > 고객에게 설명할때는 "통계적으로 분석하여 예상한 범위를 벗어날 확률"이라고 이야기할 수 있을 듯  

  </details>
- p-value는 요즘 시대에도 여전히 유효할까요? 언제 p-value가 실제를 호도하는 경향이 있을까요?  
  <details markdown="1">
  <summary>[답안]</summary>
  
  > p-value는 관측량이 많아지면 낮아질 수 있다. 새로운 가설의 표본이 커지면 표본 오차가 작아지기 때문이다. 빅데이터 시대에는 데이터의 양이 늘어나면서 p-value가 기존 관행처럼 5% 이하이더라도 유의미하지 않을 수 있다.  
  > 
  > 참고1: [https://angeloyeo.github.io/2020/03/29/p_value.html](https://angeloyeo.github.io/2020/03/29/p_value.html)  
  > 참고2: [https://boxnwhis.kr/2016/04/15/dont_be_overwhelmed_by_pvalue.html](https://boxnwhis.kr/2016/04/15/dont_be_overwhelmed_by_pvalue.html)  

  </details>
- A/B Test 등 현상 분석 및 실험 설계 상 통계적으로 유의미함의 여부를 결정하기 위한 방법에는 어떤 것이 있을까요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 1. AA test  
  >   A와 B를 비교하기 전에 분산된 트래픽에 모두 A안을 보여주고, 동일한 Variation이 관측되는지 확인하는 방법
  > 2. p-value  
  >  통계적 가설이 재현되지 않는 예외 경우의 비율

  </details>
- R square의 의미는 무엇인가요? 
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 결정 계수로서, 가설의 설명력을 의미한다.  
  > 
  > $$ R ^ 2 = ( 1- \frac {추정 모형 MSE} {평균 관측 값의 MSE }) $$  
  > 값이 1에 가까울수록 추정 모형의 설명력(regression line)이 높고, data에 fit하다고 해석할 수 있다.
  >
  > 참조 : (https://jinchory.tistory.com/332)[https://jinchory.tistory.com/332]


  </details>
- 평균(mean)과 중앙값(median)중에 어떤 케이스에서 뭐를 써야할까요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > ![](https://www.dropbox.com/s/ck7e8ga0pz3a2sa/mean_median_mode.png?raw=1)  
  > 위의 이미지는 평균, 중앙값, 최빈값(mode)를 표현한다.  
  > 데이터의 분포 모양에 따라 적절한 값을 골라쓴다.

  </details>  
- 중심극한정리는 왜 유용한걸까요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 중심극한정리 : "모집단이 「평균이 $$ μ $$이고 표준편차가 $$ σ $$인 임의의 분포」을 이룬다고 할 때, 이 모집단으로부터 추출된 표본의 「표본의 크기 $$ n $$이 충분히 크다」면 표본 평균들이 이루는 분포는 「평균이 $$ μ $$ 이고 표준편차가 $$ frac { σ } { \sqrt { n } } $$ 인 정규분포」에 근접한다.  
  >   
  > "모집단의 분포에 상관 없이" 표본이 크면 표본 평균들의 분포가 정규 분포로 수렴한다는 점을 이용하여 수학적 확률 판단이 가능해진다.
  > 참조 : [https://drhongdatanote.tistory.com/57](https://drhongdatanote.tistory.com/57)

  </details>  
- 엔트로피(entropy)에 대해 설명해주세요. 가능하면 Information Gain도요.
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 엔트로피(Entropy)는 무질서성을 의미한다. 통계에서는 관측 범위 내에서 데이터의 분포가 고르면 엔트로피가 낮다고 하며, 분포가 특정 범위로 몰려있으면 엔트로피가 높다고 할 수 있다.  정보이득(Information Gain)은 엔트로피를 기반으로 정보량을 측정하여 "어떤 특징을 선택함으로서 데이터를 더 잘 구분할 수 있는가?"를 의미한다.   
  >  
  > $$ T $$ 는 Training Data, $$ a $$ 는 어떤 특징(attribute)을 의미할 때,
  > $$ InformationGain(T, a) = Entropy(T) - Entropy(T|a) $$ 이다.
  > $$ Entropy(T|a) $$ 는 a를 선택했을 때 T의 엔트로피 값을 의미한다.

  </details> 
- 요즘같은 빅데이터(?)시대에는 정규성 테스트가 의미 없다는 주장이 있습니다. 맞을까요?
- 어떨 때 모수적 방법론을 쓸 수 있고, 어떨 때 비모수적 방법론을 쓸 수 있나요?
- “likelihood”와 “probability”의 차이는 무엇일까요?
- 통계에서 사용되는 bootstrap의 의미는 무엇인가요.
- 모수가 매우 적은 (수십개 이하) 케이스의 경우 어떤 방식으로 예측 모델을 수립할 수 있을까요?
- 베이지안과 프리퀀티스트간의 입장차이를 설명해주실 수 있나요?
- 검정력(statistical power)은 무엇일까요?
- missing value가 있을 경우 채워야 할까요? 그 이유는 무엇인가요?
- 아웃라이어의 판단하는 기준은 무엇인가요?
- 콜센터 통화 지속 시간에 대한 데이터가 존재합니다. 이 데이터를 코드화하고 분석하는 방법에 대한 계획을 세워주세요. 이 기간의 분포가 어떻게 보일지에 대한 시나리오를 설명해주세요
- 출장을 위해 비행기를 타려고 합니다. 당신은 우산을 가져가야 하는지 알고 싶어 출장지에 사는 친구 3명에게 무작위로 전화를 하고 비가 오는 경우를 독립적으로 질문해주세요. 각 친구는 2/3로 진실을 말하고 1/3으로 거짓을 말합니다. 3명의 친구가 모두 "그렇습니다. 비가 내리고 있습니다"라고 말했습니다. 실제로 비가 내릴 확률은 얼마입니까?
- 필요한 표본의 크기를 어떻게 계산합니까?
- Bias를 통제하는 방법은 무엇입니까?
- 로그 함수는 어떤 경우 유용합니까? 사례를 들어 설명해주세요

##### [목차로 이동](#contents)

## 분석 일반 
- 좋은 feature란 무엇인가요. 이 feature의 성능을 판단하기 위한 방법에는 어떤 것이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > 좋은 feature는 데이터셋에서 자주 등장해야하며 분명하고 명확한 의미가 부여되어야 한다.
  > 
  > 참고 : [https://developers.google.com/machine-learning/crash-course/representation/qualities-of-good-features?hl=ko](https://developers.google.com/machine-learning/crash-course/representation/qualities-of-good-features?hl=ko)

  </details>

- "상관관계는 인과관계를 의미하지 않는다"라는 말이 있습니다. 설명해주실 수 있나요?
  <details markdown="1">
  <summary>[답안]</summary>
  
  > **상관관계** : 어떤 변인 x의 값과 다른 변인 y의 값이 함께 변할 때, x와 y의 관계  
  > **인과관계** : 어떤 변인 x의 값이 변하면, 그로 인해서 다른 변인 y의 값이 변할 때, x와 y의 관계
  >
  > 어떤 요인에 의해 x와 y의 값이 동시에 영향을 받아서 변할 수 있다. 영향을 받는 외부 요인을 함께 가진다면 x와 y는 상관관계일 수 있지만 이것으로 x와 y의 인과관계를 설명할 수는 없다.

  </details>
- A/B 테스트의 장점과 단점, 그리고 단점의 경우 이를 해결하기 위한 방안에는 어떤 것이 있나요?
- 각 고객의 웹 행동에 대하여 실시간으로 상호작용이 가능하다고 할 때에, 이에 적용 가능한 고객 행동 및 모델에 관한 이론을 알아봅시다.
- 고객이 원하는 예측모형을 두가지 종류로 만들었다. 하나는 예측력이 뛰어나지만 왜 그렇게 예측했는지를 설명하기 어려운 random forest 모형이고, 또다른 하나는 예측력은 다소 떨어지나 명확하게 왜 그런지를 설명할 수 있는 sequential bayesian 모형입니다.고객에게 어떤 모형을 추천하겠습니까?
- 고객이 내일 어떤 상품을 구매할지 예측하는 모형을 만들어야 한다면 어떤 기법(예: SVM, Random Forest, logistic regression 등)을 사용할 것인지 정하고 이를 통계와 기계학습 지식이 전무한 실무자에게 설명해봅시다.
- 나만의 feature selection 방식을 설명해봅시다.
- 데이터 간의 유사도를 계산할 때, feature의 수가 많다면(예: 100개 이상), 이러한 high-dimensional clustering을 어떻게 풀어야할까요?

##### [목차로 이동](#contents)

## 머신러닝
- Cross Validation은 무엇이고 어떻게 해야하나요?
  <details markdown="1">
  <summary>[답안]</summary>

    > 가지고 있는 전체 data를 나누어서 형성한 각 batch가 모두 한번씩은 validation set으로 활용되도록 각 iteration 마다 validation set을 번갈아 바꿔 사용한다.  
    > 주로 Train 데이터가 적을 때 사용한다.
  </details>
  
- 회귀 / 분류시 알맞은 metric은 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > **회귀** : `MSE` -> Ground Truth와 Pred. 값의 차이(continuous)를 나타낼 수 있는 metric  
  > **분류** : `Precision`, `Recall`, `Accuracy` -> 분류 결과(discrete)의 신뢰도를 나타낼 수 있는 통계 metric
  </details>
  
- 알고 있는 metric에 대해 설명해주세요(ex. RMSE, MAE, recall, precision ...)
  <details markdown="1">
  <summary>[답안]</summary>

  > ![파일_001](https://user-images.githubusercontent.com/12293076/66627396-8d07d280-ec36-11e9-8f93-6bd0cad570f0.png)
  </details>
- 정규화를 왜 해야할까요? 정규화의 방법은 무엇이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 데이터를 mini-batch로 쪼개거나, 네트워크의 깊이가 깊어짐에 따라 각 요소들의 분포값이 의도와 달라져서 학습에 차질이 생기는 경우가 발생하기 때문.  
  > **Batch Normalization** : 각 training batch를 미리 정해둔 평균, 분산값으로 정규화한다.  
  > **Weight Normalization** : 동일한 층에 위치한 뉴런들이 가진 weight를 Normalize한다.  

  </details>
- Local Minima와 Global Minima에 대해 설명해주세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > Global minima는 Gradient Descent 방법으로 학습시에, 해당 도메인에 대해 가장 낮은 cost를 가질 수 있는 weight가 위치한 지점이다.  
  > Local Minima는 Gradient Descent로 Global Minima를 찾아가는 과정에서 주변에 지역적으로 Gradient 하강, 상승 구간이 존재하여 빠질 수 있는 가짜 Minima이다.

  </details>
- 차원의 저주에 대해 설명해주세요
  <details markdown="1">
  <summary>[답안]</summary>

  > 차원의 저주는 한 샘플을 특정짓기 위해 많은 정보(다양한 차원의)를 수집할수록 오히려 학습이 어려워짐을 말한다.

  </details>
- Dimension reduction기법으로 보통 어떤 것들이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > ### 기본 방법 : Projection, Manifold 
  > - Projection  
  >   일반적으로, 고차원 데이터의 어떤 특성은 큰 변화가 없고, 어떤 특성은 다른 특성과 연관하여 크게 변하곤 한다. 이를 '데이터가 고차원 공간에서 저차원 subspace(부분 공간)에 위치한다' 라고 하는데, 이것은 고차원 데이터의 특성 중, 일부로 해당 데이터를 표현할 수 있음을 의미한다. 이때, 고차원 데이터를 저차원 subspace로 투영하여 차원을 줄인다.
  > - Manifold  
  >   데이터가 국소적으로는 유클리드 공간에 있으나, 대역적으로는 다른 위상을 가지는 경우 manifold 공간에 있다고 할 수 있다. 고차원 데이터가 실제로는 저차원 manifold에 있다고 가정하고 학습을 진행하여 문제 난이도를 낮출 수 있으나 항상 성공적이지 않기에 가정 전에 데이터를 살펴보는 주의가 필요하다.
  > _[참조: https://excelsior-cjh.tistory.com/167](https://excelsior-cjh.tistory.com/167)_
  > ### 심화 방법: PCA, NMF, LDA  

  </details>
- PCA는 차원 축소 기법이면서, 데이터 압축 기법이기도 하고, 노이즈 제거기법이기도 합니다. 왜 그런지 설명해주실 수 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > PCA는 분산이 최대가 되도록 하는 축을 찾고, 그 축과 직교하면서 분산이 최대가 되도록 하는 축을 이어 찾아나가는 방식으로 데이터를 간단히 표현합니다. 이 과정에서 차원이 축소되며, 투영 변환을 반대로 수행하면 데이터의 복원이 가능하고, PCA로 찾은 축들 중, 분산이 적은 축들을 제거함으로서 노이즈를 줄일 수 있습니다.

  </details>
- LSA, LDA, SVD 등의 약자들이 어떤 뜻이고 서로 어떤 관계를 가지는지 설명할 수 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > **LSA(Latent Semantic Analysis, 잠재의미분석)** : SVD를 이용하여 차원을 축소  
  > **LDA(Latent Dirichlet Allocation, 잠재디리클레할당)** : 주제별 단어분포와 문서별 주제 분포를 학습하여 Topic Modeling 하는 방법  
  > **SVD(Singular Value Decomposition)** : 특이값 분해  
  >
  > LSA와 SVD를 이용하여 데이터를 분해하고, 함축된 의미(Latent Semantic)를 추출하여 이를 통해 데이터를 압축하거나 차원을 축소한다. 특징 출현 횟수를 사용하는 LSA의 구조에 확률 모델을 도입하여 특징 출현 확률 기반으로 설계된 pLSA(Probabilistic Latent Semantic Analysis)는 SVD대신 NMF(Non-negative Matrix Factorization)를 이용하였다. 이후, LDA는 주제별 단어분포와 문서별 주제분포 모두 고려하되, 디리클레 분포를 따르도록 설계하여 주제를 뽑아낸다.  
  >
  > _[참조: https://bab2min.tistory.com/585](https://bab2min.tistory.com/585)_

  </details>
- Markov Chain을 고등학생에게 설명하려면 어떤 방식이 제일 좋을까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Markov Chain에서 가장 중요한 개념은 n회의 상태가 n-1회의 상태에만 영향을 받는다는 가정이다.  
  > 따라서 독립 시행으로 일어나는 정사면체 주사위, 정육면체 주사위, 동전 던지기 게임을 상태(state)로, 해당 게임의 결과에 따라 다른 게임으로 종목을 바꾸는 것을 전이(transition) 생각할 수 있다. 전이 조건을 정하고, 특정한 순서대로 게임을 하게 될 확률을 계산해보는 것으로 설명할 수 있을 것이다.

  </details>
- 텍스트 더미에서 주제를 추출해야 합니다. 어떤 방식으로 접근해 나가시겠나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 간단히 unigram, bigram, TF-IDF로 키워드를 추출하거나 LDA를 이용한다.

  </details>
- SVM은 왜 반대로 차원을 확장시키는 방식으로 동작할까요? 거기서 어떤 장점이 발생했나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > SVM을 비선형 분류 모델로 사용하기 위해 저차원 공간의 데이터를 고차원 공간으로 매핑하여 선형 분리가 가능한 데이터로 변환하여 처리한다.  
  >
  > _[참조: https://ratsgo.github.io/machine%20learning/2017/05/30/SVM3/](https://ratsgo.github.io/machine%20learning/2017/05/30/SVM3/)_

  </details>
- 다른 좋은 머신 러닝 대비, 오래된 기법인 나이브 베이즈(naive bayes)의 장점을 옹호해보세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > 1. 결과 도출을 위해 조건부 확률만 계산하면 되므로 매우 빠르고, 메모리를 많이 차지하지 않는다.  
  > 2. 데이터가의 특징들이 서로 독립되어 있을 때 좋은 결과를 얻을 수 있다.
  > 3. 데이터의 양이 적더라도 학습이 용이하다.  
  
  </details>
- Association Rule의 Support, Confidence, Lift에 대해 설명해주세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > ![](https://www.dropbox.com/s/utsfodbxj9uihv5/%ED%8C%8C%EC%9D%BC%202019.%2010.%2021.%20%EC%98%A4%EC%A0%84%2010%2054%2032.jpeg?raw=1)
  >  
  > * **Support** : X와 Y 두 item이 얼마나 자주 발생하는지를 의미  
  > * **Confidence** : X가 발생했을 떄, Y도 포함되어 있는 비율  
  > * **Lift** : X가 발생하지 않았을 때의 Y 발생 비율과 X가 발생했을 때 Y 발생 비율의 대비. 숫자가 1보다 크거나 작은 정도에 따라 연관성을 파악할 수 있다.  
  > 
  > _[참조 : https://rfriend.tistory.com/191](https://rfriend.tistory.com/191)_

  </details>
- 최적화 기법중 Newton’s Method와 Gradient Descent 방법에 대해 알고 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Newton's Method는 연속적이고 미분 가능한 함수에 대해 무작위 값을 대입해서 결과값의 변화 추이를 통해 원하는 해를 근사하는 방법이다. Gradient Descent 방법은 함수의 극대, 극소를 찾는 방법이며, 마찬가지로 지점에서의 미분값에 따라 대입할 값을 갱신하므로 그 근본적인 방법은 동일하다.

  </details>
- 머신러닝(machine)적 접근방법과 통계(statistics)적 접근방법의 둘간에 차이에 대한 견해가 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > [주관적 견해] 머신러닝과 통계적 접근방법은 서로 교집합이 큰 종목이다. 이 두 방법의 가장 큰 차이점은 문제 해결 방법의 수립 과정이라고 생각한다. 전자의 경우 머신이 스스로 학습하여 결정하며, 후자의 경우엔 사람이 직접 해결 방법을 설정한다. 통계적으로 사람이 직접 해결 방법을 명확히하기 어려운 문제에 대해 머신러닝 모델을 활용할 수 있다. 하지만 모델 설계 이외에는 제한적으로 사람이 개입되는 머신러닝의 특성상, 기본적으로 오류 가능성을 내포하고 있으며 복잡한 모델일수록 오류가 발생할 수 있는 지점에 대한 예측이 어렵다. 오류 상황에 대한 대응이 필수적인 문제의 경우에는 통계적 접근 방법을 이용하는 것이 더 유리할 수 있다.

  </details>
- 인공신경망(deep learning이전의 전통적인)이 가지는 일반적인 문제점은 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Parameter updating 전략과 각종 Normalization의 미비로 인해 step에 따른 Gradient의 움직임을 효과적으로 제어하기 힘들었기 때문에 모델이 깊게 설계할 수 없었다. 따라서 머신러닝을 사용해야하는 문제는 주로 비선형성이 큰 경우가 많았기 때문에, 얕은 구조의 인공신경망 모델로는 성공적인 대응을 할 수 없는 문제가 있었다.  

  </details>
- 지금 나오고 있는 deep learning 계열의 혁신의 근간은 무엇이라고 생각하시나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > [주관적 견해] 여러 Gradient Descent Optimizer들과 Normalization 기법들이 등장하고, GPU를 통한 부동소수점 연산의 효율화로 인해 Matrix 연산이 빨라지면서 사실상 중단되었던 인공신경망 분야가 한걸음 더 전진한 것이라고 생각한다.  

  </details>
- ROC 커브에 대해 설명해주실 수 있으신가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > ROC 커브는 FPR(False Positive Rate)와 TPR(True Positive rate)을 각각 x축과 y축으로 놓은 그래프이다. FPR은 GT(Grount Truth)가 거짓인 케이스에 대해 참이라고 잘못 예측한 비율이며, TPR은 GT가 참인 케이스에 대해 참이라고 잘 예측한 비율이다. 따라서 전자는 작을수록 성능이 좋고, 후자는 클수록 성능이 좋다.  
  > 두 Parameter에 대해 성능이 좋을수록 그래프 좌상단의 꼭짓점으로 다가가게 되며, 그래프 하단의 면적이 넓어지는 형태를 가지고 있다. 
  >  
  > ![](https://www.dropbox.com/s/9izjybk7aawtof0/ROC%20curve.png?raw=1)

  </details>
- 여러분이 서버를 100대 가지고 있습니다. 이때 인공신경망보다 Random Forest를 써야하는 이유는 뭘까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 일반적으로 각 단계별로 의존적인 end-to-end 인공신경망과 달리 Random Forest는 여러개의 독립적인 decision tree를 이용하여 결과를 투표하는 형식이므로 다수의 서버에서 병렬적인 처리가 용이하다.

  </details>
- K-means의 대표적 의미론적 단점은 무엇인가요? (계산량 많다는것 말고)
  <details markdown="1">
  <summary>[답안]</summary>

  > K-means 알고리즘은 K개의 Cluster를 미리 입력받아 Expectation과 Maximization 단계를 거듭하여 군집화를 수행한다. 단점들은 다음과 같다.  
  > 1. 초기에 사용자가 해당 데이터에 몇개의 Cluster가 있는지 알고 있어야 한다.  
  > 2. K개의 Cluster를 초기화하는 방법에 따라 결과에 큰 차이가 있다.  
  > 3. 클러스터의 모양이 구형이 아닌 경우, 적용이 힘들다.
  > 4. Maximization단계에서 각 데이터들과의 거리를 기준으로 Cluster 중심점의 위치를 업데이트하므로 데이터의 노이즈에 민감하게 반응한다.  

  </details>
- L1, L2 정규화에 대해 설명해주세요
  <details markdown="1">
  <summary>[답안]</summary>

  > L1, L2 정규화는 모델 학습시에 값이 너무 큰 파라메터의 영향력을 줄이는 전략이다. L1 정규화는 Cost Function에 가중치의 크기(절대값)를 더해주고, L2 정규화는 가중치 크기의 제곱을 더해줌으로써 가중치가 너무 크지 않은 방향으로 학습을 유도한다. 이것은 학습된 모델의 범용성을 높이는 효과를 준다.
  >  
  > _[참조 : https://light-tree.tistory.com/125](https://light-tree.tistory.com/125)_  

  </details>
  
- XGBoost을 아시나요? 왜 이 모델이 캐글에서 유명할까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > XGBoost는 머신러닝에서 각 Feature의 중요도를 계산할 수 있는 툴이다. 문제를 세부 단위로 쪼개어 정확도를 예측하는 부스팅 기법과 모든 리프들이 최종 결과 스코어를 내는 CART(Classification And Regression Tree) 기법을 앙상블하여 사용한다. 학습과 분류가 빠르고 캐글의 머신러닝 경연 우승자 중, 다수가 이 툴을 사용하여 주목받았다. 
  >  
  > _[참조 : https://brunch.co.kr/@snobberys/137](https://brunch.co.kr/@snobberys/137)_  

  </details>

- 앙상블 방법엔 어떤 것들이 있나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 앙상블은 머신러닝에서 여러 모델들을 학습시켜 각 모델의 결과를 합쳐서 더 나은 결과를 내는 방법을 말한다. 
  > 앙상블 방법에는 서로 다른 모델들을 학습시켜 결과를 투표하는 Voting, 같은 모델을 다른 Train data로 학습시키는 Bagging(**B**ootstrap **agg**regat**ing**)과 Pasting, 여러개의 약한 분류기를 결합하여 높은 성능의 모델을 도출하는 Boosting 등이 있다.
  >  
  > _[참조 : https://excelsior-cjh.tistory.com/166](https://excelsior-cjh.tistory.com/166)_

  </details>

- SVM은 왜 좋을까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > SVM은 데이터들을 선형 분리하며 최대 마진의 초평면을 찾는 크게 복잡하지 않은 구조이며, 커널 트릭을 이용해 차원을 늘리면서 비선형 데이터들에도 좋은 결과를 얻을 수 있다. 또한 이진 분류 뿐만 아니라 수치 예측에도 사용될 수 있다. Overfitting 경향이 낮으며 노이즈 데이터에도 크게 영향을 받지 않는다.
  >  
  > _[참조 : https://excelsior-cjh.tistory.com/166](https://excelsior-cjh.tistory.com/166)_

  </details>
- feature vector란 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > feature는 data를 구성하는 개별 example를 나타내는 '측정가능한 요소'를 나타낸다. feature vector는 한 example이 가지고 있는 feature들로 만든 vector를 의미한다.

  </details>
- 좋은 모델의 정의는 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 좋은 모델은 여러가지 의미가 있을 수 있다고 생각한다. 상황에 따라 최고의 성능을 내는 모델이 좋은 모델일 수도 있고, 가장 적은 리소스로 적절한 성능을 내는 모델이 좋은 모델일 수도 있다. 일반적으로는 필요한 리소스만으로 좋은 성능을 내는, 최적화가 잘된 모델이 좋은 모델이라 볼 수 있을 것 같다.

  </details>
- 50개의 작은 의사결정 나무는 큰 의사결정 나무보다 괜찮을까요? 왜 그렇게 생각하나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 큰 의사결정나무 하나를 쓰는 것보다 50개의 작은 의사결정나무를 사용하여, 그들의 결과를 취합하는 것이 경우에 따라 더 좋은 성능을 발휘할 수 있을 것이다.  
  > 비선형성이 강한 문제를 해결하는데 있어, 모든 feature를 사용하여 학습한 하나의 큰 의사결정나무보다 feature 일부를 이용하여 학습된 작은 의사결정나무들이 과적합의 정도가 덜할 것이며, 데이터 노이즈에도 강할 것이기 때문이다.  

  </details>
- 스팸 필터에 로지스틱 리그레션을 많이 사용하는 이유는 무엇일까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 스팸 필터링 분야에서 좋은 성능을 보이는데다가 해석이 용이하며, 계산 비용도 매우 저렴하기 때문이다.

  </details>
- OLS(ordinary least squre) regression의 공식은 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > ![](https://www.dropbox.com/s/t906c7zv8asj406/OLS-regression.jpeg?raw=1)

  </details>


##### [목차로 이동](#contents)

## 딥러닝
## 딥러닝 일반
- 딥러닝은 무엇인가요? 딥러닝과 머신러닝의 차이는?
  <details markdown="1">
  <summary>[답안]</summary>

  > 딥러닝은 인공신경망을 깊은 구조로 설계하여 비선형성이 높은 문제들을 해결하는 방법을 통칭한다.  
  > 딥러닝은 머신러닝의 일종이며 모델 내부의 결정 과정을 해석하기 불가능한 '블랙 박스'구조로 되어있다.
  > 보통, 딥러닝이 일반적인 머신러닝 기법보다 더 많은 데이터를 필요로 하며, 계산량이 압도적으로 많다.

  </details>
- 왜 갑자기 딥러닝이 부흥했을까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > [주관적 견해] 딥러닝의 기반 구조인 인공신경망은 1950년대부터 이미 있었던 개념이다.  
  > 그 당시에도 문제의 비선형도가 높아지면 인공신경망의 층을 충분히 깊게 설계해야하고, 모델 학습을 안정적으로 수행해야함을 알고 있었다. 하지만 과거에는 이를 실험적으로 뒷받침할 수 있는 하드웨어적 성능이 충분치 못했고 이에 따라 학습 전략 수립에도 차질이 있었다.  
  > 이후, GPU를 통한 부동소수점 연산의 효율화와 함께 하드웨어 문제가 완화되어, 깊은 층을 효과적으로 학습할 수 있는 방안들(Batch normalization, Optimizing Strategy 등)에 대한 연구가 활발히 이뤄질 수 있었다. 따라서 기술적 완성도가 높아졌고, 시기적으로도 Big Data 시대를 맞이하며 학습에 필요한 데이터 공급이 이전보다 수월해진 것도 결정적인 영향을 끼쳤다. 하드웨어 성능, 소프트웨어적 전략, 데이터 공급의 3가지 문제가 절묘하게 해결된 것이 딥러닝 부흥의 이유라고 생각한다.

  </details>

- 마지막으로 읽은 논문은 무엇인가요? 설명해주세요.
  <details markdown="1">
  <summary>[답안]</summary>

  > (현재 기준으로는 `Stabilizing the Lottery Ticket Hypothesis`) - 답안 PASS

  </details>
- Cost Function과 Activation Function은 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Cost Function은 모델이 생산한 결과와 목표 결과를 어떻게 비교할 것인가를 의미한다. 두 결과의 차이를 의미하는 Cost는 Optimizer에 의해 Parameter가 갱신될 때, Step Size를 얼마나 크게 가져갈 것인가에 결정적 영향을 끼친다. 
  > Activation Function은 뉴런이 유입되는 신호로부터 도출하는 값을 정제하는 역할을 한다. `Sigmoid`, `Relu`, `Hyperbolic Tangent` 등의 Activation Function를 통해, 어떤 값을 버리고, 어떤 값을 내보낼지를 결정 할 수 있다.   

  </details>
- Tensorflow, Keras, PyTorch, Caffe, Mxnet 중 선호하는 프레임워크와 그 이유는 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > PyTorch. 상속을 기반으로 한 쉬운 모델 설계 방법과 직관적인 텐서 컨트롤이 마음에 들기 때문이다.

  </details>
- Data Normalization은 무엇이고 왜 필요한가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Data Normalization은 입력 데이터의 최소, 최대값을 일정 범위(0~1) 내로 조절하는 것이다. 이를 통해, 특정 입력 데이터가 결과에 대해 과도한 영향을 미칠 수 있는 지위를 획득하는 것을 방지할 수 있고, 모델의 학습을 원활하게 만든다.

  </details>
- 알고있는 Activation Function에 대해 알려주세요. (Sigmoid, ReLU, LeakyReLU, Tanh 등)
  <details markdown="1">
  <summary>[답안]</summary>

  > ![](https://www.dropbox.com/s/mkh6czvzd0vhtxt/Activation-functions.jpeg?raw=1)

  </details>
- 오버피팅일 경우 어떻게 대처해야 할까요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 1. Training Data를 더 마련한다.  
  > 2. k-fold cross validation을 사용한다.   
  > 3. Regularization을 사용한다.  
  > 4. Dropout 을 사용한다.  
  > 5. Parameter 수를 줄인다.  

  </details>

- 하이퍼 파라미터는 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 하이퍼 파라미터는 모델의 학습에 필요한 수동 설정값이다. 

  </details>
- Weight Initialization 방법에 대해 말해주세요. 그리고 무엇을 많이 사용하나요?
  <details markdown="1">
  <summary>[답안]</summary>

  > Sigmoid와 같은 S자 형태의 Activation Function을 사용할 때는, 정규 분포 모양의 Weight Initialization이 효과적이지만 일반적인 상황에서는 Xiavier, ReLu에는 He를 사용한다.  
  >  ```python
  > # Xiavier - Normal Distribution
  > w = np.random.randn(n_inp, n_out) / sqrt(n_inp)
  > # He - Normal Distribution
  > w = np.rnadom.randn(n_inp, n_out) / sqrt(n_inp / 2)
  >  ```
  > _[참조: https://gomguard.tistory.com/184](https://gomguard.tistory.com/184)_

  </details>
- 볼츠만 머신은 무엇인가요?
  <details markdown="1">
  <summary>[답안]</summary>

  > 볼츠만 머신은 가시층(Visible Layer)와 은닉층(Hidden Layer), 총 두 개의 층으로 신경망을 구성하는 방법이다. 볼츠만 머신은 모든 뉴런이 연결되어 있는 완전 그래프 형태이며, _제한된_ 볼츠만 머신(RBM)에서는 같은 층의 뉴런들은 연결되어 있지 않은 모양이다. 기본적으로 단층구조이며, 확률 모델이다. 분류나 선형 회귀 분석 등에 사용될 수 있다.
  > 특히 DBN(Deep Belief Network)에서는 RBM들을 쌓아올려, 각 볼츠만 머신을 순차적으로 학습시킨다.

  </details>
- 요즘 Sigmoid 보다 ReLU를 많이 쓰는데 그 이유는?
  <details markdown="1">
  <summary>[답안]</summary>

  > Sigmoid는 항상 1보다 작은 수를 리턴한다. 이에 의해 Gradient Vanishing 현상이 일어나, 층을 깊게 설계할 수 없기 때문이다. 또한 계산 비용 측면에서도 ReLU가 Sigmoid에 비해 훨씬 경제적이다.

  </details>
	- Non-Linearity라는 말의 의미와 그 필요성은?
    <details markdown="1">
    <summary>[답안]</summary>

    > 비선형성. 선형 결합으로 해 집합을 표현할 수 없는 성질을 말한다. 다시 말하면, 결과값이 입력값들의 사칙연산 조합으로 표현되지 않는 것이다.   
    > 우리가 해결하고 싶은 대부분의 실제 문제들은 비선형성을 띄고 있다. 하지만 만약 모델이 선형적인 방법론으로 학습한다면 비선형 문제를 풀 수 없을 것이다. 따라서 학습시에 비선형성을 가진 방법론을 추가함으로써 여기에 대응해야 할 필요가 있다.

    </details>
	- ReLU로 어떻게 곡선 함수를 근사하나?
    <details markdown="1">
    <summary>[답안]</summary>

    > ReLU는 선형 부분과 비선형 부분이 결합된 모양이다. ReLU가 반복 적용되면서 선형 부분의 결합이 이루어지고, 곡선 함수를 근사할 수 있게 된다.

    </details>
	- ReLU의 문제점은?
    <details markdown="1">
    <summary>[답안]</summary>

    > ReLU는 0이하의 입력을 모두 0으로 내보낸다. 이 과정에서 정보의 손실이 불가피하다. 이런 단점을 보완하기 위해 Leaky ReLU와 같이 0이하 값도 출력에 반영하는 활성화 함수들이 등장했다.

    </details>
	- Bias는 왜 있는걸까?
    <details markdown="1">
    <summary>[답안]</summary>

    > Bias는 함수의 모양을 변경하지 않고 Shift해주어 정답에 근사할 수 있도록 도와준다.

    </details>
- Gradient Descent에 대해서 쉽게 설명한다면?
  <details markdown="1">
  <summary>[답안]</summary>

  > Gradient Descent는 파라미터에 대해 오차값을 미분하여 그 기울기값(Gradient)를 구하고, 경사가 하강하는 방향으로 파라미터를 업데이트 하는 방법이다.

  </details>
	- 왜 꼭 Gradient를 써야 할까? 그 그래프에서 가로축과 세로축 각각은 무엇인가? 실제 상황에서는 그 그래프가 어떻게 그려질까?
    <details markdown="1">
    <summary>[답안]</summary>

    > ![](https://www.dropbox.com/s/b3z3d6f9ue2vel8/Cost-Gradient.jpeg?raw=1)  
    > Gradient가 양수이면 올라가는 방향이며 음수이면 내려가는 방향이다. 실제 상황에서는 Gradient 그래프가 0을 중심으로 진동하는 모양이 될 것이다.  

    </details>
	- GD 중에 때때로 Loss가 증가하는 이유는?
    <details markdown="1">
    <summary>[답안]</summary>

    > ![](https://www.dropbox.com/s/vbwby7kugy9flei/Cost-Gradient2.jpeg?raw=1)  
    > 실제로 사용되는 GD에서는 Local Optima를 피하기 위해 Momentum 등의 개념을 도입한 RMSprop, Adam 등의 Optimization 전략을 사용한다.    
    > 각 Optimization 전략에 따라 Gradient가 양수인 방향으로도 parameter update step을 가져가는 경우가 생길 수 있으며, 이 경우에는 Loss가 일시적으로 증가할 수 있다.  

    </details>
	- 중학생이 이해할 수 있게 더 쉽게 설명 한다면?
    <details markdown="1">
    <summary>[답안]</summary>

    > Gradient Descent는 위 아래로 구불구불한 산길을 눈을 감고 지나면서 지금 밟고 있는 땅의 경사 방향을 토대로 가장 고도가 낮은 지점을 찾아나가는 것이다.  

    </details>
	- Back Propagation에 대해서 쉽게 설명 한다면?
    <details markdown="1">
    <summary>[답안]</summary>

    > 신경망의 최종 단계에서 계산된 오차를 변화량을 바탕으로 이전 단계의 파라미터를 업데이트 하는 방향을 설정하는 방법이다.

    </details>
- Local Minima 문제에도 불구하고 딥러닝이 잘 되는 이유는?
  <details markdown="1">
  <summary>[답안]</summary>

  > Optimization 전략을 통해 local Minima 문제를 어느정도 피할 수 있기 때문이다.

  </details>
	- GD가 Local Minima 문제를 피하는 방법은?
    <details markdown="1">
    <summary>[답안]</summary>

    > Momentum 등의 개념을 도입한 RMSprop, Adam 등의 Optimization 전략을 사용한다.

    </details>
	- 찾은 해가 Global Minimum인지 아닌지 알 수 있는 방법은?
    <details markdown="1">
    <summary>[답안]</summary>

    > Global Minima가 정확히 어디에 존재하는지는 알 수 없다. 다만, 학습에 사용하지 않은 Test Dataset에 대한 성능을 평가하는 것으로 모델이 Global Minima에 가까운지 유추할 수 있다.

    </details>
- Training 세트와 Test 세트를 분리하는 이유는?
    <details markdown="1">
    <summary>[답안]</summary>

    > 실제 데이터에 대한 모델의 성능을 평가하기 위해, Training data로 사용했던 데이터를 모델의 평가에 활용하지 않는다.

    </details>
	- Validation 세트가 따로 있는 이유는?
    <details markdown="1">
    <summary>[답안]</summary>

    > 모델 학습 과정 중, Training data와 분리된 Validation data로 모델을 평가하여 그 결과를 학습에 반영하므로써 Training Data에 대한 Overfitting을 방지하는 효과가 있다.

    </details>
	- Test 세트가 오염되었다는 말의 뜻은?
    <details markdown="1">
    <summary>[답안]</summary>

    > Test data에 Training data와 일치하거나, 매우 유사한 데이터들이 포함되어 Test data가 General한 상황에서의 성능 평가를 수행하지 못함을 말한다.

    </details>
	- Regularization이란 무엇인가?
    <details markdown="1">
    <summary>[답안]</summary>

    > 단순히 Cost function의 값이 작아지는 방향으로 모델을 학습하면 특정 가중치가 과도하게 커지는 현상이 나타날 수 있다. Regularization의 개념은, Cost Function을 계산할 때 가중치의 절대값 만큼을 더해주는 방법으로 특정 가중치의 값이 너무 커지는 현상을 억제하는 것이다.

    </details>
- Batch Normalization의 효과는?
  <details markdown="1">
  <summary>[답안]</summary>

  > 은닉층에서의 입력값을 Normalize함으로써 입력 분포를 조정 할 수 있다. 은닉층의 입력 분포가 조정되면 학습 편의성이 개선되어 수렴 속도가 빨라지고, Local Optima를 피할 가능성이 높아진다.  
  >  
  >  참조 : [https://light-tree.tistory.com/132](https://light-tree.tistory.com/132) / [https://light-tree.tistory.com/139](https://light-tree.tistory.com/139)

  </details>
	- Dropout의 효과는?
    <details markdown="1">
    <summary>[답안]</summary>

    > Dropout은 노드들의 연결을 무작위로 끊는 방식으로, 하나의 노드가 너무 큰 가중치를 가져 다른 노드들의 학습을 방해하는 현상을 억제한다. 이를 통해 모델의 일반화 성능을 높이고, Overfitting을 방지한다.  

    </details>
	- BN 적용해서 학습 이후 실제 사용시에 주의할 점은? 코드로는?
    <details markdown="1">
    <summary>[답안]</summary>

    > 학습시에는 각 mini-Batch 단위의 평균과 분산을 이용해 Normalize하지만 실제 사용시에는 네트워크에 입력되는 Batch의 단위가 더 적을 수 있기 때문에 미리 학습 데이터에서 뽑아낸 평균, 분산을 이용해야한다.  
    > code : [https://pytorch.org/docs/stable/nn.html#batchnorm1d](https://pytorch.org/docs/stable/nn.html#batchnorm1d)
    </details>

	- GAN에서 Generator 쪽에도 BN을 적용해도 될까?
    <details markdown="1">
    <summary>[답안]</summary>

    > 일반적인 GAN에서 Generator의 Output Layer와 Discriminator의 Input Layer에는 BN을 적용하지 않는다. 그 이유는 Discriminator가 조작되지 않은 Generator의 결과물의 정확한 값으로 mean, scale을 학습하기 위함이다.
    
    </details>
- SGD, RMSprop, Adam에 대해서 아는대로 설명한다면?  
  <details markdown="1">
    <summary>[참조]</summary>  

    > ![optimizers](https://www.dropbox.com/s/k03ffo5rjlwc03z/optimizers.png?raw=1)  
    
  </details>  

	- SGD에서 Stochastic의 의미는?
  <details markdown="1">
    <summary>[답안]</summary>

    > Stochastic Gradient Descent는 Train Dataset을 전부 이용하는 Batch Gradient Descent와 달리, mini-batch를 이용하여 loss function을 이용한다. 'Stochastic'은 mini-batch가 전체 train dataset에서 무작위로 선택된다는 것을 의미한다.
    
  </details>
	- 미니배치를 작게 할때의 장단점은?
  <details markdown="1">
    <summary>[답안]</summary>

    > Mini-batch의 사이즈가 작으면 한 iteration의 계산량이 적어지기 때문에 step당 속도가 빨라지며 적은 Graphic Ram으로도 학습이 가능하는 장점이 있다. 하지만 Target의 Variance가 큰 경우, mini-batch-size가 너무 작을 때는 학습데이터에 대한 대표성이 떨어져 학습의 일반성(Generalization)이 불리할 수 있다.
    
  </details>
	- 모멘텀의 수식을 적어 본다면?
  <details markdown="1">
    <summary>[답안]</summary>

    > $$w(t) = m \cdot w(t-1) - \alpha \frac{\partial}{\partial w} \operatorname {Cost}(w)$$  
    > $$m$$은 모멘텀 상수 (default=0.9)
    
  </details>
- 딥러닝할 때 GPU를 쓰면 좋은 이유는?  
  <details markdown="1">
    <summary>[답안]</summary>

    > GPU에는 부동소수점 계산에 특화된 수많은 코어가 있어, Matrix Multiplication이 핵심인 Deep Learning에서 병렬 처리를 수행 하기에 유리하다.
    
  </details>
	- 학습 중인데 GPU를 100% 사용하지 않고 있다. 이유는?
  <details markdown="1">
    <summary>[답안]</summary>

    > GPU를 100% 활용하지 못하는 경우는 두가지로 나뉜다. 첫번째는 GPU의 그래픽 메모리를 충분히 활용하지 못하는 경우, 두번째는 GPU의 연산 능력을 충분히 활용하지 못하는 경우이다.
    > 그래픽 메모리가 100%가 아닌 경우, 작은 Batch Size가 문제일 수 있다. 또는 Multi-GPU 환경에서 Data Parallel 방법으로 GPU 로드를 분산할 시, 각 GPU의 output을 한 GPU로 모아서 loss 계산을 하여 GPU 메모리 사용량의 불균형이 발생할 수 있다.
    > 연산 능력이 100%가 아닌 경우에는 모델의 계산 과정에서 CPU 병목이 원인일 수 있다.
    
  </details>

	- GPU를 두개 다 쓰고 싶다. 방법은?
  <details markdown="1">
    <summary>[답안]</summary>

    > Pytorch에서는 `DataParallel` 또는 `DistributedDataParallel`을 이용해서 모델과 데이터를 multi-gpu 또는 multi-device에 배분할 수 있다.
    
  </details>
	- 학습시 필요한 GPU 메모리는 어떻게 계산하는가?
  <details markdown="1">
    <summary>[답안]</summary>

    > GPU 메모리의 사용량에 주로 영향을 미치는 요소는 모델의 weights(+biases)와 data tensor이다.
    > Trainable Parameter의 개수에 4byte(FP16) 또는 8byte(FP32)를 곱하면 DL 모델의 GPU 메모리 사용량을 대략적으로 알 수 있다. 그리고 여기에 input data의 tensor size를 더하면 된다.
    >
    > $$Total\_GRAM = Model\_Size + Batch\_Size \times Tensor\_Size + \alpha$$  
    > _$$\alpha$$에는 Momentum 등을 이용하는 Optimizer의 variable, model forward 과정에서 쓰이는 temporary variable 등이 있다.$$_
    
  </details>
- TF, Keras, PyTorch 등을 사용할 때 디버깅 노하우는?
- 뉴럴넷의 가장 큰 단점은 무엇인가? 이를 위해 나온 One-Shot Learning은 무엇인가? 

##### [목차로 이동](#contents)

## 컴퓨터 비전
- OpenCV 라이브러리만을 사용해서 이미지 뷰어(Crop, 흑백화, Zoom 등의 기능 포함)를 만들어주세요
  <details markdown="1">
    <summary>[답안]</summary>

    ```
    img = cv2.imread('img.png', cv2.IMREAD_COLOR)
    x1, y1, x2, y2 = 100, 100, 200, 200
    cropped = img[y1:y2, x1:x2, :]

    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    zoom = cv2.resize(cropped, dsize=(0, 0), fx=2.0, fy=2.0, interpolation=cv2.INTER_LINEAR)
    ```
    
  </details>
- 딥러닝 발달 이전에 사물을 Detect할 때 자주 사용하던 방법은 무엇인가요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 간단하게는 grayscale에서 adaptive threshold를 이용한 segmentation을 이용했고, 특징 검출을 이용한 object detection도 자주 사용되었다. 대표적으로는 orb feature extraction 등이 있다.
    
  </details>
- Fatser R-CNN의 장점과 단점은 무엇인가요?
  <details markdown="1">
    <summary>[답안]</summary>  

    > ![](https://www.dropbox.com/s/ct1koxpgmrawo9p/RCNN-series.jpg?raw=1)  
    > 
    > Fast RCNN에서는 전체 이미지에 대한 CNN Feature Extract 결과를 RoI Polling한 후, Selective Search를 통해 Region Proposal을 수행한다. Faster RCNN에서는 Extracted Feature에 Region Proposal Network라고 하는 일종의 CNN을 바로 적용하여 Selective Search에서 발생하는 병목을 줄였습니다. 하지만 여전히 마지막 단계에서 NMS(Non-Maximum-Suppression)를 이용하기 때문에 병목은 존재합니다.
    > 
    > **참조**
    > - RCNN Series : [https://seongkyun.github.io/papers/2019/01/06/Object_detection/](https://seongkyun.github.io/papers/2019/01/06/Object_detection/)
    > - Selective Search : [https://m.blog.naver.com/laonple/220918802749](https://m.blog.naver.com/laonple/220918802749)
    
  </details>
- dlib은 무엇인가요?
  <details markdown="1">
    <summary>[답안]</summary>

    > Dlib은 C++ 으로 작성된 크로스 플랫폼 라이브러리이다. 주로 얼굴 검출에 사용된다. HOG(Histogram of Oriented Gradients) Feature를 이용한 방법 혹은 CNN을 통한 얼굴 검출 및 인식 전반을 손쉽게 이용할 수 있다.
    
  </details>
- YOLO의 장점과 단점은 무엇인가요?
  <details markdown="1">
    <summary>[답안]</summary>

    > YOLO는 CNN에 전체 이미지를 한번만 통과시키기 때문에 RCNN 계열의 방법보다 속도가 빠르다는 장점이 있지만, Faster-RCNN이나 Mask-RCNN보다 정확도는 조금 떨어진다는 단점이 있다. 다양한 기법을 조합했기 때문에 구현이 까다롭고 그리드 기반으로 Object Detection을 하기 때문에 한번에 최대로 검출 할 수 있는 물체의 수가 제한되는 점도 단점이다.
    
  </details>
- 제일 좋아하는 Object Detection 알고리즘에 대해 설명하고 그 알고리즘의 장단점에 대해 알려주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > YOLO-v5는 비교적 큰 커뮤니티 덕분에 편리하게 이용할 수 있으며, 특히 [https://github.com/ultralytics/yolov5](https://github.com/ultralytics/yolov5) 레포지토리에서는 손쉽게 옵션을 변경할 수 있고 가변적인 모델 사이즈 변경을 지원하여 Real time 속도의 모델부터 SOTA에 버금가는 정확도 중심의 큰 모델까지 편리하게 이용할 수 있어 가장 선호한다. 장단점은 위의 문항 답안과 같다.
    
  </details>
- 그 이후에 나온 더 좋은 알고리즘은 무엇인가요?  
  <details markdown="1">
    <summary>[답안]</summary>

    > Object detection을 적용한 target의 domain에 따라 "더 좋은" 알고리즘은 다르다고 볼 수 있다. 예를들어, text detection 분야에서는 grid 기반의 yolo보다 pixel 단위로 heatmap을 그리는 u-net 기반의 [CRAFT](https://github.com/clovaai/CRAFT-pytorch)의 성능이 더 좋다.  
    > 가장 보편적으로 이용하는 COCO dataset을 이용한 sota모델 중, 현재는 swin-transformer를 이용한 모델이 현재 최상위권에 있다. swin-transformer는 convolution 대신 이미지를 patch 단위로 자르고, cyclic-shift라는 방식으로 patch merging하여 feature extraction하는 방식을 이용한다.  
    > _참조: [https://paperswithcode.com/sota/object-detection-on-coco](https://paperswithcode.com/sota/object-detection-on-coco)_
    
  </details>
- Average Pooling과 Max Pooling의 차이점은?
  <details markdown="1">
    <summary>[답안]</summary>

    > Pooling은 Feature map에서 feature 수를 감소시키는 역할을 한다.  
    > Average Pooling은 kernel window에 해당하는 값들의 평균을 대표로, Max Pooling은 가장 큰 값을 대표로 feature 수를 감소시킨다.  
    > Max pooling은 kernel 영역 내에서 가장 두드러지는 값을 남기고, average는 영역 내의 모든 값을 고려하는 효과가 있다.    
    
  </details>
- Deep한 네트워크가 좋은 것일까요? 언제까지 좋을까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 일반적으로 Deep한 네트워크는 Capacity가 높아서 그만큼 Complex한 문제를 풀기에 적합하다. 하지만 target task에 비해 네트워크가 과도하게 Deep한 경우 overfitting이 일어날 수 있으며, 학습 난이도가 높아질 수 있다.
    
  </details>
- Residual Network는 왜 잘될까요? Ensemble과 관련되어 있을까요?
- CAM(Class Activation Map)은 무엇인가요?
- Localization은 무엇일까요?
- 자율주행 자동차의 원리는 무엇일까요?
- Semantic Segmentation은 무엇인가요?
- Visual Q&A는 무엇인가요?
- Image Captioning은 무엇인가요?
- Fully Connected Layer의 기능은 무엇인가요?
- Neural Style은 어떻게 진행될까요?
- CNN에 대해서 아는대로 얘기하라
	- CNN이 MLP보다 좋은 이유는?
	- 어떤 CNN의 파라미터 개수를 계산해 본다면?
	- 주어진 CNN과 똑같은 MLP를 만들 수 있나?
	- 풀링시에 만약 Max를 사용한다면 그 이유는?
	- 시퀀스 데이터에 CNN을 적용하는 것이 가능할까?

##### [목차로 이동](#contents)

## 자연어 처리
- One Hot 인코딩에 대해 설명해주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > ![](https://www.dropbox.com/s/4co60k7p7y4f8i4/one-hot-encoding.png?raw=1)
    > one-hot encoding은 토큰(token)을 신경망(Neural net)에 입력하기 위해 Embedding하는 방법 중 하나다. 특정 토큰을 matrix로 만들기 위해 vocab size N개의 차원을 가지는 벡터를 만들고, 특정 Index만 1값을 주고 나머지를 0으로 하는 방식으로 토큰을 벡터화한다. 
    
  </details>

- POS 태깅은 무엇인가요? 가장 간단하게 POS tagger를 만드는 방법은 무엇일까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > POS(Part-Of-Speech) 태깅은 문장 내의 단어들을 형태소 분석하여 태그 붙여주는 방법을 말한다. 형태소 분석을 위해 쓸 수 있는 여러 라이브러리가 있다. 그 중, Python 기준으로 가장 인기가 많은 POS Tagger는 NLTK이다. 아래와 같은 간단한 import 만으로 형태소 분석기를 즉시 이용 가능하다.
    > ```
    >>> import nltk
    >>>from nltk.tokenize import word_tokenize
    >>> text = word_tokenize("Hello welcome to the world of to learn Categorizing and POS Tagging with NLTK and Python")
    >>> nltk.pos_tag(text)  
    >  
    > [('Hello', 'NNP'), ('welcome', 'NN'), ('to', 'TO'), ('the', 'DT'), ('world', 'NN'), ('of', 'IN'), ('to', 'TO'), ('learn', 'VB'), ('Categorizing', 'NNP'), ('and', 'CC'), ('POS', 'NNP'), ('Tagging', 'NNP'), ('with', 'IN'), ('NLTK', 'NNP'), ('and', 'CC'), ('Python', 'NNP')]
    > ```
    
  </details>
- 문장에서 "Apple"이란 단어가 과일인지 회사인지 식별하는 모델을 어떻게 훈련시킬 수 있을까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > "Apple" 단어를 포함한 문장들과 과일, 회사 Label을 pair한 학습 데이터를 만든다. 단어를 판단할 때 문맥을 구성하는 다른 토큰들을 고려할 수 있는 모델을 선정한다. 예를 들어 n-gram으로 주변에 출현한 토큰을 저장하고, 과일일때와 회사일때의 주변 토큰을 통계적으로 방법을 이용할 수 있고, LSTM이나 Transformer, BERT와 같은 비교적 최신 모델을 이용하여 class가 2개인 텍스트 분류 모델을 훈련할 수 있다.
    
  </details>
- 음성 인식 시스템에서 생성된 텍스트를 자동으로 수정하는 시스템을 어떻게 구축할까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 두가지 방법이 있다.
    > 1. 텍스트에 대한 교정 데이터를 구축하여 번역 모델을 학습시키는 방법으로 텍스트 교정기를 학습한다. 이 경우, 입력은 음성 인식 시스템에서 생성된 텍스트가 되며, 결과는 교정된 텍스트가 된다.
    > 2. 마찬가지로 텍스트에 대한 교정 데이터를 구축한 뒤, Multi-modal 구조로 Sound data와 text를 입력으로 받아 교정 텍스트를 출력하는 모델을 구축한다.
    
  </details>
- 잠재론적, 의미론적 색인은 무엇이고 어떻게 적용할 수 있을까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 잠재 의미 색인(LSI, Latent Semantic Indexing) 혹은 잠재 의미 분석(LSA, Latent Semantic Analysis)은 BoW(Bag of Words)에 기반한 단어의 출현 빈도수를 이용한 수치화는 자연어에서 발생하는 동의어(Synonymy) 혹은 다의어(Polysemy)와 같은 문제를 다룰 수 없기에, 이를 보완하기 위해 만들어진 방법이다. 새로운 문서에 공존하는 단어들을 통계적으로 분석하여 단어의 의미를 문서단위로 다르게 예측한다.
    > 참조 : [https://sens.tistory.com/354](https://sens.tistory.com/354)
    
  </details>
- 영어 텍스트를 다른 언어로 번역할 시스템을 어떻게 구축해야 할까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 1. 영어 텍스트와 번역 목표 언어가 pair로 구성된 데이터를 구비한다.  
    > 2. 2021년 현재 비교적 높은 성능을 보이는 BERT, ELECTRA 등의 Transformer기반 모델의 pre-trained Language Model을 구비한다.  
    > 3. `1`에서 준비한 데이터 쌍을 이용하여 fine-tune 학습을 수행한다.
    > 4. REST API를 지원하는 web-framework를 이용하여 서비스를 구성한다.
    > 5. Docker를 이용하여 container 빌드하여 서비스한다.
    
  </details>
- 뉴스 기사를 주제별로 자동 분류하는 시스템을 어떻게 구축할까요?
- Stop Words는 무엇일까요? 이것을 왜 제거해야 하나요?
- 영화 리뷰가 긍정적인지 부정적인지 예측하기 위해 모델을 어떻게 설계하시겠나요?
- TF-IDF 점수는 무엇이며 어떤 경우 유용한가요?
- 한국어에서 많이 사용되는 사전은 무엇인가요?
- Regular grammar는 무엇인가요? regular expression과 무슨 차이가 있나요?
- RNN에 대해 설명해주세요
- LSTM은 왜 유용한가요?
- Translate 과정 Flow에 대해 설명해주세요
- n-gram은 무엇일까요?
- PageRank 알고리즘은 어떻게 작동하나요?
- depedency parsing란 무엇인가요?
- Word2Vec의 원리는?
	- 그 그림에서 왼쪽 파라메터들을 임베딩으로 쓰는 이유는?
	- 그 그림에서 오른쪽 파라메터들의 의미는 무엇일까?
	- 남자와 여자가 가까울까? 남자와 자동차가 가까울까?
	- 번역을 Unsupervised로 할 수 있을까?

##### [목차로 이동](#contents)

## 강화학습
- MDP는 무엇일까요?
  <details markdown="1">
    <summary>[답안]</summary>

    > **마르코프 결정 과정(MDP, Markov Decision Process)**  
    > 확률적 의사결정 정책 $$\pi$$와 상태 $$s$$ 를 정의하고, 의사결정자가 상태 $$s$$에서 취할 행동과 그 결과값을 $$\pi(s) = a$$ 로 구하여, 확률적으로 주어지는 보상의 누적합을 최대화시키는 정책 $$\pi$$를 구하는 것을 의미한다.
      
  </details>
- 가치함수는 무엇일까요? 수식으로도 표현해주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > 강화학습에는 아래의 두가지 가치함수가 있다.
    > 1. **상태가치함수**  
    >   의사결정자가 각 상태(state)에 대한 가치를 예상하는데 이용하는 함수이다. 현재 상태에서 다음 상태에 도달했을 때의 가치 기댓값을 계산한다. 여기서 "어떤 행동"을 하여 다음 상태에 도달할지에 대한 고려는 빠져있다.  
    > $$v$$는 상태가치함수, $$s$$는 상태, $$E$$는 기댓값(평균), $$t$$는 시점, $$G$$는 가치 또는 보상을 의미할 때,  
    > $$v(s) = E[G_t|S_t=s]$$ 와 같이 표현할 수 있다.
    > 2. **행동가치함수**  
    > 의사결정자가 특정 상태에서 특정 행동을 했을때의 가치 기댓값으로, "행동"에 대한 고려가 포함되어 있다.   
    > $$q$$는 행동가치함수, $$a$$는 특정 행동일 때,  
    > $$q_{\pi} (s,a) = E_{\pi}[G_t | S_t = s, A_t = a]$$ 와 같이 표현할 수 있다.
    > 각 가치함수로 계산된 기댓값과 시뮬레이터를 통해 얻은 실제값을 비교하여 가치 함수를 업데이트 하는 방식으로 모델 학습을 수행한다.
    
    
    > 참조 : [https://dana-study-log.tistory.com/entry/~~~](https://dana-study-log.tistory.com/entry/%EA%B0%95%ED%99%94%ED%95%99%EC%8A%B5-%EC%9D%B4%EB%A1%A0-%EA%B0%80%EC%B9%98%ED%95%A8%EC%88%98%EC%99%80-Q%ED%95%A8%EC%88%98%EC%83%81%ED%83%9C-%EA%B0%80%EC%B9%98%ED%95%A8%EC%88%98%EC%99%80-%ED%96%89%EB%8F%99-%EA%B0%80%EC%B9%98%ED%95%A8%EC%88%98)
  </details>
- 벨만 방정식은 무엇일까요? 수식으로도 표현해주세요
- 강화학습에서 다이나믹 프로그래밍은 어떤 의미를 가질까요? 한계점은 무엇이 있을까요?
- 몬테카를로 근사는 무엇일까요? 가치함수를 추정할 때 어떻게 사용할까요?
- Value-based Reinforcement Learning과 Policy based Reinforcement Learning는 무엇이고 어떤 관계를 가질까요?
- 강화학습이 어려운 이유는 무엇일까요? 그것을 어떤 방식으로 해결할 수 있을까요?
- 강화학습을 사용해 테트리스에서 고득점을 얻는 프로그램을 만드려고 합니다. 어떻게 만들어야 할까요?

##### [목차로 이동](#contents)

## GAN
- GAN에 대해 아는대로 설명해주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > **Generative Adverserial Network**  
    > Generator(G)와 Discriminator(D)로 역할이 나뉜 두 모델이 경쟁하며 학습하는 구조의 모델을 의미한다. D는 입력되는 데이터가 Real인지 Fake인지 구분하는 역할을 수행하며, G는 Real과 구분하기 어려운 Fake를 생성하여 D를 속인다. Loss의 구성이 흥미롭다. D는 Real 데이터와 Fake 데이터를 섞어, Real과 Fake의 구분 능력을 Loss Function으로 하여 학습하고, G는 D가 Fake를 Real로 인식하도록하는 방향으로 학습한다. 수식으로 설명하면 다음과 같다.
    >  $$min_{G}max_{D}Loss(D, G)=CrossEntropy(D(x))+CrossEntropy(D(G(z)))$$
    > $$z는 G가 입력으로 가정하는 데이터 분포, latent vector(잠재벡터)이다.$$
      
  </details>
- GAN의 단점은 무엇인가요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 1. Generator와 Discriminator 모델들의 밸런스를 잡기 어렵기 때문에 학습이 대체로 불안정하고 수렴이 어렵다.  
    > 2. 학습 완료된 Generator의 성능 평가를 사람이 직접 해야한다.
      
  </details>
- LSGAN에 대해 설명해주세요
  <details markdown="1">
    <summary>[답안]</summary>

    > Discriminator가 Real과 Fake를 구분할 때 CrossEntorpy 대신 Least square error를 이용한다. 학습 과정에서 최소화해야하는 Entropy는 모델이 "긴가민가"할 때 높은 값으로 나온다. 이는 역설적으로 모델이 높은 확신도로 맞춘 데이터에 대해서는 거의 학습하지 않는다는 것을 의미한다. 일반적인 Classfication 모델에서는 문제가 안되지만, GAN에서는 이 점이 Generator의 학습을 저해하는 요소이다. 따라서 최대한 "모든" 데이터에 대해 학습할 수 있도록 Squared error를 최소화하는 Loss을 채택한 것이 LSGAN이다.
      
  </details>
- GAN이 왜 뜨고 있나요?
  <details markdown="1">
    <summary>[답안]</summary>

    > 먼저, Generator와 Discriminator를 경쟁시키면서 학습한다는 발상이 흥미롭다. 학습 난이도가 높기 때문에 새로운 학습 전략이 필요했는데, KL-divergence 이후에 JS-divergence, Wasserstein distance 등이 대두되며 기존에 생성 모델으로 주로 이용하던 VAE에 비해 개성 있고 형태적으로 더 다양한 결과를 얻을 수 있게 되었다. 이를 통해 GAN이 생성 모델의 새로운 패러다임으로 떠오르고 있다.
      
  </details>
- Auto Encoder에 대해서 아는대로 얘기하라
	- MNIST AE를 TF나 Keras등으로 만든다면 몇줄일까?
	- MNIST에 대해서 임베딩 차원을 1로 해도 학습이 될까?
	- 임베딩 차원을 늘렸을 때의 장단점은?
	- AE 학습시 항상 Loss를 0으로 만들수 있을까?
	- VAE는 무엇인가?
- 간단한 MNIST DCGAN을 작성한다면 TF 등으로 몇줄 정도 될까? 
	- GAN의 Loss를 적어보면?
	- D를 학습할때 G의 Weight을 고정해야 한다. 방법은?
	- 학습이 잘 안될때 시도해 볼 수 있는 방법들은?

##### [목차로 이동](#contents)

## 추천 시스템
- 추천 시스템에서 사용할 수 있는 거리는 무엇이 있을까요?
<details markdown="1">
    <summary>[답안]</summary>

    > Recommendation은 주로 top k개의 결과에 대해 평가하는 경우가 많다. Mean Average Precision 또는 Mean Average Recall 등을 이용할 수 있다.
      
  </details>
- User 베이스 추천 시스템과 Item 베이스 추천 시스템 중 단기간에 빠른 효율을 낼 수 있는 것은 무엇일까요?
- 성능 평가를 위해 어떤 지표를 사용할까요?
- Explicit Feedback과 Implicit Feedback은 무엇일까요? Impicit Feedback을 어떻게 Explicit하게 바꿀 수 있을까요?
- Matrix Factorization은 무엇인가요? 해당 알고리즘의 장점과 단점은?
- SQL으로 조회 기반 Best, 구매 기반 Best, 카테고리별 Best를 구하는 쿼리를 작성해주세요
- 추천 시스템에서 KNN 알고리즘을 활용할 수 있을까요?
- 유저가 10만명, 아이템이 100만개 있습니다. 이 경우 추천 시스템을 어떻게 구성하시겠습니까?
- 딥러닝을 활용한 추천 시스템의 사례를 알려주세요
- 두 추천엔진간의 성능 비교는 어떤 지표와 방법으로 할 수 있을까요? 검색엔진에서 쓰던 방법을 그대로 쓰면 될까요? 안될까요?
- Collaborative Filtering에 대해 설명한다면?
- Cold Start의 경우엔 어떻게 추천해줘야 할까요?
- 고객사들은 기존 추천서비스에 대한 의문이 있습니다. 주로 매출이 실제 오르는가 하는 것인데, 이를 검증하기 위한 방법에는 어떤 것이 있을까요? 위 관점에서 우리 서비스의 성능을 고객에게 명확하게 인지시키기 위한 방법을 생각해봅시다.

##### [목차로 이동](#contents)

---