---
title: "[리액트 공식 문서 정리] 3. State 관리하기"
date: "2025-04-14"
category: "react"
tags: ["react", "official docs"]
---

<https://ko.react.dev/learn/managing-state>

## 목차

- [(1) State를 사용해 Input 다루기: UI를 선언적인 방식으로 생각하기](#1-state를-사용해-input-다루기-ui를-선언적인-방식으로-생각하기)
- [(2) State 구조 선택하기: State 구조화 원칙](#2-state-구조-선택하기-state-구조화-원칙)
- [(3) 컴포넌트 간 State 공유하기: State 끌어올리기](#3-컴포넌트-간-state-공유하기-state-끌어올리기)
- [(4) State를 보존하고 초기화하기](#4-state를-보존하고-초기화하기)
- [(5) State 로직을 reducer로 작성하기: reducer로 state를 다루는 방법](#5-state-로직을-reducer로-작성하기-reducer로-state를-다루는-방법)
- [(6) Context를 사용해 데이터를 깊게 전달하기](#6-context를-사용해-데이터를-깊게-전달하기)
- [(7) Reducer와 Context로 앱 확장하기](#7-reducer와-context로-앱-확장하기)

<br />

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

<br />

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

<br />

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

<br />

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

<br />

## (5) State 로직을 reducer로 작성하기: reducer로 state를 다루는 방법

### useStae -> useReducer 방법

#### 1. state를 설정하는 것에서 action을 dispatch 함수로 전달하는 것으로 바꾸기

- 기존 useState 함수 : state를 설정하여 React에게 “무엇을 할 지”를 지시

```jsx
function handleAddTask(text) {}
function handleChangeTask(task) {}
function handleDeleteTask(taskId) {}

return (
  <>
    <h1>Prague itinerary</h1>
    <AddTask onAddTask={handleAddTask} />
    <TaskList
      tasks={tasks}
      onChangeTask={handleChangeTask}
      onDeleteTask={handleDeleteTask}
    />
  </>
);
```

- useReducer 함수 : `action`을 전달하여 “사용자가 방금 한 일”을 지정
  - dispatch 함수에 넣어준 객체를 `action`이라고 한다.
  - dispatch 함수는 액션 객체(action)를 전달받아 상태를 어떻게 업데이트할지 결정한다.

```jsx
function handleAddTask(text) {
  dispatch(
    // "action" 객체:
    {
      type: "added",
      id: nextId++,
      text: text,
    }
  );
}

function handleChangeTask(task) {
  dispatch({ type: "changed", ~ });
}

function handleDeleteTask(taskId) {
  dispatch({ type: "deleted", ~ });
}
```

#### 2. reducer 함수 작성하기

- if/else 문을 사용해도 되지만 reducer 함수에서는 switch 문을 자주 사용하며, 가독성에 도움이 된다
- reducer 함수 : (현재 state, action 객체) 선언

```jsx
function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

#### 3. 컴포넌트에서 reducer 사용하기

- useReducer hook은 두 개의 인자를 넘겨받고

  1. reducer 함수
  2. 초기 state 값

- 두개의 인자를 반환한다.
  1.  state를 담을 수 있는 값
  2.  dispatch 함수 (사용자의 action을 reducer 함수에게 “전달하게 될”)

```jsx
// useState()
const [tasks, setTasks] = useState(initialTasks);

// useReducer()
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```

### useState vs useReducer

| 항목        | useState                 | useReducer                               |
| ----------- | ------------------------ | ---------------------------------------- |
| 코드 크기   | 짧고 간단한 코드         | 초기 작성 코드가 많지만 반복 줄이기 유리 |
| 가독성      | 단순한 경우 우수         | 복잡한 로직일수록 더 명확함              |
| 디버깅      | 흐름 추적 어려울 수 있음 | action 기반 디버깅이 쉬움                |
| 테스팅      | 컴포넌트에 종속적        | 순수 함수로 독립적인 테스트 가능         |
| 복잡한 로직 | 처리 어려움              | 복잡한 상태 로직 처리에 유리함           |
| 상태 변경   | setState로 직접 업데이트 | action 객체를 dispatch로 전달            |
| 코드 구성   | 직관적이고 간단함        | 구조적이며 유지보수에 적합               |
| 추천 상황   | 단순한 상태              | 다양한 동작(action)이 필요한 상태        |

### reducer 잘 작성하기

1. 순수 함수로 작성하기
   - 같은 입력(state, action)에 대해서는 항상 **같은 출력**(newState)을 반환해야 한다.
   - **사이드 이펙트**를 포함하면 안 된다.
   - 배열이나 객체를 **직접 변경하지 말고**, 불변성을 유지한 상태로 업데이트해야 한다.
2. 하나의 action은 하나의 사용자 상호작용을 설명해야 함
   - **모든 action은 명확한 의도**를 가지고 있어야 하며, 로그를 통해 상호작용의 흐름을 쉽게 추적할 수 있어야 한다.

### Immer로 간결한 reducer 작성하기

<br />

## (6) Context를 사용해 데이터를 깊게 전달하기

### Context란?

- Context는 React 컴포넌트 트리 전체에 데이터를 전달할 수 있게 해주는 방법이다.
- props를 여러 단계에 걸쳐 전달하지 않아도 하위 컴포넌트가 값을 읽을 수 있다.
- context는 정적인 값뿐 아니라 상태(state)나 변경 가능한 값도 전달 가능하다.
- 렌더링마다 context 값이 변경되면 관련된 컴포넌트들이 자동으로 업데이트된다.

### Context: Props 전달하기의 대안

1. Context 생성하기

   - `createContext()`는 context 객체를 생성
   - 인수로는 기본값을 전달 (context provider가 없을 때 사용됨)

     ```jsx
     import { createContext } from "react";

     export const LevelContext = createContext(1);
     ```

2. Context 사용하기

   - `useContext(Context)`를 사용해서 context 값을 가져옴
   - 반드시 React 컴포넌트의 최상단에서 사용해야 함 (조건문/반복문 내부는 X)

     ```jsx
     import { useContext } from "react";
     import { LevelContext } from "./LevelContext.js";

     export default function Heading({ level, children }) {
       const level = useContext(LevelContext);

       const Tag = `h${level}`;
       return <Tag>{children}</Tag>;
     }
     ```

3. Context 제공하기

   - Context.Provider를 사용해서 하위 트리에 값을 제공

     ```jsx
     import { LevelContext } from "./LevelContext.js";

     export default function Section({ level, children }) {
       return (
         <section>
           <LevelContext.Provider value={level}>
             {children}
           </LevelContext.Provider>
         </section>
       );
     }
     ```

4. 같은 컴포넌트에서 context를 사용하며 제공하기

   - 각 Section은 자신보다 한 단계 위의 레벨을 Context로부터 받아서, 자식에게 level + 1을 자동으로 전달한다.
   - 이제 `<Heading>`과 `<Section>` 모두 context를 통해 깊이를 자동으로 판단 가능하다.

     ```jsx
     import { useContext } from "react";
     import { LevelContext } from "./LevelContext.js";

     export default function Section({ children }) {
       const level = useContext(LevelContext);

       return (
         <section className="section">
           <LevelContext value={level + 1}>{children}</LevelContext>
         </section>
       );
     }
     ```

### Context로 중간 컴포넌트 지나치기

- context는 중간에 `<div>`나 다른 커스텀 컴포넌트가 있어도 상관없이 동작한다.
  - 예: Heading 컴포넌트가 어디 있든 가장 가까운 LevelContext를 참조한다.

### Context를 사용하기 전에 고려할 것

1. Props 전달하기로 충분한 경우

   - 여러 props가 여러 단계를 거쳐도 괜찮은 구조일 수 있다.
     - 깊이 1~2단계라면 props 전달로도 충분하다.
   - 데이터 흐름이 명확해져 유지보수가 쉽다.

2. 중간 컴포넌트가 데이터를 사용하지 않는 경우

   - 중간 컴포넌트를 함수로 추출하고 children으로 넘기면 간단해진다.

     ```jsx
     <!-- 비추천 -->
     <Layout posts={posts} />
     ```

     ```jsx
     <!-- 추천 -->
     <!-- 이 구조가 중간 컴포넌트를 줄이기 쉬움 -->
     function Layout({ children }) {
       return <main>{children}</main>;
     }

     <Layout>
       <Post />
     </Layout>;
     ```

### Context 사용 예시

- **테마 지정하기**: 다크 모드 / 라이트 모드 같은 시각적 테마 전달
- **현재 계정**: 인증된 사용자 정보 전달
- **라우팅**: 현재 페이지 위치 전달
- **상태 관리**: 앱 전역 상태(예: 장바구니, 알림 상태 등)를 여러 컴포넌트에 전달

<br />

## (7) Reducer와 Context로 앱 확장하기

- `useReducer`: 상태 업데이트 로직을 하나로 통합
- `Context`: 상태와 dispatch 함수를 깊은 트리 구조에 전달 (prop drilling 제거)

### 1단계. Context 생성

- 상태 업데이트를 하나의 reducer 함수로 통합해 관리한다.
- tasksReducer는 added, changed, deleted 액션 처리

  ```jsx
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  // 현재 tasks 리스트를 제공
  export const TasksContext = createContext(null);

  // action을 dispatch하는 함수 제공
  export const TasksDispatchContext = createContext(null);
  ```

### 2단계. State과 dispatch 함수를 context에 넣기

```jsx
import { useReducer } from "react";
import { TasksContext, TasksDispatchContext } from "./TasksContext.js";
import { tasksReducer, initialTasks } from "./tasksReducer.js";

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        <h1>Day off in Kyoto</h1>
        <AddTask />
        <TaskList />
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

### 3단계. 트리 안에서 context 사용하기

```jsx
import { useContext } from "react";
import { TasksContext, TasksDispatchContext } from "./TasksContext.js";

const tasks = useContext(TasksContext);
const dispatch = useContext(TasksDispatchContext);
```

### 리팩토링: Provider 분리

```jsx
export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

```jsx
function TaskApp() {
  return (
    <TasksProvider>
      <AddTask />
      <TaskList />
    </TasksProvider>
  );
}
```
