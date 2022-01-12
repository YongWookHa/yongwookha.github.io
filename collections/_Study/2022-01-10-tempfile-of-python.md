---
layout: post
title: 파이썬 tempfile
subtitle: 임시 파일과 디렉터리 생성
tags: [DEVELOP, STUDY]
cover-img: assets/img/tempfile-python.png
comments: true
---

임시 파일은 프로그램 동작 중에 데이터를 임시적으로 보관하거나, 영구 파일을 생성하는 과정에서 사용된다. Word, Excel 같은 문서 편집기 뿐만 아니라 영상, 사진 등의 `data`를 생성하는 프로그램에서는 예상할 수 없는 오류에 의해 작업 중이던 정보를 잃어버리는 것을 대비하고, 작업 내용을 복원하기 위해 임시 파일을 이용한다.

프로그래밍 중에 저장될 필요는 없지만 파일 포멧으로 데이터를 다뤄야하는 경우에 임시 파일을 이용한다. 간단하게 말하면 임시 파일은 **"이름이 필요 없는 파일"을 생성하고 이용 후에 삭제하는 방식"**이라고 생각할 수 있다.

하지만 이런 임시 파일이 적절한 타이밍에 자동으로 삭제되지 않는다면 storage를 낭비하는 주범이 될 수도 있다. 매번 임시적으로 파일을 생성하고 삭제하는 것이 불편하다면, 파이썬에서는 [tempfile](https://docs.python.org/ko/3/library/tempfile.html) 라이브러리를 쓰는 것이 좋은 솔루션이 될 수 있다.

# Tempfile

- `tempfile.TemporaryFile` : 임시 파일 반환 _자동 삭제_
- `tempfile.NamedTemporaryFile` : 이름이 있는 임시 파일 반환 _자동 삭제_
- `tempfile.SpooledTemporaryFile` : 파일에 데이터가 쓰여지기 전에 먼저 메모리에 스풀링. 나머지는 TemporaryFile과 같음 _자동 삭제_
- `tempfile.TemporaryDirectory` : 임시 디렉토리 반환 _자동 삭제_
- `tempfile.mkstemp` : 임시 파일 생성, _수동 삭제 필요_
- `tempfile.mkdtemp` : 임시 폴더 생성, _수동 삭제 필요_
- `tempfile.gettempdir` : 임시 파일이 생성되는 디렉터리 이름 반환

`mode`는 default 값이 `w+b`이므로 byte type으로 써야한다. 제일 간단하게 사용할 수 있는 방법은 `with`문과 함께 써서 임시 파일의 life scope를 가시적으로 제한하는 것이다.

```python
import tempfile
import some_library

with tempfile.TemporaryFile(mode="w+t", encoding='utf8') as fp:
    fp.write("temporal data")
    fp.seek(0)
    some_library(fp)

```  

`with`문 없이 객체로 관리한다면 `close()`해주어야 삭제까지 할 수 있다.

```python
import tempfile

fp = tempfile.TemporaryFile()
fp.write(b"temporal data")
fp.seek(0)
fp.read()
# b"temporal data"
fp.close()
```

