---
title: "[웹스터디] 2021 JS 퀴즈"
date: "2021"
category: "javascript"
tags: ["javascript", "algorithms"]
---

# 웹스 2021 JS 퀴즈

## 목차

- [2021.09.18](#20210918)
- [2021.09.25](#20210925)

<br>

## 2021.09.18

### 01.

```javascript
/********** 1. **********/

function func() {
  let i = 5,
    j = 4,
    k = 1,
    l,
    m;
  l = i > 5 || k != 0;
  m = j <= 4 && k < 1;
  document.write(l);
  document.write(m);
}
func();

/* <answer> truefalse */
```

### 02.

```javascript
/********** 2. **********/

function func() {
  let i = 10,
    j = 10,
    k = 30;
  i /= j;
  j -= i;
  k %= j;
  document.write(i);
  document.write(j);
  document.write(k);
}
func();

/* <answer> 193 */
```

### 03.

```javascript
/********** 3. 오답 **********/

function func() {
  for (i = 1; i <= 7; i++) {
    for (j = 1; j <= i; j++) {
      document.write(j);
    }
    document.write("<br>");
  }
}
func();

/* <answer>
    1
    12
    123
    1234
    12345
    123456
    1234567 */
```

### 04.

```javascript
/********** 4. **********/

function func() {
  let i = 10,
    hap = 0;
  while (i > 1) {
    i--;
    if (i % 3 == 1) {
      hap += i;
    }
  }
  document.write(hap);
}
func();

/* <answer> 12 */
```

### 05.

```javascript
/********** 5. **********/

function func() {
  let a = [];
  for (i = 1; i <= 5; i++) {
    a += [i];
  }
  document.write(a);
}
func();

/* <answer> 12345*/
```

### 06.

```javascript
/********** 6. **********/

function func() {
  let a = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    cnt = 0;

  for (i = 0; i < a.length; i++) {
    if (a[i] % 2 != 0) {
      cnt = cnt + 1;
    }
  }
  document.write(cnt);
}
func();

/* <answer> 5 */
```

### 07.

```javascript
/********** 7. **********/

function func(data, exp) {
  let result = 1;
  for (i = 0; i < exp; i++) {
    result = result * data;
  }
  document.write(result);
}
func(2, 5);

/* <answer> 32 */
```

### 08.

```javascript
/********** 8. **********/

function func(data, exp) {
  let a = [2, 3, 2, 4, 5, 6, 7, 2, 3, 3, 2];
  let value = 0;
  for (i = 0; i < a.length; i++) {
    if (a[i] == 2) {
      value++;
    }
  }
  document.write(value);
}
func();

/* <answer> 4 */
```

### 09.

```javascript
/********** 9. 오답 **********/

function func() {
  let i,
    j = 0;
  for (i = 0; i < 8; i++) {
    i += i;
  }
  document.write(i, "<br>");
  document.write(j, "<br>");
}
func();

/* <answer>
    15
    0    */
```

### 10.

```javascript
/********** 10. 오답 **********/

function func() {
  let a = 12,
    b = 8,
    c = 2,
    d = 3;
  a /= b - c * d;

  document.write(a);
  document.write(b);
}
func();

/* <answer> 68 */
```

### 11.

```javascript
/********** 11. **********/

function func13() {
  let a, b, c, result;
  a = 10;
  b = 20;
  c = 30;
  result = a < b ? b : c;

  document.write(result);
}
func13();

/* <answer> 20 */
```

### 12.

```javascript
/********** 12. 오답 **********/

function func() {
  let a = 0,
    sum = 0;
  while (a < 10) {
    a++;
    if (a % 2 == 1) continue;
    document.write(a);
    sum += a;
  }
  document.write(sum);
}
func();

/* <answer> 24681030 */
```

### 13.

```javascript
/********** 13. 오답 **********/

function func() {
  let i = 0,
    sum = 0;
  while (1) {
    i++;
    if (i > 10) break;
    if (i % 5 == 0) continues;
    sum += i;
    document.write(i);
  }
  document.wirte(sum);
}
func();

/* <answer> 1234 */
```

### 14.

```javascript
/********** 14. **********/

function testAvg(arrData) {
  let sum = 0;
  for (let i = 0; i < arrSubject.length; i++) {
    sum += Number(arrSubject[i]);
  }
  let avg = sum / arrData.length;
  return avg;
}
let arrSubject = ["100", "90", "80", "70"];
let result = testAvg(arrSubject);

document.write(result);

func();

/* <answer> 85 */
```

### 15.

```javascript
/********** 15. **********/

function func() {
  let color = ["#311b92", "#1a237e", "#1b5e20", "#827717"];

  for (let i = 0; i < color.length; i++) {
    document.write(color[i]);
  }
}
func();

/* <answer> #311b92#1a237e#1b5e20#827717 */
```

### 16.

```javascript
/********** 16. 오답 **********/

function func(a, b, c) {
  let answer;
  if (a < b) answer = a;
  else answer = b;
  if (c < answer) answer = c;
  document.write(answer);
}
func(2, 5, 1);

/* <answer> 1 */
```

### 17.

```javascript
/********** 17. 오답 **********/

function func(n) {
  let answer;
  answer = Math.ceil(n / 12);
  return answer;
}
document.write(func(178));

func();

/* <answer> 15 */
```

### 18.

```javascript
/********** 18. **********/

function func(s) {
  let answer = "";
  for (let x of s) {
    if (x == "w") answer += "#";
    else answer += x;
  }
  return answer;
}

let str = "webstoryboy";
document.write(func(str));

/* <answer> #ebstoryboy */
```

### 19.

```javascript
/********** 19. **********/

function func(s, t) {
  let answer = 0;
  for (let x of s) {
    if (x === t) answer++;
  }
  return answer;
}
let str = "webstoryboy";
document.write(func(str, "b"));

/* <answer> 2 */
```

### 20.

```javascript
/********** 20. **********/

function func(s) {
  let answer = "";
  for (let x of s) {
    if (x === x.toUpperCase()) {
      answer += x.toLowerCase();
    } else {
      answer += x.toUpperCase();
    }
  }
  return answer;
}
document.write(func("webStorBOY"));

/* <answer> WEBsTORboy */
```

<br>

## 2021.09.25

### 01.

```javascript
/********** 1. **********/
const arr = [100, 200, 300, 400, 500];

let max;
for (let i = 0; i < arr.length; i++) {
  if (arr[i] > arr[0]) {
    max = arr[i];
  }
}

document.write(max);

/* <answer> 500 */
```

### 02.

```javascript
/********** 2. **********/
function solution(arr) {
  let answer = [];

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] > 5) {
      answer.push(arr[i]);
    }
  }
  return answer;
}

let arr = [7, 3, 9, 5, 6, 12];
document.write(solution(arr));

/* <answer> 7,9,6,12 */
```

### 03.

```javascript
/********** 3. **********/
let arr = [130, 135, 148, 140, 145, 150, 150, 153];

function solution(arr) {
  let answer = 1,
    max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      answer++;
      max = arr[i];
    }
  }
  document.write(answer + " / ");
  document.write(max);
}
solution(arr);

/* <answer> 5 / 153 */
```

### 04.

```javascript
/********** 4. **********/
function solution(s) {
  let answer = "";

  for (let i = 0; i < s.length; i++) {
    if (s.indexOf(s[i]) === 3) {
      answer = s[i];
    }
  }
  document.write(answer);
}
solution("webstoryboy");

/* <answer> s */
```

### 05.

```javascript
/********** 5. 오답 **********/
let str = ["good", "time", "good", "time", "student"];

function solution(s) {
  let answer;

  answer = s.filter(function (v, i) {
    document.write(i);
  });
}

solution(str);

/* <answer> 01234 */
```

### 06.

```javascript
/********** 6. 오답 **********/
function solution(s) {
  let answer;
  let mid = Math.floor(s.length / 2);

  if (s.length % 2 === 1) {
    answer = s.substring(mid, mid + 1);
  } else {
    answer = s.substring(mid - 1, mid + 1);
  }
  return answer;
}
document.write(solution("webstoryboy"));

/* <answer> o */
```

### 07.

```javascript
/********** 7. **********/
let str = ["teacher", "time", "student", "beautiful", "good"];

function solution(s) {
  let answer = "";
  let max = 0;

  for (let x of s) {
    if (x.length > max) {
      max = x.length;
      answer = x;
    }
  }
  return answer;
}
document.write(solution(str));

/* <answer> beautiful */
```

### 08.

```javascript
/********** 8. **********/
function solution(s, t) {
  let answer = 10;

  for (let x of s) {
    if (x === t) answer--;
  }
  return answer;
}
let str = "COMPUTERPROGRAMMING";
document.write(solution(str, "R"));

/* <answer> 7 */
```

### 09.

```javascript
/********** 9. 오답 **********/
let arr = [20, 7, 23];

function solution(arr) {
  let answer = arr;
  let sum = answer.reduce((a, b) => a + b, 0);
  return sum;
}
document.write(solution(arr));

/* <answer> 50 */
```

### 10.

```javascript
/********** 10. **********/
arr = [25, 23, 13, 47, 53, 17, 33];

function solution(day, arr) {
  let answer = 0;

  for (let x of arr) {
    if (x % 10 == day) answer++;
  }
  return answer;
}
document.write(solution(3, arr));

/* <answer> 4 */
```

### 11.

```javascript
/********** 11. **********/
arr = [12, 77, 38, 41, 53, 92, 85];

function solution(arr) {
  let answer = [];
  let sum = 0,
    min = 10000;

  for (let x of arr) {
    if (x % 2 === 1) {
      sum += x;
      if (x < min) min = x;
    }
  }
  answer.push(sum);
  answer.push(min);
  return answer;
}
document.write(solution(arr));

/* <answer> 256,41 */
```

### 12.

```javascript
/********** 12. **********/
let arr = [5, 7, 1, 3, 2, 9, 11];

function solution(arr) {
  let answer,
    min = 100;

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] < min) {
      min = arr[i];
    }
  }
  answer = min;
  return answer;
}
document.write(solution(arr));

/* <answer> 1 */
```

### 13.

```javascript
/********** 13. **********/
function solution(n) {
  let answer = 0;

  for (let i = 1; i <= n; i++) {
    answer = answer + i;
  }
  return answer;
}
document.write(solution(10));

/* <answer> 55 */
```

### 14.

```javascript
/********** 14. **********/
function solution() {
  let arr = [7, 3, 9, 5, 6, 12];

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > arr[i - 1]) {
      document.write(arr[i]);
    }
  }
}
solution();

/* <answer> 9612 */
```

### 15.

```javascript
/********** 15. 오답 **********/
let str = "g0en2T0s8eSoft";

function solution(str) {
  let answer = "";

  for (let x of str) {
    if (x == 0 || x == 8) {
      answer++;
    }
  }
  return answer;
}
document.write(solution(str));

/* <answer> 3 */
```

### 16.

```javascript
/********** 16. **********/
let str = "teachermode";

function solution(s, t) {
  let answer = [];
  let p = 1000;

  for (let x of s) {
    if (x === t) {
      answer.push(p);
    }
  }
  return answer;
}
document.write(solution(str, "e"));

/* <answer> 1000,1000,1000 */
```

### 17.

```javascript
/********** 17. **********/
function solution(s) {
  let answer = "";
  let cnt = 1;
  s = s + " ";

  for (let i = 0; i < s.length - 1; i++) {
    if (s[i] === s[i + 1]) {
      cnt++;
    } else {
      answer += s[i];
      if (cnt > 1) {
        answer += String(cnt);
      }
      cnt = 1;
    }
  }
  return answer;
}
let str = "KKHSSSSSSSE";
document.write(solution(str));

/* <answer> K2HS7E */
```

### 18.

```javascript
/********** 18. **********/
let str = "ABCD";

function solution(s) {
  let answer;
  let stack = [];

  for (let x of s) {
    if (x === "C") {
      stack.pop();
    } else {
      stack.push(x);
    }
  }
  answer = stack.join("");
  return answer;
}
document.write(solution(str));

/* <answer> AD */
```

### 19.

```javascript
/********** 19. **********/
function func() {
  let a = [1, 2, 3, 4, 5, 6, 7, 8, 9],
    cnt = 0;

  for (i = 0; i < a.length; i++) {
    if (a[i] % 2 == 0) {
      cnt = cnt + 1;
    }
  }
  document.write(cnt);
}
func();

/* <answer> 4 */
```

### 20.

```javascript
/********** 20. **********/
function solution() {
  for (let i = 0; i < 5; i++) {
    for (let j = 0; j <= i; j++) {
      document.write("*");
    }
    document.write("<br>");
  }
}
solution();

/* <answer>
 *
 **
 ***
 ****
 *****     */
```
