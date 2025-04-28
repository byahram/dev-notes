---
title: "[드림코딩] JS 강의"
date: "2022-09-30"
category: "javascript"
tags: ["javascript", "드림코딩"]
---

# [드림코딩] JS 강의

## Table of Contents

- [자바스크립트 입문 '기초' 강의](#자바스크립트-입문-기초-강의)
- [자바스크립트 입문 '기본' 강의](#자바스크립트-입문-기본-강의)

<br>

## 자바스크립트 입문 '기초' 강의

### 01. 자바스크립트의 역사와 현재 그리고 미래 (JavaScript, ECMAScript, JQuery, Babel, Node.js)

<https://www.youtube.com/watch?v=wcsVjmHrUQg&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2>

<https://github.com/dream-ellie/learn-javascript>

### 02. 콘솔에 출력, script async와 defer의 차이점

#### 자바스크립트 공식 사이트

- <https://developer.mozilla.org/ko/>
- <https://www.ecma-international.org/>

#### script async와 defer의 차이

- async
  - 스크립트를 다운로드 하는 동안 문서(HTML)이 중단되지 않는다.
  - 스크립트를 다운로드 됐을 때 곧바로 평가가 실행한다.
  - 먼저 다운로드 된 순서대로 실행한다.
- defer
  - 스크립트를 다운로드하는 동안 문서(HTML)이 중단되지 않는다.
  - 문서(HTML)을 완전히 다 읽은 후에 실행한다.
  - 정의된 순서대로 실행된다.
- 'use strict' : added in ES5, use this for Vanilla JavaScript

### 03. data types, let vs var, hosting

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/variable.js>

### 04. operator, if for loop 코드리뷰 팁

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/operator.js>

### 05. Arrow Function은 무엇인가? 함수의 선언과 표현

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/function.js>

#### TypeScript vs JavaScript 비교 사이트

<https://www.typescriptlang.org/play?#code/GYVwdgxgLglg9mABAGzgcwBQFsCmBnPAQzRwEpEBvAKEVsQgTzmRwDpVNcDiyBuKgL5A>

### 06. 클래스와 오브젝트의 차이점(class vs object), 객체지향 언어 클래스 정리

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/class.js>

#### javaScript 참고서

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference>

#### CLASS vs OBJECT

- Class
  - template
  - declare once
  - no data in
  - 붕어빵을 만들 수 있는
- Object
  - instance of a class
  - created many times
  - data in
  - 팥 붕어빵, 크림 붕어빵

### 07. Object 넌 뭐니?

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/object.js>

#### javaScript Object 참고

<https://developer.mozilla.org/en-US/docs/web/javascript/reference/global_objects/object>

### 08. JavaScript Array 개념과 APIs 총정리

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/variable.js>

### 09. 유용한 10가지 배열 함수들. Array APIs 총정리

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/array-api.js>

### 10. JSON 개념 정리와 활용 방법 및 유용한 사이트 공유

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/json.js>

#### JSON (JavaScript Object Notation)

- Object { Key : value }
- simplest data interchange format
- lightweight text-based structure
- easy to read
- key-value pairs
- used for serialization and transmission of data between the network the network connection
- independent programming language and platform

#### ETC

- HTTP (Hypertext Transfer Protocal)
- AJAX (Asynchronous JavaScript And XML)
- XHR (XMLHttpRequest)
- XML

#### 유용한 사이트

- <https://www.jsondiff.com/>
- <https://jsonbeautifier.org/>
- <https://jsonparser.org/>
- <https://tools.learningcontainer.com/json-validator/>

### 11. 비동기 처리의 시작 콜백 이해하기 JavaScript Callback

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/async/callback.js>

### 12. 프로미스 개념부터 활용까지 JavaScript Promise

**Promise** - JavaScript에서 제공하는 비동기를 간편하게 처리할 수 있도록 도와주는 Object

1. <https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/async/promise.js>

2. <https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/async/callback-to-promise.js>

### 13. 비동기의 꽃 JavaScript async와 await 그리고 유용한 Promise APIs

<https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/async/async.js>

<br>

## 자바스크립트 입문 '기본' 강의

### 00. 자바스크립트 함수 기본편

```javascript
// 함수 선언
function doSomething() {
  console.log("hello");
}

function doSomething(add) {
  console.log(add);
  const result = add(2, 3);
  console.log(result); // 5
}

function add(a, b) {
  const sum = a + b;
  return sum;
}
```

```javascript
// 함수 호출
doSomething(); // hello
doSomething(add);
/**add(a, b) {
 *      const sum = a + b;
 *      return sum;
 * }
 */
doSomething(add()); // NaN

const result = add(1, 2);
console.log(result); // 3
```

### 01. 변수 | primitive 타입과 object의 차이점

#### primitive

```javascript
// primitive - number, string, boolean, null, undefined
let number = 2;
let num = "2";
console.log(number);
console.log(number2);

number2 = 3;
console.log(number);
console.log(number2);
```

#### object

```javascript
//object
let obj = {
  // 새 할당 X
  name: "ellie",
  age: 5,
};
console.log(obj.name); // ellie

let obj2 = obj;
console.log(obj2.name); // ellie

obj.name = "james";
console.log("-----");
console.log(obj.name); // james
console.log(obj2.name); // james
```

### 02. 함수 | 함수 정의, 호출, 그리고 콜백함수

```javascript
const num = 1;
const num2 = 2;
const result = num + num2;
console.log(result); // 3

const num3 = 1;
const num4 = 2;
const result2 = num3 + num4; // 지저분하다 -> 함수생

// 함수
function add(a, b) {
  return a + b;
}

const sum = add(3, 4);
console.log(sum); // 7
```

```javascript
function add(num1, num2) {
  return num1 + num2;
}

const doSomething = add;
const result = doSomething(2, 3);
console.log(result); // 5
const result2 = add(2, 3);
console.log(result2); // 5
```

```javascript
function add(num1, num2) {
  return num1 + num2;
}

function surprise(operator) {
  const result = operator(2, 3); // add(2, 3); 같음
  console.log(result);
}
surprise(add); // 5
```

```javascript
function divide(num1, num2) {
  return num1 / num2;
}
function surprise(operator) {
  const result = operator(2, 3); // divide(2, 3); 같음
  console.log(result);
}
surprise(divide);
```

### 03. 연산자 | boolean의 모든것 && 연산자

```javascript
// false : 0, -0, '', undefined, NaN, null
// true : -1, 'hello'

if (true) {
  console.log("true!"); // true!
} else {
  console.log("false!");
}

let num; // undefined
if (num) {
  console.log("true!");
} else {
  console.log("false!"); // false!
}
```

### 04. 클래스 | 클래스 예제와 콜백함수 최종 정리

```javascript
class Counter {
  constructor() {
    this.counter = 0;
  }

  increase() {
    this.counter++;
    console.log(this.counter);

    if (this.counter % 5 === 0) {
      console.log("yo");
    }
  }
}

const coolCounter = new Counter();
coolCounter.increase(); // 1
coolCounter.increase(); // 2
coolCounter.increase(); // 3
coolCounter.increase(); // 4
coolCounter.increase(); // 5 yo
```

```javascript
class Counter {
    constructor() {
        this.counter = 0;
    }

    increase(runIf5Times) {
        this.counter++;
        console.log(this.counter);

        if(this.counter % 5 === 0) {
            runIf5Times(this.counter);
        }
    }
}

function coolCounter = new Counter();

function printSomething(num) {
    console.log(`yo! ${num}`);
}
function alertNum(num) {
    alert(`Wow! ${num}`);
}

coolCounter.increase(printSomething);        // 1
coolCounter.increase(printSomething);        // 2
coolCounter.increase(printSomething);        // 3
coolCounter.increase(printSomething);        // 4
coolCounter.increase(printSomething);        // yo! 5

coolCounter.increase(printSomething);        // 6
coolCounter.increase(printSomething);        // 7
coolCounter.increase(printSomething);        // 8
coolCounter.increase(printSomething);        // 9
coolCounter.increase(alertNum);        // alert 창 popup
```

```javascript
class Counter {
  constructor(runEveryFiveTimes) {
    this.counter = 0;
    this.callback = runEveryFiveTimes;
  }

  increase(runIf5Times) {
    this.counter++;
    console.log(this.counter);

    if (this.counter % 5 === 0) {
      this.callback(this.counter);
    }
  }
}

const coolCounter = new Counter(printSomething);

function printSomething(num) {
  console.log(`yo! ${num}`);
}
function alertNum(num) {
  alert(`Wow! ${num}`);
}

const printCounter = new Counter(printSomething);
const alertCounter = new Counter(alertNum);
```

### 05. 자바스크립트 최신 문법(ES6, ES11)

**es6** - <https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/es6-11/es6.js>

**es11** - <https://github.com/byahram/2022-webs/blob/main/study/javascript/BasicJavaScriptByDreamCoding/es6-11/es11.js>

### 06. 자바스크립트 프로처럼 쓰는 팁 예제들

<https://github.com/byahram/2022-webs/tree/main/study/javascript/BasicJavaScriptByDreamCoding/pro-tips>
