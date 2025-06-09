# react-hook(2) - useEffect, useLayoutEffect
## useEffect? 
 **컴포넌트가 렌더링된 이후 발생하는 “부수 효과(side effect)“를 처리**하는 데 사용됩니다.

```jsx
useEffect(() => {
  // 여기에 실행할 코드 (effect)를 작성
  return () => {
    // 선택사항: cleanup 코드 (컴포넌트가 사라질 때 실행됨)
  };
}, [의존성배열]);
```

| **항목**          | **설명**                             |
| --------------- | ---------------------------------- |
| 첫 번째 인자         | 부수 효과를 실행할 함수                      |
| return 값 (선택사항) | 컴포넌트가 사라질 때 실행할 정리(clean-up) 함수    |
| 두 번째 인자         | 의존성 배열. 여기에 있는 값이 바뀔 때만 effect 재실행 |
### 의존성 배열의 사용

| **의존성 배열** | **설명**                              |
| ---------- | ----------------------------------- |
| []         | 처음 마운트될 때 한 번만 실행 (ex. 초기 데이터 불러오기) |
| [count]    | count가 바뀔 때만 effect 실행              |
| 생략         | 매 렌더링마다 실행됨 (비추천: 성능 문제 발생 가능)      |
### **useEffect의**  **동작 순서 (Lifecycle 기반)**
React 함수형 컴포넌트가 렌더링되는 흐름은 다음과 같습니다:

```
1. 컴포넌트 함수 실행 → JSX 리턴 (DOM 트리 준비)
2. DOM 렌더링 완료됨 (브라우저에 반영됨)
3. useEffect 내부 함수 실행
4. 다음 렌더링 전 → 이전 useEffect의 cleanup 함수 실행 (필요 시)
```

```jsx
useEffect(() => {
  console.log('Effect 실행됨');

  return () => {
    console.log('cleanup 실행됨');
  };
}, [count]);
```
- 컴포넌트가 처음 렌더링되면 → Effect 실행됨
- count가 바뀌면:
    1. 먼저 기존 cleanup 실행됨
    2. 그 다음 새로운 Effect가 실행됨
### **useEffect는** **비동기적으로 실행된다**
- useEffect는 컴포넌트 **렌더링 후 비동기적으로 실행**됩니다.
- 즉, 렌더링 중에는 실행되지 않으며, **화면이 그려진 후 브라우저가 idle 상태일 때 실행**됩니다.
- 그래서 useEffect 안에 DOM 접근, API 호출 등을 넣어도 안전할 수 있다.

### **그럼**  **useEffect(() => async ...)**  **는 왜 안 될까?**
 useEffect의 첫 번째 인자는 **동기 함수**여야 합니다.
하지만 비동기 작업은 내부에서 처리할 수 있어요.
#### **❌ 잘못된 코드**
```jsx
useEffect(async () => {
  const res = await fetch('/api/data');
  ...
}, []);
```

> 이유: useEffect는 **비동기 함수가 cleanup 함수를 반환할 수 없기 때문**이에요. 즉, Promise를 반환하면 React가 cleanup인지 구분을 못 해요.
#### **✅ 올바른 패턴**

```jsx
useEffect(() => {
  // 비동기 함수는 useEffect 안에서 따로 선언해서 실행
  const fetchData = async () => {
    const res = await fetch('/api/data');
    const json = await res.json();
    console.log(json);
  };
  fetchData();
}, []);
```


#### 클래스 컴포넌트와 비교

| **클래스 컴포넌트**         | **함수형 컴포넌트**                                |
| -------------------- | ------------------------------------------- |
| componentDidMount    | useEffect(() => {...}, [])                  |
| componentDidUpdate   | useEffect(() => {...}, [의존성])               |
| componentWillUnmount | useEffect(() => { return () => {...} }, []) |

## useLayoutEffect ?
useEffect 와 비슷하지만 **실행 타이밍이 다르기 때문에 사용하는 목적이 달라집니다**
```jsx
useLayoutEffect(() => {
  // DOM이 그려지기 직전에 동기적으로 실행
  return () => {
    // cleanup 함수 (선택사항)
  };
}, [deps]);
```

> **렌더링된 직후, 브라우저가 화면에 그리기 전에 실행되는 Hook**
> 즉, **화면에 그리기 전에 처리할 작업을 수행**할 수 있음

