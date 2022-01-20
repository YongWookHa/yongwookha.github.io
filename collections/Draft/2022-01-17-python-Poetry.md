---
layout: post
title: Poetry
subtitle: python packaging and dependency management
tags: [DEVELOP, STUDY]
cover-img: /assets/img/poetry.png
comments: true
---

최근에는 내가 다루는 거의 모든 프로젝트가 Python 기반이다 보니, 접하게 되는 라이브러리도 정말 다양하다. 그동안 [anaconda](https://www.anaconda.com/)를 통해 개발 과정에서의 dependencies를 관리했었는데, 개인 프로젝트에서는 크게 문제 없었지만 아무래도 pip과 conda를 혼용해서 패키지를 설치하다보니, 팀 프로젝트 운영에서 패키지의 버전 관리 및 공유가 매끄럽지는 못했다.

[Poetry](https://python-poetry.org/)는 각 프로젝트별로 완전히 격리된 개발 환경을 구성하고, 사용되는 패키지의 버전을 파일에 명시하는 것으로 확실한 의존성 관리를 가능케한다.

포스팅 내용은 poetry의 [official documents](https://python-poetry.org/docs)를 기준으로 작성했다.

## Install

1. python 설치  
   이용하고자 하는 python의 버전을 특정하여 설치한다. 나는 3.9.10버전을 개발 표준으로 프로젝트를 생성했다.  
   ```bash
   python --version
   > Python 3.9.10
   ```
2. Poetry 설치  
    [installation script](https://install.python-poetry.org/)를 다운받고 이를 python으로 실행한다.
    ```bash
    python install-poetry.py
    ```  
    또는  
    ```bash  
    # linux
    POETRY_HOME=[원하는 위치] python install-poetry.py
    # windows
    set POETRY_HOME=[원하는 위치] && python install-poetry.py
    ```
    위 명령어를 통해 설치가 끝나면 Poetry가 설치된 로컬 주소가 나온다. 이를 환경 변수에 추가해주고 shell을 재실행하면 터미널에서 `poetry` 명령어를 바로 이용할 수 있다.  
3. 설치 확인
    ```bash
    poetry --version
    ```
4. tab completion (linux)
    [enable-tab-completion-for-bash-fish-or-zsh](https://python-poetry.org/docs/master/#enable-tab-completion-for-bash-fish-or-zsh)

## Configuration

프로젝트를 생성하기 전에 Peotry에서 지원하는 설정값들을 확인해보자.

```bash
poetry config --list
```

위 명령어의 결과는 아래와 같다.  

```  
cache-dir = "<local cache-dir>"
experimental.new-installer = true
installer.parallel = true
virtualenvs.create = true
virtualenvs.in-project = null
virtualenvs.path = "{cache-dir}/virtualenvs"
```

`in-project` 설정이 null 또는 false로 되어 있으면 local의 특정 디렉토리(virtualenvs.path)에 virtual env가 생성된다. 나의 경우에는 프로젝트 폴더 내에 모든 내용을 저장하고 싶어서 아래 명령어와 함께 true 값을 줬다.

```bash
poetry config virtualenvs.in-project true
```

각 설정값에 대한 자세한 설명은 [available-settings](https://python-poetry.org/docs/master/configuration/#available-settings) 문서를 참조하자. 

## Project setup

Poetry의 설치와 세팅을 마쳤다면, 이제 프로젝트를 직접 생성해보자.

```bash
poetry new poetry-demo
```  

위 명령어를 입력하면 다음과 같은 기본 디렉토리가 생성된다.

```
poetry-demo
├── pyproject.toml
├── README.rst
├── poetry_demo
│   └── __init__.py
└── tests
    └── __init__.py
```

`README.rst`는 취향에 따라 `README.md`로 수정해도 좋다. _[`1.2.0a2`](https://github.com/python-poetry/poetry/commit/affabe04c8cdfaa63d7d87b36107fd1003048688)에서 readme format을 사용자가 설정 가능하도록 업데이트되었다._

`pyproject.toml`에는 프로젝트의 dependencies가 저장된다. 파일 내부에는 아래와 같은 정보들이 자동으로 입력된다. 프로젝트를 생성하면 [too.poetry] 영역을 수정하면 된다. 이후에 `poetry add <package>` 명령어를 통해 이 파일에 내가 원하는 dependency를 추가할 수 있다. _`add` 커맨드는 적절한 버전을 찾고, 해당 패키지의 서브 패키지들을 함께 설치 후, `pyproject.toml`에 기록한다._

```
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = "Poetry 실험용 데모 프로젝트"
authors = ["My Team <github.com/<my-team-name>"]
readme = "README.md"
packages = [{include = "poetry_demo"}]

[tool.poetry.dependencies]
python = "^3.6"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

각 dependencies의 버전 정보 기입 방법은 [Dependency specification](https://python-poetry.org/docs/master/dependency-specification/)문서에서 다룬다. 버전 번호 규칙은 [semver](https://semver.org/lang/ko/)의 정의를 따르고 있다.

- Caret requirements `^`
  0이 아닌 가장 왼쪽 숫자가 변하지 않는 한, 업데이트 가능
  <details markdown="1">
    <summary>[예시]</summary>

    | REQUIREMENT | VERSIONS ALLOWED |
    | --- | --- |
    | ^1.2.3 | >=1.2.3 <2.0.0 |
    | ^1.2 | >=1.2.0 <2.0.0 |
    | ^1 | >=1.0.0 <2.0.0 |
    | ^0.2.3 | >=0.2.3 <0.3.0 |
    | ^0.0.3 | >=0.0.3 <0.0.4 |
    | ^0.0 | >=0.0.0 <0.1.0 |
    | ^0 | >=0.0.0 <1.0.0 | 

  </details>
- Tilde requirements `~`
  가장 오른쪽 숫자만 업데이트만 가능
  <details markdown="1">
    <summary>[예시]</summary>

    | REQUIREMENT | VERSIONS ALLOWED |
    | --- | --- |
    | ~1.2.3 | >=1.2.3 <1.3.0 |
    | ~1.2 | >=1.2.0 <1.3.0 |
    | ~1 | >=1.0.0 <2.0.0 |

  </details>
- Wildcard requirements (`*`)
  `*` 위치의 최신 버전
  <details markdown="1">
    <summary>[예시]</summary>

    | REQUIREMENT | VERSIONS ALLOWED |
    | --- | --- |
    | * | >=0.0.0 |
    | 1.* | >=1.0.0 <2.0.0 |
    | 1.2.* | >=1.2.0 <1.3.0 |

  </details>

- Exact & multiple requirements
  특정 버전 넘버만 기입하면 exact, 여러 버전이 필요하다면 쉼표로 분리 가능 e.g. `>= 1.2, < 1.5`

이외에, github 주소와 브랜치를 이용하는 `git` dependencies나 로컬 `path`, 라이브러리 파일의 주소를 이용하는 `url`을 지원한다. 이외에도 파이썬 버전에 따른 버전 나누기 등의 다양한 방식의 versioning을 지원한다.

## Run

격리된 virtual environment로 스크립트를 실행하는 방법은 `poetry run`으로 1회성 실행 방법과 `poetry shell`을 이용해서 shell을 생성하는 방법이 있다. 

```
poetry run python <my-script>.py -args
poetry run pytest
```

```
poetry shell
```

## `poetry install`

`pyproject.toml`에 적혀있는 dependency를 설치할 때는 `poetry install`를 이용한다. `--only` 옵션으로 특정 패키지만 설치하거나 `--without` 옵션으로 특정 패키지는 설치하지 않을 수도 있다. `--sync` 옵션은 `pyproject.toml`에 맞게 최신화할 때 사용한다. 이외에도 여러 [옵션들](https://python-poetry.org/docs/master/cli/#options-1)이 있다.

```bash
poetry install
poetry isntall --only test,docs
poetry install --without test,docs
poetry install --sync
```

## `poetry update`

