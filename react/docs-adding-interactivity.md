---
title: "[리액트 공식 문서 읽기] 2. 상호작용 추가하기"
date: "2025-04-07"
category: "react"
tags: ["react", "official docs"]
---

## 목차

- [(1) 이벤트에 응답하기](#1-이벤트에-응답하기)
- [(2) State: 컴포넌트의 기억 저장소](#2-state-컴포넌트의-기억-저장소)
- [(3) 렌더링 그리고 커밋](#3-렌더링-그리고-커밋)
- [(4) 스냅샷으로서의 State](#4-스냅샷으로서의-state)
- [(5) State 업데이트 큐](#5-state-업데이트-큐)
- [(6) 객체 State 업데이트하기](#6-객체-state-업데이트하기)
- [(7) 배열 State 업데이트하기](#7-배열-state-업데이트하기)

<br />

## (1) 이벤트에 응답하기

React에서는 사용자 상호작용(클릭, 마우스 호버, 폼 인풋 포커스 등)에 반응하기 위해 이벤트 핸들러 함수를 사용한다.

### 이벤트 핸들러란?

사용자의 상호작용(클릭, 호버, 입력 등)에 반응하는 **사용자 정의 함수**로, 보통 컴포넌트 내부에 정의된다.

- 함수명은 `handleClick`, `handleMouseEnter`처럼 보통 handle로 시작한다.
- JSX에서는 `onClick`, `onMouseEnter` 등과 같은 prop 형태로 핸들러를 전달한다.
- 이벤트 핸들러는 함수 자체를 전달해야 하며, 즉시 실행(`handleClick()`) 형태로 전달하면 안 된다.

### 이벤트 핸들러 작성 방법

1. 이벤트 핸들러 기본 사용법

   ```tsx
   export default function Button() {
     function handleClick() {
       alert("You clicked me!");
     }

     return <button onClick={handleClick}>Click me</button>;
   }
   ```

2. 인라인 이벤트 핸들러 사용

   ```tsx
   {/* 인라인 */}
   <button onClick={function handleClick() { alert('You clicked me!');}}>

   {/* 올바른 예시 */}
   {/* <button onClick={handleClick}> */}
   {/* <button onClick={() => alert('...')}> */}
   <button onClick={() => alert('You clicked me!')}>Click me</button>

   {/* 잘못된 예시 */}
   {/* <button onClick={handleClick()}> */}
   {/* <button onClick={alert('...')}> */}
   <button onClick={alert('You clicked me!')}>
   ```

   - `<button onClick={handleClick}>`은 handleClick 함수를 전달한다.
   - `<button onClick={() => alert('...')}>`은 () => alert('...') 함수를 전달한다.
   - `onClick={handleClick()}`처럼 괄호를 붙이면 즉시 실행되어 이벤트가 발생하지 않아도 호출된다.

### 이벤트 핸들러에 Props 전달

컴포넌트 내부에서 정의되므로, 이벤트 핸들러는 props에 접근할 수 있다.

1. 핸들러 내에서 prop 사용

```tsx
interface AlertButtonProps {
  message: string;
  children: React.ReactNode;
}

// 클릭시 message prop의 내용을 포함한 alert를 표시
function AlertButton({ message, children }: AlertButtonProps) {
  return <button onClick={() => alert(message)}>{children}</button>;
}

export default function Toolbar() {
  return (
    <div>
      <AlertButton message="Playing!">Play Movie</AlertButton>
      <AlertButton message="Uploading!">Upload Image</AlertButton>
    </div>
  );
}
```

2. 핸들러를 prop으로 전달

   - 컴포넌트가 그 부모 컴포넌트로부터 받은 prop을 이벤트 핸들러로 전달한다.

```tsx
interface ButtonProps {
  onClick: () => void;
  children: React.ReactNode;
}

function Button({ onClick, children }: ButtonProps) {
  return <button onClick={onClick}>{children}</button>;
}

interface PlayButtonProps {
  movieName: string;
}

// PlayButton은 handlePlayClick을 Button 내 onClick prop으로 전달한다.
function PlayButton({ movieName }: PlayButtonProps) {
  function handlePlayClick(): void {
    alert(`Playing ${movieName}!`);
  }
  return <Button onClick={handlePlayClick}>Play "{movieName}"</Button>;
}

// UploadButton은 () => alert('Uploading!')을 Button 내 onClick prop으로 전달한다.
function UploadButton() {
  return <Button onClick={() => alert("Uploading!")}>Upload Image</Button>;
}

export default function Toolbar() {
  return (
    <div>
      <PlayButton movieName="Kiki's Delivery Service" />
      <UploadButton />
    </div>
  );
}
```

3. 커스텀 핸들러 prop 명명

   - 사용자 정의 컴포넌트에서는 이벤트 핸들러 prop 이름을 자유롭게 지정할 수 있다.
   - 관습적으로 이벤트 핸들러 prop의 이름은 on으로 시작하여 대문자 영문으로 이어진다.

```tsx
// EX1
interface ButtonProps {
  onSmash: () => void;
  children: React.ReactNode;
}

function Button({ onSmash, children }: ButtonProps) {
  return <button onClick={onSmash}>{children}</button>;
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert("Playing!")}>Play Movie</Button>
      <Button onSmash={() => alert("Uploading!")}>Upload Image</Button>
    </div>
  );
}

// EX2
interface ToolbarProps {
  onPlayMovie: () => void;
  onUploadImage: () => void;
}

function Toolbar({ onPlayMovie, onUploadImage }: ToolbarProps) {
  return (
    <div>
      <Button onClick={onPlayMovie}>Play Movie</Button>
      <Button onClick={onUploadImage}>Upload Image</Button>
    </div>
  );
}

function Button({
  onClick,
  children,
}: {
  onClick: () => void;
  children: React.ReactNode;
}) {
  return <button onClick={onClick}>{children}</button>;
}

export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert("Playing!")}
      onUploadImage={() => alert("Uploading!")}
    />
  );
}
```

### 이벤트 전파와 제어

- 이벤트는 기본적으로 버블링(bubbling)된다. 즉, 자식 요소에서 발생한 이벤트가 부모 요소로 전파된다.
- 부모 컴포넌트에 닿지 못하도록 막으려면 e.stopPropagation()를 호출한다.

```tsx
//  해당 버튼의 onClick이 먼저 실행되고 -> 부모인 <div>의 onClick이 뒤이어 실행될 것
// e.stopPropagation()을 사용하지 않은 경우 (버블링 발생)
export default function Toolbar() {
  return (
    <div onClick={() => alert("Clicked toolbar")}>
      <button onClick={() => alert("Clicked button")}>Click Me</button>
    </div>
  );
}

// e.stopPropagation()을 사용하여 버블링 방지
interface ButtonProps {
  onClick: () => void;
  children: React.ReactNode;
}

function Button({ onClick, children }: ButtonProps) {
  return (
    <button
      onClick={(e: React.MouseEvent<HTMLButtonElement>) => {
        e.stopPropagation();
        onClick();
      }}
    >
      {children}
    </button>
  );
}
```

#### `e.stopPropagation` vs `e.preventDefault `

- `e.stopPropagation()`: 이벤트가 상위 컴포넌트로 전파되는 것을 막습니다.
- `e.preventDefault()`: 폼 제출, 링크 이동 등 브라우저의 기본 동작을 막습니다.

```jsx
export default function Signup() {
  return (
    <form
      onSubmit={(e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        alert("Submitting!");
      }}
    >
      <input />
      <button>Send</button>
    </form>
  );
}
```

<br />

## (2) State: 컴포넌트의 기억 저장소

컴포넌트는 **state(상태)**를 사용하여 사용자 입력, 네트워크 응답 등 변화하는 데이터를 기억할 수 있다. 상태가 변경되면 컴포넌트는 자동으로 다시 렌더링된다.

### useState 훅으로 state 변수를 추가하는 방법

useState 훅은 React 컴포넌트에 state 변수를 추가할 수 있다.

- `useState(0)`의 0은 초기값.
- `count`: 현재 상태 값
- `setCount`: 상태를 업데이트하는 함수

```tsx
import { useState } from "react";

function MyComponent() {
  const [count, setCount] = useState<number>(0);
}
```

### useState 훅이 반환하는 한 쌍의 값

`useState`는 배열 형태의 한 쌍(두 가지 값)을 반환한다

- 첫 번째 요소: `value`
  - 현재 상태 값.
  - 렌더링 간에 데이터를 유지하기 위한 state 변수.
- 두 번째 요소: `setValue`
  - 상태를 변경하는 함수.
  - 변수를 업데이트하고 React가 컴포넌트를 다시 렌더링하도록 유발하는 state setter 함수
- `setValue(newValue)`를 호출하면 컴포넌트가 다시 렌더링되며 변경된 state 값을 반영한다.
- state는 직접 수정하지 않고, **갱신 함수(set 함수)**를 통해 변경해야 합니다.

```tsx
const [value, setValue] = useState<string>(initialValue);
```

### 둘 이상의 state 변수를 추가하는 방법

`useState`는 하나의 컴포넌트에 여러 개 사용할 수 있다.

- state는 독립적으로 동작하며, 원하는 만큼 사용할 수 있다.
- 관련된 데이터를 하나의 state 객체로도 관리할 수 있지만, 반드시 그래야 하는 건 아니다.

```tsx
function Form() {
  const [name, setName] = useState<string>("");
  const [age, setAge] = useState<number>(0);
}
```

### state를 지역적이라고 하는 이유

- state는 컴포넌트 내부에 선언된 지역 변수처럼 작동한다.
- 즉, 컴포넌트가 제거되면 해당 state도 함께 사라지고 다른 컴포넌트에서는 접근하거나 사용할 수 없다.
- 같은 컴포넌트를 여러 번 렌더링하면 각 인스턴스는 독립적인 state를 가진다.

```tsx
function Counter() {
  const [count, setCount] = useState<number>(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}

// 부모 컴포넌트의 state는 자식이 직접 변경할 수 없고, prop을 통해 전달해야 한다.
interface ChildProps {
  count: number;
}

function Child({ count }: ChildProps) {
  return <p>Count is {count}</p>;
}

function Parent() {
  const [count, setCount] = useState<number>(0);
  return <Child count={count} />;
}
```

<br />

## (3) 렌더링 그리고 커밋

### React에서 렌더링의 의미

- **렌더링(rendering)**이란 컴포넌트를 실행해서 JSX를 반환하고, React가 이를 기반으로 UI를 어떻게 보여줄지 계산하는 과정이다.
- 이 작업은 메모리 상에서만 수행되며, 아직 실제 DOM에는 아무런 변경도 없다.

### 1단계: 렌더링 트리거

React는 다음과 같은 경우에 컴포넌트를 다시 렌더링한다.
렌더링이 트리거되면, React는 해당 컴포넌트와 그 하위 컴포넌트들을 다시 실행하여 새 JSX를 생성한다.

- state가 변경되었을 때
- props가 변경되었을 때
- 부모 컴포넌트가 렌더링되었을 때

```tsx
// Image.tsx
const Image = () => {
  return <img src="logo.png" alt="로고" />;
};

export default Image;

// main.tsx
import React from "react";
import { createRoot } from "react-dom/client";
import Image from "./Image";

const root = createRoot(document.getElementById("root")!);
root.render(<Image />);
```

### 2단계: React 컴포넌트 렌더링

- 트리거가 발생하면, React는 컴포넌트 함수를 실행하여 새로운 JSX 트리를 만든다.
- 이 과정은 메모리에서만 수행되며, 아직 DOM에는 아무 변화도 없다.
- React는 이 새로운 트리를 이전 트리와 비교할 준비를 한다.
- 렌더링의 흐름
  - 초기 렌더링: 루트 컴포넌트부터 호출.
  - 업데이트 렌더링: state 또는 props가 변경된 컴포넌트부터 호출.
  - 이 과정은 재귀적으로 진행된다. 어떤 컴포넌트가 또 다른 컴포넌트를 반환하면, React는 그것도 렌더링하며 화면에 보여줄 내용을 완전히 파악할 때까지 계속 반복한다.
- 렌더링은 순수한 계산이어야 한다.
  - 동일한 입력 → 항상 동일한 JSX 출력이 나와야 한다.
  - 이전의 state나 외부 변수/객체를 직접 변경해서는 안 된다.
  - 이런 규칙을 지키지 않으면 예측 불가능한 버그가 생기기 쉽다.
  - 개발 중에는 React Strict Mode를 활용하면, 컴포넌트 함수를 두 번 호출하여 순수성 위반을 쉽게 발견할 수 있다.

### 3단계: React가 DOM에 변경사항을 커밋

- React는 **이전 엘리먼트 트리와 새로운 트리를 비교(diffing)**하여 변경된 부분만을 찾아낸다.
- 필요한 경우에만 실제 DOM을 업데이트하고, 브라우저에 화면을 다시 그리도록 요청한다.
- 이 DOM 업데이트 작업을 **커밋(commit)**이라고 부른다.

### 렌더링과 커밋은 별개의 단계

- 렌더링: 컴포넌트 실행 → JSX 생성 → 메모리에서 비교 준비
- 커밋: 변경된 부분이 있을 경우에만 → 실제 DOM 업데이트
- 렌더링이 발생해도 DOM이 바뀌지 않을 수 있으며, 변경 사항이 없다면 커밋도 발생하지 않는다.

<br />

## (4) 스냅샷으로서의 State

- React의 state는 렌더링 시점의 값에 고정된 스냅샷처럼 작동한다.
- state를 변경하더라도 현재 렌더링에는 영향을 주지 않고, 다음 렌더링에서 반영된다.

### React에서의 state 특징

- useState로 선언한 변수는 렌더링 시점의 고정된 값이다.
- 렌더링 중에는 이 값을 직접 변경할 수 없고, setState()를 통해 다음 렌더링을 예약한다.
- 즉, state는 "지금 이 UI에 반영되어야 할 값"의 스냅샷이다.

### 리렌더링 흐름 이해하기

- 컴포넌트 함수가 호출되며 렌더링 시작
- 이때의 state 값이 컴포넌트에 전달됨 (스냅샷)
- setState() 호출 → 즉시 값이 바뀌는 게 아니라 업데이트 예약
- React는 렌더링 종료 후, 새 state로 다시 렌더링

```jsx
<form onSubmit={(e) => {
  e.preventDefault();
  setIsSent(true);    // 리렌더링 예약 (다음 렌더링에서 isSent = true)
  sendMessage(message);
}}>
```

- `setIsSent(true)`는 즉시 값을 바꾸지 않고, 다음 렌더링에서 isSent를 true로 반영해달라고 예약하는 역할을 한다.
- 이후 React는 컴포넌트를 다시 호출해 새로운 UI 스냅샷을 만들고, 이를 DOM에 반영한다.

### state 업데이트 시기 및 방법: 비동기 & 일괄 처리

- React는 여러 setState() 호출을 묶어서(batch) 한 번에 처리한다.
- 렌더링 중의 state는 고정되어 있기 때문에 state를 여러 번 변경하더라도, 모두 같은 기준값을 참조하게 된다.

```tsx
// setNumber(1)이 세 번 호출된 것과 동일하며, 최종적으로는 1만 렌더링
// 잘못된 방식 (number는 고정된 스냅샷)
const wrongUpdate = () => {
  setNumber(number + 1);
  setNumber(number + 1);
  setNumber(number + 1); // 결과는 1
};

// 올바른 방식 (함수형 업데이트)
const correctUpdate = () => {
  setNumber((n) => n + 1);
  setNumber((n) => n + 1);
  setNumber((n) => n + 1); // 결과는 3
};
```

### setState 직후 값이 반영되지 않는 이유

- 이벤트 핸들러도 렌더링 시점에 정의되므로, 그 안에서 사용되는 state 값도 스냅샷이다.
- `setState()` 이후 바로 값을 참조하더라도 변경된 값을 바로 알 수 없다(이전 값 출력).

```jsx
// 경고창에는 5가 아닌 이전 값인 0이 출력된다.
setNumber(number + 5);
alert(number); // 여전히 이전 값 출력

// 비동기적으로 setTimeout으로 알림을 보여줘도 마찬가지다.
setNumber(number + 5);
setTimeout(() => {
  alert(number); // 여전히 이전 렌더링의 0을 참조
}, 3000);
```

### 이벤트 핸들러 내에서 state를 바로 쓰고 싶을 땐?

state의 최신 값을 기준으로 동작하려면 다음과 같은 방식이 필요하다.

- 함수형 업데이트 사용
- useEffect로 변경 후 동작 정의

```jsx
<button onClick={() => {
  setNumber(number + 1); // number는 현재 렌더링의 값
  alert(number); // 변경되지 않음
}}>
```

```jsx
// 최신 값을 기반으로 누적
setNumber((n) => n + 1);
```

<br />

## (5) State 업데이트 큐

### 상태 업데이트 큐 (State Update Queue)

- React는 상태를 즉시 변경하지 않고, 상태 변경 요청을 **큐(queue)**에 저장한다.
- 컴포넌트는 렌더링 중에는 상태를 변경할 수 없기 때문에, React는 모든 setState() 호출을 큐에 넣고, 이후에 일괄 처리한다.
- 최종 상태는 큐에 저장된 모든 업데이트를 순차적으로 적용한 결과다.

### Batching (일괄 처리)

- 여러 개의 상태 업데이트를 한 번의 렌더링으로 처리하는 방식이다.
- 이는 렌더링 횟수를 줄여 성능을 최적화한다.
- React는 이벤트 핸들러, useEffect, 비동기 처리 등에서 상태 업데이트를 자동으로 batching한다.

### 여러 번 호출된 setState

최신 값을 기준으로 연산이 필요한 경우에는 함수형 업데이트를 사용한다.

```tsx
setNumber((n) => n + 1); // “state 값으로 무언가를 하라”고 지시하는 방법
setNumber((n) => n + 1);
setNumber((n) => n + 1);
```

- React는 이 함수들을 큐에 저장하고 이전 상태를 기반으로 각 함수를 실행해 최종 상태를 계산한다.

```tsx
const [count, setCount] = useState<number>(0);

const handleClick = () => {
  setCount((prev) => prev + 1);
  setCount((prev) => prev + 1);
  setCount((prev) => prev + 1);
};

// 실제 실행 로직은 다음과 같음
const queue = [
  (n: number) => n + 1,
  (n: number) => n + 1,
  (n: number) => n + 1,
];
let state = 0;
for (const update of queue) {
  state = update(state);
}
// state === 3
```

### 업데이터 함수 인수의 이름 규칙

업데이터 함수 인수의 이름은 해당 state 변수의 첫 글자로 지정하는 것이 일반적이다.

```tsx
setEnabled((e) => !e);
setLastName((ln) => ln.reverse());
setFriendCount((fc) => fc * 2);
```

<br />

## (6) 객체 State 업데이트하기

- React의 state는 불변성을 유지해야 한다.
- 기존 객체를 직접 수정하지 않고, 새로운 객체를 생성하여 업데이트해야 리렌더링이 제대로 일어난다.
- React는 상태 변경 여부를 **얕은 비교(shallow comparison)**로 판단한다.

```tsx
const [position, setPosition] = useState({ x: 0, y: 0 });

// position이라는 기존 객체를 직접 수정함
// setPosition()을 호출하지 않았고, 기존 객체 참조를 유지 → React가 변경 감지 못함
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}}

// 기존 객체를 복사하거나 새로운 객체를 생성해서 setPosition에 전달
// React는 새 객체로 인식하고 컴포넌트를 리렌더링함
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```

### 지역 변경

```tsx
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);

setPosition({
  x: e.clientX,
  y: e.clientY,
});
```

이 방식은 새로운 객체를 만들어서 수정한 후 상태로 설정하므로 문제 없음

### 전개 연산자(Spread)로 객체 복사

폼에서 단 한 개의 필드만 수정하고, 나머지 모든 필드는 이전 값을 유지하고 싶을 때

```tsx
setPerson({
  firstName: e.target.value, // input의 새로운 first name
  lastName: person.lastName,
  email: person.email,
});

setPerson({
  ...person, // 이전 필드를 복사
  firstName: e.target.value, // 새로운 부분은 덮어쓰기
});
```

```tsx
function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input
          name="email"
          value={person.email}
          onChange={handleChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

### 중첩된 객체 갱신하기

```tsx
const [person, setPerson] = useState({
  name: "Niki de Saint Phalle",
  artwork: {
    title: "Blue Nana",
    city: "Hamburg",
    image: "https://i.imgur.com/Sd1AgUOm.jpg",
  },
});

// city를 바꾸기 위해서는 먼저 새로운 artwork 객체를 생성한 뒤, 그것을 가리키는 새로운 person 객체를 만들어야 한다.
const nextArtwork = { ...person.artwork, city: "New Delhi" };
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);

setPerson({
  ...person, // 다른 필드 복사
  artwork: {
    // artwork 교체
    ...person.artwork, // 동일한 값 사용
    city: "New Delhi", // 하지만 New Delhi!
  },
});
```

### Immer로 간결한 갱신 로직 작성하기

- Immer는 편리하고, 변경 구문을 사용할 수 있게 해주며 복사본 생성을 도와주는 인기 있는 라이브러리
- Immer를 사용하면 작성한 코드는 “법칙을 깨고” 객체를 변경하는 것처럼 보일 수 있다.

```tsx
updatePerson((draft) => {
  draft.artwork.city = "Lagos";
});
```

<br />

## (7) 배열 State 업데이트하기

React에서는 상태(state)가 변경될 때 UI가 자동으로 업데이트되지만, 배열을 상태로 다룰 때는 배열을 **불변성**을 지키며 변경해야 한다.

### 배열 업데이트의 기본 개념

- 배열을 직접 수정하는 대신, **새로운 배열을 만들어서** 상태를 업데이트해야 한다.
- 불변성을 지키기 위해, 기존 배열을 변경하지 않고 새로운 배열을 만들어서 상태를 업데이트한다.

|      | 비선호 (배열을 변경)      | 선호 (새 배열을 반환)        |
| ---- | ------------------------- | ---------------------------- |
| 추가 | push, unshift             | concat, [...arr] 전개 연산자 |
| 제거 | pop, shift, splice        | filter, slice                |
| 교체 | splice, arr[i] = ... 할당 | map                          |
| 정렬 | reverse, sort             | 배열을 복사한 이후 처리      |

### 배열에 항목 추가하기

```jsx
const [items, setItems] = useState([1, 2, 3]);

const addItem = (newItem) => {
  setItems((prevItems) => [...prevItems, newItem]);
};
```

### 배열에서 항목 제거하기

```jsx
const [items, setItems] = useState([1, 2, 3]);

const removeItem = (itemToRemove) => {
  setItems((prevItems) => prevItems.filter((item) => item !== itemToRemove));
};
```

### 배열 변환하기

```jsx
const [numbers, setNumbers] = useState([1, 2, 3]);

const doubleNumbers = () => {
  setNumbers((prevNumbers) => prevNumbers.map((number) => number * 2));
};
```

### 배열 내 항목 교체하기

```jsx
const [items, setItems] = useState(["apple", "banana", "cherry"]);

const replaceItem = (oldItem, newItem) => {
  setItems((prevItems) =>
    prevItems.map((item) => (item === oldItem ? newItem : item))
  );
};
```

### 배열에 항목 삽입하기

```jsx
const [items, setItems] = useState(["apple", "banana", "cherry"]);

const insertItemAt = (index, newItem) => {
  setItems((prevItems) => [
    ...prevItems.slice(0, index),
    newItem,
    ...prevItems.slice(index),
  ]);
};
```

### 배열에 기타 변경 적용하기

```jsx
const [items, setItems] = useState(["apple", "banana", "cherry"]);

const reverseItems = () => {
  setItems((prevItems) => [...prevItems].reverse());
};

const sortItems = () => {
  setItems((prevItems) => [...prevItems].sort());
};
```

### 배열 내부의 객체 업데이트하기

```jsx
const [users, setUsers] = useState([
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
]);

const updateUserName = (id, newName) => {
  setUsers((prevUsers) =>
    prevUsers.map((user) =>
      user.id === id ? { ...user, name: newName } : user
    )
  );
};
```

### Immer로 간결한 업데이트 로직 작성하기

- Immer를 사용하면 복잡한 상태 업데이트 로직을 더 간결하고 직관적으로 작성할 수 있다.

```jsx
import produce from "immer";

const [users, setUsers] = useState([
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
]);

const updateUserName = (id, newName) => {
  setUsers((prevUsers) =>
    produce(prevUsers, (draft) => {
      const user = draft.find((user) => user.id === id);
      if (user) {
        user.name = newName;
      }
    })
  );
};
```
