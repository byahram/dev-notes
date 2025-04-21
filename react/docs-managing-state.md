---
title: "[리액트 공식 문서 정리] 3. State 관리하기"
date: "2025-04-14"
category: "react"
tags: ["react", "official docs"]
---

<https://ko.react.dev/learn/managing-state>

## 목차

- [(1) State를 사용해 Input 다루기: UI를 선언적인 방식으로 생각하기]()
- []()
- []()
- []()
- []()
- []()
- []()

## (1) State를 사용해 Input 다루기: UI를 선언적인 방식으로 생각하기

### 1) 컴포넌트의 다양한 시각적 state 확인하기

사용자가 볼 수 있는 UI의 모든 “state”를 시각화해야 한다.

- **empty**: 입력값 없음, 비활성화된 제출 버튼
- **typing**: 입력 중, 활성화된 제출 버튼
- **submitting**: 제출 중, 비활성화 + 로딩 스피너
- **success**: 성공 메시지 표시
- **error**: 오류 메시지 표시

### 2) 무엇이 state 변화를 트리거하는지 알아내기

상태 변화는 두 가지로 발생한다:

- **휴먼 인풋**: 입력, 클릭 등
  - 이벤트 핸들로가 필요할 수도 있다.
- **컴퓨터 인풋**: 응답, 타이머 등

예시 흐름:

- 텍스트 인풋 변경 시(휴먼) → typing
- 제출 버튼 클릭(휴먼) → submitting
- 성공 응답(컴퓨터) → success
- 실패 응답(컴퓨터) → error

### 3) 메모리의 state를 useState로 표현하기

필요한 상태만 useState로 구현한다.

```jsx
const [answer, setAnswer] = useState("");
const [error, setError] = useState(null);
const [status, setStatus] = useState("typing"); // 'typing' | 'submitting' | 'success'

// 좋은 방법이 곧바로 떠오르지 않는다면 모든 시각적 state를 커버할 수 있는 확실한 것을 먼저 추가한다.
const [isEmpty, setIsEmpty] = useState(true);
const [isTyping, setIsTyping] = useState(false);
const [isSubmitting, setIsSubmitting] = useState(false);
const [isSuccess, setIsSuccess] = useState(false);
const [isError, setIsError] = useState(false);
```

### 4) 불필요한 state 변수를 제거하기

상태 간 모순 가능성을 줄이고 버그를 예방한다.

- isEmpty → answer.length === 0
- isError → error !== null
- isSuccess → error === null
- 여러 boolean → 하나의 status로 통합

### 5) state 설정을 위해 이벤트 핸들러를 연결하기

명령형 코드보다 길어도 유지보수성과 확장성이 높다.

- 입력 시: setAnswer, setStatus('typing')
- 제출 시: setStatus('submitting'), async 요청
- 응답 성공: setStatus('success')
- 응답 실패: setError(...), setStatus('typing')

## (2) State 구조 선택하기: State 구조화 원칙

### 1) 연관된 state 그룹화하기

- 두 개의 state 변수가 항상 함께 변경된다면, 단일 state 변수로 통합하는 것이 좋다.
- state 변수가 객체인 경우 다른 필드를 명시적으로 복사하지 않고 하나의 필드만 업데이트할 수 없다.
  - ex) `setPosition({ ...position, x: 100 })`

```jsx
// 단일
const [x, setX] = useState(0);
const [y, setY] = useState(0);

// 다중
const [position, setPosition] = useState({ x: 0, y: 0 });
```

### 2) State의 모순 피하기

- 여러 state 조각이 서로 모순될 수 있는 구조는 피해야 한다.
- ex: `isSending`과 `isSent`을 동시에 true로 설정하면 모순이 발생할 수 있다.

```jsx
// isSending & isSent
const [isSending, setIsSending] = useState(false);
const [isSent, setIsSent] = useState(false);

async function handleSubmit(e) {
  e.preventDefault();
  setIsSending(true);
  await sendMessage(text);
  setIsSending(false);
  setIsSent(true);
}

// status
const [status, setStatus] = useState("typing"); // 'typing' | 'sending' | 'sent'

async function handleSubmit(e) {
  e.preventDefault();
  setStatus("sending");
  await sendMessage(text);
  setStatus("sent");
}
```

### 3) 불필요한 state 피하기

- 컴포넌트의 props나 기존 state에서 계산 가능한 정보는 별도의 state로 관리하지 않습니다.
- ex: 리스트의 길이를 별도로 state로 관리하지 않고, items.length로 계산합니다.
- ex: lastName state을 별도로 가지지 않고 firstName, lastName state을 이용한다.

### 4) State의 중복 피하기

- 동일한 데이터를 여러 state 변수에 중복 저장하지 않습니다.
- 중복된 state는 동기화 문제를 일으킬 수 있습니다.

```jsx
const initialItems = [
  { title: "pretzels", id: 0 },
  { title: "crispy seaweed", id: 1 },
  { title: "granola bar", id: 2 },
];

// 중복된 예시
const [items, setItems] = useState(initialItems);
const [selectedItem, setSelectedItem] = useState(items[0]);

// 수정 후
const [items, setItems] = useState(initialItems);
const [selectedId, setSelectedId] = useState(0);
```

### 5) 깊게 중첩된 state 피하기

- 깊게 중첩된 구조는 업데이트가 어렵고 오류를 유발할 수 있다.
- 가능한 한 평탄한 구조로 state를 구성한다.

## (3) 컴포넌트 간 State 공유하기: State 끌어올리기

