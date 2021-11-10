---
layout: post
title: 나의 딥러닝 모델 Dockerize하기
tags: [STUDY, DEVELOP, MACHINE_LEARNING]
cover-img: /assets/img/dockerize.jpg
comments: true
---

내가 구현한 딥러닝 모델을 Prediction API로 deploy하는 것은 크게 어렵지 않습니다. [flask](https://flask.palletsprojects.com/en/2.0.x/), [fastapi](https://fastapi.tiangolo.com/ko/) 등을 이용하면 RESTful API로 쉽게 구현할 수 있습니다. 데모 수준에서 프로토타입을 만들 때는 매우 유용하지만, 하지만 실제 서비스에서는 `cli`로 실행하는 수준으로는 문제가 생길 수 있습니다. 요청 수에 유연하게 대처해야하는 상황에서는 서비스를 Dockerize할 필요가 있습니다.

최근 회사 업무로, 모델 구성이나 학습에 필요한 parameter를 전달받아 학습을 수행하고, 지정된 위치에 결과와 log를 저장하도록 **training** 코드를 Dockerize했던 경험을 기록합니다.

## Dockerize  

로컬에서 개발한 서비스를 [Docker](https://www.docker.com/)를 이용하여 모든 소프트웨어 의존성을 패키징하여 `container`라고 불리는 표준 단위로 제공하는 것을 의미합니다. `container`는 말 그대로 모든 의존성을 contain(포함하다)하고 있는 단위이기 때문에 개발 결과를 매우 범용적으로 release하는 방법입니다.

## Steps

먼저 내가 개발한 소프트웨어를 `Docker image`로 패키징하여 배포해야합니다. 그러면 이 `Docker image`를 이용하여 `container`를 생성하여 서비스할 수 있습니다. `Docker image`와 `container`는 객체지향 언어에서 class와 instance의 관계라고 생각할 수 있습니다.

### 0. Source code packaging  
소스코드를 `.tar` 파일로 압축합니다.

```
📦<image_name>.tar
 ┣ 📂src
 ┃ ┣ 📜datasets.py
 ┃ ┣ 📜models.py
 ┃ ┣ 📜utils.py
 ┃ ┗ ...
 ┣ 📜main.py
 ┗ 📜requirements.txt
 📜Dockerfile
 📜docker-compose.yml
```

### 1. Docker Image 생성 (`Dockerfile` 작성)  
Docker image 빌드를 위해 `Dockerfile`을 작성합니다. 가장 기본이 되는 Image로부터 시작해서, 필요한 의존성을 쌓아 올려 환경을 구축합니다. 아래의 예시에서는 python3.8.12 버전이 설치된 linux([debian](https://www.debian.org/releases/index.ko.html)의 buster release)에 위에서 압축한 `.tar` 파일을 압축해제하고, `📜requirements.txt`에 기록된 라이브러리를 설치하게 됩니다.

```
FROM python:3.8.12-slim-buster

WORKDIR /

ADD <image_name>.tar /

RUN python -m pip install --upgrade pip
RUN python -m pip install -r requirements.txt
```

이외의 작성 방법에 관련한 정보는 [문서](https://docs.docker.com/engine/reference/commandline/build/)에서 참조 가능합니다.

Dockerfile을 작성하고 나면, 이를 build해서 Image를 생성합니다.  

```
docker build . -t <image_name>
```
![](https://www.dropbox.com/s/ftnvxx875jg0mx4/docker_build.gif?raw=1)
