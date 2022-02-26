---
layout: post
title: PEP-8 요약
subtitle: 꼭 읽어봐야할 파이썬 코드 스타일 가이드
tags: [DEVELOP, STUDY]
cover-img: /assets/img/pep-8-summary.png
comments: true
---

Code convention의 존재 이유는, 코드는 쓰이는 양보다 읽히는 양이 훨씬 더 많기 때문이다. 결국, **가독성**을 높이고 여러 사람의 협업 결과물이 마치 한 사람이 만든 것 처럼 **일관성**있게 하려는 목적이다. 

이번 포스트에서는, 전 포스트에서 정리했던 PEP-8의 **거의** 전문을 적당히 덜어내고 필요하겠다 싶은 부분만 요약한다. 전문은 아래 링크를 참조하자. 

> **PEP8 전문** : [https://yongwookha.github.io/Study/2022-02-11-pep8](https://yongwookha.github.io/Study/2022-02-11-pep8)

Code convention은 docstring, code style을 포함하므로, [PEP-8 (Style Guide)](https://www.python.org/dev/peps/pep-8/)을 다루기 전에, [PEP-257 (Docstring)](https://www.python.org/dev/peps/pep-0257/)과 [PEP-20(The Zen of Python)](https://www.python.org/dev/peps/pep-0020/)을 먼저 확인한다.

# Docstring

Docstring은 `module`, `function`, `class`, or `method`를 정의할 때, 설명을 위해 기술되는 텍스트다. 이러한 Docstring은 `object`의 `__doc__` attribute로 지정된다.

모든 `module`은 보통 docstring을 가지고 있어야하며, `module`에 의해 export되는 모든 `function`과 `class` 또한 docstring을 가지고 있어야한다. `__init__`과 같은 Public method도 마찬가지다.

이렇게 적히는 docstring은 코드에서 documentation과 같은 역할을 하게 된다. Python bytecode 컴파일러에서 docstring 인식되지 않는다.

> docstring : `"""` 로 둘러싸인 텍스트
> comment : `#` 이후의 텍스트

## One-line Docstring

One-line docstring은 간단하고 명확하다.

```python
def kos_root():
    """Return the pathname of the KOS root directory."""
```

Docstring 전에 공백 라인을 포함시키지 않는다. Docstring은 `function`이나 `method`의 효과를 설명이 아닌 명령으로 표현한다. 예를들어, "이 함수는 A를 하고 B를 리턴합니다."가 아니라 "A하여 B리턴"과 같이 간단하게 적는다.

```python
# only in C function

def function(a, b):
    """function(a, b) -> list"""
```

위와 같은 형식은 함수 내부를 확인할 수 없는 built-in 같은 C function 에만 적합하다. 일반적인 상황에서는 아래와 같이 쓰는 것이 좋다.

```python
def function(a, b):
    """Do X and return a list."""
```

##  Multi-line Docstring

Multi-line의 경우 Docstring의 구성이 나뉜다. 그 구성은 다음과 같다.

1. One-line Docstring과 같은 요약
2. 한 줄 띄우기
3. 상세한 설명

```python
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """
    if imag == 0.0 and real == 0.0:
        return complex_zero
    ...
```

요약 부분 은 자동 indexing tool 등에서 이용될 수 있다. 한 줄에 맞추는게 중요하고, 나머지 Docstring과는 빈 줄로 구분되어야 한다. 요약 줄은 여는 따옴표(`"""`) 뒤에 또는, 바로 아래 줄에 쓰도록 한다. 그리고 모든 Docstring은 여는 따옴표와 같은 indent에 쓴다. docstring을 모두 작성하면 따옴표를 닫고, 한 줄을 띄우고 작성한다. 

이외에, `script`, `module`, `package`, `function`, `class`에 따라 참고해야할 사항을 아래에 정리한다.

| 대상 | 참고 사항 |
| --- | --- |
| `script` | - "usage" 메시지를 사용할 수 있어야함. (`--help` 옵션 등을 이용) <br/>- 이러한 Docstring은 script의 기능이나 커맨드 라인 문법, 환경변수, 파일 등을 기술한다. <br/>- "usage"메시지는 새로운 유저가 이해할 수 있을 정도로 가능하면 상세하고 충분하게 작성. |
| `module` | - 외부로 export되는 `class`, `exception`, `function`들의 목록과 각각의 "one-line summary"을 간략하게 기술 |
| `package` | - 외부로 export되는 `module`, `subpackage`들의 목록과 각각의 "one-line summary"을 간략하게 기술 |
| `function` | - 함수의 동작과 arguments, return value(s), side-effects, exceptions, 호출 제약사항을 요약 서술 <br/>- Optional arguments는 표시되어야 함 |
| `class` | - public method와 instance들의 목록에 대한 요약 서술 <br/>- subclass가 있고, 추가된 interface가 있는 경우, 해당 목록에 대해 별도로 표시 <br/> - `__init__` method를 포함한 개별 method들은 각자의 Docstring을 가짐. |

# The Zen of Python

[PEP-20)](https://www.python.org/dev/peps/pep-0020/)에 기록된 파이썬의 디자인 원칙을 나타내는 _한편의 시_ 다.

```
아름다운게 못생긴 것보다 낫다.
분명한게 모호한 것보다 낫다.
간단한게 복잡한 것보다 낫다.
복잡한게 어려운 것보다 낫다.
수평이 계층보다 낫다.
여유로운게 밀집된 것보다 낫다.
가독성은 중요하다.
특별한 경우도 규칙을 어길 정도로 특별하진 않다.
허나 실용성은 순수성에 우선한다.
에러는 결코 조용히 지나가지 말아야한다.
명시적으로 지나가도록 한게 아니라면.
모호성 앞에서 추측하겠다는 유혹을 거부하라.
어떤 일이든 바람직하고 유일하며 명확한 방법이 있다.
비록 처음에는 그대에게 그 길이 명확해보이지 않더라도
지금 하는게 아예 안하는 것보다 낫다.
종종 아예 안하는게 지금 "당장"하는 것보다 나을 때도 있지만.
설명하기 어렵다면, 나쁜 아이디어다.
설명하기 쉽다면, 좋은 아이디어다.
Namespace는 아주 좋은 아이디어다. 막 쓰자.
```  

> **요약** : 가독성은 중요하다.

# PEP-8

PEP-8의 한국어 번역본은 [여기](https://zerosheepmoo.github.io/)에서 확인할 수 있다. 전문을 모두 읽어보는 것이 권장된다.
아래에서는 PEP-8의 내용 중, 중요하다 생각되는 몇가지 항목을 발췌한다.

## Code Layout

### 들여쓰기  

4개의 space로 들여쓰기를 하자.
  
  ```python
  # Correct:

  # 괄호 기준 정렬
  foo = long_function_name(var_one, var_two,
                          var_three, var_four)

  # argument 이후 부분과의 분리를 위해 추가 들여쓰기
  def long_function_name(
          var_one, var_two, var_three,
          var_four):
      print(var_one)

  # indentation level을 하나 더 주기
  foo = long_function_name(
      var_one, var_two,
      var_three, var_four)
  ```

  ```python
  # Wrong:

  # 세로줄 안맞음. -> 첫 번째 줄의 argument 없애야 함.
  foo = long_function_name(var_one, var_two,
      var_three, var_four)

  # 다음 줄과의 가독성이 떨어짐 -> argument 부분에 추가 indentation을 주어야 함.
  def long_function_name(
      var_one, var_two, var_three,
      var_four):
      print(var_one)
  ```

`if`문의 조건 부분이 여러 줄을 할당해야 할만큼 길다면, `if` 두 글자와 띄어쓰기(` `) 하나, 여는 괄호(`(`)를 조합하면 자연스럽게 4-space indent를 만들 수 있다는 점을 염두에 두자. 

  ```python
  # No extra indentation.
  if (this_is_one_thing and
      that_is_another_thing):
      do_something()

  # Add a comment, which will provide some distinction in editors
  # supporting syntax highlighting.
  if (this_is_one_thing and
      that_is_another_thing):
      # Since both conditions are true, we can frobnicate.
      do_something()

  # Add some extra indentation on the conditional continuation line.
  if (this_is_one_thing
          and that_is_another_thing):
      do_something()
  ```

위의 예시에서 두번째와 세번째처럼, 주석이나 extra indentation을 통해 조건문을 다음 코드들과 시각적으로 분리한다. 

`list`나 `function`, `class` 등이 multi-line consturct로 구성되는 경우에는 아래처럼, 닫는 괄호를 리스트의 마지막 줄의 첫번째 글자에 맞추거나 construct의 첫번째 글자에 맞춘다.

  ```python
  my_list = [
      1, 2, 3,
      4, 5, 6,
      ]
  result = some_function_that_takes_arguments(
      'a', 'b', 'c',
      'd', 'e', 'f',
      )
  ```  

  ```python
  # 또는

  my_list = [
      1, 2, 3,
      4, 5, 6,
  ]
  result = some_function_that_takes_arguments(
      'a', 'b', 'c',
      'd', 'e', 'f',
  )
```

### 한 줄의 최대 길이

모든 라인은 79자. docstring은 72자. -> 에디터 이용 편의. 피치못한 경우 백슬래시(`\`)를 이용  
  ```python
  with open('/path/to/some/file/you/want/to/read') as file_1, \
      open('/path/to/some/file/being/written', 'w') as file_2:
      file_2.write(file_1.read())
  ```

### Binary Operator와 line break  

```python
# Wrong:
# operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)

# Correct:
# easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

### Blank Lines

`function` 과 `class`의 정의는 두 줄의 공백으로 구분한다. `class`안의 method는 한 줄의 공백으로 구분한다. 연관된 `function`들끼리 그룹을 나누기 위해 추가적인 공백 라인을 이용할 수도 있다.

### Imports

`import`는 항상 소스 코드의 최상단, Module docstring의 바로 뒤에 작성하며 다음의 순서로 배치하며 1-3번 그룹 사이에는 공백 라인을 추가한다.

1. Standard library imports
2. Related 3rd party imports
3. Local application / library specific imports

라이브러리는 라인을 구분하는 것이 원칙이다.

```python
# Wrong:
import sys, os

# Correct:
import os
import sys

# Correct:
from subprocess import Popen, PIPE
```

코드 가독성과 에러 메시지에서 오류 패키지 위치를 명확하게 알려줄 수 있다는 점에서 Absolute import가 권장된다. 하지만 복잡한 구성의 패키지에서는 꼭 Absolute import를 고수할 필요 없이, Relative import를 이용하는 것도 괜찮다.

```python
from .sibling import example
from foo.bar.myclass import myClass
```

Wildcard(`*`) imports는 namespace에 어떤 name들이 있는지 불명확하게 하므로 사용하지 않도록 한다. 내부 API를 외부에 공개하는 경우에만 제한적으로 이용하자.

```python
# Wrong:
from <module> import *
```

### Pet Peeves (거슬리게 하는 것)

쓸데 없는 whitespace를 피하라. 다음의 상황에서는 공백을 없앤다.

- 괄호 바로 안 공백  
  ```python
  # Correct:
  spam(ham[1], {eggs: 2})

   # Wrong:
  spam( ham[ 1 ], { eggs: 2 } )
  ```

- trailing comma(`,`)와 닫는 괄호 사이 공백
  ```python
  # Correct:
  foo = (0,)

  # Wrong:
  bar = (0, )
  ```

- `,`, `;`, `:` 바로 전 공백
  ```python
  # Correct:
  if x == 4: print(x, y); x, y = y, x

  # Wrong:
  if x == 4 : print(x , y) ; x , y = y , x
  ```
- slice에서 `:`은 binary operator와 같이 작동. `:` 양쪽에는 같은 개수의 공백이 있어야 함.
  ```python
  # Correct:
  ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
  ham[lower:upper], ham[lower:upper:], ham[lower::step]
  ham[lower+offset : upper+offset]
  ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
  ham[lower + offset : upper + offset]

  # Wrong:
  ham[lower + offset:upper + offset]
  ham[1: 9], ham[1 :9], ham[1:9 :3]
  ham[lower : : upper]
  ham[ : upper]
  ```

- 여는 괄호 바로 앞
  ```python
  # Correct:
  spam(1)
  dct['key'] = lst[index]

  # Wrong:
  spam (1)
  dct ['key'] = lst [index]
  ```

- assignment operator 앞, 하나 이상의 공백
  ```python
  # Correct:
  x = 1
  y = 2
  long_variable = 3

  # Wrong:
  x             = 1
  y             = 2
  long_variable = 3
  ```

- 우선도가 다른 operator를 함께 사용하는 경우, low priority operator 좌우에 공백을 하나 추가해서 가독성을 높이는 것은 좋다.
  ```python
  # Correct:
  i = i + 1
  submitted += 1
  x = x*2 - 1
  hypot2 = x*x + y*y
  c = (a+b) * (a-b)

  # Wrong:
  i=i+1
  submitted +=1
  x = x * 2 - 1
  hypot2 = x * x + y * y
  c = (a + b) * (a - b)
  ```

 - 한 라인에 여러 구문을 적는 것은 지양하자.  
  ```python
  # Correct:
  if foo == 'blah':
      do_blah_thing()
  do_one()
  do_two()
  do_three()
  ```
  ```python
  # Wrong:
  if foo == 'blah': do_blah_thing()
  do_one(); do_two(); do_three()
  ```  

### Inline Comments  

무분별한 inline comment 사용은 지양한다.

Inline comment는 코드 구문과 같은 줄에 위치한다. 구문 후에 _최소_ 두 칸을 띄우고 `#`, 그리고 한 칸을 더 띄우고 시작한다.

코드가 명확한 경우에는 inline comment를 쓰지 않는다. 이는 오히려 주의를 산만하게 하기 때문이다. 코드의 의미를 설명할 때는 유용하게 사용 가능하다. 예를들면 아래와 같다.

  ```python
  # Wrong:
  x = x + 1                 # Increment x

  # Correct:
  x = x + 1                 # Compensate for border
  ```
 
## Naming Conventions

### Overriding Principle

공개 API의 일부인 경우, 구현보다는 사용 측면의 convention을 따른다.

### Descriptive Naming Styles  

> Descriptive : 설명, 기술적인, 서술적인 → 표현 방식

다양한 Naming Style이 있다. naming style의 용도보다도, 어떤 것들이 있는지를 파악하고 있으면 도움이 된다.

| 번호 | 예시 | 설명 |
| --- | --- | --- |
| 1 | `b` | Single lowercase letter |
| 2 | `B` | Single uppercase letter |
| 3 | `lowercase` | 소문자만 이용 |
| 4 | `lower_case_with_underscores` | 소문자 + `_`로 Word단위 구분 |
| 5 | `UPPERCASE` | 대문자만 이용 |
| 6 | `UPPER_CASE_WITH_UNDERSCORES` | 대문자 + `_`로 Word단위 구분 |
| 7 | `CapitalizedWords` | 대문자 시작, 대문자로 Word단위 구분 |
| 8 | `mixedCase` | 소문자 시작, 대문자로 Word단위 구분 |
| 9 | `Capitalized_words_With_Underscores` | 대문자 시작, `_`와 대문자로 Word단위 구분 |

아래는 `_`를 이용하는 Special case에 대한 설명이다.

| 번호 | 예시 | 설명 |
| --- | --- | --- |
| 1 | `_single_leading_underscore` | 약한(weak) '내부 사용'을 표시 <br/>eg. `from M import *`는 `_`로 시작하는 이름의 object를 import하지 않음.|
| 2 | `single_trailing_underscore_` | Python keyword와의 충돌을 피하기 위해 이용 <br/>`tkinter.Toplevel(master, class_='ClassName')` |
| 3 | `__double_leading_underscore` | Name Mangling(상속 시에 attribute name 충돌을 피하기 위해 인터프리터가 변수 이름을 자동으로 변경함) |
| 4 | `__double_leading_and_trailing_underscore__` | magic object 또는 dunder object |


### Prescriptive: Naming Conventions

> Prescriptive : 규범적인, 규정하는 → 지켜야하는 방식

#### Names to Avoid

`I`, `l`, `O`를 Single character로 표기하지 않는다.

#### Package and Module Names

`package`와 `module`은 짧은 소문자의 이름을 가져야 한다. `Module`은 필요하다면 `_`도 이용 가능하지만, `Package`에서는 사용을 지양한다.

> `module`은 특정 목적을 위해 `object`들을 모아놓은 `.py` 파일이다.
> `package`는 `module`을 모아놓은 namespace이다.
> ```
> Student(Package)
> ├─ __init__.py (Constructor)
> ├─ details.py (Module)
> ├─ marks.py (Module)
> └─ collegeDetails.py (Module)
> ```

#### Class Names

`class` 이름은 일반적으로 `CapitalizedWords` Convention을 사용한다.

인터페이스가 문서화되고 주로 callable으로 사용되는 경우, `function`의 Convention을 따를 수 있다.

#### Type Variable Names

[PEP 484](https://www.python.org/dev/peps/pep-0484)에서 Type Hinting을 다루고 있다. Type variable은 짧은 이름의 `CapitalizedWords`를 이용한다.

covariant 또는 contravairant로 사용하는 경우, 아래와 같이 `_co`, `_contra`를 붙이는 것을 권장한다.

```python
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

#### Exception Names

`exception`은 `class`여야하기 때문에, `class`의 Convention을 따른다. Exception name 뒤에 "Error"를 붙여 `exception`임을 명시하자.

#### Global Variable Names

Convention과는 크게 관련있지 않지만, Global variable은 하나의 모듈 안에서만 이용하길 바란다. `function`의 convention을 따른다.

Wildcard(`*`)를 통해 `from M import *`로 쓰이도록 만든 `module`은 `__all__` 메커니즘을 이용해서 `_`으로 시작하는 "non-public" object를 export하지 않도록 한다.

#### Function and Variable Names

`variable`과 `function` name은 모두 소문자로 적고, `_`를 이용해서 단어를 구분한다.

`mixedCase`(camelCase)는 기존에 이미 그렇게 쓰인 라이브러리에 적용해도 좋다.

#### Function and Method Arguments

instance method 앞에는 항상 `self`를 붙인다.

class method 앞에는 항상 `cls`를 붙인다.

`function`의 인자가 예약 keyword와 충돌한다면, `_`를 해당 인자의 이름 뒤에 붙여주는 것이 좋다. 예를 들어, `class_`가 `clss`보다 낫다.

#### Method Names and Instance Variables

소문자만 사용하고, `_`로 단어를 구분하는 `function`의 Convention을 따른다. 

non-public method는 `_`로 시작한다.

subclass와 이름이 충돌하는 것을 막기 위해 `__`를 이름 앞에 붙여서 파이썬의 name mangling rule을 이용한다.

> **Mangling Rule**  
> 파이썬은 `__`가 이름 앞에 붙은 attribute를 발견하면 이름을 변환한다. 예를들어, class `Foo`가 `__a`라는 attribute를 가진다고 하자. 우리는 `__a`에 접근할 때, `Foo.__a`를 사용하는게 아니라 `Foo._Foo__a`를 이용해야 한다.  

일반적으로, 이름 앞에 붙는 `__`는 subclass의 attribute와 충돌을 막을 목적으로만 사용해야한다. 

#### Constants

상수는 보통 module level에서 정의되며 대문자로만 사용하여 `_`로 단어를 구분한다. _ex) `MAX_OVERFLOW`, `TOTAL`_

#### Designing for Inheritance

`class`의 method와 instance variable을 만들 때는 항상 public인지 non-bulic인지 정해야 한다. 만약 잘 모르겠다면, non-public을 선택하자. non-public을 public으로 만드는게 public을 non-public으로 만드는 것보다 쉽다.

public attribute는 class 내부 로직을 모르는 사람이 사용할 것으로 기대되는 attribute이고, 이전 버전과 호환되지 않는 수정 사항을 피하기 위한 방법이다. non-public attribute는 3rd-party에서 사용되지 않을 의도로 만들며, 개발자는 해당 attribute가 수정되지 않거나 없어지지 않을거라고 보장하지 않는다.

파이썬에서는 진정한 의미의 private attribute는 없으므로 "private"이라는 용어를 쓰지 않는다.

다른 언어에서 "protected"라고 불리는 subclass API를 살펴보자. 어떤 `class`는 상속을 통해 용도를 확장하거나 수정하도록 설계된다. 이러한 `class`를 디자인할 때에는 attribute의 용도에 대해 명확하고 명시적인 결정을 내려야 한다.

- public attribute의 이름 앞에는 `_`를 붙이지 않는다.
- public attribute의 이름이 파이썬 예약어와 충돌한다면, 이름의 뒤에 `_`를 붙인다.
- 간단한 public data attribute라면, 복잡한 accessor/mutator를 만드는 것 보다는 그냥 attribute name을 노출하는 것이 낫다. 간단한 attribute data지만, functional한 동작이 필요하다면 [`property`](https://docs.python.org/3/library/functions.html#property)를 이용하자.
  - attribute의 functional 동작이 side-effect를 발생시키지 않도록 주의하자
  - 연산량이 많은 경우 property를 쓰지 말자.
- subclass에서 사용될 목적으로 `class`를 만들면서, 특정 attribute는 subclass에서 이용되지 않길 바란다면, `__`로 시작하는 이름을 붙인다. (mangling)
  - 만약 subclass에서도 같은 class 이름과 attribute 이름을 사용한다면, 여전히 충돌 가능성이 있음에 유의하자.
  - name mangling은 debugging과 `__getattr__()`를 쓸 때 조금 불편할 수 있다.
  - 모두가 name mangling을 좋아하지는 않을 수 있다.

#### Public and Internal Interfaces

모든 하위 호환성 보장은 public 인터페이스에만 적용된다. 따라서, 사용자가 public과 내부 인터페이스를 명확하게 구별할 수 있도록 하는 것은 중요하다.

Documentation에서 따로 명시하지 않는 한, 문서화된 인터페이스는 public으로 간주된다. 그리고 문서화되지 않았다면, 모두 내부 인터페이스로 본다.

더 친절하게, `module` 내에서 `__all__`을 이용하여 public API의 list를 명시할 수 있다. 만약, `__all__`이 빈 list라면, 해당 모듈에는 public API가 없는 것이다.

```python
__all__ = ['Foo', 'Bar']
```

`__all__`으로 public API를 명시했더라도, 여전히 내부 인터페이스의 이름에는 `_`를 앞에 붙여야 한다.

내부적인 namespace를 이용하는 인터페이스가 있다면 이 인터페이스 또한 내부 인터페이스로 취급한다.

### Programming Recommendations

파이썬에는 비슷한 동작을 하는 몇가지 방법들이 있다. 아래는 그 중, 권장되는 방법을 소개한다.

- string을 연결할 때 `a += b`, `a = a + b`보다는 `''.join([a, b])`를 이용한다. `a`와 `b`가 string이며, linear time에 수행됨을 명확히 할 수 있다.
- `None`과 같은 singleton(유일한 객체)와의 비교에는 항상 `is` 또는 `is not`을 이용한다. `==`는 쓰지 않는다.
- `if x is not None`을 쓰려고 할 때, `if x`로 쓰는 것은 주의하자. `x`에 `None`이 아닌 객체가 들어올 때, if문이 false가 될 수 있는 경우가 있다.
- `not ... is` 보다는 `is not`을 쓴다.
  ```python
  # Correct:
  if foo is not None:
  ```   
  ```python
  # Wrong:
  if not foo is None:
  ```
- 다양한 비교를 위해 ordering operation을 구현할 때에는 모든 비교 연산자(`__eq__`, `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__`)를 모두 구현해주는 것이 가장 좋다. 품을 줄이기 위해서 [`functools.total_ordering()`](https://docs.python.org/ko/3/library/functools.html#functools.total_ordering) decorator를 이용하는 것도 좋다. `__eq__`와 나머지 비교 연산자 중 하나만 작성하면 나머지는 자동으로 작성된다.
- `lambda`를 identifier에 바로 bind해서 쓰지 말자. 
  ```python
  # Correct:
  def f(x): return 2*x

  # Wrong:
  f = lambda x: 2*x
  ```
  전자는 tracebacks에 유리하고, 후자는 "할당"을 통해 labmda의 장점이 모두 사라진다.
- `BaseException`보다는 `Exception`을 상속하여 예외를 구현한다. BaseException를 직접 상송하는 것은 예외가 catch되는 것이 거의 대부분 잘못된 경우에 사용한다.
  예외 처리에는 "문제가 생겼다." 보다는 "무엇이 잘못되었는가?"에 초점을 두자.
- 예외의 연결을 정확하게 쓰자. `raise X from Y`를 통해 original traceback을 유지할 수 있다.
- 예외 처리에서는 모든 exception을 잡는 것 보다는 가능하면 상세한 exception을 명시한다.
  ```python
  try:
      import platform_specific_module
  except ImportError:
      platform_specific_module = None
  ```
- `try/except` 구문에서 `try`에 들어가는 코드는 에러가 발생 가능한 최소한으로 제한한다.
  ```python
  # Correct:
  try:
      value = collection[key]
  except KeyError:
      return key_not_found(key)
  else:
      return handle_value(value)

  # Wrong:
  try:
      # Too broad!
      return handle_value(collection[key])
  except KeyError:
      # Will also catch KeyError raised by handle_value()
      return key_not_found(key)
  ```
- 코드의 일부에서 local resource를 써야하는 경우 `with` 문을 이용해서 사용한 후 clean up 하도록 한다.
- `function` 또는 `method`가 특정 resource를 이용하는 경우에는 Context manager를 이용한다.
  ```python
  # Correct:
  with conn.begin_transaction():
      do_stuff_in_transaction(conn)
  ```
  ```python
  # Wrong:
  with conn:
      do_stuff_in_transaction(conn)
  ```
  위 예시의 후자에서는 `__enter__`와 `__exit__`에 대한 아무런 정보를 주지 않고 사용 후에 connection을 종료해버린다. 명시적인 것이 중요하다.
- 일관성을 지키자. return 구문에서도 일관성이 중요하다. 뭔가를 return 하는 구문이 있다면, 아무것도 return 하지 않는 구문도 명시적으로 `return None`을 쓰자.
  ```python
  # Correct:

  def foo(x):
      if x >= 0:
          return math.sqrt(x)
      else:
          return None

  def bar(x):
      if x < 0:
          return None
      return math.sqrt(x)
  ```  
  ```python
  # Wrong:

  def foo(x):
      if x >= 0:
          return math.sqrt(x)

  def bar(x):
      if x < 0:
          return
      return math.sqrt(x)
  ```
- str의 앞, 뒤를 slicing할 때는 `''.startswith()` 또는 `''.endswith()`를 쓰자.
  ```python
  # Correct:
  if foo.startswith('bar'):
  ```  
  ```python
  # Wrong:
  if foo[:3] == 'bar':
  ```
- object의 타입 비교는 항상 `isinstance()`를 쓴다.
  ```python
  # Correct:
  if isinstance(obj, int):
  ```
  ```
  # Wrong:
  if type(obj) is type(1):
  ```
- 빈 sequence(string, list, tuple)은 `false`라는 사실을 이용하자.
  ```python
  # Correct:
  if not seq:
  if seq:
  ```  
  ```python
  # Wrong:
  if len(seq):
  if not len(seq):
  ```
- `boolean`을 `==`으로 비교하지 말자.
  ```python
  # Correct:
  if greeting:

  # Wrong:
  if greeting == True:
  if greeting is True:
  ```
- `try/fianlly` 구문 안에 flow control(`return`, `break`, `continue`)를 넣지 말자.
  ```python
  # Wrong:
  def foo():
      try:
          1 / 0
      finally:
          return 42
  ```

#### Function Annotations

- [PEP-484](https://www.python.org/dev/peps/pep-0484/)의 type hinting을 사용한다. _type hinting은 비교적 최근에 채택되었으며, 파이썬의 Standard library도 점진적으로 수용중인 표준이다._
- `.py`의 상단에 다음의 코드를 작성하면, `type checker`가 function annotation을 무시하도록 설정할 수 있다.
  ```python
  # type: ignore
  ```
- `linter`와 같이 type checker는 option이며 별개의 도구다. 파이썬 인터프리터는 type checking 오류에 대한 메시지를 출력하거나, annotation에 의해 동작을 변경하지 않는다.
- type checker의 이용은 자유이지만, 코드 완성도를 높이는데 도움이 된다.

#### Variable Annotations

[PEP-526](https://www.python.org/dev/peps/pep-0526/)에서 variable annotation을 소개한다. Style은 function annotation과 비슷하다.

- `module`, `class`, `variable`의 annotation은 `:` 후에 한 칸을 띄운다.
- `:` 전에 띄우지 않는다.
- 오른쪽에 assignment(`=`)가 있다면, `=`의 양 옆은 한 칸씩 띄운다.
  ```python
  # Correct:

  code: int

  class Point:
      coords: Tuple[int, int]
      label: str = '<unknown>'
  ```
  ```python
  # Wrong:

  code:int  # No space after colon
  code : int  # Space before colon

  class Test:
      result: int=0  # No spaces around equality sign
  ```
