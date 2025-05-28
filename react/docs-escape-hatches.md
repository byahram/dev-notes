---
title: "[리액트 공식 문서 정리] 4. 탈출구"
date: "2025-05-12"
category: "react"
tags: ["react", "official docs"]
---

<https://react.dev/learn/escape-hatches>

## 목차

- [(1) Ref로 값 참조하기]()
- [(2) Ref로 DOM 조작하기]()
- [(3) Effect로 동기화하기]()
- [(4) Effect가 필요하지 않은 경우]()
- [(5) 반응형 effects의 생명주기]()
- [(6) Effect에서 이벤트 분리하기]()
- [(7) Effect 의존성 제거하기]()
- [(8) 커스텀 Hook으로 로직 재사용하기]()

<br />

## (1) Ref로 값 참조하기

### Refs란?

- React에서 DOM 요소나 값에 직접 접근할 수 있게 해주는 Hook (useRef).
- 이벤트 핸들러에서만 필요한 정보이고, 값 변경 시 리렌더링이 필요 없을 때 사용.
- useRef(initialValue) → { current: initialValue } 객체 반환.
- 숫자, 문자열, 객체, 함수 등 무엇이든 저장할 수 있음.
- State와 달리 변경해도 리렌더링이 발생하지 않음.
- **단방향 데이터 흐름을 벗어나는 수단(escape hatch)**

### 컴포넌트에 Ref를 추가하기

- ref.current를 통해 값에 접근.
- React가 추적하지 않으므로 변경해도 렌더링되지 않음.

```jsx
// useRef Hook을 가져와 컴포넌트에 Ref를 추가
import { useRef } from "react";

export default function Counter() {
  // useRef Hook을 호출하고 참조할 초깃값을 유일한 인자로 전달
  let ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
  }

  return <button onClick={handleClick}>Click me!</button>;
}
```

### Ref와 State의 차이점

| Ref                                         | State                                        |
| ------------------------------------------- | -------------------------------------------- |
| { current: initialValue } 객체 반환         | 재 값과 Setter 함수 [value, setValue]를 반환 |
| 값 변경해도 리렌더링 안 됨                  | 값 변경하면 리렌더링 발생                    |
| current 값 직접 수정 (mutable)              | set함수를 통해 변경 (immutable)              |
| 렌더링 중에는 current 읽기/쓰기 X           | 언제든 읽을 수 있으나 각 렌더에 고유함       |
| 직접 연결되지 않거나 DOM·외부 시스템 참조용 | UI와 직접 연결된 값 관리용                   |

### Ref를 언제 사용할까?

- 브라우저 API, 외부 라이브러리 등 React 외부와 통신할 때.
- 렌더링과 무관한 값 저장 (예: DOM 참조, 타이머 ID, 이전 값).
- 렌더링 중 필요한 정보라면 Ref 대신 State 사용.
- Ref는 즉시 변경 가능, State는 렌더마다 고유한 Snapshot 유지.

<br />

## (2) Ref로 DOM 조작하기

### Ref를 언제, 왜 써야 할까?

- 특정 노드에 포커스 이동, 스크롤 위치 변경, 위치·크기 측정 등 React가 직접 제공하지 않는 작업을 할 때 필요하다.
- React의 **탈출구(Escape hatch)**로, 꼭 필요한 경우에만 사용할 것.
- 주로 포커스 관리, 스크롤 제어, 브라우저 API 호출 등에서 사용.
- React가 관리하는 DOM 노드는 직접 수정하지 말고, 수정이 필요하다면 React가 건드릴 이유가 없는 부분만 건드리는게 좋다.

### ref로 노드 가져오기

- `useRef`로 ref 객체를 만들고, JSX에서 `<div ref={myRef}>`처럼 전달.
- 초기에는 `myRef.current`가 `null`이며, React가 해당 DOM 노드를 만들면 그 참조가 `myRef.current`에 들어감.

