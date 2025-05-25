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

- 이벤트 핸들러에게만 필요한 정보이고 변경이 일어날 때 리렌더링이 필요하지 않을때 사용한다.
- DOM 요소에 직접 접근해야 할 때 (예: 포커스, 스크롤 제어 등) 사용
- **단방향 데이터 흐름을 벗어나는 수단(escape hatch)**

### 컴포넌트에 Ref를 추가하기

- ref.current 프로퍼티를 통해 해당 Ref의 current 값에 접근한다.
- 이 값은 React가 추적하지 않기 때문에 렌더링이 발생하지 않음.

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

| Ref                                     | State                                        |
| --------------------------------------- | -------------------------------------------- |
| { current: initialValue } 객체 반환     | 재 값과 Setter 함수 [value, setValue]를 반환 |
| 렌더링 발생하지 않음                    | 렌더링 발생함                                |
| 렌더링 중 current 값을 수정 및 업데이트 | 리렌더링 대기열에 넣어야 함                  |
| `ref.current`                           | `[value, setValue]` 구조 사용                |
| 렌더링 중 읽기 쓰기 X                   | 언제든 읽기 가능                             |
| 렌더링 로직에 영향을 주지 않는 값 저장  | 렌더링 로직에 사용되는 값 저장               |

<br />

## (2) Ref로 DOM 조작하기

- 특정 노드에 포커스를 옮기거나, 스크롤 위치를 옮기거나, 위치와 크기를 측정하기 위해서 React가 관리하는 DOM 요소에 접근해야 할 때 ref 사용.

### ref로 노드 가져오기

```jsx
// DOM 노드에 접근하기 위해 useRef Hook을 가져온다.
import { useRef } from "react";

// Hook을 사용
const myRef = useRef(null);

// ref를 DOM 노드를 가져와야하는 JSX tag에 ref 어트리뷰트로 전달
<div ref={myRef}>
```

```jsx
//이렇게 브라우저 API 사용
function scrolling() {
  myRef.current.scrollIntoView();
}
```

### 다른 컴포넌트의 DOM 노드 접근하기

- React의 **탈출구(Escape hatch)**로, 직접 DOM 조작은 코드 안정성을 깨트릴 수 있으니 꼭 필요한 경우에만 사용할 것.
- 내장 컴포넌트
  - (예: \<input />)에 ref를 주면 → ref.current는 실제 DOM 노드를 가리킨다.
- 사용자 정의 컴포넌트
  - (예: \<MyInput />)에 ref를 바로 주면 → 기본적으로 null이 주어진다 (즉, 그 컴포넌트 인스턴스를 직접 가리키지 않는다).
- 부모에서 자식으로 ref 전달하기

  - 부모에서 useRef()로 ref 생성.
  - 자식에게 prop처럼 ref를 넘겨주고, 자식 컴포넌트는 이걸 내장 컴포넌트에 연결.

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

### React가 ref를 부여할 때

<br />

## (3) Effect로 동기화하기

### Effect란?

- Effect는 React 컴포넌트가 화면에 렌더링된 후 실행되는 코드이다.
- 예: DOM을 직접 조작하거나, 네트워크 요청, 구독 설정, 타이머 설정 등 React의 순수 렌더링 로직 외부의 작업을 처리할 때 사용한다.

### Effect가 이벤트와 다른 점

- 이벤트 핸들러는 사용자의 행동(예: 클릭, 입력)에 직접 반응한다.
- Effect는 사용자 입력 없이도 렌더링과 동기화되는 작업을 처리한다. 즉, 렌더링 결과에 맞춰 렌더링 이후 실행된다.

### 컴포넌트에서 Effect를 선언하는 방법

- React의 useEffect 훅을 사용합니다:

```jsx
// 1단계 : Effect를 선언
import { useEffect } from "react";

function MyComponent() {
  useEffect(() => {
    // 실행할 작업 (Effect)
    // 이곳의 코드는 *모든* 렌더링 후에 실행된다.
    // useEffect는 화면에 렌더링이 반영될 때까지 코드 실행을 “지연”시킨다.
  }, [의존성]);
  // 2단계 : 의존성 지정
  // useEffect 안의 콜백은 렌더링 후 실행되며, 의존성 배열에 따라 실행 여부가 결정된다.
}
```

- **불필요한 Effect 재실행을 건너뛰는 방법**
  - useEffect의 두 번째 인자인 의존성 배열([])을 올바르게 설정합니다.
    - [] → 컴포넌트 최초 마운트 시 한 번만 실행.
    - [a, b] → 마운트될 때 실행, a 또는 b 중 하나라도 변경된 경우에 실행.
  - 주의: 의존성 배열에 필요한 값을 빠뜨리면 최신 값과 동기화되지 않음.

### 개발 중에 Effect가 두 번 실행되는 경우를 다루는 방법

- 클린업 함수를 구현하면 된다. (클린업 : 수행하던 작업을 중단하거나 되돌리는 역할)

  ```jsx
  useEffect(() => {
    const id = setInterval(() => {
      /* 작업 */
    }, 1000);
    return () => clearInterval(id); // 정리
  }, []);
  ```

- **프로덕션(실제 서비스)**에서는 Effect가 한 번만 실행됩니다.

<br />

## (4) Effect가 필요하지 않은 경우

<br />

## (5) 반응형 effects의 생명주기

<br />

## (6) Effect에서 이벤트 분리하기

<br />

## (7) Effect 의존성 제거하기

<br />

## (8) 커스텀 Hook으로 로직 재사용하기