#### **useLayoutEffect의**  **동작 순서 (Lifecycle 기반)**

```
1. 컴포넌트 함수 실행 → JSX 리턴
2. 실제 DOM에 반영 (렌더 완료)
3. ✅ useLayoutEffect 실행 (동기적)
4. 🖼 브라우저가 화면을 그리기 전 (paint 전)
5. 이후 useEffect 실행 (비동기적)
```

### useEffect 와의 차이

| **항목**        | useEffect                    | useLayoutEffect            |
| ------------- | ---------------------------- | -------------------------- |
| 실행 시점         | 렌더링 후, **화면이 그려진 후 (비동기)**   | 렌더링 후, **화면이 그려지기 전 (동기)** |
| 주 용도          | API 요청, 이벤트 등록, 로직 수행 등      | DOM 측정/조작, 레이아웃 변경 감지 등    |
| UI 깜빡임 발생 가능성 | 있음                           | 없음 (화면 그리기 전에 실행되기 때문)     |
| 실행 방식         | 비동기 (requestIdleCallback 느낌) | 동기 (렌더-페인트 사이 blocking)    |

### useLayoutEffect 는 언제 사용? 
> 화면에 실제로 그려지기 전에 **정확한 DOM 측정**, **스타일 계산**, **스크롤 이동** 등을 하고 싶을 때 사용합니다.
```jsx
// DOM 크기 측정
useLayoutEffect(() => {
  const box = document.getElementById('my-box');
  console.log(box.getBoundingClientRect()); // 정확한 위치와 크기 확인
}, []);
```


```jsx
// 자동 스크롤 조정
useLayoutEffect(() => {
  const chatBox = document.getElementById('chat');
  chatBox.scrollTop = chatBox.scrollHeight;
}, [messages])
```
만약 위의 동작들을 useEffect 로 하게되면 스크롤조정이 paint 이후에 실행되서 살짝 깜빡일 수 있습니다.

### useLayoutEffect 특징
- useLayoutEffect는 **동기적**으로 실행되므로, **렌더링을 블로킹**합니다.
    - 즉, 실행 시간이 길면 **UI 렌더링이 지연**되어 사용자 경험에 나쁠 수 있다.
- 무조건 useLayoutEffect를 쓴다고 더 정확하거나 빠른 건 아닙니다.
    - 대부분의 경우 useEffect로 충분하고, 깜빡임/위치 조정 등 **레이아웃이 관여된 작업**만 useLayoutEffect 사용하면 좋다.
### ✏️ **useLayoutEffect** **특징 요약**
1. **렌더링 직후, 화면 그리기 전(paint 이전)에 동기적으로 실행**됩니다.
    → DOM이 렌더된 직후지만, 브라우저가 화면에 표시하기 전에 실행됩니다.

2. **동기적으로 실행되므로 렌더링을 일시적으로 차단(block)**합니다.
    → 실행 시간이 길면 사용자 경험에 영향을 줄 수 있습니다.

3. **DOM 측정이나 레이아웃 계산이 필요한 작업**에 적합합니다.
    → 예: getBoundingClientRect(), 스크롤 위치 조정 등

4. **화면 깜빡임을 방지할 수 있습니다.**
    → 렌더링 직후 레이아웃 조정을 하면 사용자에게 화면 전환이 부드럽게 느껴집니다.

5. **cleanup 함수를 반환할 수 있습니다.**
    → 다음 렌더링 직전 또는 언마운트 시 호출되어 리소스를 정리할 수 있습니다.

6. **useEffect와 문법은 같지만, 실행 타이밍이 다릅니다.**
    → 필요하지 않으면 useEffect를 사용하는 것이 일반적으로 더 적절하고 성능에 유리합니다.


요약정리

|**항목**|**useEffect**|**useLayoutEffect**|
|---|---|---|
|실행 시점|렌더링 후, paint 후|렌더링 후, paint 전|
|실행 방식|비동기 (비차단)|동기 (차단)|
|주요 목적|API 호출, 비동기 로직|DOM 측정, 레이아웃 조정|
|깜빡임 여부|깜빡일 수 있음|깜빡임 방지 가능|
|성능 영향|적음|렌더 차단 → 신중하게 사용|