```jsx
// 1. useRef Hook 가져오기
import { useRef } from "react";

// 2. Hook 사용
const myRef = useRef(null);

// 3. JSX에서 ref 전달
<div ref={myRef}></div>;

// 4. 이벤트나 브라우저 API에서 사용
function scrolling() {
  myRef.current.scrollIntoView();
}
```

- React는 커밋 단계에서 `ref.current`를 설정한다:
  - DOM 변경 전 → null로 초기화.
  - DOM 변경 후 → 해당 노드로 업데이트.
- 대부분의 ref 사용은 이벤트 핸들러 안에서 처리한다.
- 특정 이벤트가 없으면 useEffect 안에서 처리해야 할 수도 있다.

### 다른 컴포넌트의 DOM 노드 접근하기

- **내장 컴포넌트** (예: `<input />`)에 ref를 주면 → `ref.current`는 실제 DOM 노드를 가리킨다.
- **사용자 정의 컴포넌트** (예: `<MyInput />`)에 ref를 바로 주면 → 기본적으로 null.
- **부모 컴포넌트에서 자식 컴포넌트로** ref를 prop처럼 전달하려면:

  - 부모에서 useRef()로 ref 생성.
  - 자식에게 prop으로 넘기고, 자식에서 이걸 내장 컴포넌트에 연결.

    ```jsx
    import { useRef } from "react";

    // 자식 컴포넌트
    function MyInput({ ref }) {
      return <input ref={ref} />;
    }

    // 부모 컴포넌트
    function MyForm() {
      const inputRef = useRef(null);
      return <MyInput ref={inputRef} />;
    }
    ```

<br />

## (3) Effect로 동기화하기

### Effect란?

- 이벤트와 달리 Effect는 특정 상호작용이 아니라 렌더링 자체에 의해 발생한다.
- Effect는 React 외부 시스템(서버, 네트워크, 써드파티 API 등)과 컴포넌트를 동기화할 때 사용한다.
- 예: 서버 연결 설정, 분석용 로그 전송, React로 작성되지 않은 위젯 제어, 애니메이션 트리거, 데이터 페칭.
- 기본적으로 모든 렌더링(초기 렌더링 포함) 후 실행한다.

### 컴포넌트에서 Effect를 선언하는 방법

1. Effect 선언하기

   - `useEffect`는 렌더링 후(커밋 후) 실행되며 코드 실행을 “지연”시킨다.

2. Effect의 의존성 지정하기

   - `useEffect` 호출 시 두 번째 인자로 의존성 배열 지정한다.
   - 모든 렌더링 후 실행할 필요는 없음 → 의존성 배열로 실행 시점 제어한다.
     - 빈 배열(`[]`): 컴포넌트 “마운팅” 시 한 번만 실행한다.
     - `[a, b]`: 마운트 시 실행 + a 또는 b가 변경될 때 실행한다.
     - 의존성이 동일할 경우 React는 Effect를 건너뛴다.
     - 의존성은 내부 코드에서 자동으로 결정되며, 선택할 수 없다.

3. 클린업 함수 추가 (필요한 경우)
   - 연결 → 연결 해제, 구독 → 구독 취소, fetch → 취소/무시처럼, Effect에서 중단·취소·정리가 필요할 수 있다.
   - 이때 클린업 함수를 Effect에서 반환한다.

```jsx
// 1단계 : Effect를 선언
import { useEffect } from "react";

function MyComponent() {
  useEffect(() => {
    // Effect 실행 (모든 렌더링 후 실행됨)
    // useEffect는 화면에 렌더링이 반영될 때까지 코드 실행을 “지연”시킨다.

    return () => {
      // 클린업 실행 (중단·취소·정리)
    };
  }, [의존성]);
  // 2단계 : 의존성 지정
  // useEffect 안의 콜백은 렌더링 후 실행되며, 의존성 배열에 따라 실행 여부가 결정된다.
}
```

### 개발 중 주의할 점

- Strict Mode에서는 개발 환경에서 컴포넌트를 두 번 마운트해 Effect를 스트레스 테스트.
- Effect가 중단되면 React는 클린업 함수를 호출.
- 사용자 경험은 배포 환경(한 번 실행)과 개발 환경(설정 → 클린업 → 설정 반복)에서 동일해야 함 → 클린업 함수 구현 필수.

