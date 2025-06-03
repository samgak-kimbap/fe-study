---
layout: post
title: 리액트 props&state&useReucer&Immer.js
author: balancelife99
date: 2025-06-03 15:17:00 +0900
categories: [REACT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [props, state, useReducer, Immer.js]
---

1. props와 state의 차이
2. Props가 컴포넌트간에 전달받는 것이라고 했는데 자식에서 부모로도 전달할 수 있는가
3. React에서 state의 불변성을 유지하라는 말이 있는데 이에 대해 설명해달라
4. 리듀서 내부에서 불변성을 지키는 이유는? 전개 연산자의 단점을 해결할 수 있는 방법은 무엇인가

이 네가지 질문에 대한 답변을 위한 공부이다.

# props & state

리액트 라이브러리에서의 props 와 state 란?

# props란?

props는 **properties(속성)**의 줄임말로, React에서 **컴포넌트 간 데이터를 전달**할 때 사용하는 객체이다.
정확히 말하면, **부모 컴포넌트가 자식 컴포넌트에게 값을 넘겨줄 때 사용되는 읽기 전용(Read-Only) 데이터**이다.

## props 특징

1. "단방향 전달"
   props는 항상 **부모 → 자식** 방향으로만 전달된다. 자식 컴포넌트는 받은 props를 화면에 출력하거나 로직에 사용할 수 있지만, 그 값을 변경할 수는 없다.

2. "객체 구조"
   React 컴포넌트는 사실상 JavaScript 함수(function) 또는 **클래스(class)**로 작성된 코드야.
   이때 props는 그 함수에 전달되는 **하나의 인자(argument)**이며, JavaScript 객체(Object)\*\* 형태로 전달된다.

```jsx
function Greeting(props) {
  console.log(typeof props); // "object"
  console.log(props); // 예: { name: "민준", age: 25 }
  return <div>{props.name}</div>;
}
```

즉, props는 일반적인 객체처럼 다음과 같은 특징을 그대로 가짐: - key-value 구조로 되어 있음 - 점 표기법(props.name) 또는 구조 분해 할당(const { name } = props) 가능 - 얕은 복사, Object.keys(), for...in 등 JS의 객체 연산 사용 가능

3. "props는 컴포넌트의 첫 번째 인자로 자동 전달된다."

알다시피 React에서 우리가 사용하는 컴포넌트는 사실 JavaScript 함수이다.
예를 들어:

```jsx
function Welcome(props) {
  return <h1>리액트 세상에 어서오세요, {props.name}님!</h1>;
}
```

이라는 props 라는 이름의 매개변수를 받는 함수가 있다고 했을 때,

우리는 이 함수를 JSX 문법을 통해 컴포넌트로 사용하며, 사용할때는 아래와 같이 사용한다.

```jsx
<Welcome name="빈지노" />
```

그런데 React 는 이 코드를 내부적으로 **함수 호출** 로 변환해서 처리한다.

실제로는 아래처럼 동작하는 것과 매우 비슷하다.

```js
Welcome({ name: "민준" });
```

→ 즉, `<Welcome name="빈지노" />` 이 JSX 문법의 한 줄이
→ js에서 `Welcome()` 이라는 함수에 `{ name: "빈지노" }` 객체를 첫 번째 인자로 자동 전달하는 것과 같아.
이 말이 바로

"props는 컴포넌트의 첫 번째 인자로 자동 전달된다"
라는 의미.

"React에서는 모든 props를 하나의 객체로 묶어서 컴포넌트에 넘기기 때문에, 이 객체 하나가 함수의 '첫 번째이자 유일한 인자'가 된다."

그래서 "첫 번째 인자"라는 표현은 기술적으로 정확한 말이고, "단 하나의 인자" 라고 이해해도 무방해보인다.

---

# state란?

- `state`는 영어로 **상태**를 뜻하지만, React에서는 **컴포넌트의 "변경 가능한 데이터"**를 의미한다.
- JavaScript에서 `let`, `const`로 변수를 선언하듯이,  
  React에서는 **UI에 영향을 주는 값**을 state로 관리한다.

## state는 자바스크립트 객체다.

```jsx
const [state, setState] = useState({ key: value });
```

- state는 그냥 일반적인 JavaScript 객체다.
- 별도의 특수한 자료형이 아니고, `{}` 안에 원하는 구조로 값을 정의할 수 있다.

---

## 클래스형 vs 함수형 컴포넌트의 state

### 클래스형 컴포넌트

```jsx
class LikeButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = { liked: false };
  }

  handleClick = () => {
    this.setState({ liked: true });
  };
}
```

- 클래스 컴포넌트에서는 `this.state`로 초기값을 정의하고,  
  상태를 바꿀 땐 반드시 `this.setState()`를 써야 한다.

### 함수형 컴포넌트 (Hook)

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

- `useState()`를 사용해서 상태를 만들고,  
  `set함수`를 통해 상태를 변경한다.

---

## state는 직접 수정하면 안 된다.

```js
this.state = { name: "react" }; // ❌ 직접 수정
this.setState({ name: "react" }); // ✅ setState로 수정
```

- state를 직접 바꾸면 React는 변경을 감지하지 못해서 화면이 업데이트되지 않는다.
- 반드시 `setState()`나 `set함수()`로 상태를 바꿔야 컴포넌트가 다시 렌더링된다.

---

## 언제 state를 써야 할까?

UI에 영향을 주는 값이면 state로 관리해야 한다.

예시:

- 버튼 클릭 횟수
- 입력창 값
- 모달 열림 여부
- 서버에서 받아온 데이터 등

화면에 영향을 주지 않는 임시 값이나 내부 계산값은 `state` 말고 일반 변수나 `ref`로 처리하는 게 더 적절하다.

---

## state는 최소한만 유지해야 한다

- 계산으로 얻을 수 있는 값은 굳이 state로 만들 필요 없다.

```js
const [items, setItems] = useState([]);
const itemCount = items.length; // 이렇게 계산해서 쓰면 된다
```

불필요하게 많은 state를 만들면 렌더링이 과하게 일어나서 성능이 떨어질 수 있다.

---

## 정리

- state는 React 컴포넌트의 내부에서 정의하고 사용하는 **변경 가능한 데이터**다.
- 자바스크립트 객체 형태이고, 반드시 `set함수`를 통해 변경해야 한다.
- 렌더링에 직접 영향을 주는 값만 state로 관리해야 한다.

---

이렇게 props와 state에 대해 간단히 알아 보았다.
면접에 나오는 다음 네 질문에 대해 정리하고 블로깅을 마치겠다.

## 1. props와 state의 차이

props는 컴포넌트 외부에서 주어진 읽기 전용 데이터,
state는 컴포넌트 내부에서 관리하는 변경 가능한 데이터

---

## 2. Props가 컴포넌트간에 전달받는 것이라고 했는데 자식에서 부모로도 전달할 수 있는가?

**직접적으로는 불가능하다.**  
props는 **부모 → 자식 방향으로만 일방향 전달(one-way binding)**된다.

하지만 **"간접적으로는 가능하다"**.  
→ 방법은? **자식에게 함수를 props로 내려주고, 자식이 그 함수를 호출하는 방식**을 사용한다.

#### 예제: 자식 → 부모로 데이터 전달하는 패턴

```jsx
// 부모 컴포넌트
function Parent() {
  const handleReceive = (data) => {
    console.log("자식으로부터 받은 값:", data);
  };

  return <Child onSend={handleReceive} />;
}

// 자식 컴포넌트
function Child({ onSend }) {
  return (
    <button onClick={() => onSend("🔥 자식에서 생성한 data")}>
      부모에게 전달
    </button>
  );
}
```

1. Parent는 handleReceive라는 함수를 정의하고,
2. 그 함수를 onSend라는 이름으로 자식에게 props로 전달
3. Child는 버튼 클릭 시 onSend("🔥 자식에서 생성한 data")를 호출해서 데이터를 역방향으로 전달

`handleRecieve` 함수, aka `onSend` 에서 '🔥 자식에서 생성한 data' 를 역방향으로 받았으니
결과는 `console.log("자식으로부터 받은 값:", 🔥 자식에서 생성한 data");` 가 되어
"자식으로부터 받은 값:", 🔥 자식에서 생성한 data 가 출력된다.

→ 이렇게 하면 자식 컴포넌트에서 부모 컴포넌트로 데이터를 넘기는 것이 가능하다.

#### 이 패턴을 사용하는 상황

- 입력값을 자식 컴포넌트에서 받고 부모가 처리해야 할 때
- 이벤트(클릭, 입력 등)를 자식이 감지하고 부모에게 알릴 때
- 부모의 상태(state)를 자식이 제어해야 할 때

## 3. React에서 state의 불변성을 유지하라는 말이 있는데 이에 대해 설명해달라

### 불변성이란?

불변성(immutability)이란 **데이터를 직접 변경하지 않고, 복사본을 만들어 새로운 값을 할당하는 방식**을 말한다.  
즉, 기존 객체를 그대로 두고 **새로운 객체를 만들어 교체하는 것**이다.

### ✅ 왜 React에서는 불변성을 유지해야 할까?

React는 state가 바뀌었는지를 확인할 때,  
객체 내부를 깊게 비교하지 않고 **얕은 비교(shallow comparison)**로 판단한다.

- 얕은 비교(shallow comparison)는 객체의 **참조값(주소)**만 비교하여 같은지 판단하는 방식.
- 내부 속성 값이 같아도 주소가 다르면 다르다고 판단하고, 주소가 같으면 내부가 바뀌어도 같다고 본다.

즉, **참조값(주소값)**만 보고 변경 여부를 판단한다.

```jsx
const obj1 = { a: 1 };
const obj2 = { a: 1 };

obj1 === obj2; // false (서로 다른 객체)
```

→ 이처럼 객체가 바뀌었는지를 확인할 때는 **주소값**만 본다.  
→ 그래서 객체 내부 값을 바꿔도 주소가 같으면 React는 "변화 없음"으로 판단해서 **렌더링하지 않는다.**

---

### ❌ 불변성을 지키지 않은 방식

```jsx
const [form, setForm] = useState({ name: "", email: "" });

const handleNameChange = (e) => {
  form.name = e.target.value; // ❌ 직접 수정
  setForm(form); // ❌ 참조값 그대로라 변화 감지 안 됨
};
```

→ form 객체의 주소는 그대로이기 때문에, React는 변화된 줄 모름
→ 렌더링 안 되거나 UI 반영 안 될 수 있음

---

### ✅ 불변성을 지킨 올바른 방식

````jsx
// 입력 폼 상태 관리
const [form, setForm] = useState({ name: "", email: "" });

// 이름 입력값 변경 시
const handleNameChange = (e) => {
  setForm({
    ...form,
    name: e.target.value,
  });
};```
→ 기존 form 객체를 복사하고, name만 덮어써서 새로운 객체로 교체함

---


### 정리

React는 참조값(주소)을 기준으로 상태 변경을 판단하기 때문에,
기존 값을 직접 수정하지 않고 **새로운 객체나 배열을 만들어 교체하는 방식(불변성 유지)**을 사용해야 한다.

---

## 리듀서 내부에서 불변성을 지키는 이유는?, 전개 연산자의 단점을 해결할 수 있는 방법은 무엇인가?

### 리듀서(reducer)란?
리듀서는 **상태(state)를 업데이트하는 함수**이다.
현재 상태(state)와 액션(action)을 받아서,
**새로운 상태를 반환(return)**하는 역할을 한다.

 리듀서는 **직접 상태를 변경하지 않고**,
**새로운 상태를 만들어 반환**해야 한다. → 불변성 유지!

이름은 배열의 `.reduce()` 함수에서 따온 개념이다.
```js
[1, 2, 3].reduce((acc, cur) => acc + cur, 0); // 결과: 6
→ 현재 상태(acc)와 새로운 값(cur)을 받아서
→ 누적된 결과를 만들어낸다는 점에서 유사하다.
````

### 리듀서 기본 구조 (함수형 컴포넌트 기준)

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { ...state, count: state.count + 1 };
    case "decrement":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};
