---
layout: post
title: OCR 엔진 개발기
subtitle: feat. open source
tags: [MACHINE_LEARNING, DEVELOP]
cover-img: assets/img/OCR_cover.jpg
comments: true
mathjax: true
---

R&D팀에서 현업에서 일을 한지도 벌써 2년을 꽉 채웠습니다. 그동안 회사에서는 특히 OCR 관련 업무를 주로 진행했는데, 야외 환경(text in the wild)부터 고문서까지 다양한 환경에 대해 OCR을 적용하는 경험을 할 수 있었습니다. 최근에는 다양한 오픈 소스 레포지토리가 공개되어있어 논문을 읽으면 거의 곧장 실험 및 검증을 할 수 있는 코드로 접근이 가능합니다. 그동안의 경험 중, OCR에 흥미를 가지고 새롭게 공부해보고자 하거나, 업무적 필요에 의해 간단한 OCR 솔루션을 구현해보려는 분들께 도움이 될 수 있을까 하여 오픈소스 기반으로 OCR 엔진을 직접 만드는 과정을 정리해보려고 합니다.

*Github에 [Toy project Repository](https://github.com/YongWookHa/Open-OCR-Engine)를 생성했습니다.*  

학습 전반은 Pytorch Lightning을 이용하여 GPU, TPU를 가리지 않도록 구현했습니다. 학습데이터 생성과 모델 학습 및 서빙을 직접 수행하면서 대략적인 OCR 흐름을 파악하는데 도움이 되면 좋겠습니다.

## 🔎 OCR (Optical Character Recognition) ?

OCR의 핵심 기능인 문자 검출(Detection) 모델과 문자 인식(Recognition) 모델을 어떻게 학습하느냐에 따라 End-to-End 방법 또는 Ensemble 방법으로 나눌 수 있습니다. 전자의 경우에는 문자 검출 모델과 인식 모델을 동시에 학습하고, 후자의 경우 별도 학습하게 됩니다.  

[Google OCR 서비스 관련 논문](https://das2018.cvl.tuwien.ac.at/media/filer_public/85/fd/85fd4698-040f-45f4-8fcc-56d66533b82d/das2018_short_papers.pdf#page=23)에서 구글의 web-based OCR 서비스 구성을 엿볼 수 있는데, Google은 Ensemble 방식을 이용하는 것으로 보입니다.

![](https://www.dropbox.com/s/zjkvt6cm3pv2f7x/google_ocr_structure.jpg?raw=1)  
논문에 실린 Figure에 따르면, 가장 먼저 입력 이미지에서 pixel-level heatmap으로 문자를 검출(Detection)하고, 문자의 방향(Direction)과 종류(Script) 분석 단계를 거친 후에 CNN위에 bi-LSTM을 붙여서 인식(Recognition)을 수행하는 방식으로 진행됩니다. 실제로 End-to-End 모델보다는 Ensemble이 더 좋은 결과를 얻을 수 있을 뿐만아니라 그림과 같이, 검출 이후에 문자 방향(Direction), 문자 종류(Script) 인식 등, 여러가지 결과 보조 수단을 추가할 수 있어 유연한 솔루션 구축이 가능합니다.

## ⚙ 어떤 모델을 쓸까?

NAVER 산하의 인공지능 연구소, `Clova AI`가 Github에 공개한 모델들을 이용하면 꽤 만족스러운 결과를 얻을 수 있습니다. 우리는 아래에 소개되는 두가지 모델을 이용할 예정입니다. 함께 링크된 공식 Repository에 방문하면 코드와 Paper를 참조할 수 있습니다.

- Text Detection : [CRAFT-Text-Detector](https://github.com/clovaai/CRAFT-pytorch)  
    `CRAFT`의 경우 모델 학습을 위한 코드는 공식 제공하지 않으므로 직접 구축해야합니다. `CRAFT`의 학습을 위해 간단히 인지해야할 배경지식이 있으니 잠시 확인합시다. 기존에 Text detection을 목적으로 구축된 Open data들은 대부분 Word 단위로 Labeling 되어 있습니다. 따라서 대부분의 Detection 모델들이 Word detection을 목적 Task로 했습니다. `CRAFT`는 기존의 Word detection Task를 Character detection task로 바라보는 방법으로 높은 성능 개선을 이뤘습니다. 이 말은 Word 단위로 생성된 데이터를 Character 단위로 쪼개야함을 의미합니다. Rule base로 학습 데이터를 Character level로 수정해야하는데, 이 코드는 공식 공개되지 않았습니다. 다음 단락에서 굳이 이 배경지식에 대한 설명을 하는 이유를 변명하겠습니다.  

    `CRAFT`의 학습에는 두가지 스테이지가 있습니다. Strong supervision과 Weak supervision 입니다. Strong의 경우 컴퓨터로 랜더링한 인조적 데이터를 이용해서 확실한(Strong) Ground truth를 생성하여 학습하는 방법입니다. 위에서 말한 Word level data의 Character level화가 필요하지 않습니다. 이와 달리 Weak supervision은 Word level로 구축된 실제 데이터를 Character level로 변환하여 확실하지는 않은(Weak) Ground truth를 이용해서 학습합니다.  

    우리가 학습할 OCR-Engine에 소개된 `CRAFT` 모델은 Strong supervision 방식만을 이용합니다. 이 부분을 쉽게 따라할 수 있도록 Reimplement해서 공유합니다. 대신, 실제 데이터에서 조금 더 의미있는 결과를 얻을 수 있게하기 위해 두가지 종류의 학습데이터를 이용하겠습니다.  

- Text Recognition : [deep-text-recognition-benchmark](https://github.com/clovaai/deep-text-recognition-benchmark)   
    `Deep-text-recognition-benchmark`는 말그대로, 새로운 모델의 소개가 아닌, 기존 모델들의 benchmark입니다. Text recognition 분야에서 인정받는 여러가지 모델들을 조합해서 쉽게 학습할 수 있도록 만들어진 매우 멋진 Repository입니다. 학습 및 테스트 코드까지 완전 공개되어 있기에, 감사히 이용할 수 있겠습니다.  

우리가 만들어볼 OCR 엔진은 매우 단순한 구조입니다. Text detection 모델 뒤에 Skew correction을 붙이고 곧바로 Text recognition 모델을 이어 붙이는 형태입니다. 학습 과정에서는 Text detection을 Stage 1으로 진행하고, TPS(Thin Plate Spline)라는 Deep learning 기반 Skew correction과 Text recognition을 Stage 2로 학습합니다.  

![구조 그림](https://www.dropbox.com/s/yb49iw3my0ymzws/open-ocr-structure.jpg?raw=1)

## 🧬 학습 데이터

Repository에서는 아래 이미지와 같이 신문과 유사한 회색 노이즈 배경 위에 글자를 랜더링하는 소스 코드를 제공합니다. Computer-generated data를 이용하면 정확한 Ground truth를 얻을 수 있기 때문에 위에서 언급한 Strong supervise learning이 가능합니다.

![](https://www.dropbox.com/s/m06dnj5m85y5zwy/generated_1.jpg?raw=1)  


## 🎉 서빙  
모델의 학습을 완료하고 나면, 위의 구조대로 모델을 서빙하는 API를 구축할 수 있습니다. 각 모델들을 연결하여 위의 구조로 결과를 내놓는 OCR engine을 Flask를 이용하여 매우 간단하게 API화 할 수 있습니다. 이렇게 API를 실행하면, RESTful-API를 통해 Request-Response 형식으로 결과를 얻을 수 있습니다.

**[https://github.com/YongWookHa/Open-OCR-Engine](https://github.com/YongWookHa/Open-OCR-Engine)**

> OCR-Engine을 개선할 수 있는 방법이 많이 있습니다. 좋은 아이디어를 가지고 계시다면 issue에 남겨주시고, 함께 만들어가면 더 없이 좋을 것 같습니다.


## 🔗 Reference 
- What Is Wrong With Scene Text Recognition Model Comparisons? Dataset and Model Analysis : [https://arxiv.org/abs/1904.01906](https://arxiv.org/abs/1904.01906)
- Character Region Awareness for Text Detection : [https://arxiv.org/abs/1904.01941](https://arxiv.org/abs/1904.01941)