<br />

## (4) Effect가 필요하지 않은 경우

### 불필요한 Effect란?

- 렌더링을 위해 데이터를 변환하는 데 Effect가 필요하지 않음.
- 렌더링 후 외부 시스템(서버, 브라우저 API, 구독 등)과 동기화할 때만 필요.
- 내부 계산, 이벤트 처리, 단순 UI 변화에는 불필요.
- 불필요한 Effect는 코드 복잡도만 늘리고 성능에 손해.

### Effect가 필요 없는 상황

1. props 또는 state에 따라 state 업데이트하기

- 렌더링 중 계산 가능. Effect로 따로 state에 저장할 필요 없음.

```jsx
// 🔴 잘못된 방식 (Effect 불필요한데 사용함)
//  - firstName과 lastName이 바뀔 때마다 Effect로 fullName state를 업데이트.
//  - fullName은 그냥 계산된 값이므로 state로 따로 보관할 필요가 없음.
const [fullName, setFullName] = useState("");
useEffect(() => {
  setFullName(firstName + " " + lastName);
}, [firstName, lastName]);

// ✅ 좋은 방식 (렌더링 중 계산)
//  - fullName은 렌더링 중에 직접 계산할 수 있음.
//  - 별도의 state나 Effect 없이도 항상 최신 값 유지.
const fullName = firstName + " " + lastName;
```

2. 비용이 많이 드는 계산 캐싱하기

- 렌더링 중 실행되지만 useMemo로 캐싱, Effect 불필요.

```jsx
// 🔴 잘못된 방식 (불필요한 state + Effect)
//  - isibleTodos는 todos와 filter로부터 계산 가능한 값.
//  - 굳이 state로 따로 저장할 필요 없음.
const [visibleTodos, setVisibleTodos] = useState([]);
useEffect(() => {
  setVisibleTodos(getFilteredTodos(todos, filter));
}, [todos, filter]);

// ✅ 좋은 방식 (렌더링 중 계산)
//  - getFilteredTodos()가 무겁지 않다면, 매 렌더링마다 직접 계산해도 충분.
const visibleTodos = getFilteredTodos(todos, filter);

//  - 계산이 비싸다면 useMemo로 캐싱.
//  - todos나 filter가 바뀔 때만 다시 계산.
const visibleTodos = useMemo(() => {
  return getFilteredTodos(todos, filter);
}, [todos, filter]);
```

3. prop 변경 시 모든 state 초기화

- key prop 변경으로 React가 컴포넌트를 새로 만들게 하면 state 자동 초기화.

```jsx
// 🔴 잘못된 방식 (Effect로 state 초기화)
//  - userId prop이 변경될 때 Effect로 수동 초기화.
useEffect(() => {
  setComment("");
}, [userId]);

// ✅ 좋은 방식 (key 사용으로 자동 초기화)
//  - React는 key prop이 바뀌면 해당 컴포넌트와 내부 state를 완전히 새로 마운트함.
const [comment, setComment] = useState("");
<Profile userId={userId} key={userId} />;
```

4. prop이 변경될 때 일부 state 조정하기

- 렌더링 중 계산할 수 있는 것은 state로 따로 관리할 필요 없음.

```jsx
// 🔴 잘못된 방식 (Effect로 prop 변경 감지 후 state 조정)
//  - items prop이 변경될 때마다 Effect로 selection을 수동 리셋.
useEffect(() => {
  setSelection(null);
}, [items]);

// ✅ 좋은 방식 (렌더링 중 계산)
//  - 선택된 항목 ID만 state로 관리하고, 선택된 항목(selection)은 렌더링 중에 계산.
const [selectedId, setSelectedId] = useState(null);
const selection = items.find((item) => item.id === selectedId) ?? null;
```

5. 이벤트 핸들러 간 로직 공유

