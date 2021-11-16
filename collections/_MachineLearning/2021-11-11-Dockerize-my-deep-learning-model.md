---
layout: post
title: 나의 딥러닝 모델 Dockerize하기
subtitle: Training Container
tags: [STUDY, DEVELOP, MACHINE_LEARNING]
cover-img: /assets/img/dockerize.png
comments: true
---

내가 구현한 딥러닝 모델을 Prediction API로 deploy하는 것은 크게 어렵지 않습니다. [flask](https://flask.palletsprojects.com/en/2.0.x/), [fastapi](https://fastapi.tiangolo.com/ko/) 등을 이용하면 RESTful API로 쉽게 구현할 수 있습니다. 데모 수준에서 프로토타입을 만들 때는 매우 유용하지만, 하지만 실제 서비스에서는 `cli`로 실행하는 수준으로는 문제가 생길 수 있습니다. 요청 수에 유연하게 대처해야하는 상황에서는 서비스를 Dockerize할 필요가 있습니다.

최근 회사 업무로, 모델 구성이나 학습에 필요한 parameter를 전달받아 학습을 수행하고, 지정된 위치에 결과와 log를 저장하도록 **training** 코드를 Dockerize했던 경험을 기록합니다. 포스트 내용 중에는 꼭 따르지 않아도 되는 규칙(_예를들어 소스코드 패키징 방법 등_)이 있습니다. 감안해주시면 도움이 될 것 같습니다.

<br/>

--- 

<br/>

# 🐳 Dockerize  

로컬에서 개발한 서비스를 [Docker](https://www.docker.com/)를 이용하여 모든 소프트웨어 의존성을 패키징하여 `container`라고 불리는 표준 단위로 제공하는 것을 의미합니다. `container`는 말 그대로 모든 의존성을 contain(포함하다)하고 있는 단위이기 때문에 개발 결과를 매우 범용적으로 release하는 방법입니다.

먼저 내가 개발한 소프트웨어를 `Docker image`로 패키징하여 배포해야합니다. 그러면 이 `Docker image`를 이용하여 `container`를 생성하여 서비스할 수 있습니다. `Docker image`와 `container`는 객체지향 언어에서 class와 instance의 관계라고 생각할 수 있습니다.

<br/>

--- 

<br/>

## 👟 step1. Source code packaging  
소스코드 패키징은 여러가지 방법이 있습니다. 보통은 git으로 형상관리를 하지만, 이 포스트에서는 편의상 압축파일로 관리한다고 가정합니다. 특정 버전의 소스코드를 `.tar` 파일로 압축합니다. 

```
📦<source-code>.tar
 ┣ 📂outputs
 ┣ 📂data
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

위의 구조에서 따라야하는 사항은 다음과 같습니다.

1. model 객체화
 모델(`src/models.py`)은 `train` method를 가진 class로 구현합니다. 또는 `src/agent.py`를 생성하여 `train` 과정을 별도로 구성해도 무방합니다.
2. main.py 
main.py에서는 arguments로 parameter 값을 입력받아 dataloader, model 등의 학습 구성 요소를 initialize하고, train을 수행합니다. 예시는 아래와 같습니다.  

```python
# main.py
from src.resnet import Resnet18

if __name__ == "__main__":
    import argparse
    
    parser = argparse.ArgumentParser()
    parser.add_argument("--epochs", type=int, default=10, help="total num of epochs")
    parser.add_argument("--lr", type=float, default=2e-4, help="learning rate")
    parser.add_argument("--bs", type=int, default=128, help="batch size")
    parser.add_argument("--num_worker", type=int, default=8, help="number of worker for dataloader")
    parser.add_argument("--log_frequency", type=int, default=10, help="tensorboard log frequency (iterations)")
    parser.add_argument("--train_data", type=str, help="train data directory")
    parser.add_argument("--val_data", type=str, help="validate data directory")
    parser.add_argument("--pretrained", type=str, help="pretrained checkpoint path")
    args = parser.parse_args()

    print("config:", args)
    agent = Resnet18(args)
    agent.train()
```  

3. `src`내 추가 디렉토리 구성은 자유입니다.  
4. `data`에서 데이터를 불러옵니다. runtime에서 volume mount를 통해 데이터에 접근할 수 있습니다.
5. 모델이 학습 과정에서 생성하는 `checkpoints`와 `tensorboard log`는 `outputs` 폴더에 저장합니다. (추후 volume mount)

<br/>

--- 

<br/>

## 👟step2. Docker Image 생성 (`Dockerfile` 작성)  
Docker image 빌드를 위해 `Dockerfile`을 작성합니다. 가장 기본이 되는 Image로부터 시작해서, 필요한 의존성을 쌓아 올려 환경을 구축합니다. 자주 사용되는 몇가지 문법에 대해 간단히 설명합니다.

- **`FROM`** : Base가 되는 Docker Image
- **`ENV`** : 빌드시에 필요한 환경변수 정의
- **`WORKDIR`** : CWD 설정
- **`ADD`** : 압축 해제하여 복사(`tar`, `tar.gz`) 혹은 원격지 파일을 이미지 내부로 복사
- **`COPY`** : Host의 파일 또는 디렉토리를 이미지 내부로 복사
- **`RUN`** : Base Image에 Layer를 쌓을 때, 패키지 설치 명령어와 함께 이용
- **`CMD`** : `docker run`에서 명령어가 생략될 시, default로 적용
- **`ENTRYPOINT`** : `docker run`에서 실행되는 명령어

### 🎨 예시  
```dockerfile
FROM pytorch/pytorch:1.10.0-cuda11.3-cudnn8-runtime

WORKDIR /

ADD <source-code>.tar /

RUN python -m pip install --upgrade pip
RUN python -m pip install -r requirements.txt
```

위의 예시에서는 python3.8.12 버전이 설치된 linux([debian](https://www.debian.org/releases/index.ko.html)의 buster release)에 위에서 압축한 `.tar` 파일을 압축해제하고, `📜requirements.txt`에 기록된 라이브러리를 설치하게 됩니다. 

이외의 작성 방법에 관련한 정보는 [공식 문서](https://docs.docker.com/engine/reference/builder/)에서 참조 가능합니다.

Dockerfile을 작성하고 나면, 이를 빌드해서 Image를 생성합니다.  

```shell
> docker build . -t <my-image-name>
```
![](https://www.dropbox.com/s/ftnvxx875jg0mx4/docker_build.gif?raw=1)

빌드가 성공했다면, `docker images` 명령어를 입력하면 내가 만든 `<my-image-name>`을 찾을 수 있습니다.

<br/>

--- 

<br/> 

## 👟step3. Container 생성 (`docker-compose.yml` 작성)

빌드된 이미지를 이용해서 `container`를 생성합니다. `cli`를 통해 `container`에 필요한 arguments를 직접 입력해주는 `docker run`과, 미리 `docker-compose.yml` 파일에 arguments를 모두 입력해놓고 실행 시 불러오는 `docker compose`가 있습니다. 

### - docker run

docker에서 가장 많이 사용하게 되는 `run`에는 다양한 option들이 있습니다. 그 중, 몇가지만 간략하게 확인하자.

- `-v`, `--volume` : [host-src]:[container-dest] 저장 공간 bind
- `-d`, `--detach` : 백그라운드 실행
- `-p`, `--port` : [host-port]:[container-port] 포트 포워딩
- `--gpus` : 사용할 gpu 입력 (ex1. '"device=0,2"') (ex2. all)
- `--rm` : container 상태가 exit이 되면 자동으로 삭제

```bash
docker run --gpus all \
    -v [host-src]:/outputs <my-image-name> \
    "--parameter_name_1" "--parameter_value_1" \
    "--parameter_name_2" "--parameter_value_2" \
    ...
```

_자세한 option 내용은 [공식 문서](https://docs.docker.com/engine/reference/run/) 참조_

python script에 전달할 parameter는 container setting 이후에 string type으로 넘겨줄 수 있습니다.

### - docker compose  

아래에서는 `docker-compose.yaml` 파일 작성 자동화에 대한 내용을 다룹니다.

<details>
<summary>🔽 내용 펼치기</summary>
<div markdown="1">       

`docker-compose.yml`에서 자주 쓰이는 문법은 다음과 같습니다.

- **`version`** : docker compose version
- **`services`** : 서비스 나열
- **`ports`** : "<HOST>:<CONTAINER>"로 포트를 연결. _string 명시 권장_
- **`volumes`** : "<HOST>:<CONTAINER>"로 저장 공간 연결
- **`command`** : container 내부에서 실행될 명령어 지정
- **`container_name`** : container명을 지정. single-container service에서만 이용 가능.


### 🎨 예시  
```yaml
version: "3.9"
services:
  dq_chinese_ocr:
    image: <my-image-name>
    container_name: <my-container-name>
    ports:
      - "5000:5000"
    volumes:
      - /logs/<my-container-name>:"/outputs"
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]
    command: 
      - python
      - main.py
      - --1st_parameter_name
      - 1st_parameter_value(string)
      - --2st_parameter_name
      - 2st_parameter_value(string)
      - ...
```

위 예시에서는 `<my-image-name>`를 이용해서 새로운 `container`를 만들고, 로 포트를 맞춘 후, `volumes`를 통해 저장 공간을 마운트합니다.  
생성되는 `container` 내부에서 `command`를 입력해서 서비스를 실행합니다.

API에서는 모델마다 요구되는 parameter가 다르기 때문에 `command`의 구성이 항상 변합니다. 이에 따라 모델 종류에 따라 docker-compose.yml 파일을 대신 작성하는 helper 함수를 이용하는 것이 좋습니다. helper 함수 예시는 아래와 같습니다.

```python
def fill_docker_compose_helper(draft: dict, params: dict) -> None:
    """
    python script에 전달되는 arguments를 제외한 모든 docker-compose 요소가
    채워진 상태에서 command에 필요한 arguments를 채워넣어 docker-compose dict를
    완성시키는 코드
    
    input:
        draft: 모델의 기본 docker-compose 형식 (command 제외)
        params: command에 입력될 arguments를 key, value로 구성한 dict
    output:
        완성된 docker-compose dict
    """
    def search_command(d: dict, path=[]):
        """
        채워넣어야 할 "command"가 어디있는지 찾아주는 helper
        """
        for k, v in d.items():
            if k == "command":
                return path
            elif isinstance(v, dict):
                res = search_command(v, path+[k])
                if res:
                    return res
    
    target = draft
    for p in search_command(draft):
        target = target[p]
    for k, v in params.items():
        target["command"].append(k)
        target["command"].append(v)
        
    # import yaml
    # with open('yaml.yaml', 'w') as f:
    #     yaml.dump(draft, f, sort_keys=False)
        
    return draft
```

마찬가지로 자세한 파일 작성 방법은 다음 [공식 문서](https://docs.docker.com/compose/compose-file/compose-file-v3/)를 참조합시다. _현재 최신 버전은 v3임에 유의합니다_

마지막으로, 아래 명령어를 통해 container를 실행하고 로그를 확인할 수 있습니다. 백그라운드에서 실행하길 원한다면 명령어 마지막에 `-d`를 붙여줍니다.

```shell
> docker compose up
```

</div>
</details>

<br/>

--- 

<br/>

# 📑 참조  
- [Docker Runtime Arguments\. Last night I fell down the rabbit hole… \| by Alex Galea \| Medium](https://galea.medium.com/docker-runtime-arguments-604593479f45)  
- [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)  
- [https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)  
- [https://docs.docker.com/compose/compose-file/compose-file-v3/](https://docs.docker.com/compose/compose-file/compose-file-v3/)  
