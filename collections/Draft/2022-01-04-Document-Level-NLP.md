---
layout: post
title: Document Level NLP
subtitle: Long BERT
tags: [MACHINE_LEARNING, NLP]
cover-img: /assets/img/
comments: true
---

BERT를 위시한 Transformer 계열의 모델들이 발전하면서 `word`, `sentence` Level에서의 NLP는 어느정도 만족할만한 성능을 보이고 있다. [🤗Hugging Face](https://huggingface.co/) 등의 커뮤니티는 이제 일반적인 오픈소스를 넘어서 하나의 플랫폼으로 진화하고 있다. 


## Sentence Ranking
> **[2018] Ranking Sentences for Extractive Summarization with Reinforcement Learning**  
> Loss 계산에 Cross Entropy를 쓰지 말자. 최종 스코어를 만드는 BLEU나 ROUGE를 이용해서 Reinforcement learning하는게 효과적.  

> **[2020] An Unsupervised Semantic Sentence Ranking Scheme for Text Documents**  
> Wow

> **[2020] ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT**  
> BERT 계열의 Document Level Task에서 Computational Cost Reduction을 할 수 있는 `Late Interaction` 방법 제안  
> ![](https://www.dropbox.com/s/njvjt0imgy3cgnn/ColBERT.png?raw=1)
    