- 버튼 클릭 같은 이벤트는 이벤트 핸들러에서 직접 처리, Effect 불필요.

```jsx
// 🔴 잘못된 방식 (Effect 내부에서 이벤트성 로직 처리)
//  - product가 바뀔 때마다 무조건 알림을 띄움.
//  - 사용자의 특정 행동(예: 버튼 클릭)으로만 발생해야 할 알림을, Effect로 전역적으로 실행.
//  - 사용자 의도와 관계없이 잘못된 시점에 실행될 위험.
useEffect(() => {
  if (product.isInCart) {
    showNotification(`Added ${product.name} to the shopping cart!`);
  }
}, [product]);

// ✅ 좋은 방식 (이벤트 핸들러 안에서 로직 처리)
//  - 알림은 **사용자 행동(버튼 클릭)**으로만 발생.
//  - 이벤트 핸들러 안에서 로직을 직접 호출해 정확한 시점에 실행.
function buyProduct() {
  addToCart(product);
  showNotification(`Added ${product.name} to the shopping cart!`);
}
```

6. POST 요청 보내기

- 사용자의 특정 행동으로만 발생하므로 이벤트 핸들러 안에서 처리.

```jsx
// 🔴 잘못된 방식 (Effect로 이벤트성 POST 요청 처리)
//  - state 변화 감지로 POST 요청을 보내면, 실제 이벤트(예: 버튼 클릭) 대신 렌더링 흐름에 따라 발생.
//  - 사용자 행동에 의한 요청은 Effect가 아니라 이벤트 핸들러에서 처리해야 정확함.
const [jsonToSubmit, setJsonToSubmit] = useState(null);
useEffect(() => {
  if (jsonToSubmit !== null) {
    post("/api/register", jsonToSubmit);
  }
}, [jsonToSubmit]);

// ✅ 좋은 방식 (이벤트 핸들러에서 POST 처리)
//  - 폼 제출이라는 사용자 행동에서만 요청이 발생.
//  - 이벤트 핸들러 안에서 처리해 실행 시점이 명확하고, 불필요한 렌더링 감지 제거.
function handleSubmit(e) {
  e.preventDefault();
  post("/api/register", { firstName, lastName });
}
```

7. 연쇄 계산

- 여러 state(goldCardCount, round)가 연속 변할 때, Effect로 연결하지 말고 이벤트 핸들러에서 한 번에 처리.
- 계산은 이벤트 핸들러 안에서 다 처리할 수 있음.

```jsx
// 🔴 잘못된 방식 (Effect 체인으로 state 연결)
//  - state 변경 → Effect 실행 → 또 state 변경 → 또 Effect 실행 연쇄적으로 state-Effect 연결되어 코드 흐름이 복잡해짐.
//  - 디버깅 어려움 + 예측 불가능한 흐름 발생.
useEffect(() => {
  if (card !== null && card.gold) {
    setGoldCardCount((c) => c + 1);
  }
}, [card]);

useEffect(() => {
  if (goldCardCount > 3) {
    setRound((r) => r + 1);
    setGoldCardCount(0);
  }
}, [goldCardCount]);

useEffect(() => {
  if (round > 5) {
    setIsGameOver(true);
  }
}, [round]);

useEffect(() => {
  alert("Good game!");
}, [isGameOver]);

// ✅ 좋은 방식 (이벤트 핸들러 안에서 모두 처리)
//  - 관련된 state 업데이트를 이벤트 핸들러 안에서 한 번에 처리.
//  - Effect 없이도 상태 전이 로직을 명확히 컨트롤 가능.
const isGameOver = round > 5;

function handlePlaceCard(nextCard) {
  if (isGameOver) {
    throw Error("Game already ended.");
  }

  setCard(nextCard);

  if (nextCard.gold) {
    if (goldCardCount <= 3) {
      setGoldCardCount(goldCardCount + 1);
    } else {
      setGoldCardCount(0);
      setRound(round + 1);

      if (round === 5) {
        alert("Good game!");
      }
    }
  }
}
```

8. 애플리케이션 초기화

