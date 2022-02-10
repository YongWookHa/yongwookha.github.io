---
layout: post
title: OCR 최신 동향
tags: [STUDY, DEVELOP, MACHINE_LEARNING]
cover-img: /assets/img/OCR_cover_2.jpg
comments: true
mathjax: true
---

이번 포스트에서는 최근의 OCR 분야의 최신 동향을 알아보고 정리해보려고 한다. 전통적으로, OCR 분야는 대상에 따라 두 개의 영역으로 나누어져 왔다. 최근에는 Deep Learning 기반의 모델들이 SOTA 리스트를 거의 장악하게 되면서 대상간 방법론의 차이가 거의 없어지는 추세이며, Structured text의 경우 LaTeX 태그들을 포함하여 결과를 내도록 학습하는 차이가 있다.

- **Structured Text**  
  ![](https://www.dropbox.com/s/pkqwdozwea48ujy/structured_text.png?raw=1)  
  통제되는 환경에서 촬영된 인쇄체와 필기체 또는 Structured text on document를 대상으로 하는 OCR

- **Unstructured Text**  
  ![](https://www.dropbox.com/s/2yqgro8gcac0eef/unstructured_text.png?raw=1)  
  야외를 포함한 일반 이미지. General한 환경에서 촬영된 이미지에 출현하는 텍스트를 대상으로 하는 OCR

---

# 국내 기업 동향

## 🚩 NAVER

![](https://www.dropbox.com/s/gsrn8evzl3w912o/clova_ai_ocr.png?raw=1)

Clova AI에서 서비스되는 [DEMO](https://clova.ai/ocr)는 Clova AI에서 낸 두 논문에 기반하는 것으로 알려져 있음. `General OCR` 서비스에서 Scene Text Recognition을 지원한다. 2022년 2월 현재, `영수증`, `신용카드`, `사업자등록증`, `고지서`, `명함`, `신분증`으로 문서의 종류에 따라 API를 분리한 것으로 짐작했을 때, 각 문서 종류에 따라 전처리와 후처리의 방법을 달리하여 서비스하는 것으로 보인다. Clova-ai에서 [CVPR](https://openaccess.thecvf.com/menu) 컨퍼런스에서 공개한 OCR 관련 논문과 Repository는 아래와 같다.

### NAVER - Text Detection  
  ![](https://www.dropbox.com/s/esaw3ej7ixfdrst/craft.png?raw=1)  
  [Github-Repository](https://github.com/clovaai/CRAFT-pytorch) / [Paper](https://arxiv.org/abs/1904.01941)  
  watershed 기반의 학습 데이터 전처리 방법을 이용하여 데이터를 1차 가공한 후, 잘 알려진 U-Net 기반 네트워크를 이용하여 학습

### NAVER - Text Recognition  
  ![](https://www.dropbox.com/s/ioiv0kpprspp8sk/deep_text_recognition.png?raw=1)  
  [Github-Repository](https://github.com/clovaai/deep-text-recognition-benchmark) / [Paper](https://arxiv.org/abs/1904.01906)  
  검출된 텍스트의 인식을 위해 모델들을 조합하여 객관적인 Benchmark 방법을 제안함. TPS+ResNet+BiLSTM+CTC 조합이 가장 우수함.


## 🚩 Kakao

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZIE_1tq6xFk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

22년 2월 4일에 업로드된 "if(kakao) 2021 : OCR Model 개편 진행기"를 참조하면 Kakao OCR의 구성 키워드는 아래와 같다.

| 항목 | 특징 |
| --- | --- |
| 구조 | Text Detection, Text Recognition 모델 분리 |
| Text Detection Model | `Character-level Output`, `Model-based Clustering`, `Orientation Prediction`, `Simple Postprocessing(No NMS)` |
| Text Recognition Model | `Less Resources(No TPS)`, `Transformer Only Model`, `Fixed Length Input` |

조금 더 자세한 내용은 아래와 같이 정리할 수 있다.

### Kakao - Text Detection  
  ![](https://www.dropbox.com/s/x3szq7mplxdjebq/kakao-ocr-detection-structure.png?raw=1)  
  ResNet과 같은 Feature Extractor를 통해 Word, Charactor의 `Centerness`, `Offset`, `Regression`을 각 Head에서 예측하고 이들을 조합하여 결과 도출  
  ![](https://www.dropbox.com/s/r4zfpqzx6z7chte/kakao-ocr-detection-data.png?raw=1)  
  CRAFT의 학습 방식과 유사하게 SynthText를 이용하여 Weakly Supervised로 학습함.  

### Kakao - Text Recognition  
  ![](https://www.dropbox.com/s/kaqop7f5pwbf6wu/kakao-ocr-recognition-structure.png?raw=1)  
  ConvNet으로 Feature Extract하여 Patch를 만들고 Transformer에 입력  
  ![](https://www.dropbox.com/s/llvtg6n5u82ra1p/kakao-ocr-recognition-data.png?raw=1)  
  다양한 loss를 이용해서 학습함. `Supervised`, `Contrastive Learning`, `Semi-supervised Learning`을 혼용함.

---

#  해외 기업 동향

## 🚩 Google

![](https://www.dropbox.com/s/zjkvt6cm3pv2f7x/google_ocr_structure.jpg?raw=1)  

2018년 Google Research에서 공개한 Google OCR structure이다. 공개 이후 시간이 흘렀기 때문에 현재는 구조적으로 달라져있을 수 있다. [Google - Cloud Vision API](https://cloud.google.com/vision/docs/ocr)에서 Demo 결과를 확인해보면, Bounding Box가 Word와 Character별로 리턴되는 것을 알 수 있다. 위의 KAKAO에서 이용하는 방식대로 Multi-head를 이용하여 Word, Character Level Bounding Box를 각각 얻은 후 Recognition을 수행하는 것으로 보인다.

이외에도 구글은 [Vision-Transformer](https://arxiv.org/abs/2010.11929)등을 통해 Vision Task에서 Attention을 이용하려는 움직임을 보여오고 있으며, 이는 OCR에도 적용되고 있을 것으로 보인다.


## 🚩 Microsoft

Microsoft의 Asure에서 제공하는 OCR는 Paragraph 단위로 텍스트 검출 영역(Region)을 확보하고 line detection 후 word 단위의 결과로 리턴하는 것으로 확인된다. Character Level Bounding Box가 리턴되지 않는다.

![](https://www.dropbox.com/s/ma2ls0hvylwbd0f/ms-ocr-reading-order-example.png?raw=1)

Document OCR에서는 `readingOrder`와 같은 옵션으로 위의 이미지와 같이 일부 Structured text에 대한 대응이 가능하다.

이 밖에, Microsoft Research 팀은 [Swin-Transformer](https://arxiv.org/abs/2103.14030)등의 Vision 분야의 SOTA 모델들을 공개하고 있는데, 이들 모델을 OCR의 인식기에 탑재했을 가능성이 높다.

## 🚩 Meta (구 Facebook)

![](https://www.dropbox.com/s/q5sy0wtsoypztq4/meta-textOCR.png?raw=1)

Meta는 별도의 OCR 서비스를 오픈하지는 않았지만, 지속적으로 관련 연구 논문을 내고 있다. 2021년 Facebook AI Research 팀에서 발표한 [TextOCR](https://research.facebook.com/publications/textocr-towards-large-scale-end-to-end-reasoning-for-arbitrary-shaped-scene-text/)에서는 Scene Text 대상 End-to-End 모델에서 활용 가능한 새로운 학습 방법을 제시했다. 논문에 따르면, `image`-`text region`-`text`-`QA`로 구성된 TextVQA data를 이용하여 multi-modal으로 학습한 경우, 같은 모델을 이용했을 때 최대 20%의 성능 향상을 거둘 수 있다.

---

# Open-Source 진영  

## 🚩 Tesseract  

가장 잘 알려진 오픈소스 OCR Engine. 22년 2월 현재 43.7k의 ⭐를 보유하고 있음. `V.4` 부터는 인식기를 LSTM를 기반으로 하여 legacy보다 더 나은 성능을 발휘하고 있다. [100개가 넘는 언어와 35개 이상](https://tesseract-ocr.github.io/tessdoc/Data-Files-in-different-versions.html)의 스크립트를 지원한다.

![](https://www.dropbox.com/s/3zl3ghlt6dm5rgz/Tesseract-OCR-engine-architecture.png?raw=1)

위와 같은 구조로 구성되어 있으며 순차적 설명은 아래와 같다.

1. 입력 이미지를 Adaptive Thresholding으로 Gray화
2. 글자의 연결 상태를 Image Processing 기반으로 파악
3. 라인과 단어 검출
4. 단어별 인식 (LSTM)
5. 단어 사전과 인식 결과 비교하며 Post Processing
6. 결과 도출


## 🚩 EasyOCR  

[https://github.com/JaidedAI/EasyOCR](https://github.com/JaidedAI/EasyOCR)에 공개된 OCR 솔루션이다. 22년 2월 현재 13.7k의 ⭐를 보유하고 있음. 메인 모델은 NAVER Clova-ai에서 공개한 모델들을 조합했다.

![](https://www.dropbox.com/s/ch85yd6uix3s2ua/easyocr_framework.jpg?raw=1)

프레임워크의 구조는 위의 그림과 같으며 각 단계에서 처리되는 내용은 아래와 같이 정리할 수 있다.

1. Pre-Process
  Image format -> GRAY 변환
2. CRAFT Text Detector 
  [NAVER Clova AI의 CRAFT 모델](https://github.com/clovaai/CRAFT-pytorch)[1](#참조) 사용
  텍스트가 출현 좌표 정보 얻음 (Poly)
3. Mid-Process
  Poly 좌표를 crop, transform해서 image list로 변환
4. Recognition Model
  ResNet으로 Feature Extract하여 LSTM, CTC으로 인식
  [NAVER Clova AI의 Deep Text Recognition](https://github.com/clovaai/deep-text-recognition-benchmark)과 유사함.
5. Decoder
  Greedy, Beam search, Word beam search 중 이용가능
6. Post Process
  Direction에 따라 결과 도출

> 참조 : [EasyOCR.readtext()](https://github.com/JaidedAI/EasyOCR/blob/048e8ecb52ace84fb344be6c0ca847340816dfff/easyocr/easyocr.py#L374)

---  

# Techniques  

## 🚩 TPS

![](https://www.dropbox.com/s/f1gd491i5uad04w/TPS.gif?raw=1)

[Spatial Transformer Network](https://arxiv.org/abs/1509.05329) 기반의 [RARE Network](https://www.researchgate.net/figure/Fiducial-points-and-the-TPS-transformation-Green-markers-on-the-left-image-are-the_fig3_301881079) 모듈. RARE Netwrok는 입력 이미지를 Affine Transform하여 OCR시에 Feature Extraction을 원활하게 돕는 역할을 한다. 주로 휘어져있는 텍스트를 교정하므로 Rectification이라고 한다. 모듈이기 때문에 독립적인 RARE Network를 인식기 레이어 앞에 붙여서 사용한다.

$$\begin{align} \begin{pmatrix} x_i^s \\ y_i^s \end{pmatrix} = \mathcal{T}_{\theta}(G_i) = \mathtt{A}_\theta \begin{pmatrix} x_i^t \\ y_i^t \\ 1 \end{pmatrix} = \begin{bmatrix} \theta_{11} \,\; \theta_{12} \,\, \theta_{13} \\ \theta_{21} \,\; \theta_{22} \,\; \theta_{23} \end{bmatrix} \begin{pmatrix} x_i^t \\ y_i^t \\ 1 \end{pmatrix} \end{align}$$

위의 식에서 $$\mathtt{A}_\theta$$로 표현되는 Affine Transform은 사용시에 정의하는대로 이용할 수 있다. 위에서 $$\theta_{11}$$ ~ $$\theta_{23}$$의 6개 parameter는 각각 scale, rotation, translation, skew, cropping을 표현한다.

_참조: [https://jamiekang.github.io/2017/05/27/spatial-transformer-networks/](https://jamiekang.github.io/2017/05/27/spatial-transformer-networks/)_

Module Layer가 추가되는 만큼, GPU에서 연산해야할 Parameter의 양도 늘어난다. 성능-리소스 간의 trade-off가 발생한다.


## 🚩 Post-OCR Processing Techniques 

OCR 모델 출력 이후에 `Error Correction` 모델을 추가하여 성능 향상을 기대할 수 있다. 보통, 인식 대상 텍스트에 Context가 존재하거나 빈번하게 출현하는 특정 Wrong case가 다수 존재하는 경우에 좋은 결과를 얻을 수 있다.

![](https://www.dropbox.com/s/xmukjakid4mvsgq/post-ocr.png?raw=1)

`Error Correction` 모델은 가장 간단한 Mannual approaches부터 word-level approaches, context-dependent approaches가 있다.

```
Post-OCR processing approaches
├── Manual approaches
└── (Semi-)automatic approaches
    ├── Isolated-word approaches
    │   ├── Merging OCR outputs
    │   ├── Lexical approaches
    │   ├── Error models
    │   ├── Topic-based language models
    │   └── Other models
    └── Context-dependent approaches
        ├── Language models
        │   ├── Statistical language models
        │   └── Neural network-based language models
        ├── Feature-based machine learning models
        └── Sequence-to-sequence (Seq2Seq) models
            ├── Traditional Seq2Seq models
            └── Neural network Seq2Seq models
```

Error case에 대한 통계 데이터를 보유하고 있는 경우, 적용해봄직하다.

> **참조**   
> _Survey of Post-OCR Processing Approaches_  
> [https://dl.acm.org/doi/pdf/10.1145/3453476](https://dl.acm.org/doi/pdf/10.1145/3453476)
>
> _ICDAR 2019 Competition on Post-OCR Text Correction_  
> [https://hal.archives-ouvertes.fr/hal-02304334/document](https://hal.archives-ouvertes.fr/hal-02304334/document)

---  

# Datasets

## 🚩 영어

| dataset | examples |
| --- | --- |
| Training Dataset | [MJSynth](https://www.robots.ox.ac.uk/~vgg/data/text/), [SynthText](https://www.robots.ox.ac.uk/~vgg/data/scenetext/), [TextVQA](https://textvqa.org) |
| Evaluation Dataset | [IIIT](http://cvit.iiit.ac.in/projects/SceneTextUnderstanding/IIIT5K.html), [Street View Text](http://www.iapr-tc11.org/mediawiki/index.php/The_Street_View_Text_Dataset), [IC03](http://www.iapr-tc11.org/mediawiki/index.php/ICDAR_2003_Robust_Reading_Competitions), [IC13](https://rrc.cvc.uab.es/?ch=2), [IC15](https://rrc.cvc.uab.es/?ch=4), [CUTE](http://cs-chan.com/downloads_CUTE80_dataset.html) |

## 🚩 한글

| dataset | examples |
| --- | --- |
| Training Dataset | [SynthText](https://github.com/youngkyung/SynthText_kr) |
| Evaluation Dataset | [한국어 글자체 이미지 - Text in the Wild](https://github.com/youngkyung/SynthText_kr), [다양한 형태의 한글 문자](https://aihub.or.kr/aidata/33987), [공공행정문서 OCR](https://aihub.or.kr/aidata/30724) |  