```

- state: 현재 상태
- action: 어떤 변화가 일어났는지를 설명하는 객체 (type 필수)
- return: 새로운 상태 객체

### useReducer 사용 방법

```js
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

- state : 현재 상태 값
- dispatch(action) : 리듀서에 액션을 보냄 -> 상태 업데이트

사용 예 :

```js
<button onClick{() => dispatch({ type: "increment" })}>+</button>
```

-> `dispatch`가 reducer에 `{ type: "increment" } 를 전달하고, reducer는 새로운 state를 만들어서 반환한다.

### 언제 useState 대신 useReducer 쓰면 좋을까?

- 상태가 여러 값으로 구성돼 있고, 업데이트 로직이 복잡할 때
- 여러 종류의 액션이 발생할 때 (increase, decrease, reset 등)
- 컴포넌트 외부에서 상태 업데이트 로직을 분리하고 싶을 때

| 상황                                              | `useReducer`가 많이 쓰이는 이유               |
| ------------------------------------------------- | --------------------------------------------- |
| 복잡한 폼(form) 입력 상태 관리                    | `name`, `email`, `password` 등 필드가 많을 때 |
| 상태 변경 액션이 여러 개일 때                     | `add`, `remove`, `toggle`, `reset` 등         |
| 컴포넌트가 크고 상태 변경 로직을 분리하고 싶을 때 | reducer 함수로 바깥에 빼두면 가독성 👍        |
| Redux를 흉내낸 구조가 필요할 때                   | 작은 규모의 전역 상태처럼 씀                  |

---

다시 질문으로 돌아와서,

## 리듀서 내부에서 불변성을 지키는 이유는?

리듀서 내부에서 불변성을 지키는 이유도 동일하다.
리듀서는 “규칙”만 있는 함수라 개발자가 그 안에서 state.value = 123 같이 직접 상태를 바꾸면,
React나 Redux는 그걸 감지하지 못하거나, 잘못 작동할 수 있다.
다시 한번 정리하지만 React나 상태의 참조값이 바뀌었는지를 기준으로 리렌더링 여부를 판단하기 때문에,
리듀서 내부에서 불변성을 지키지 않으면 변경을 감지하지 못하거나, 예기치 않게 동작할 수 있다.

## 전개연산자의 단점과 극복할 수 있는 방법은?

우선 전개연산자의 단점을 보자.

```jsx
const nextState = {
  ...state,
  user: {
    ...state.user,
    profile: {
      ...state.user.profile,
      name: "민준",
    },
  },
};
```

- 중첩이 깊어질 수록 `...` 가 반복되어 코드가 길고 복잡해진다.
- 고로 버그가 나기 쉽다.
- 가독성이 안좋다.

> 극복 방법으로는 Immer.js 라는 녀석이 있다!

"직접 수정하는 것처럼 작성하지만 내부적으로는 불변성을 지켜주는" 라이브러리다."

### 전개 연산자 vs Immer.js

전개연산자 코드이다.

```js
const nextState = {
  ...state,
  user: {
    ...state.user,
    name: "프렌치 토스트",
  },
};
```

Immer 사용 코드이다.

```js
import produce from "immer";

const nextState = produce(state, (draft) => {
  draft.user.name = "프렌치 토스트";
});
```

위 꼴로 Immer.js 를 사용할 수 있는데, Immer 를 사용하면 마치 직접 수정하듯이 수정해도 내부적으로는 **"복사 + 변경 추적 + 새로운 객체 생성"**을 자동으로 해준다.

1. currentState를 얕게 복사함
2. draft라는 가짜 객체를 만들어서 네가 수정한 내용을 기록
3. 그 변경사항을 바탕으로 새로운 객체를 만들어서 반환

---

정리하자면,
전개 연산자는 얕은 복사에는 간편하지만,
중첩된 객체를 업데이트할 땐 코드가 길어지고 실수하기 쉽다.
이를 해결하기 위해 Immer.js를 사용하면,
직접 수정하는 것처럼 작성하면서도 내부적으로 불변성이 유지되기 때문에
코드 가독성과 안정성이 모두 좋아진다.