두 컴포넌트의 상태가 항상 함께 변경되어야 할 경우, 각 자식 컴포넌트에서 상태를 제거하고 가장 가까운 공통 부모 컴포넌트로 상태를 옮긴 후, props를 통해 자식 컴포넌트에 전달하는 방식이다.

### Accordion 컴포넌트 예시

```jsx
import { useState } from "react";

// 1. 자식 컴포넌트 Panel에서 state 제거하고, prop에 isActive 추가
function Panel({ title, children, isActive }) {
  // const [isActive, setIsActive] = useState(false);
  return <section className="panel">Panel</section>;
}

export default function Accordion() {
  // 3. 공통 부모에 state 추가하기
  const [activeIndex, setActiveIndex] = useState(0);

  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      {/* 2. 하드 코딩된 데이터를 부모 컴포넌트로 전달하기 */}
      <Panel
        title="About"
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        panel1
      </Panel>
      <Panel
        title="Etymology"
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        panel2
      </Panel>
    </>
  );
}
```

1. 자식 컴포넌트에서 state 제거하기

   - 각 Panel 컴포넌트에서 isActive 상태를 제거하고, 대신 isActive를 props로 받도록 수정한다.

2. 하드 코딩된 데이터를 부모 컴포넌트로 전달하기

   - Accordion 컴포넌트에서 어떤 패널이 활성화되어야 하는지를 결정하고, 해당 정보를 각 Panel에 props로 전달한다.

3. 공통 부모에 state 추가하기

   - Accordion 컴포넌트에 activeIndex 상태를 추가하여 현재 활성화된 패널의 인덱스를 추적하고, 이를 기반으로 각 Panel의 isActive 값을 설정합니다.

## (4) State를 보존하고 초기화하기

### React는 컴포넌트를 트리 상의 위치(Tree position)로 식별한다

- 상태는 DOM 요소나 props가 아닌, **React Element 트리에서의 위치**에 따라 유지된다.
- 같은 위치에 렌더링되면 상태 유지, 위치가 바뀌면 초기화된다.
- 렌더링할 때 state를 유지하고 싶다면, **트리 구조가 같아야** 한다.

### 상태가 보존되는 경우

1. 같은 컴포넌트를 동일한 위치에 두 번 렌더링

   - JSX에서 각 counter는 다른 위치이므로 서로 다른 상태를 가진다.

     ```jsx
     const counter = <Counter />;

     return (
       <div>
         {counter}
         {counter}
       </div>
     );
     ```

2. 조건부 렌더링 시 위치가 고정된 경우

   - 각각의 <Counter />는 고정된 위치에 있어 상태가 보존된다.

     ```jsx
     const [showB, setShowB] = useState(true);

     return (
       <div>
         <Counter />
         {showB && <Counter />}
       </div>
     );
     ```

3. 동일한 컴포넌트를 props만 바꿔 조건부 렌더링

   - 같은 자리의 같은 컴포넌트는 state를 보존합니다.
   - 두 경우 모두 동일한 위치에 동일한 컴포넌트가 렌더링되므로 상태가 보존된다.

     ```jsx
     const [isFancy, setIsFancy] = useState(false);

     return (
       <div>
         {isFancy ? <Counter isFancy={true} /> : <Counter isFancy={false} />}
       </div>
     );
     ```

### 상태가 초기화되는 경우

1. 같은 위치에 다른 컴포넌트를 렌더링

   - 서로 다른 컴포넌트 (`<p>` vs `<Counter />`)를 같은 위치에 조건부 렌더링하면, React는 다른 트리로 판단하여 상태를 초기화한다.

     ```jsx
     const [isPlayerA, setIsPlayerA] = useState(true);

     return (
       <div>
         {isPlayerA ? <Counter person="Taylor" /> : <Counter person="Sarah" />}
       </div>
     );
     ```

### 같은 위치에서 같은 컴포넌트를 상태 분리(초기화)해야 하는 경우

- 아래 예시처럼 `<Counter />` 컴포넌트를 같은 위치에서 props만 바꿔 렌더링하면, React는 같은 컴포넌트 인스턴스로 인식 → 상태가 유지됨
- 하지만 Taylor와 Sarah를 각각 다른 사람으로 간주하고 각자의 상태를 따로 유지하고 싶다면, 상태가 초기화되도록 명시적으로 분리해야 한다.

  1. 해결 방법 1: 다른 위치에 렌더링
  2. 해결 방법 2. key 사용으로 명시적 초기화

  ```jsx
  const [isPlayerA, setIsPlayerA] = useState(true);

  return (
    {/* 초기화 발생 가능 */}
    <div>
      {isPlayerA ? <Counter person="Taylor" /> : <Counter person="Sarah" />}
    </div>

    {/* 해결 방법 1. 다른 위치에 렌더링 */}
    {/* 두 컴포넌트를 완전히 분리해 각각 고유 위치에 렌더링 → 상태 보존된다. */}
    <div>
      {isPlayerA && <Counter person="Taylor" />}
      {!isPlayerA && <Counter person="Sarah" />}
    </div>

    {/* 해결 방법 2. key 사용으로 명시적 초기화 */}
    {/* 각 컴포넌트에 고유한 key를 부여하면, React는 상태를 새로 초기화한다. */}
    {/* key가 다르면 React는 새 인스턴스로 판단하여 기존 상태를 버리고 초기화한다. */}
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
    </div>
  );
  ```

## (5) State 로직을 리듀서로 작성하기

###

###

###

###

## (6) Context를 사용해 데이터를 깊게 전달하기

## (7) Reducer와 Context로 앱 확장하기