- 앱 로드 시 한 번만 실행되는 코드 → 루트 모듈이나 엔트리 포인트에 넣기.
- Effect에 넣으면 Strict Mode에서 두 번 실행됨; 모듈 레벨 코드로 빼는 게 안전.

```jsx
// 🔴 잘못된 방식 (Effect로 한 번만 실행될 로직 처리)
//  - Effect는 Strict Mode에서 개발 중 두 번 실행될 수 있음 (의도적 스트레스 테스트).
//  - 앱 전체에서 단 한 번만 실행돼야 하는 로직(예: 초기화, 글로벌 설정)은 Effect에 의존하면 위험.
useEffect(() => {
  loadDataFromLocalStorage();
  checkAuthToken();
}, []);

// ✅ 좋은 방식 (모듈 최상단 또는 실행 플래그 사용)
//  - didInit 플래그로 실제 실행 횟수를 제어.
//  - Strict Mode의 이중 실행에도 실제 작업은 한 번만 발생.
//  - 더 나은 방식은 아예 Effect 대신 앱 엔트리 포인트나 모듈 최상단에서 처리.
let didInit = false;

useEffect(() => {
  if (!didInit) {
    didInit = true;
    loadDataFromLocalStorage();
    checkAuthToken();
  }
}, []);
```

9. 부모-자식 간 데이터 전달

- 자식에서 onChange로 부모 콜백 호출.
- Effect에서 부모 호출하지 말고 이벤트 핸들러에서 바로 처리.

```jsx
// 🔴 잘못된 방식 (Effect로 부모에게 state 변경 알리기)
//  - 자식 컴포넌트에서 isOn이 바뀔 때 Effect로 부모에게 알림.
//  - Effect는 렌더링 후 실행되기 때문에 부모-자식 간 데이터 흐름이 느리고 복잡해짐.
useEffect(() => {
  onChange(isOn);
}, [isOn, onChange]);

// ✅ 좋은 방식 1 (이벤트 안에서 즉시 부모 알림)
//  - 이벤트가 발생한 순간 자식 state와 부모 업데이트를 함께 처리.
//  - React가 모든 업데이트를 일괄 처리(batch)하므로 렌더링이 한 번만 발생.
function updateToggle(nextIsOn) {
  setIsOn(nextIsOn);
  onChange(nextIsOn);
}

// ✅ 좋은 방식 2 (완전히 부모가 상태 제어)
//  - 자식은 상태를 전혀 갖지 않고, 상태 제어를 전적으로 부모에게 맡김.
//  - 자식은 props로 내려받은 값만 표시, 상태 변화는 부모가 모두 처리.
function Toggle({ isOn, onChange }) {
  function handleClick() {
    onChange(!isOn);
  }
}
```

```jsx
// 🔴 잘못된 방식 (Effect로 부모에게 데이터 전달)
//  - 자식 컴포넌트가 Effect로 렌더링 후 부모에게 데이터 전달.
function Child({ onFetched }) {
  const data = useSomeAPI();

  useEffect(() => {
    if (data) {
      onFetched(data);
    }
  }, [onFetched, data]);
}

// ✅ 좋은 방식 (부모가 직접 데이터 관리, 자식에 내려줌)
//  - 부모가 데이터를 직접 가져와 자식에게 props로 전달.
//  - React의 단방향 데이터 흐름을 따르며, 데이터 흐름이 명확.
function Parent() {
  const data = useSomeAPI();
  return <Child data={data} />;
}

function Child({ data }) {}
```

### Effect가 필요한 상황

1. 외부 저장소 구독하기

- 외부 이벤트 리스너는 컴포넌트 라이프사이클에 맞춰 구독/해제가 필요.

