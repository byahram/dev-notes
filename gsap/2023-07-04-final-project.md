---
title: "[범쌤] Part.01 GSAP Basic - Final Project"
date: "2023-07-04"
category: "gsap"
tags: ["gsap", "basic"]
---

# [범쌤] Part.01 GSAP Basic - Final Project

- <https://gsap.com/docs/v3/>
- [인프런 강의](https://www.inflearn.com/course/%EC%9B%B9-%EC%95%A0%EB%8B%88%EB%A7%A4%EC%9D%B4%EC%85%98-gsap-1/dashboard)

<br>

## 프로젝트 리뷰

- HTML Structure
- Basic Animation
- Timeline Defaults
- GSDevTools
- Fine-turning
- Un-styled Content

### Final Project(Start)

#### <https://codepen.io/kindtigerr/pen/VwQoVvY>

### Final Project(Finished)

#### <https://codepen.io/byahram/pen/KKroVOg>

<br>

## 기초 애니메이션

```javascript
// 애니메이션 시퀀스를 설정
const tl = gsap.timeline();

tl.from(".stage", { opacity: 0 })
  .from("h1", { x: -50, opacity: 0 })
  .from("h2", { x: 50, opacity: 0 })
  .from("p", { x: -50, opacity: 0 })
  .from("button", { y: 30, opacity: 0 })
  .from(".planet > img", { scale: 0, stagger: 0.1, opacity: 0 });
```

<br>

## 타임라인 기본값 설정

#### 👉 참고 문서

- <https://greensock.com/docs/v3/GSAP/gsap.defaults()>
- <https://greensock.com/docs/v3/GSAP/gsap.config()>

```javascript
// 타임라인 기본값을 사용하면 타임라인의 모든 트윈에 동일한 속성 값을 적용할 수 있다.
const tl = gsap.timeline({ defaults: { opacity: 0, ease: "back" } });

tl.from(".stage", { ease: "linear" })
  .from("h1", { x: -50 })
  .from("h2", { x: 50 })
  .from("p", { x: -50 })
  .from("button", { y: 30 })
  .from(".planet > img", { scale: 0, stagger: 0.1 });
```

<br>

## GSDevTools

#### 👉 참고 문서 : <https://greensock.com/gsdevtools?ref=6234>

GSDevTools은 애니메이션을 scrub하고 속도를 변경해볼 수 있으며, 특정 구간 반복등 애니메이션 작업에 있어 굉장히 편리한 컨트롤러를 제공한다. CodePen에서는 무료로 사용할 수 있다.

```javascript
GSDevTools.create();
```

<br>

## FOUC

#### 👉 참고 문서

- <https://ko.wikipedia.org/wiki/FOUC>
- <https://developer.mozilla.org/ko/docs/Web/CSS/visibility>

Flash of Un-styled Content (FOUC) 는 스타일이 지정되어 있지 않은 요소들이 화면에 랜더링 될 경우 콘텐츠의 깜빡이는 플래시 효과를 나타내는 용어이다.

예를 들어 웹폰트가 로드되기 전에 페이지 렌더링 상태에서 기본 글꼴이 나오고 적용된 웹폰트로 변경되는 것이다.

사용자에게 가장 빠른 로딩 경험을 제공 하기 위해 \<body> 태그 끝나기 전에 사용자 정의 스크립트를 로드하는 것이 좋고 또는 defer 명령어를 사용하는 것도 좋은 방법중 하나이다.

또한, GSAP과 Javascript를 사용하여 FOUC를 제거할 수 있다.

**FOUC를 피하기 위한 단계별 수행 항목**

1. 보이지 않아야 할 요소를 감싸고 있는 전체 부모 요소에게 CSS visibility: hidden 속성 부여하기
2. GSAP의 Special 속성인 autoAlpha : 0 설정
3. 애니메이션 코드를 init() 함수로 래핑
4. 로드 이벤트 리스너를 사용하여 페이지가 로드 된 후 init() 함수를 호출

<br>
