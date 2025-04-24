---
title: "[vue] Options API vs Composition API"
date: "2024-04-11"
category: "vue"
tags: ["vue", "options api", "composition api"]
---

# [vue] Options API vs Composition API

Vue.js 개발 스타일에는 Options API와 Composition API 두 가지 방식이 있다.

<br />

## Table of Contents

- [Options API](#options-api)
- [Composition API](#composition-api)
- [Options API vs Composition API](#options-api-vs-composition-api)

<br />

## Options API

data, methods 및 mounted 같은 객체를 사용하여 컴포넌트 로직을 정의하는 개발 스타일.

Options API에서 옵션으로 정의된 속성은 컴포넌트 인스턴스를 가리키는 함수 내부의 this에 노출된다.

그래서 this.데이터이름 this.함수이름 이런 식으로 네이밍 접근이 가능하다.

### data

Data 메서드는 해당 컴포넌트에서 사용될 state. 즉, 데이터들을 관리해주는 곳이다.

data에서 반환된 속성들은 반응적인 상태가 되어 this에 노출된다.

### methods

Methods는 속성 값을 변경하고 업데이트를 할 수 있는 함수이며, 템플릿 내에서 이벤트 핸들러로 바인딩이 가능하다.

Methods에서 반환된 함수들은 data에서 반환된 속성과 마찬가지로 this에 노출된다.

### LifeCycle

생멍주기 훅(Lifecycle hooks)은 컴포넌트 생명주기의 여러 단계에서 호출한다.

<br />

## Composition API

import 키워드를 통해서 가져온 Vue.js 내장 API 함수 혹은 속성들을 사용하여 컴포넌트 로직을 정의하는 개발 스타일.

SFC에서 컴포지션 API는 일반적으로 \<script setup>\</script>과 함께 사용한다.

> setup 속성은 컴파일 시 의도된 대로 올바르게 동작할 수 있게 코드를 변환하도록 하는 힌트이다.

> 즉, 컴포지션 API를 사용한다고 선언 및 정의를 해주기 위해 사용되는 키워드가 setup이라고 보면된다.

### ref, reactive

Composition API에서는 반응성 있는 데이터를 만들어줄 경우, ref 또는 reactive 키워드를 통하여 변수를 선언해준다.

```javascript
const count = ref(0); // 초기값을 0으로 설정
const obj = reactive({ name: "test", age: 30 });
```

### methods

Composition API에서는 methods라는 객체를 선언할 필요가 없기 때문에 함수를 그냥 만들어 사용하면 된다.

```javascript
// ref로 참조한 데이터에 접근 할 경우에는 value로 접근한다.
function increment() {
  count.value++;
}
```

### LifeCycle

생멍주기 훅(Lifecycle hooks)은 컴포넌트 생명주기의 여러 단계에서 호출한다.

<br />

## Options API vs Composition API

어떤 개발 스타일이 더 좋고 나쁘고는 없다.

본인 취향에 맞게 개발하면 된다.

협업에 있어는 Options API의 코드가 가독성이 좋을 경우도 있었기 떄문에 맡은 프로젝트에 따라 선택하면 된다.