```jsx
// 🔴 이상적이지 않은 방식 (Effect로 직접 구독)
//  - Effect로 직접 브라우저 이벤트를 구독·해제.
//  - 잘 동작하긴 하지만, React가 제공하는 표준 Hook(useSyncExternalStore)을 쓰는 것이 더 안전하고 일관됨.
//  - 특히 Strict Mode 같은 환경에서 구독 관리가 꼬일 위험.
const [isOnline, setIsOnline] = useState(true);

useEffect(() => {
  function updateState() {
    setIsOnline(navigator.onLine);
  }

  updateState();

  window.addEventListener("online", updateState);
  window.addEventListener("offline", updateState);
  return () => {
    window.removeEventListener("online", updateState);
    window.removeEventListener("offline", updateState);
  };
}, []);

// ✅ 좋은 방식 (useSyncExternalStore 사용)
//  - useSyncExternalStore를 사용해 구독·해제를 안전하게 관리.
//  - 동일한 subscribe 함수로 여러 번 호출해도 React가 자동으로 최적화.
//  - 서버 렌더링과 클라이언트 렌더링 모두에서 잘 작동.
function useOnlineStatus() {
  return useSyncExternalStore(
    subscribe,
    () => navigator.onLine,
    () => true
  );
}
```

2. 데이터 가져오기

- useEffect(() => { fetchData(); }, [query])
- 렌더링 후 외부 데이터 요청은 React가 관리할 수 없으므로 Effect 필요.

```jsx
// 🔴 문제점 (정리 로직 없이 Effect에서 fetch 실행)
//  - query나 page가 변경될 때마다 fetch 요청 실행.
//  - 하지만 이전 요청이 완료되기 전에 새 요청이 오면 race condition(데이터 꼬임) 발생 가능.
//  - 클린업(취소·무시) 로직 없이 단순 fetch만 하면 안정성이 떨어짐.
useEffect(() => {
  fetchResults(query, page).then((json) => {
    setResults(json);
  });
}, [query, page]);

// ✅ 개선 방향 (필요 시 정리 로직 추가)
//  - 요청 취소 기능 추가 (AbortController 사용).
//  - 마지막 요청만 반영하도록 무시 로직 추가.
```

<br />

## (5) 반응형 effects의 생명주기

- 컴포넌트는 마운트, 업데이트, 마운트 해제 과정을 가짐.
- Effect는 “언제 어떤 동기화를 시작/중지할지”를 React에 알려주는 장치.
- props나 state 같은 값이 바뀌면 이 사이클이 여러 번 반복됨.

### Effect 기본 예제

- roomId가 바뀔 때마다 → 이전 연결 끊고, 새로 연결
- React가 props/state를 바탕으로 effect를 다시 실행함

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();

    return () => connection.disconnect();
  }, [roomId]);
}
```

### Effect를 여러 개로 쪼개야 하는 경우

- 방문 기록과 채팅 연결은 독립적 → Effect를 나눔
- “실제로 다른 동기화 작업”이기 때문

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId); // 방문 기록
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // 채팅 연결
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
}
```

### Effect 종속성(dependencies)이란?

- [] 안에 들어가는 의존성 목록
- 이 값이 바뀌면 effect를 다시 실행하라는 React의 약속
- 포넌트 본문에서 선언한 값 (`props`, `state`, `context`, `계산된 변수` 등)은 다 반응형 → 반응형 값 → 포함 O
- 절대 안 바뀌는 `상수` (const serverUrl = ...) → 포함 X
- 컴포넌트 외부에서 선언된 안정적인 함수 → 포함 X

```jsx
useEffect(() => {
  const connection = createConnection(serverUrl, roomId);
  connection.connect();
  return () => connection.disconnect();
}, [roomId]); // <- 여기에 적힌 것들
```

### 빈 의존성 배열 []의 의미

- 딱 한 번 (마운트 시 실행, 언마운트 시 cleanup)
- 이후 props/state가 바뀌어도 다시 실행 안 함
- 내부에서 반응형 값(roomId, serverUrl 등)을 읽는데 의존성 배열에 빠지면 → 린트 오류, 버그 발생 가능

<br />

## (6) Effect에서 이벤트 분리하기

### Effect는 언제 쓰나?

