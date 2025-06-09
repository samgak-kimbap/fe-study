---
layout: post
title: useReducer,useRef,customHooks
author: balancelife99
date: 2025-06-09 15:32:00 +0900
categories: [REACT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [useReducer, useRef, customHooks]
---

오늘은 useReducer와 useRef, 그리고 커스텀 Hooks 에 대해 알아보겠다.

그전에 Hooks 라고 하는 것은 무엇일까

# Hooks

Hook은 React 버전 16.8부터 React 요소로 새로 추가되었다.

Hook을 이용하여 기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있다고 한다.

리액트에서는 클래스형 컴포넌트와 함수형 컴포넌트가 있다.

그중 Hook은 함수형 컴포넌트에서 상태(state)나 생명주기 기능(lifecycle 등)을 사용할 수 있게 해주는 함수들이다.

예전에는 클래스 컴포넌트에서만 state, componentDidMount, shouldComponentUpdate 같은 리액트의 핵심 기능을 쓸 수 있었다.

하지만 클래스 문법은 복잡하고, this 바인딩도 까다롭고, 로직 재사용이 어렵단 단점이 있었다.

그래서 함수형 컴포넌트에서도 이런 기능들을 간단하게 쓸 수 있도록 만든 게 바로 Hook이다.

대표적인 Hook들로는 다음과 같이 있다.

| Hook                                     | 설명                                         |
| ---------------------------------------- | -------------------------------------------- |
| `useState`                               | 상태(state) 관리                             |
| `useEffect`                              | 생명주기 기능 (ex: 마운트, 언마운트 시 동작) |
| `useContext`                             | 전역 상태 관리 (Context API 사용 시)         |
| `useRef`                                 | DOM 참조 또는 값 기억                        |
| `useCallback`, `useMemo`                 | 성능 최적화                                  |
| `useReducer`                             | 복잡한 state 로직 관리 (Redux 느낌)          |
| `useLayoutEffect`, `useImperativeHandle` | 고급 생명주기, 커스텀 동작                   |

이 중 오늘은 useReducer와 useRef에 대해 알아보려 한다.

# useReducer

useReducer는 컴포넌트에 `reducer` 라는 것을 추가하는 Hook이다.

이 때 reducer란 "현재 상태(state)"와 "액션(action)"을 받아서, "새로운 상태"를 반환하는 순수 함수이다.

useState보다 복잡한 상태 변화(예: 여러 동작 타입, 상태 분기 처리)가 필요할 때 적합하다.

즉 `상태를 바꾸는 로직을 담은 함수` 이다.

#### reducer 구조

```js
function reducer(state, action) {
  // action에 따라 state를 어떻게 바꿀지 결정
  return newState;
}
```

### useReducer 기본문법

```js
import { useReducer } from "react";

//  1. 초기 상태 정의
const initialState = { count: 0 }; // 객체가 아니어도 상관없지만 여러 상태를 함께 관리할때 useReducer 를 쓰니 객체 형식이 보편적이다.

//  2. reducer 함수 정의 ,(상태를 바꾸는 로직을 담은 함수)
const reducer = (state, action) => {
  switch (
    action.type // 다른 조건문도 사용가능하지만 switch-case문을 가장 많이 쓴다고 합니다.
  ) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return initialState;
    default:
      return state; // 예상치 못한 액션을 방지. 기존 상태를 그대로 유지하려는 목적을 가진 코드
  }
};

//  3. 컴포넌트에서 useReducer 사용
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState); // dispatch = 보내다, '이 액션을 reducer 에게 보내줘' 라는 뜻

  return (
    <div>
      <p>현재 카운트: {state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </div>
  );
}
```

dispatch로 액션을 보낸다.

1. 버튼 클릭 => dispatch 호출됨
2. dispatch({type:'increment'}) 가 reducer 함수에 액션을 전달함.
3. reducer 함수에서 action.type (=increment) 를 읽어서 count 상태가 변경됨.
4. 컴포넌트가 리렌더링 됨.

### 요약 한 줄

> `useReducer`는 **복잡한 상태를 명확하게 구조화해서 관리**할 수 있게 해주는 강력한 React Hook이다.

---

# useRef

useRef는 1. **변하지 않는 값을 저장**하거나, 2. **DOM 요소에 직접 접근할 수 있게** 해주는 React Hook 이라고 한다.

## 1. 변하지 않는 값을 저장

React에서 컴포넌트가 리렌더링되면 함수가 다시 실행되기 때문에,
보통의 변수(let, const)에 값을 저장하면 리렌더링 시 다시 초기화된다.

하지만 useRef는 이런 변수와 다르게, **리렌더링이 되어도 값이 유지**된다.

### 예시

```js
function Example() {
  let count = 0;
  console.log("렌더됨");

  function handleClick() {
    count++;
    console.log(count); // 항상 1만 출력됨
  }

  return <button onClick={handleClick}>Click</button>;
}
```

count는 함수 안의 지역 변수라서 렌더링될 때마다 초기화 된다.
그래서 항상 0이 되고 handleClick을 눌러도 항상 1만 출력된다.

#### useRef 로 해결

```js
import { useRef } from "react";

function Example() {
  const countRef = useRef(0);

  function handleClick() {
    countRef.current++;
    console.log(countRef.current); // 클릭할 때마다 증가함
  }

  return <button onClick={handleClick}>Click</button>;
}
```

countRef.current는 리렌더링되어도 초기화되지 않고 유지된다.

useRef는 컴포넌트가 리렌더링되어도 유지되어야 하는 "숨겨진 기억장소" 같은 역할

#### 그런데 useState를 써서 counc를 담으면 되지 않나?

라는 의문이 들 수 있다. 이전 상황에서는 명확히 useRef 를 쓰는 것이 훨씬 바람직하다.

```js
const [count, setCount] = useState(0);

function handleClick() {
  setCount((prev) => prev + 1);
  console.log(count); // 여기선 이전 값이 찍힐 수 있음 (비동기)
}
```

이렇게 useState를 활용해서 구현할 수 있지만, 명확히 다른 차이가 존재한다.

1. count 값이 바뀌면, useState는 리렌더링 된다. 그러나 useRef는 리렌더링 되지 않는다.
2. count 상태가 useState는 값이 즉시 반영이 되지 않을 수도 있다. 이유는 React가 성능 최적화를 위해 여러 상태 변경을 모아서(batch) 한 번에 리렌더링하려고 비동기적으로 처리하기 때문이다. (업데이트 큐에 등록함)

고로 화면에 보여줄 값 이라면 useState 사용이 적합하고,
렌더링이 필요 없는 내부 변수를 다룰때는 useRef 사용이 적합하다.

## DOM 요소에 직접 접근

React는 기본적으로 "선언형" 방식이기 때문에 직접 DOM을 건드리는 방식은 잘 안 쓰지 않고 보통 상태(state)로 흐름을 관리한다.

하지만 특정 상황에서는 직접 HTML 요소를 제어해야 할 때가 있다.

상태와 props로 UI를 제어하는 게 원칙이지만, 그걸로 할 수 없는 특수한 작업을 위해 열어둔 공식적인 비상구가 useRef.

input에 포커스를 주는 예시를 보자.
`포커스(input 창 마우스 눌렀을 때 깜빡이면서 텍스트 입력가능한 상태를 표시해주는 효과)`

```jsx
import { useRef } from "react";

function FocusInput() {
  const inputRef = useRef(null); // DOM 요소를 참조할 ref 생성

  const handleClick = () => {
    inputRef.current.focus(); // DOM 요소에 직접 접근해서 포커스 줌
  };

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={handleClick}>포커스 주기</button>
    </div>
  );
}
```

물론 document.querySelector 나 getElementById 같은 방식도 가능하다.
그리고 클래스 컴포넌트일때는 createRef 라는 방식도 있고
콜백 ref 라는 방식도 존재한다.

| 방식               | 사용 가능?             | 권장 여부                |
| ------------------ | ---------------------- | ------------------------ |
| `useRef`           | ✅ 함수형 컴포넌트에서 | ✅ 가장 직관적이고 안전  |
| `querySelector` 등 | ✅ 가능은 함           | ❌ React 방식 아님       |
| `createRef`        | ❌ (함수형에선)        | ❌ 클래스 컴포넌트용     |
| 콜백 ref           | ✅ 가능함              | ⚠️ 특별한 상황 외 비권장 |

#### 콜백 ref 궁금한 사람들을 위해 예시

```js
function FocusInput() {
  let inputElement = null;

  const setRef = (element) => {
    inputElement = element;
  };

  const handleClick = () => {
    inputElement && inputElement.focus();
  };

  return (
    <>
      <input ref={setRef} />
      <button onClick={handleClick}>Focus</button>
    </>
  );
}
```

#### 그래서 결론은

그러나 React 함수형 컴포넌트에선 DOM 접근은 useRef가 가장 안전하고 표준적인 방식이다.
다른 방식도 존재하지만, 대부분 구식이거나 리액트 철학에 어긋난다고 한다.

# 커스텀 Hooks

커스텀 Hooks란,

여러 컴포넌트에서 공통으로 사용하는 로직을 하나의 함수로 추출해 재사용할 수 있게 만든 사용자 정의 Hook이다.

즉, React의 useState, useEffect처럼 use로 시작하는 내가 직접 만든 함수형 로직 묶음이다.

### 커스텀 Hooks 만들 때 규칙

1. 이름은 반드시 use 로 시작. -> React가 Hook 임을 인식하기 위해!
2. 최상위에서만 호출. -> 조건문, 반복문 안에서 부르면 안됨. 내부에서 조건 분기나 조절은 콜백 안에서 처리 (useEffect,useState 도 동일한 규칙)

#### 2-1 잘못된 예시

```js
function useBadHook() {
  if (someCondition) {
    useEffect(() => {
      // 조건 만족할 때만 실행하려고?
    }, []);
  }
}
```

#### 2-2 올바른 예시

```js
function useGoodHook(someCondition) {
  useEffect(() => {
    if (someCondition) {
      // 조건 안에서 로직 분기 O
    }
  }, [someCondition]);
}
```

3. 렌더링이나 DOM 접근 목적이 아닌, 로직 재사용용으로 제작, 즉 UI 가 아니라 비즈니스 로직이나 상태관리용으로 만들어야 한다.

#### 3-1 잘못된 예시

```jsx
function useSomething() {
  return <div>안됨 ❌</div>; // ❌ Hook은 JSX 반환하면 안 됨. 이건 useState도 마찬가지
}
```

React 컴포넌트처럼 렌더링 트리에 포함되지 않고, 컴포넌트로 인식되지 않기 때문에 JSX를 리턴해도 아무 일도 안 일어남. useState도 마찬가지다. 값을 저장하고 업데이트 할 수 있도록 해주는 도구일 뿐이다.

---

useReducer와 useRef 둘다 상태관리를 효율적으로 해주는 훅, 훅은 곧 도구의 개념으로 와닿게 되었다.
