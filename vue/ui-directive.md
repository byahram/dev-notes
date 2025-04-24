---
title: "[vue] UserInterface v-디렉티브"
date: "2024-04-11"
category: "vue"
tags: ["vue", "ui directive"]
---

# [vue] UserInterface v-디렉티브

## Table of Contents

- [1. 선언적 렌더링](#1-선언적-렌더링)
- [2. Class와 Style 바인딩](#2-class와-style-바인딩)
- [3. 조건부 렌더링](#3-조건부-렌더링)
- [4. 리스트 렌더링](#4-리스트-렌더링)

<br />

## 1. 선언적 렌더링

Vue.js 데이터바인딩의 가장 기본이 되는 건 선언적 렌더링이다.

데이터 바인딩의 가장 기본적인 형태는 이중 중괄호 {{ }} 문법을 사용한 텍스트 보관법이다.

이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석한다.

실제 HTML을 출력하려면 v-html 디렉티브를 사용한다.

```html
<template>
  <div>{{ rawHtml }}</div>
  <h1 v-html="rawHtml2"></h1>
</template>

<script>
  export default {
    data() {
      return {
        rawHtml: "이것은 텍스트입니다.",
        rawHtml2: '<span style="color: red">이것은 빨간색입니다.</span>',
      };
    },
  };
</script>
```

<br />

## 2. Class와 Style 바인딩

### 1) Class 바인딩

HTML 클래스 바인딩을 하기 위해선 v-bind:class (축약형- :class)를 사용한다.

객체로 바인딩 되며 클래스를 동적으로 토글하기 위해 객체를 :class에 전달할 수 있다.

```html
<template>
  <div v-bind:class="{ active: isActive }">클래스 바인딩 테스트입니다.</div>
  <div :class="{ active: isActive }">클래스 바인딩 테스트 입니다.</div>
  <!-- isActive는 state 영역(data 영역)에 선언한 임의의 변수이며 이 변수는 true/false 값을 가진다. -->

  <button @click="change">버튼</button>
</template>

<script>
  export default {
      data() {
          return {
              isActive: true,
          }
      },
      methods: {
          change() {
              this.isActive = !this.isActive,
          }.
      }
  }
</script>

<style scoped>
  h2.active {
    color: green;
  }
</style>
```

### 2) Style 바인딩

:style은 HTML Element의 style 속성에 해당하는 자바스크립트 객체에 대해 바인딩을 지원한다.

```html
<template>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  <!-- 스타일 바인딩에서 해당 프로퍼티 속성은 카멜 케이스로 작성한다. -->

  <h3 style="color: red; font-size: 24px;">스타일 바인딩 테스트입니다.</h3>
  <h3 :style="{ color: fontColor, fontSize: fontSize + 'px'}">
    스타일 바인딩 테스트입니다.
  </h3>
</template>

<script>
  export default {
    data() {
      return {
        fontColor: "#888888",
        fontSize: 48,
      };
    },
  };
</script>
```

<br />

## 3. 조건부 렌더링

- v-show 디렉티브는 v-if와 사용법이 크게 다르지 않다.

- 대체로 동일하지만 v-if와의 차이점은 v-show가 있는 엘리먼트는 항상 렌더링 되어 DOM에 남아있다는 것이다.

- v-show는 엘리먼트의 display CSS 속성만 전환이 된다.

- 일반적으로 v-if는 전환 비용이 더 높고, v-show는 초기 렌더링 비용이 더 높다.

- 따라서 실행 중에 조건이 변경되지 않을 경우에는 v-if를 사용하는 것이 좋다.

### v-if / v-else-if / v-else

### v-show

<br />

## 4. 리스트 렌더링

리스트 렌더링이란 배열 데이터를 기반으로 동일한 구조의 UI를 반복호출하는 기능을 말한다.

리스트 렌더링은 v-for 디렉티브를 사용한다.

- v-for 디렉티브를 사용하여 배열을 리스트로 렌더링한다.
- v-for 디렉티브는 item in items 형식의 특별한 문법이 필요하다
  - item: 배열 내 반복되는 엘리먼트의 별칭 / items: 선언한 배열 데이터
- v-for를 객체의 속성을 반복하는 데 사용 가능하다.
- 순회 순서: 해당 객체를 Object.keys()를 호출한 결과에 기반

```javascript
const items = ref([{ message: 'Foo' }, { message: 'Bar' }]);

<li v-for="item in items">{{item.message }}</li>
```

```html
<template>
  <div>
    <li v-for="item in sampleArray">{{ item }}</li>
    <li v-for="user in otherArray">{{ user.id }} / {{ user.name }}</li>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        sampleArray: ["a", "b", "c", "d"],
        otherArray: [
          {
            id: 0,
            name: "John",
          },
          {
            id: 1,
            name: "Kim",
          },
          {
            id: 2,
            name: "Lee",
          },
          {
            id: 3,
            name: "Park",
          },
        ],
      };
    },
  };
</script>
```