- 컴포넌트가 화면에 존재하는 한 유지해야 하는 동기화 작업 (예: 서버 연결, 구독, 타이머)
- 사용자의 상호작용과 관계없이 계속 유지되어야 하는 작업
- 사용자가 이 컴포넌트를 어떻게 열었는지, 어떤 액션을 했는지 관계없이 roomId가 바뀌면 연결도 새로 갱신되어야 함 → Effect에서 처리

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
}
```

### 이벤트 핸들러는 언제 쓰나?

- 특정 사용자 상호작용에만 반응하는 코드 (예: 클릭, 입력, 제출)
- 상태(state)나 props를 읽을 수 있지만, 렌더링마다 자동으로 실행되지 않음
- 버튼을 누를 때만 sendMessage(message) 실행됨
- 클릭 이벤트가 없으면 아무 일도 안 함 -> Effect로 처리할 필요 없음

```jsx
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState("");

  function handleSendClick() {
    sendMessage(message);
  }

  return (
    <>
      <input value={message} onChange={(e) => setMessage(e.target.value)} />
      <button onClick={handleSendClick}>전송</button>
    </>
  );
}
```

### Effect vs. 이벤트 핸들러 비교

| 구분            | Effect                              | 이벤트 핸들러                             |
| --------------- | ----------------------------------- | ----------------------------------------- |
| 언제 실행?      | 렌더링 시 / 의존성 값이 바뀔 때     | 사용자가 상호작용할 때 (예: 클릭, 입력)   |
| 의존성 필요?    | ✅ 필요 (반응형 값에 따라 재실행됨) | ❌ 필요 없음 (상호작용 없으면 실행 안 됨) |
| 예시            | 서버 연결, 구독, 타이머             | 버튼 클릭, 폼 제출                        |
| 다시 실행 조건? | 의존성 값 변경                      | 동일한 이벤트 발생                        |

### 핵심정리

- Effect = 렌더링과 동기화되는 작업
- 이벤트 핸들러 = 사용자의 개별 상호작용 처리
- 컴포넌트 안에서 선언된 값은 다 반응형으로 보고, Effect 의존성에 넣을지 판단
- 둘을 섞지 않고 분리하면 코드가 더 명확하고 버그가 줄어듦

<br />

## (7) Effect 의존성 제거하기

### 의존성(dependencies)이란?

- `useEffect(() => { ... }, [의존성])` Effect 안에서 사용하는 모든 반응형 값(props, state 등)을 넣어야 함
- 이렇게 해야 최신 값으로 동기화됨
- 린트는 빠진 의존성이 있으면 경고함

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ roomId는 반응형 값이라 넣어야 함
}
```

### 빈 의존성 배열([])은 언제?

- 의존하는 값이 없을 때만 → 마운트 시 한 번만 실행됨

```jsx
useEffect(() => {
  console.log("처음에만 실행!");
}, []); // ✅ 의존성 없음
```

### 의존성 추가 대신 코드를 바꿔야 할 때

- 의존성이 마음에 안 든다고 의존성 배열을 억지로 바꾸지 말 것
- 의존성을 줄이려면 코드를 먼저 바꿔야 함
- 코드 → 의존성 결정됨 (반대 아님)

### 벤트 로직은 Effect 안에 넣지 말기

- 이벤트는 이벤트 핸들러로! Effect는 동기화용으로만!

```jsx
// 올바른 방식:
function Form() {
  function handleSubmit() {
    post("/api/register");
    showNotification("등록 성공!");
  }

  return <button onClick={handleSubmit}>제출</button>;
}

// 잘못된 방식 (submitted state로 Effect에서 감지):
function Form() {
  const [submitted, setSubmitted] = useState(false);

  useEffect(() => {
    if (submitted) {
      post("/api/register");
      showNotification("등록 성공!");
    }
  }, [submitted]);

  function handleSubmit() {
    setSubmitted(true);
  }
}
```

### 업데이터 함수로 의존성 줄이기

- 업데이터 함수 (setX(prev => ...))를 쓰면 Effect 안에서 최신 state 값을 직접 읽지 않아도 됨 → 의존성에서 제외 가능

