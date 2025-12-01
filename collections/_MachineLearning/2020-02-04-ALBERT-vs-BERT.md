---
layout: post
title: A Light BERT, ALBERT (2020) 
subtitle: BERT와 비교하여 간단 정리
tags: [MACHINE_LEARNING, NLP]
cover-img: /assets/img/albert-einstein.webp
comments: true
---

NLP계를 주름잡던 BERT(2018)가 세상에 발표된지도 얼마 되지 않았는데, 역시 딥러닝은 한달이 멀다하고 새로운 모델들이 쏟아지고 있습니다. 역시 발전 순서는 매번 비슷한 것 같습니다.   

1. 누군가 새로운 유형의 네트워크를 발표한다.  
2. 유사한 네트워크 구조에 파라메터 개수를 늘려서 네트워크 성능을 최대한 끌어내는 것을 시도  
3. 비대해진 네트워크를 성능 감소를 최소화하며 최적화  

이번에도 역시 Transformer가 제시한 새로운 네트워크 구조를 BERT와 XLNet 같은 논문에서 최대 성능을 이끌어내고, ALBERT와 같은 논문으로 최적화하고 있습니다. 오늘의 주제는 ALBERT입니다. 이름에서부터 대놓고 BERT의 후속작이라, 기존 BERT 모델과의 차이점과 개선 아이디어 위주로 간단하게 정리해봅니다.  

자세한 내용은 [Google AI Blog - ALBERT](https://ai.googleblog.com/2019/12/albert-lite-bert-for-self-supervised.html)를 참조하세요.  

-----------------------------------------

2020년 상반기. 대부분의 NLP Tasks  SOTA 는 **ALBERT** -> ICLR 2020 ACCEPTED

## 1. ALBERT의 성능에는 무엇이 기여하는가?


**IDEA** : BERT는 크다. 그냥 막 쓰기에는 너무 크다. 그래서 이걸 줄여야겠다.  

---  
![ALBERT](https://www.dropbox.com/s/vxa7myiarjj79yt/BERT-vs-ALBERT.png?raw=1)   

---  

### A. Allocate the model's capacity more efficiently  

어디서 무엇을 학습해야하는가에 대한 고민.  

*   Input-Level Embedding   
    문맥에 상관없는 표현(Context Independent)을 학습시키자 -> relatively-low demension(e.g., 128)   
*   Hidden-Layer Embedding   
    문맥과 연관된 표현(Context Dependent)을 학습시키자 -> higer demension(e.g., 768)   

이 아이디어를 통해 Projection Layer의 parameter 수를 80% 감소시켰지만, 성능 손실은 거의 Zero (SQuAD2.0 80.4 -> 80.3   

  

### B. Parameter Sharing  

기존의 Transformer 기반 모델들(BERT, XLNet, RoBERTa)는 독립된 Layer들이 쌓여있었음. 그러나 각 층들이 비슷한 연산을 수행하는 것을 확인.  
같은 Layer를 쌓아올려서 Parameter 수를 감소시킴. (Attention-feedforward 블록에서 90% 감소 / 전체에 모델에서 70% 감소)  
성능 손실은 미미함 (80.3 -> 80.0)

  
### C. Scale up the model AGAIN

기존 BERT-base 모델의 Parameter 수는 110M, ALBERT-base 는 12M  
전체 모델의 Parameter 개수가 90% 감소하였기 때문에 Hidden Layer의 Embedding size를 키울 여력이 생김.  
Hidden layer를 10~20배 키울 수 있음. 논문의 ALBERT-xxlarge 실험에서는 hidden-size 4096 이용. (BERT-large의 Hidden-size 1024)  
BERT-large에 비해 30% 적은 Parameter 수임에도 큰 성능 향상 얻음 (SQuAD 83.9 -> 88.1)  



