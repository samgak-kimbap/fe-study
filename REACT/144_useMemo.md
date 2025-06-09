---
layout: post
title: userMemo
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
tags: [userMemo]
---

`useMemo`는 React 16.8에서 도입된 훅(Hook) 중 하나입니다. hooks는 2019년 발표되었고, 이때부터 함수 컴포넌트에서도 상태와 사이드 이펙트를 관리할 수 있게 되었어요.

비싼(f)가격연산(예: 큰 배열 필터링, 정렬, 복잡한 계산)의 결과를 메모이제이션해, 종속성이 변경되지 않으면 재계산을 건너뛰기 위한 용도입니다.

## 내부적으로 직접 구현해보기

- callback()은 useMemo(() => someFunction(), [deps])에서 넘긴 계산 함수입니다.

- deps는 의존성 배열입니다. 이전 deps와 달라지지 않았다면 callback을 다시 실행하지 않고 이전 값을 반환합니다.

- Object.is는 일반적인 ===보다 더 정밀한 비교(예: NaN === NaN → false, Object.is(NaN, NaN) → true)를 합니다.

```ts
let memoizedValue;
let memoizedDeps;

function useMemo(callback, deps) {
  if (!memoizedDeps || !areDepsSame(deps, memoizedDeps)) {
    // deps가 처음이거나 변경되었으면 새로 계산
    memoizedValue = callback();
    memoizedDeps = deps;
  }
  return memoizedValue;
}

function areDepsSame(newDeps, oldDeps) {
  if (newDeps.length !== oldDeps.length) return false;
  for (let i = 0; i < newDeps.length; i++) {
    if (Object.is(newDeps[i], oldDeps[i]) === false) {
      return false;
    }
  }
  return true;
}

```

## useMemo(calculateValue, dependencies)

컴포넌트의 최상위 레벨에 있는 ‘useMemo’를 호출하여 재렌더링 사이의 계산을 캐싱합니다.

```ts
import { useMemo } from 'react';

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab]
  );
  // ...
}
```

### 매개변수

- **calculateValue**: 캐싱하려는 값을 계산하는 함수입니다. 순수해야 하며 인자를 받지 않고, 모든 타입의 값을 반환할 수 있어야 합니다. React는 초기 렌더링 중에 함수를 호출합니다. 다음 렌더링에서, React는 마지막 렌더링 이후 `dependencies`가 변경되지 않았을 때 동일한 값을 다시 반환합니다. 그렇지 않다면 `calculateValue`를 호출하고 결과를 반환하며, 나중에 재사용할 수 있도록 저장합니다.

- **dependencies**: `calculateValue` 코드 내에서 참조된 모든 반응형 값들의 목록입니다. 반응형 값에는 props, state와 컴포넌트 바디에 직접 선언된 모든 변수와 함수가 포함됩니다. 만약 linter가 React용으로 설정된 경우 모든 반응형 값이 의존성으로 올바르게 설정되었는지 확인할 수 있습니다. 의존성 목록은 일정한 수의 항목을 가져야 하며, `[dep1, dep2, dep3]`와 같이 인라인 형태로 작성돼야 합니다. React는 Object.is 비교를 통해 각 의존성 들을 이전 값과 비교합니다.

### 반환값

초기 렌더링에서 `useMemo`는 인자 없이 `calculateValue`를 호출한 결과를 반환합니다.

다음 렌더링에서, 마지막 렌더링에서 저장된 값을 반환하거나(종속성이 변경되지 않은 경우), `calculateValue`를 다시 호출하고 반환된 값을 저장합니다.

### 주의 사항

- useMemo는 Hook이므로 컴포넌트의 최상위 레벨 또는 자체 Hook에서만 호출할 수 있습니다. 반복문이나 조건문 내부에서는 호출할 수 없습니다. 만일 호출이 필요하다면 새 컴포넌트를 추출하고 상태를 그 안으로 옮겨야 합니다.
- Strict Mode에서는 , React는 실수로 발생한 오류를 찾기 위해 계산 함수를 두 번 호출합니다. 이는 개발 환경에서만 동작하는 방식이며, 실제 프로덕션 환경에는 영향을 미치지 않습니다. (원래 그래야 하는 것처럼) 연산 함수가 순수하다면, 로직에는 영향을 미치지 않습니다. 호출 결과 중 하나는 무시됩니다.
- React는 캐싱 된 값을 버려야 할 특별한 이유가 없는 한 버리지 않습니다. 예를 들어, 개발 단계에서 컴포넌트 파일을 편집할 때 React는 캐시를 버립니다. 개발과 프로덕션 환경 모두에서는 컴포넌트가 초기 마운트 중에 일시 중단되면 React는 캐시를 버립니다. 앞으로 React는 캐시를 버리는 것을 활용하는 더 많은 기능을 추가할 수 있습니다. 예를 들어, 앞으로 React에 가상화된 목록에 대한 기본적인 지원이 추가된다면 가상화된 테이블 뷰포트에서 스크롤 되는 항목에 대한 캐시를 버리는 것이 합리적일 것입니다. 이는 성능 최적화를 위해 useMemo에만 의존한다면 괜찮을 것입니다. 그러나 이는 상태 변수나 ref를 사용하는 것이 더 적합할 수 있습니다.

성능 최적화를 위해서만 `useMemo`를 사용해야 합니다. 이 기능이 없어서 코드가 작동하지 않는다면 근본적인 문제를 먼저 찾아서 수정하세요. 그 후 `useMemo`를 사용하여 성능을 개선해야 합니다.