```jsx
useEffect(() => {
  connection.on("message", (receivedMessage) => {
    setMessages((msgs) => [...msgs, receivedMessage]);
  });
}, [roomId]); // ✅ messages는 의존성 필요 없음 (업데이터 함수 사용)
```

### 객체·함수를 의존성에 넣을 때 주의

- 객체나 함수는 렌더링마다 새로 생성되므로, 의존성으로 넣으면 Effect가 매번 재실행됨
- 필요하다면 `useMemo`나 `useCallback`으로 감싸서 안정화해야 함

```jsx
const options = { serverUrl, roomId };
useEffect(() => {
  const connection = createConnection(options);
  connection.connect();
  return () => connection.disconnect();
}, [options]);
```

### (실험적) useEffectEvent

- React의 실험적 API인 useEffectEvent는 Effect 안에서 안정적으로 최신 값을 읽도록 도와줌

```jsx
const onVisit = useEffectEvent((visitedRoomId) => {
  logVisit(visitedRoomId, notificationCount);
});

useEffect(() => {
  onVisit(roomId);
}, [roomId]);
```

<br />

## (8) 커스텀 Hook으로 로직 재사용하기

### 왜 커스텀 훅이 필요할까?

- 컴포넌트끼리 UI는 다르지만 같은 로직을 공유해야 할 때
- 예전에는 HOC, render props 같은 복잡한 패턴을 썼지만 → 이제는 커스텀 훅으로 훨씬 간단히 해결 가능

### 커스텀 훅이란?

- 이름이 use로 시작하는 함수
- 내부에서 React 훅들 (useState, useEffect, 등) 사용 가능
- props나 state 같은 React 컨셉과 잘 연결됨

```jsx
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }
    function handleOffline() {
      setIsOnline(false);
    }

    window.addEventListener("online", handleOnline);
    window.addEventListener("offline", handleOffline);

    return () => {
      window.removeEventListener("online", handleOnline);
      window.removeEventListener("offline", handleOffline);
    };
  }, []);

  return isOnline;
}
```

### 커스텀 훅 사용법

- 같은 로직 (useOnlineStatus)을 두 컴포넌트에서 재사용

```jsx
function StatusBar() {
  const isOnline = useOnlineStatus();
  return <h1>{isOnline ? "✅ 온라인" : "❌ 오프라인"}</h1>;
}

function SaveButton() {
  const isOnline = useOnlineStatus();
  return <button disabled={!isOnline}>저장</button>;
}
```

### 커스텀 훅 만들 때 주의할 점

- 컴포넌트나 다른 훅처럼 동작함 (렌더링마다 실행됨)
- 훅 안에서 React 훅 규칙 (훅은 최상위에서만 호출, 조건문 안에서 호출 금지)을 지켜야 함
- 커스텀 훅 자체는 단순히 JS 함수지만, 반응형 값과 연결됨

### 컴포넌트와 커스텀 훅의 차이

| 구분      | 컴포넌트          | 커스텀 훅                        |
| --------- | ----------------- | -------------------------------- |
| 반환값    | JSX UI 요소       | 값(데이터), 함수                 |
| 사용 위치 | JSX 안            | 컴포넌트 최상위 (렌더링 함수 안) |
| 예        | `<MyComponent />` | `const result = useMyHook()`     |

### 훅을 커스텀 훅 안에서 재사용

- 커스텀 훅 안에서도 다른 훅들을 자유롭게 사용할 수 있음

```jsx
function useFancyOnlineStatus() {
  const isOnline = useOnlineStatus();
  return isOnline ? "✨ 온라인!" : "💀 오프라인!";
}
```

### 최종 핵심 정리

- 커스텀 훅은 이름이 use로 시작하는 재사용 가능한 함수
- 로직만 캡슐화, UI는 캡슐화하지 않음
- 여러 컴포넌트에서 같은 로직을 반복하지 않게 만들어줌
- React 훅 규칙을 반드시 지켜야 함
- 다른 훅들을 조합해 더 복잡한 커스텀 훅도 만들 수 있음
