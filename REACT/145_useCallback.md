---
layout: post
title: useCallback과 앞으로
author: haeran
date: 2025-06-09 21:07:00 +0900 
categories: [REACT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [useCallback]
---

사실 내부적으로 `useCallback(fn, deps)`는 아래와 완전히 같은 구조입니다.

```ts
  useMemo(() => fn, deps)
```

즉, 함수 자체를 메모이제이션하는 것이고, 그 함수가 의존하는 값(deps)이 바뀌지 않으면 이전 함수 참조를 그대로 반환합니다.

## 그럼 이 메모이제이션은 왜 쓰는데?

우리는 여태껏 부모 컴포넌트가 변경되면, 자식 컴포넌트는 당연히 렌더링 된다고 생각하죠.

> "그럼 useMemo와 useCallback만을 props로 받는 자식 컴포넌트는 부모 컴포넌트가 변해도 랜더링 되지 않을수도 있어?"

정확해요! 질문에 대한 답은 **"그렇다"** 입니다.  
조건이 맞는다면 부모가 렌더링돼도 자식은 리렌더링되지 않을 수 있어요.

### 랜더링이 적어진다. = 성 능 향 상

1. **성능 향상 (Performance Boost)**
   렌더링은 React에서 컴포넌트 함수를 실행하고, **가상 DOM을 비교(diff)**하고, 실제 DOM 업데이트까지 이어지는 작업이기 떄문에, 이 작업이 많아지면 CPU 사용량과 메모리 사용량이 늘어나요.

   특히 컴포넌트 트리가 깊거나 리스트가 길면 프레임 드랍, 버벅임으로 체감되기도 하죠.

   > 예: 리스트 앱에서 1000개 항목 중 1개만 바꿨는데 999개도 같이 렌더링되면? → 큰 낭비고, 사용자 입장에선 "느려" 보이게 돼요.

2. **불필요한 연산 감소**
   렌더링이 많다는 건 그 안에서 돌아가는 useEffect, 비싼 계산, 상태 체크들도 반복된다는 뜻이에요.  
   특히 차트, 애니메이션, 필터링 로직 등은 렌더링 한 번당 많은 연산을 할 수 있어요.

   렌더링을 줄이면 이런 비용이 드는 계산도 덜 하게 되죠.

3. **앱의 반응성 향상 (Responsiveness)**
   버튼 클릭 → 화면 반응까지 걸리는 시간, 즉 UX 품질에 직결!

   렌더링 최적화는 결국 "쓸데없는 재계산 없이 필요한 부분만" 업데이트하게 해서 사용자에게 빠른 반응성을 제공합니다.

4. **배터리, 자원 효율 개선 (모바일 환경)**
   모바일 환경에서는 리렌더링이 잦으면 배터리와 발열에도 영향을 줄 수 있어요. 성능 최적화는 곧 배터리 최적화!

### useMemo와 useCallback

| 항목        | `useCallback`                     | `useMemo`                          |
| --------- | --------------------------------- | ---------------------------------- |
| **반환값**   | 메모이제이션된 **함수**                    | 메모이제이션된 **값**                      |
| **사용 목적** | 함수가 **불필요하게 다시 생성** 되지 않도록         | 값이 **불필요하게 다시 계산** 되지 않도록           |
| **일반 사용** | 콜백 함수 전달 (React.memo와 함께 주로 사용)   | 복잡한 계산 결과 재사용                      |
| **사용 예**  | 이벤트 핸들러, API 호출, setter 등         | 필터링, 정렬, 숫자 계산 등                   |
| **예시 코드** | `useCallback(() => fn(), [deps])` | `useMemo(() => compute(), [deps])` |
| **중복**    | `useMemo(() => fn, deps)`와 동일     | –                                  |

### ▶ `useCallback` 사용하는 상황

* 콜백 함수를 **자식 컴포넌트에 props로 전달** 할 때
* 콜백 함수가 deps에 따라 **의미 있는 변경** 을 가져올 때
* `React.memo`를 사용하는 자식 컴포넌트가 자주 리렌더링되는 경우

```js
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);
```

### ▶ `useMemo` 사용하는 상황

* 렌더링 시 **값 계산 비용이 높은 경우**
* 필터링, 정렬, 숫자 계산 등에서 결과를 재사용하고 싶을 때

```js
const sortedList = useMemo(() => {
  return list.sort(compareFn);
}, [list]);
```

## 함께 조합해서 쓰는 경우

함수 결과가 복잡한 계산을 포함하고 자식에 전달할 필요가 있다면 **두 훅을 조합** 합니다

```js
const expensiveFn = (x) => compute(x);

// useMemo로 계산 결과 저장
const value = useMemo(() => expensiveFn(data), [data]);

// 그 결과를 콜백에 담아서 useCallback으로 전달
const handleChange = useCallback(() => {
  console.log(value);
}, [value]);
```

## 사용 판단 기준: 비용 계산은 어떻게?

1. **계산 비용이 얼마나 높은가?**

   * 무거운 계산일수록 `useMemo`의 효과가 큼
   * 간단한 연산이면 오히려 불필요한 최적화

2. **종속성(deps)의 변경 빈도는?**

   * 자주 바뀌면 캐싱 이득이 적음

3. **렌더링 빈도는?**

   * 자주 렌더링되면서 값/함수가 바뀌지 않는다면 효과 큼

4. **참조 안정성이 중요한가?**

   * `useCallback`은 함수 참조 유지가 목적이므로, React.memo나 useEffect의 deps로 쓰이는 함수는 고정 참조가 유리

### 🔧 비용-이득 분석 예

| 조건                        | `useMemo/useCallback` 효과 |
| ------------------------- | ------------------------ |
| 값 계산이 1ms 이상              | ✅ 유의미한 성능 이득 가능          |
| 값 계산이 < 0.1ms             | ❌ 캐싱 오버헤드가 더 큼           |
| 함수가 자식 컴포넌트 props로 자주 전달됨 | ✅ useCallback 권장         |
| 함수가 내부에서만 쓰이고 deps 없음     | ❌ useCallback 불필요        |

## React 19 이후 달라진 최적화 방향

React 19의 새로운 컴파일러(일명 “React Forget” [링크](https://srj-shau.medium.com/react-without-memo-react-forget-react-optimizing-compiler-19f8c02e8930))는 컴파일 타임에 자동으로 메모이제이션 및 참조 안정화를 적용합니다.

### 특징

* **자동 메모이제이션**: 컴파일러가 빌드 타임에 JSX와 함수 내부를 분석해, 값과 함수, React 요소를 자동으로 memoize 합니다.
* **함수 참조도 자동 안정화**: 직접 `useCallback`을 쓰지 않아도 동일한 효과를 얻을 수 있습니다.
* **불필요한 리렌더링 방지**: `React.memo` 없이도 중첩된 자식 컴포넌트 리렌더링을 줄입니다.

### 가자 실전 도입

1. **컴파일러 활성화**
   * React 19(혹은 전용 컴파일러 RC)를 설치하고, Babel 또는 Next.js 설정에서 플러그인 활성화.
2. **코드 리팩터링**
   * 기존 `useMemo`, `useCallback`, `React.memo` 코드 제거 → 컴파일러가 메모이제이션 관리하도록 전환.
3. **성능 프로파일링**
   * 내장 React DevTools나 Chrome Performance 탭 사용 → 병목 구간이 있다면 명시적인 훅 사용 고려.
4. **예외 처리**
   * **외부 라이브러리 요구**나 **deep equality** 가 필요한 경우, 컴파일러가 감지하지 못할 수 있으므로 수동 메모이제이션 사용.

### 앞으로의 사용 전략

| 상황          | 기존 방식                    | React 19 이후 방식                  |
| ----------- | ------------------------ | ------------------------------- |
| 단순 연산       | `useMemo()`              | 그냥 계산하고, 렌더링 결과 프로파일링           |
| 콜백 전달       | `useCallback()`          | 참조 안정성 필요 시 React.memo/컴파일러 활용  |
| 고비용 계산      | `useMemo()`              | 여전히 사용하되, 프로파일링 기반으로 판단         |
| 자식 컴포넌트 렌더링 | React.memo + useCallback | React.memo + React 19 자동 메모이제이션 |

1. React 19부터는 “훅 과다 사용”이 아니라 “필요한 경우 최소로 사용하는 것”이 원칙입니다.
2. 새로운 컴파일러 기술 덕분에 많은 자동 최적화가 가능해졌으며, 훅은 여전히 “문제 발생 시 수동 개입 수단”으로 있어야 합니다.
3. 따라서, 지금은 불필요한 useMemo/useCallback 감축, 구조 개선, React.memo 활용, 그리고 React 19 컴파일러 능력 활용이 최선입니다.
