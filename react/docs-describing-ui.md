---
title: "[리액트 공식 문서 정리] 1. UI 표현하기"
date: "2025-03-31"
category: "react"
tags: ["react", "official docs"]
---

## 목차

- [(1) 첫번째 컴포넌트](#1-첫번째-컴포넌트)
- [(2) 컴포넌트 Import 및 Export하기](#2-컴포넌트-import-및-export하기)
- [(3) JSX로 마크업 작성하기](#3-jsx로-마크업-작성하기)
- [(4) 중괄호가 있는 JSX에서 자바스크립트 사용하기](#4-중괄호가-있는-jsx에서-자바스크립트-사용하기)
- [(5) 컴포넌트에 Props 전달하기](#5-컴포넌트에-props-전달하기)
- [(6) 조건부 렌더링](#6-조건부-렌더링)
- [(7) 리스트 렌더링](#7-리스트-렌더링)
- [(8) 컴포넌트를 순수하게 유지하기](#8-컴포넌트를-순수하게-유지하기)
- [(9) 트리로서의 UI](#9-트리로서의-UI)

<br />

## (1) 첫번째 컴포넌트

### 리액트 컴포넌트란?

- 재사용 가능한 UI 조각
- JSX 반환하는 JS 함수
- 마크업처럼 사용 가능

### 컴포넌트 사용 규칙

- JSX 내에서 HTML 요소처럼 사용 가능
- 대문자로 시작해야 컴포넌트로 인식
- 여러 번 재사용 가능
- return문은 한 줄이면 괄호 생략 가능
- 여러 줄일 경우 괄호로 묶어야 함
- 괄호 없이 줄바꿈하면 return 이후 코드는 무시됨

### 컴포넌트 예시

```tsx
const Profile= () => {
  return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
};

export default const Tab1: React.FC = () => {
  return (
    <div className="flex mt-10">
      <Profile />
      <Profile />
      <Profile />
    </div>
  );
};

export default Tab1;
```

<br />

## (2) 컴포넌트 Import 및 Export하기

### Root 컴포넌트란?

- React 앱에서 가장 처음 렌더링되는 컴포넌트
- 보통 `App.tsx`가 루트 컴포넌트 역할을 하며, 다른 모든 하위 컴포넌트를 포함
- Next.js 같은 프레임워크에서는 각 페이지가 루트 컴포넌트처럼 동작할 수 있음

### 컴포넌트를 import 하거나 export 하는 방법

- 여러 파일로 구성된 컴포넌트를 만들고 관리할 수 있다.
- 이를 위해 `import`와 `export` 키워드를 사용한다.
- JavaScript에서는 `default export`와 `named export`라는 두 가지 방법이 있다.
- 한 파일에는 하나의 `default export`만 존재할 수 있으며, `named export`는 여러 개 사용 가능하다.

```tsx
// App.tsx
const App: React.FC = () => {
  return <Gallery />;
};

export default App;

// Gallery.tsx
const Profile: React.FC = () => {
  return <img src="https://i.imgur.com/QIrZWGIs.jpg" alt="Alan L. Hart" />;
};

const Gallery: React.FC = () => {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
};

export default Gallery;
```

### Default Export

- 한 파일에 하나의 컴포넌트만 export할 때 사용
- 주로 주요 컴포넌트나 함수에 적합
- `import` 시 이름을 자유롭게 지정 가능

```tsx
// MyButton.tsx
export default function MyButton() {
  return <button>I'm a button</button>;
}

// App.tsx
import MyButton from "./MyButton";
```

### Named Export

- 여러 개의 함수나 컴포넌트를 export할 때 사용
- `import`할 때는 `export`한 이름 그대로 사용해야 함

```tsx
// MyComponents.tsx
export function Button() {
  return <button>I'm a button</button>;
}

export function Avatar() {
  return <img src="avatar.png" alt="Avatar" />;
}

// App.tsx
import { Button, Avatar } from "./MyComponents";
```

### Default vs Named 비교

| Syntax  | Export                                | Import                                  |
| ------- | ------------------------------------- | --------------------------------------- |
| Default | `export default function Button() {}` | `import Button from './button.js';`     |
| Named   | `export function Button() {}`         | `import { Button } from './button.js';` |

<br />

## (3) JSX로 마크업 작성하기

### JSX란?

- JSX는 React에서 마크업을 작성하기 위한 **JavaScript 확장 문법**이다.
- HTML과 매우 비슷하지만, JSX는 JavaScript 파일에서 UI 구조를 정의할 수 있게 해준다.
- `.tsx` 확장자는 TypeScript + JSX 조합을 의미하며, 타입 안정성과 JSX의 유연함을 동시에 제공한다.

### JSX 규칙 (JSX vs HTML 차이점)

1.  하나의 루트 엘리먼트로 반환하기

    - JSX에서 여러 태그를 반환할 경우, 반드시 하나의 부모 요소로 감싸야 한다.
    - `<div>` 또는 빈 fragment `<>...</>`를 사용할 수 있다.
    - **Fragment**: 브라우저상의 HTML 구조에 영향을 주지 않으면서 요소를 감쌀 수 있다.

    ```tsx
    return (
      <>
        <h1>제목</h1>
        <p>내용</p>
      </>
    );
    ```

2.  모든 태그는 닫아주기

    - `<img>`처럼 자체적으로 닫아주는 태그는 반드시 `<img />` 형태로 작성해야 한다.

    ```tsx
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    />
    ```

3.  대부분 속성은 캐멀 케이스로 작성한다.

    - HTML의 속성명은 JSX에서는 대부분 camelCase로 변환된다.
    - 예: class → className, for → htmlFor, stroke-width → strokeWidth 등
    - 단, aria-_, data-_ 속성은 그대로 사용 가능하다.

    | HTML      | JSX         |
    | --------- | ----------- |
    | `class`   | `className` |
    | `for`     | `htmlFor`   |
    | `onclick` | `onClick`   |

<br />

## (4) 중괄호가 있는 JSX에서 자바스크립트 사용하기

JSX에서는 `{}` 중괄호를 사용하여 JavaScript 표현식을 삽입할 수 있다.

### 중괄호 사용하기: JSX 내부에서 JavaScript 사용하기

- 속성 값에 JavaScript 변수를 전달할 때는 중괄호 `{}`를 사용한다.
- 예: `src={avatar}` → 변수 `avatar`의 값을 읽어 전달  
  반면, `src="{avatar}"`는 `"avatar"`라는 문자열로 처리된다.

  ```tsx
  const Avatar: React.FC = () => {
    const avatar: string = "https://i.imgur.com/7vQD0fPs.jpg";
    return <img className="avatar" src={avatar} alt="Gregorio Y. Zara" />;
  };
  ```

- JSX 태그 내부에서도 중괄호로 JavaScript 표현식을 삽입할 수 있다.

  ```tsx
  const TodoList: React.FC = () => {
    const name: string = "Gregorio Y. Zara";
    return <h1>{name}&apos;s To Do List</h1>;
  };
  ```

- 함수 호출도 중괄호 안에서 사용할 수 있다.

  ```tsx
  const today: Date = new Date();

  function formatDate(date: Date): string {
    return new Intl.DateTimeFormat("en-US", { weekday: "long" }).format(date);
  }

  const TodoList: React.FC = () => {
    return <h1>To Do List for {formatDate(today)}</h1>;
  };
  ```

### 이중 중괄호 사용하기: 객체를 속성으로 전달

- 객체 리터럴을 전달할 때는 중괄호 안에 또 다른 중괄호를 사용한다.
- 대표적인 예는 style 속성이다.
- 스타일 속성명은 camelCase로 작성해야 한다. (예: background-color → backgroundColor)

  ```tsx
  const TodoList: React.FC = () => {
    return (
      <ul
        style={{
          backgroundColor: "black",
          color: "pink",
        }}
      >
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
      </ul>
    );
  };
  ```

- 객체를 미리 선언하고 전달하는 방식도 가능하다.

  ```tsx
  interface Person {
    name: string;
    theme: {
      backgroundColor: string;
      color: string;
    };
  }

  const person: Person = {
    name: "Gregorio Y. Zara",
    theme: {
      backgroundColor: "black",
      color: "pink",
    },
  };

  const TodoList: React.FC = () => {
    return (
      <div style={person.theme}>
        <h1>{person.name}&apos;s Todos</h1>
        <img
          className="avatar"
          src="https://i.imgur.com/7vQD0fPs.jpg"
          alt="Gregorio Y. Zara"
        />
      </div>
    );
  };

  export default TodoList;
  ```

<br />

## (5) 컴포넌트에 Props 전달하기

### Props란?

- **props (properties)**는 **부모 컴포넌트**가 **자식 컴포넌트에게 데이터를 전달**하는 방법
- 모든 JavaScript 값(문자열, 숫자, 객체, 배열, 함수 등)을 전달할 수 있다.
- 컴포넌트의 **재사용성**과 **유지보수성**을 높여준다.

```tsx
type WelcomeProps = {
  name: string;
};

function Welcome({ name }: WelcomeProps) {
  return <h1>Hello, {name}</h1>;
}
```

### 1. 컴포넌트에 props 전달하기

- Avatar에 person (객체)와 size (숫자)를 전달

```tsx
type Person = {
  name: string;
  imageId: string;
};

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{
          name: "Katsuko Saruhashi",
          imageId: "YfeOqp2",
        }}
      />
    </div>
  );
}
```

### 2. 자식 컴포넌트 내부에서 props 읽기

```tsx
type AvatarProps = {
  person: Person;
  size: number;
};

// optional
type AvatarProps = {
  person: Person;
  size?: number;
};

function getImageUrl(person: Person, size: "s" | "m" | "l" = "s"): string {
  return `https://i.imgur.com/${person.imageId}${size}.jpg`;
}

function Avatar({ person, size }: AvatarProps) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

### prop의 기본값 지정하기

props가 전달되지 않았을 경우를 대비해 **기본값(default value)**을 설정할 수 있다.

```tsx
type AvatarProps = {
  person: Person;
  size: number;
};

function Avatar({ person, size = 100 }: AvatarProps) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

### JSX spread 문법으로 props 전달하기

props를 객체 전체로 전달하고 싶다면 JSX의 spread 문법(...)을 사용할 수 있다.

```tsx
type AvatarProps = {
  person: Person;
  size: number;
};

function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

### 자식을 JSX로 전달하기

컴포넌트에 JSX를 자식 요소로 전달할 수도 있다.

```tsx
function Card({ children }: { children: React.ReactNode }) {
  return <div className="card">{children}</div>;
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={80}
        person={{
          name: "Aklilu Lemma",
          imageId: "OKS67lh",
        }}
      />
    </Card>
  );
}
```

### 시간에 따라 props가 변하는 방식

props는 부모 컴포넌트의 상태(state)나 변수에 따라 시간에 따라 변경될 수 있다.

```tsx
function Clock({ time }: { time: Date }) {
  return <h1>{time.toLocaleTimeString()}</h1>;
}

export default function App() {
  const [now, setNow] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setNow(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  return <Clock time={now} />;
}
```

<br />

## (6) 조건부 렌더링

### 조건부로 JSX 반환하기

```tsx
import React from "react";

interface ItemProps {
  name: string;
  isPacked: boolean;
}

function Item({ name, isPacked }: ItemProps) {
  return <li className="item">{name}</li>;
}

// if/else 문
function Item2({ name, isPacked }: ItemProps) {
  // isPacked prop이 true이면 이 코드는 다른 JSX 트리를 반환한다.
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}

function Item3({ name, isPacked }: ItemProps) {
  //  조건부로 null 사용
  //  isPacked가 true라면 컴포넌트는 아무것도 반환하지 않는다.
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

export default function Tab6() {
  return (
    <>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item isPacked={true} name="Space suit" />
        <Item2 isPacked={true} name="Helmet with a golden leaf" />
        <Item3 isPacked={false} name="Photo of Tam" />
      </ul>
    </>
  );
}
```

### 조건부로 JSX 포함시키기

#### 삼항 조건 연산자 ( ? : )

```tsx
// 삼항 연산자 EX1
<li className="item">{isPacked ? name + " ✅" : name}</li>;

// 삼항 연산자 EX2. HTML 태그 사용
<li className="item">{isPacked ? <del>{name + " ✅"}</del> : name}</li>;
```

#### 논리 AND 연산자 ( && )

- 왼쪽(조건)이 true이면 오른쪽(체크 표시)의 값을 반환, 조건이 false이면 전체 표현 식이 false

```tsx
// AND 연산자 1
<li className="item">
  {name} {isPacked && "✅"}
</li>
```

#### 변수에 조건부로 JSX를 할당하기기

```tsx
function ItemWithVariable({ name, isPacked }: ItemProps): JSX.Element {
  let itemContent: string = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return <li className="item">{itemContent}</li>;
}
```

<br />

## (7) 리스트 렌더링

### `map()`을 사용한 리스트 렌더링

- 배열의 각 항목을 JSX로 변환할 때 `map()` 사용
- 각 항목은 고유한 `key`가 필요함 (React가 항목을 추적하기 위해 사용)
- 일반적으로는 고유 id를 key로 사용

```tsx
// 문자열 people 배열 선언
const people: string[] = [
  "Creola Katherine Johnson: mathematician",
  "Mario José Molina-Pasquel Henríquez: chemist",
  "Mohammad Abdus Salam: physicist",
  "Percy Lavon Julian: chemist",
  "Subrahmanyan Chandrasekhar: astrophysicist",
];

export default function Tab7() {
  const listItems = people.map((person, index) => (
    <li key={index}>{person}</li>
  ));

  return <ul>{listItems}</ul>;
}
```

### `filter()`로 조건에 맞는 항목만 렌더링하기

- `filter()`로 조건에 맞는 항목만 추출 후, `map()`으로 렌더링

```tsx
// Person 인터페이스를 만들어 타입 관리
export interface Person {
  id: number;
  name: string;
  profession: string;
  accomplishment: string;
  imageId: string;
}

// 조건 필터링 및 렌더링 할 배열 people2
export const people2: Person[] = [
  {
    id: 0,
    name: "Creola Katherine Johnson",
    profession: "mathematician",
    accomplishment: "spaceflight calculations",
    imageId: "MK3eW3A",
  },
];

function getImageUrl(person: Person): string {
  return "https://i.imgur.com/" + person.imageId + "s.jpg";
}

export default function Tab7() {
  // filter() 메서드를 사용하여 'chemist'만 추출
  // 조건에 따라 배열을 선별할 수 있음
  const chemists = people2.filter((person) => person.profession === "chemist");
  // 선별된 배열을 기반으로 요소 리스트를 생성
  const listItems2 = chemists.map((person) => (
    <li key={person.id} className="mb-4">
      <img src={getImageUrl(person)} alt={person.name} width={100} />
      <p>
        <b>{person.name}:</b> {person.profession} <br />
        Known for {person.accomplishment}
      </p>
    </li>
  ));

  return <ul>{listItems2}</ul>;
}
```

### 화살표 함수: 암시적 vs 명시적 반환

```tsx
// 화살표 함수는 => 바로 뒤에 식을 반환하기 때문에 return문이 필요하지 않다
// 간단한 JSX 반환이라면 중괄호 없이 작성 가능
const listItems = chemists.map(
  (person) => <li>...</li> // 암시적 반환!
);

// => 뒤에 { } 중괄호가 오는 경우 return을 명시적으로 작성해야 한다
// 여러 줄 또는 로직이 들어갈 때 사용
const listItems = chemists.map((person) => {
  return <li>...</li>;
});
```

### key를 사용해서 리스트 항목을 순서대로 유지하기

```tsx
<li key={person.id}>...</li>
```

- 각 배열 항목에 다른 항목 중에서 고유하게 식별할 수 있는 문자열 또는 숫자를 key로 지정해야 한다.
- key는 React가 리스트 항목을 추적하고 업데이트할 수 있도록 도와주는 고유값
- 만약 key가 없다면 React는 모든 요소를 새로 렌더링해야 해서 비효율적
- key는 형제 항목 간에 고유해야 하며, 변경되지 않아야 함
- 일반적으로 DB의 id, 혹은 고유한 문자열을 사용
- key는 변경되면 안 됨 – 변경 시 리렌더링 비용 발생

<br />

## (8) 컴포넌트를 순수하게 유지하기

### 순수함수란

- 같은 입력이 주어졌을 때 항상 같은 출력을 반환
- 함수 실행 중 외부 상태를 변경하지 않음 (사이드 이펙트 없음)

```tsx
function double(number: number): number {
  return number * 2;
}

// 순수 함수 X
let counter = 0;
function incrementCounter(): void {
  counter++; // 외부 상태 변경 → 부작용 발생
}
```

### React 컴포넌트도 순수하게 작성되어야 함

- React의 렌더링 과정은 항상 순수해야 한다.
- 컴포넌트는 props에만 의존해서 UI를 그려야 함
- 렌더링 중에는 DOM 변경, 변수 수정, 네트워크 요청 등 사이드 이펙트를 하면 안 됨

```tsx
type RecipeProps = {
  drinkers: number;
};

function Recipe({ drinkers }: RecipeProps) {
  return (
    <ol>
      <li>Boil {drinkers} cups of water.</li>
      <li>
        Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.
      </li>
      <li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Spiced Chai Recipe</h1>
      <h2>For two</h2>
      <Recipe drinkers={2} />
      <h2>For a gathering</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

### 사이드 이펙트: 의도하지 않은 결과

- 렌더링 중이 아닌 곳에서 발생하는 외부 변화 (예: 화면 업데이트, 데이터 변경, 애니메이션 시작 등)
- 컴포넌트는 JSX만 반환해야 하며, 렌더링 이전에 존재했던 객체나 변수를 변경해서는 안된다.

```tsx
// 규칙 위한 컴포넌트
let guest = 0;

function Cup(): JSX.Element {
  // 나쁜 지점: 이미 존재했던 변수를 변경하고 있습니다!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet(): JSX.Element {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

```tsx
type CupProps = {
  guest: number;
};

// guest 변수를 프로퍼티로 넘겨 사이드 이펙트 수정
function Cup({ guest }: CupProps): JSX.Element {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet(): JSX.Element {
  //   return (
  //     <>
  //       <Cup guest={1} />
  //       <Cup guest={2} />
  //       <Cup guest={3} />
  //     </>
  //   );

  const cups: JSX.Element[] = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

### 사이드 이펙트를 일으킬 수 있는 지점

1. 이벤트 핸들러
   - 사용자 액션(예: 클릭)에 반응
   - 렌더링 중에는 실행되지 않으므로 순수할 필요 없음
2. useEffect 훅
   - 이벤트 핸들러 외에 적절한 실행 지점을 찾을 수 없을 때 사용
   - 렌더링 이후에 실행되도록 React에 알려주지만 최후의 수단으로 사용하는 것이 좋음

<br />

## (9) 트리로서의 UI

### UI 트리 (컴포넌트 트리)

- React 앱은 **루트 컴포넌트**를 시작으로 여러 컴포넌트가 **중첩된 트리 구조**다.
- 이 구조는 데이터 흐름, 렌더링 최적화 등 다양한 부분에서 핵심적인 역할을 한다.
- 트리의 **루트는 최상위 컴포넌트**, 리프 노드는 자식 컴포넌트가 없는 컴포넌트다.
- 실제 브라우저의 DOM 트리 구조와 유사하다.

  ### 💡 예:

  - App → InspirationGenerator → Copyright
  - App → FancyText

### 렌더 트리 (Render Tree)

- 컴포넌트가 렌더링되면, React는 **렌더 트리**라는 내부 구조를 구성한한다.
- 렌더 트리는 UI에 **실제로 나타나는 컴포넌트들의 구조**를 나타낸다.
- **조건부 렌더링**에 따라 렌더 트리는 상황에 따라 달라질 수 있다.

  ### 💡 렌더 트리와 DOM 트리의 차이:

  - 렌더 트리는 컴포넌트 중심
  - DOM 트리는 HTML 요소 중심

### 모듈 의존성 트리

- React 앱은 수많은 **JavaScript 모듈**로 구성되며, 각 모듈은 다른 모듈을 `import`하거나 `require`한다.
- 이러한 관계를 트리로 표현한 것이 **모듈 의존성 트리**다.
- 이 트리는 Webpack, Vite 등 번들러가 **코드 분할, 번들 최적화**에 활용된다.

  ### 💡 렌더 트리와의 차이점:

  - **렌더 트리**는 UI에 표시되는 컴포넌트 중심
  - **의존성 트리**는 파일 간 `import` 관계 중심
  - 비컴포넌트 모듈(ex: utils, data.js)도 포함

### 트리 구조 이해의 중요성

- 성능 분석: 루트 → 리프 구조를 이해하면 어떤 컴포넌트가 자주 렌더되는지 파악 가능
- 상태 관리: 데이터가 어디서 시작해 어디로 흐르는지 파악 가능
- 디버깅과 유지보수에 매우 유리
