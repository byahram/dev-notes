---
title: "[웹스터디] 2022 JS 퀴즈"
date: "2022"
category: "javascript"
tags: ["javascript", "algorithms"]
---

# [웹스터디] 2022 JS 퀴즈

## 목차

- [2022.09.24](#20220924)
- [2022.10.01](#20221001)
- [2022.10.08](#20221008)
- [2022.10.22](#20221022)

<br>

## 2022.09.24

### 01.

```javascript
/********** 1. **********/
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

### 02.

```javascript
/********** 2. **********/
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

### 03.

```javascript
/********** 3. **********/
function func() {
  let a = [];

  for (i = 1; i <= 15; i += 4) {
    a += [i];
  }
  document.write(a);
}
func();

/* <answer> 15913 */
```

### 04.

```javascript
/********** 4. **********/
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

### 05.

```javascript
/********** 5. **********/
function func(data, exp) {
  let result = 1;
  for (i = 0; i < exp; i++) {
    result = result * data;
  }
  document.write(result);
}
func(3, 3);

/* <answer> 27 */
```

### 06.

```javascript
/********** 6. **********/
function func() {
  let a = [2, 3, 2, 4, 5, 6, 7, 2, 3, 3, 2];
  let value = 0;
  for (i = 0; i < a.length; i++) {
    if (a[i] == 7) {
      value++;
    }
  }
  document.write(value);
}
func();

/* <answer> 1 */
```

### 07.

```javascript
/********** 7. **********/
function func() {
  let i,
    j = 0;
  for (i = 0; i < 5; i++) {
    j += i;
  }
  document.write(i);
  document.write(j);
}
func();

/* <answer> 510 */
```

### 08.

```javascript
/********** 8. **********/
function func() {
  let a = 12,
    b = 8,
    c = 2,
    d = 3;
  a /= b - c * d;

  document.write(a);
  document.write(a * b);
}
func();

/* <answer> 648 */
```

### 09.

```javascript
/********** 9. **********/
function func() {
  let a = 0,
    sum = 0;
  while (a < 10) {
    a++;
    if (a % 2 == 1) continue;
    sum += a;
  }
  document.write(sum);
}
func();

/* <answer> 30 */
```

### 10.

```javascript
/********** 10. **********/
function func() {
  let i = 0,
    sum = 0;
  while (0) {
    i++;
    if (i > 10) break;
    if (i % 5 == 0) continue;
    sum += i;
  }
  document.write(sum);
}
func();

/* <answer> 0 */
```

### 11.

```javascript
/********** 11. **********/
function solution(data) {
  var answer = data;
  answer = answer.toLowerCase();
  return answer;
}
document.write(solution("Javascript"));

/* <answer> javascript */
```

### 12.

```javascript
/********** 12. **********/
function solution(a, b, c) {
  let answer;

  if (a < b) answer = a;
  else answer = b;

  if (c < answer) answer = c;

  return answer;
}
document.write(solution(2, 5, 1));

/* <answer> 1 */
```

### 13.

```javascript
/********** 13. **********/
function func() {
  function funA() {
    document.write("함수A가 실행되었습니다.");
  }
  funA();
  function funcB() {
    document.write("함수B가 실행되었습니다.");
  }
}
func();

/* <answer> 함수A가 실행되었습니다. */
```

### 14.

```javascript
/********** 14. **********/
function func(str = "함수가 실행되었습니다.") {
  document.write(str);
}
func();

/* <answer> 함수가 실행되었습니다. */
```

### 15.

```javascript
/********** 15. **********/
const objA = {
  a: 100,
  b: 200,
};
const objB = {
  c: "javascript",
  d: "jquery",
};
const spread = { ...objA, ...objB };

document.write(spread.c);

/* <answer> javascript */
```

### 16.

```javascript
/********** 16. **********/
let num = 0;
while (num < 20000) {
  num++;
  document.write(num);
  if (num == 10) {
    break;
  }
}

/* <answer> 12345678910 */
```

### 17.

```javascript
/********** 17. **********/
for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 5; j++) {
    document.write("*");
  }
  document.write("<br>");
}

/* <answer>
 *****
 *****
 *****
 *****
 ***** */
```

### 18.

```javascript
/********** 18. **********/
function solution(a, b) {
  let num = a > b;
  return num;
}
document.write(solution(5, 9));

/* <answer> false */
```

### 19.

```javascript
/********** 19. **********/
function solution(a, b) {
  return ((a + b) * ((a > b ? a - b : b - a) + 1)) / 2;
}
document.write(solution(5, 9));

/* <answer> 35 */
```

### 20.

```javascript
/********** 20. **********/
function solution(text) {
  return text.split("").join("/").replace("s", "site");
}
document.write(solution("webs"));

/* <answer> w/e/b/site */
```

<br>

## 2022.10.01

### 01.

```javascript
/********** 1. **********/
function func() {
  let i = 10,
    hap = 0;
  while (i > 1) {
    if (i % 3 == 1) {
      hap += i;
    }
    i--;
  }
  document.write(hap);
}
func();

/* <answer> 21 */
```

### 02.

```javascript
/********** 2. **********/
function func() {
  let a = [];
  for (i = 1; i <= 5; i++) {
    a += [i];
  }
  document.write(a[2]);
}
func();

/* <answer> 3 */
```

### 03.

```javascript
/********** 3. **********/
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

### 04.

```javascript
/********** 4. **********/
function func(data, exp) {
  let result = 1;
  for (i = 1; i < exp; i++) {
    result = result * i;
  }
  document.write(result + data);
}
func(1, 5);

/* <answer> 25 */
```

### 05.

```javascript
/********** 5. **********/
function func() {
  let a = [2, 3, 2, 4, 5, 6, 7, 2, 3, 3, 2];
  let value = 0;
  for (i = 0; i < a.length; i++) {
    if (a[i] != 2) {
      value++;
    }
  }
  document.write(value);
}
func();

/* <answer> 7 */
```

### 06.

```javascript
/********** 6. 오답 **********/
function func() {
  let i,
    j = 0;
  for (i = 0; i <= 8; i++) {
    j += i;
  }
  document.write(j + i);
}
func();

/* <answer> 45 */
```

### 07.

```javascript
/********** 7. **********/
function func() {
  let a = 12,
    b = 8,
    c = 2,
    d = 3;
  a /= b - c * d;
  document.write(a);
}
func();

/* <answer> 6 */
```

### 08.

```javascript
/********** 8. **********/
function func() {
  let a, b, c, result;
  a = 10;
  b = 20;
  c = 30;
  result = a < b ? b / a : c;
  document.write(result);
}
func();

/* <answer> 2 */
```

### 09.

```javascript
/********** 9. 오답 **********/
function func() {
  let a = 0,
    sum = 0;
  while (a < 10) {
    a++;
    if (a % 2 == 1) continue;
    sum += a;
  }
  document.write(sum);
}
func();

/* <answer> 30 */
```

### 10.

```javascript
/********** 10. 오답 **********/
function func() {
  let i = 0,
    sum = 0;
  while (0) {
    i++;
    if (i > 10) break;
    if (i % 5 == 0) continue;
    sum += i;
  }
  document.write(sum);
}
func();

/* <answer> 0 */
```

### 11.

```javascript
/********** 11. **********/
function test(arrData) {
  let result = 0;
  for (let i = 0; i < arrSubject.length; i++) {
    result += Number(arrSubject[i]);
  }
  let num = result;
  return num;
}
let arrSubject = ["100", "90", "80", "70"];
let result = test(arrSubject);

document.write(result);

/* <answer> 340 */
```

### 12.

```javascript
/********** 12. **********/
function func(a, b, c) {
  let answer;
  if (a < b) answer = a;
  else answer = b;

  if (c < answer) answer = c;
  document.write(answer);
}
func(2, 5, 10);

/* <answer> 2 */
```

### 13.

```javascript
/********** 13. **********/
function func(n) {
  let answer;
  answer = Math.floor(n / 12);
  return answer;
}
document.write(func(178));

/* <answer> 14 */
```

### 14.

```javascript
/********** 14. **********/
function func(s = "webstoryboy") {
  let answer = "";
  for (let x of s) {
    if (x != "w") answer += "#";
    else answer += x;
  }
  return answer;
}
let str = "webstoryboy";
document.write(func(str));

/* <answer> w########## */
```

### 15.

```javascript
/********** 15. 오답 **********/
function func(s, t) {
  let answer = 0;
  for (let x of s) {
    if (x !== t) answer++;
  }
  return answer;
}
let str = "wbestoryboy";
document.write(func(str, "t"));

/* <answer> 10 */
```

### 16.

```javascript
/********** 16. **********/
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
document.write(func("webStorBOY10"));

/* <answer> WEBsTORboy10 */
```

### 17.

```javascript
/********** 17. **********/
function solution(n) {
  var answer = [];
  answer = n
    .toString()
    .split("")
    .reverse()
    .map((val) => parseInt(val));
  return answer;
}
document.write(solution(12345));

/* <answer> 5,4,3,2,1 */
```

### 18.

```javascript
/********** 18. 오답 **********/
function solution(arr, divisor) {
  var answer = [];
  const div = arr.filter((el) => el % divisor == 0);
  answer = div.length > 0 ? div.sort((a, b) => a - b) : [-1];

  return answer;
}
document.write(solution([1, 2, 3, 4, 5], 2));

/* <answer> 2, 4 */
```

### 19.

```javascript
/********** 19. **********/
function solution(numbers) {
  let answer = 0;
  for (let i = 0; i < 10; i++) {
    if (!numbers.includes(i)) {
      answer += i;
    }
  }
  return answer;
}
document.write(solution([1, 2, 3, 4, 6, 7, 8, 0]));

/* <answer> 14 */
```

### 20.

```javascript
/********** 20. 오답 **********/
function solution(n) {
  for (let i = 1; i < n; i++) {
    if (n % i === 1) return i;
  }
}
document.write(solution(10));

/* <answer> 3 */
```

<br>

## 2022.10.08

### 01.

```javascript
/********** 1. **********/
function solution(a, b, c) {
  let answer = "YES",
    max;
  let tot = a + b + c;

  if (a > b) max = a;
  else max = b;

  if (c > max) max = c;
  if (tot - max <= max) answer = "NO";

  return answer;
}
document.write(solution(13, 33, 17));

/* <answer> NO */
```

### 02.

```javascript
/********** 2. **********/
function solution(a, b, c) {
  let answer;
  if (a < b) answer = a;
  else answer = b;
  if (c < answer) answer = c;
  return answer;
}
document.write(solution(2, 5, 1));

/* <answer> 1 */
```

### 03.

```javascript
/********** 3. **********/
function solution(n) {
  let answer;
  answer = Math.ceil(n / 12);
  return answer;
}
document.write(solution(178));

/* <answer> 15 */
```

### 04.

```javascript
/********** 4. **********/
function solution(n) {
  let answer = 0;
  for (let i = 1; i <= n; i++) {
    answer = answer + i;
  }
  return answer;
}
document.write(solution(5));

/* <answer> 15 */
```

### 05.

```javascript
/********** 5. **********/
function solution(arr) {
  let answer = [];
  let sum = 0,
    min = 1000;
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
arr = [12, 77, 38, 41, 53, 92, 85];
document.write(solution(arr));

/* <answer> 256,41 */
```

### 06.

```javascript
/********** 6. **********/
function solution(day, arr) {
  let answer = 0;
  for (let x of arr) {
    if (x % 10 == day) answer++;
  }
  return answer;
}
arr = [25, 23, 11, 47, 53, 17, 33];
document.write(solution(3, arr));

/* <answer> 3 */
```

### 07.

```javascript
/********** 7. 오답 **********/
function solution(arr) {
  let answer = arr;
  let sum = answer.reduce((a, b) => a + b, 0);

  for (let i = 0; i < 8; i++) {
    for (let j = i + 1; j < 9; j++) {
      if (sum - (answer[i] + answer[j]) == 100) {
        answer.splice(j, 1);
        answer.splice(i, 1);
      }
    }
  }
  return answer;
}
let arr = [20, 7, 23, 19, 10, 15, 25, 8, 13];
document.write(solution(arr));

/* <answer> 20,7,23,19,10,8,13 */
```

### 08.

```javascript
/********** 8. **********/
function solution(s) {
  let answer = "";
  for (let x of s) {
    if (x == "A") answer += "#";
    else answer += x;
  }
  return answer;
}
let str = "BANANA";
document.write(solution(str));

/* <answer> B#N#N# */
```

### 09.

```javascript
/********** 9. 오답 **********/
function solution(s) {
  let answer = s;
  answer = answer.replace(/a/g, "#");
}
let str = "javascript";
document.write(solution(str));

/* <answer> undefined */
```

### 10.

```javascript
/********** 10. **********/
function solution(s, t) {
  let answer = s.split(t).length;
  return answer - 1;
}
let str = "javascript react vue";
document.write(solution(str, "a"));

/* <answer> 3 */
```

### 11.

```javascript
/********** 11. **********/
function solution(s) {
  let answer = 0;
  for (let x of s) {
    if (x === x.toUpperCase()) answer++;
  }
  return answer;
}
let str = "KoreaTimeGood";
document.write(solution(str));

/* <answer> 3 */
```

### 12.

```javascript
/********** 12. **********/
function solution(s) {
  let answer = "";
  for (let x of s) {
    let num = x.charCodeAt();

    if (x === x.toLowerCase()) answer += x.toUpperCase();
    else answer += x;
  }
  return answer;
}
let str = "ItisTimeToStudy";
document.write(solution(str));

/* <answer> ITISTIMETOSTUDY */
```

### 13.

```javascript
/********** 13. **********/
function solution(s) {
  let answer = "";
  for (let x of s) {
    if (x === x.toUpperCase()) answer += x.toLowerCase();
    else answer += x.toUpperCase();
  }
  return answer;
}
document.write(solution("StuDY"));

/* <answer> sTUdy */
```

### 14.

```javascript
/********** 14. **********/
function solution(s) {
  // max = Number.MIN_SAFE_INTEGER; 정수값 표현 방식
  let answer = "",
    max = Number.MIN_SAFE_INTERGER;
  for (let x of s) {
    if (x.length > max) {
      max = x.length;
      answer = x;
    }
  }
  return answer;
}
let str = ["teacher", "time", "student", "beautiful", "good"];
document.write(solution(str));

/* <answer> beautiful */
```

### 15.

```javascript
/********** 15. 오답 **********/
//substring() 메서드는 string 객체의 시작 인덱스로 부터 종료 인덱스 전까지 문자열의 부분 문자열을 반환합니다.
function solution(s) {
  let answer;
  let mid = Math.floor(s.length / 2);

  if (s.length % 2) answer = s.substring(mid, mid + 1);
  else answer = s.substring(mid - 1, mid + 1);
  return answer;
}
document.write(solution("study"));

/* <answer> u */
```

### 16.

```javascript
/********** 16. 오답 **********/
function solution(s) {
  let answer = "";
  for (let i = 0; i < s.length; i++) {
    if (s.indexOf(s[i]) === i) answer += s[i];
  }
  return answer;
}
document.write(solution("ksekkset"));

/* <answer> kset */
```

### 17.

```javascript
/********** 17. 오답 **********/
function solution(s) {
  let answer;
  answer = s.filter(function (v, i) {
    return s.indexOf(v) === i;
  });
  return answer;
}
let str = ["good", "time", "good", "time", "student"];
document.write(solution(str));

/* <answer> good,time,student */
```

### 18.

```javascript
/********** 18. 오답 **********/
function solution(s) {
  let answer = "YES";
  stack = [];
  for (let x of s) {
    if (x === "(") stack.push(x);
    else {
      if (stack.length === 0) return "NO";
      stack.pop();
    }
  }
  if (stack.length > 0) return "NO";
}
let a = "(()(()))(()";
document.write(solution(a));

/* <answer> NO */
```

### 19.

```javascript
/********** 19. 오답 **********/
function solution(s) {
  let answer;
  let stack = [];
  for (let x of s) {
    if (x === ")") {
      while (stack.pop() !== "(");
    } else stack.push(x);
  }
  answer = stack.join("");
  return answer;
}
let str = "(A(BC)D)EF(G(H)(IJ)K)LM(N)";
document.write(solution(str));

/* <answer> EFLM */
```

### 20.

```javascript
/********** 20. 오답 **********/
function solution(n, k) {
  let answer;
  let queue = Array.from({ length: n }, (v, i) => i + 1);
  while (queue.length) {
    for (let i = 1; i < k; i++) queue.push(queue.shift());
    queue.shift();
    if (queue.length === 1) answer = queue.shift();
  }
  return answer;
}
document.write(solution(8, 3));

/* <answer> 7 */
```

<br>

## 2022.10.22

### 01.

```javascript
/********** 1. **********/
const arr = [1, 1, 3, 3, 0, 1, 1];

function solution(arr) {
  return arr.filter((item, idx) => item !== arr[idx + 1]);
}

document.write(solution(arr));

/* <answer> 1,3,0,1 */
```

### 02.

```javascript
/********** 2. **********/
const arr = "webstoryboy";

function solution(s) {
  const answer = "";
  return s.length % 2 == 1
    ? s[(s.length / 2) | 0]
    : s.slice(s.length / 2 - 1, s.length / 2 + 1);
}

document.write(solution(arr));

/* <answer> o */
```

### 03.

```javascript
/********** 3. **********/
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

function solution(numbers) {
  var answer = 0;
  for (let i = 0; i < numbers.length; i++) {
    answer = answer + numbers[i];
  }
  answer = answer / numbers.length;
  return answer;
}

document.write(solution(arr));

/* <answer> 5.5 */
```

### 04.

```javascript
/********** 4. **********/
const string = "webstoryboy";

function solution(s) {
  let answer = "";
  for (let i = s.length - 1; i >= 0; i--) {
    answer = answer + s[i];
  }
  return answer;
}

document.write(solution(string));

/* <answer> yobyrotsbew */
```

### 05.

```javascript
/********** 5. **********/
const num = 10;

function solution(n) {
  let answer = "";

  for (i = 0; i < n; i++) {
    if (i % 2 == 0) {
      answer += "코";
    } else {
      answer += "딩";
    }
  }
  return answer;
}

document.write(solution(num));

/* <answer> 코딩코딩코딩코딩코딩 */
```

### 06.

```javascript
/********** 6. 오답 **********/
const num = "webstoryboy";

function solution(s) {
  let answer;
  let arr = [...s];

  if (arr.length % 2 == 0) {
    answer = arr.splice(arr.length / 2 - 1, 2);
  } else {
    answer = arr.splice(Math.floor(arr.length / 2), 1);
  }
  return answer.join("");
}

document.write(solution(num));

/* <answer> o */
```

### 07.

```javascript
/********** 7. **********/
const num = [5, 9, 7, 10];

function solution(arr, divisor) {
  var answer = [];

  for (i = 0; i < arr.length; i++) {
    if (arr[i] % divisor == 0) answer.push(arr[i]);
  }
  return answer;
}

document.write(solution(num, 5));

/* <answer> 5,10 */
```

### 08.

```javascript
/********** 8. 오답 **********/
const num = [5, 9, 7, 10];

function solution(arr) {
  let sm = arr[0],
    index = 0;

  for (i = 0; i < arr.length; i++) {
    if (arr[i] < sm) {
      sm = arr[i];
      index = i;
    }
  }
  arr.splice(index, 1);
  return arr;
}

document.write(solution(num));

/* <answer> 9,7,10 */
```

### 09.

```javascript
/********** 9. **********/
const num = "webstoryboy";

function solution(s) {
  let answer = s.split("");

  for (i = 0; i < answer.length - 4; i++) {
    answer[i] = "*";
  }
  return answer.join("");
}

document.write(solution(num));

/* <answer> *******yboy */
```

### 10.

```javascript
/********** 10. **********/
function solution(arr1, arr2) {
  let answer = [];
  arr1.sort();
  arr2.sort();
  let p1 = (p2 = 0);

  while (p1 < arr1.length && p2 < arr2.length) {
    if (arr1[p1] == arr2[p2]) {
      answer.push(arr1[p1++]);
      p2++;
    } else if (arr1[p1] < arr2[p2]) p1++;
    else p2++;
  }
  return answer;
}

let a = [1, 3, 9, 5, 2];
let b = [3, 2, 5, 7, 8];

document.write(solution(a, b));

/* <answer> 2,3,5 */
```
