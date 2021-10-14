---
layout: post
title: Slidev, 흥미로운 개발자들의 ppt 도구
subtitle: open source
tags: [STUDY, DEVELOP]
cover-img: /assets/img/slidev.png
comments: true
---

출근하고 커피 한 잔을 손에 쥐고 제일 처음 하는 일은 Github에서 follow 중인 개발 능력자분들이 어제 하루동안 어떤 repository에 ⭐를 찍었는지 feed를 확인하는 일입니다. 몇 주 전에 feed에서 [slidev](https://sli.dev/)라는 오픈 소스를 발견하고 나서, 오늘 조금 남는 시간이 생겨 이 프로젝트를 유심히 들어다봤습니다. 이 프로젝트는 ppt를 작성하는 새로운 도구를 제공하는데, 여러 장점들이 있었습니다.

1. 익숙한 Markdown 기반 slide 작성
2. 손쉬운 코드 하이라이팅
3. html 태그를 이용한 element 조작
4. docker를 통한 손쉬운 설치
5. 작성한 ppt의 웹 게시 가능 (static host)
6. pdf, png exporting
7. 프레젠테이션 모드
8. 프레젠테이션 녹화
9. Drawing & Annotation


이렇게나 장점이 많은 프로젝트에 감탄하며 최근 회사에서 진행했던 세미나에서 slidev를 이용해보았고, 꽤 멋진 슬라이드를 간단히 제작할 수 있었습니다. 브라우저로 로컬 호스트에 접속하는 것으로 발표를 시작하는 것은 확실이 특별한 느낌이었습니다.

종종 free-format slide를 작성할 때가 있는데, 다음번에도 이용해보려는 마음으로 slidev의 기본적인 요소들을 기록해봅니다.

| 공식 tutorial | [https://sli.dev/guide/](https://sli.dev/guide/) |
| stackblitz에서 직접 해보기 | [![](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://sli.dev/new) |

<br/>

# 📚 Layout

각 슬라이드는 layout을 지정해주는 것으로 나뉩니다. 아래와 같이 `layout` 속성을 먼저 지정해주고, 각 속성마다 필요한 추가 정보를 기입해줍니다.
```
---
layout: cover

# random bg image
background: https://source.unsplash.com/collection/94734566/1920x1080
---
```

자주 이용했던 layout 속성을 아래 적어둡니다. 전체 목록은 링크에서 확인할 수 있습니다.

_참조 : [https://sli.dev/builtin/layouts.html](https://sli.dev/builtin/layouts.html)_

| layout | description |
| --- | --- |
| cover | 이미지를 포함한 cover page |
| intro | presentation 정보 |
| default | 기본 레이아웃 |
| center | content를 화면 중앙에 배치 |
| fact | 강조 |
| full | 좌우상하 여백 제거 |
| two-cols | 컨텐츠를 좌우로 나눔 |
| image-right | 우측 이미지 배치 |
| image | 이미지를 메인 컨텐츠로 배치 |
| end | ppt 종료 |

<br/>

# 🎓 수식 적용

LaTeX 문법과 거의 똑같은 KaTeX 문법을 지원합니다. 

`slide.md`가 있는 디렉토리에 `./setup/katex.ts` 파일을 만들고 아래 내용을 입력합니다.

```ts
import { defineKatexSetup } from '@slidev/types'

export default defineKatexSetup(() => {
  return {
    /* ... */
  }
})
```

`slide.md`에서 아래와 같이 `$`를 이용하여 수식을 입력 및 랜더링 할 수 있습니다.

```md
inline: $x_{t+1} = x_{t} - \eta_{t} \bigtriangledown f_{t}(x_{t})$
block:
    $$x_{t+1} = x_{t} - \eta_{t} \bigtriangledown f_{t}(x_{t})$$
```

_참조: [https://sli.dev/custom/config-katex.html](https://sli.dev/custom/config-katex.html)_

<br/>

# 🎨 이미지 크기 조정

아직 완성되지 않은 프로젝트라 그런지 이미지의 크기가 너무 크거나 작게 표현되는 경우가 잦았습니다. 그럴때는 보통 markdown에서 이미지를 입력하는 `![image-name](image-src)` 대신 html 태그를 직접 입력하여, `<img src="image-src" height="400" width="400">`와 같이 크기를 조절해줄 수 있습니다. 원래 markdown이 우리가 자연어에 가깝게 입력하면 이것을 html으로 변환해주는 원리이기 때문에, markdown에서 안될땐 html을 직접 쓰면 거의 대부분의 문제가 해결됩니다😉

<br/>

# 🎠 애니메이션 효과

사실 `Power Point`나 `Keynote`같은 도구에서 지원하는 애니메이션 효과는 넘사벽입니다. 화려한 효과가 꼭 필요하다면 `slidev`보다는 앞서 언급한 도구를 이용하는 것이 좋습니다. 물론 `slidev`에서 html을 직접 작성하여 날아오기 등의 몇가지 효과를 이용할 수는 있나, 크게 의미 없는 수준이라고 생각합니다.

그래도 컨텐츠 강조나 발표 구성을 위해 **나타나기**나 **숨기기** 정도의 효과는 손쉽게 이용할 수 있습니다. 여기에는 `v-click`, `v-after`, `v-click-hide`등의 태그를 이용합니다.

```html
<v-click>
    Hello-world
</v-click>

<img v-click src="image-src" height="400" width="400">
```
위처럼 작성된 slide에서는 첫 클릭시 hello-world, 두 번째 클릭 시 `img`가 나타나게 됩니다.

이 정도의 간단한 지식만으로도 깔끔한 발표자료를 만들 수 있습니다. 