---
title: "[JS] 클로저(Closure)와 익명함수란?"
date: "2022-12-09"
category: "javascript"
tags: ["javascript", "closure", "익명함수"]
---

# [JS] 클로저(Closure)와 익명함수란?

## 클로져(Closure)란?

Closure represent anonymous functions. Anonymous functions, also known as closures, allow the creation of functions which have no specified name. They are the most useful as the value of callable parameters, but they have many other uses.

클로저라고도 알려져 있는 익명 함수는 지정된 이름이 없는 함수를 만들 수 있게 한다. 호출 가능한 매개 변수의 값으로 가장 유용하지만 다른 많은 용도도 있다.

Anonymous functions are implemented using the Closure class.

익명 함수는 클로저 클래스를 사용하여 구현된다.

```php
<?php
    echo preg_replace_callback('~-([a-z])~', function ($match) {
        return strtoupper($match[1]);
    }, 'hello-world');
    // outputs helloWorld
?>
```

```javascript
$(function () {
  var selections = [];
  $(".niners").click(function () {
    // 이 클로저는 selections 변수에 접근합니다.
    selections.push(this.prop("name")); // 외부 함수의 selections 변수를 갱신함
  });
});
```

<https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures>

<br>

## 클로저(Closure) | 특징

"A closure is the combination of a functioin and the lexical environment within which that function was declared."

클로저는 함수와 그 함수가 선언됐을 때의 렌시컬 환경(Lexical environment)과의 조합이다.

```javascript
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

함수 outerFunc 내에서 내부함수 innerFunc가 선언되고 호출되었다. 이때 내부함수 innerFunc는 자신을 포함하고 있는 외부함수 outerFunc의 변수 x에 접근할 수 있다. 이는 함수 innerFunc가 함수 outerFunc의 내부에 선언되었기 때문이다.

스코프는 함수를 호출할 때가 아니라 함수를 어디에 선언하였는지에 따라 결정된다. 이를 렉시컬 스코핑(Lexical scoping)라 한다. 위 예제의 함수 innerFunc는 함수 outerFunc의 내부에서 선언되었기 때문에 함수 innerFunc의 상위 스코프는 함수 outerFunc이다. 함수 innerFunc가 전역에 선언되었다면 함수 innerFunc의 상위 스코프는 전역 스코프가 된다.

<br>

## 클로저(Closure) | 활용 예

### < 클로저 사용 없이 구현 >

위 코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드이다. increase 함수는 호출되기 직전에 전역 변수 counter의 값이 반드시 0이여야 제대로 동작한다. 하지만 변수 counter는 전역 변수이기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있다. 이는 의도치 않게 값이 변경될 수 있다는 것을 의미한다. 만약 누군가에 의해 의도치 않게 전역 변수 counter의 값이 변경됐다면 이는 오류로 이어진다. 변수 counter는 카운터를 관리하는 increase 함수가 관리하는 것이 바람직한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <p>전역 변수를 사용한 Counting</p>
    <button id="increase">+</button>
    <p id="count">0</p>

    <script>
      var increaseBtn = document.getElementById("increase");
      var count = document.getElementById("count");

      // 카운트 상태를 유지하기 위한 전역 변수
      var counter = 0;

      function increase() {
        return ++counter;
      }

      increaseBtn.onclick = function () {
        count.innerHTML = increase();
      };
    </script>
  </body>
</html>
```

### < 클로저 사용하여 구현 >

스크립트가 실행되면 즉시 실행 함수(immediately-invoked function expression)가 호출되고 변수 increase에는 함수 function () { return ++counter; }가 할당된다. 이 함수는 자신이 생성됐을 때의 렉시컬 환경(Lexical environment)을 기억하는 클로저다. 즉시실행함수는 호출된 이후 소멸되지만 즉시실행함수가 반환한 함수는 변수 increase에 할당되어 increase 버튼을 클릭하면 클릭 이벤트 핸들러 내부에서 호출된다. 이때 클로저인 이 함수는 자신이 선언됐을 때의 렉시컬 환경인 즉시실행함수의 스코프에 속한 지역변수 counter를 기억한다. 따라서 즉시실행함수의 변수 counter에 접근할 수 있고 변수 counter는 자신을 참조하는 함수가 소멸될 때가지 유지된다.

즉시 실행 함수는 한번만 실행되므로 increase가 호출될 때마다 변수 counter가 재차 초기화될 일은 없을 것이다. 변수 counter는 외부에서 직접 접근할 수 없는 private 변수이므로 전역 변수를 사용했을 때와 같이 의도되지 않은 변경을 걱정할 필요도 없기 때문이 보다 안정적인 프로그래밍이 가능하다.

```html
<!DOCTYPE html>
<html>
  <body>
    <p>전역 변수를 사용한 Counting</p>
    <button id="increase">+</button>
    <p id="count">0</p>

    <script>
      var increaseBtn = document.getElementById("increase");
      var count = document.getElementById("count");

      var increase = (function () {
        // 카운트 상태를 유지하기 위한 자유 변수
        var counter = 0;
        // 클로저를 반환
        return function () {
          return ++counter;
        };
      })();

      increaseBtn.onclick = function () {
        count.innerHTML = increase();
      };
    </script>
  </body>
</html>
```
