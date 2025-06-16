---
layout: post
title: suspense
author: balancelife99
date: 2025-06-15 23:00:00 +0900
categories: [REACT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [suspense]
---

# Suspense

웹 애플리케이션을 개발하다 보면 데이터 로딩 속도로 인해 사용자 경험이 저하되는 문제에 직면하게 됩니다. 특히 대용량 데이터를 다루는 경우에는 로딩 속도가 더욱 중요한 요소로 작용합니다.

이런 상황에서 **React Suspense**를 활용하면 데이터 로딩 상태를 보다 세련되게 처리하고, 사용자에게 더 나은 경험을 제공할 수 있습니다.

React 19부터 Suspense는 서버에서도 작동하며, `use()`와 함께 서버 사이드 렌더링에서도 사용할 수 있습니다.

Next.js의 App Router 구조에서는 서버 컴포넌트를 Suspense로 감싸 fallback 처리하는 방식이 일반적입니다.

React Suspense가 무엇이고, 왜 사용해야 하는지, 그리고 어떻게 적용할 수 있는지 알아보겠습니다.

## Suspense란?

영단어 suspense는 '미결', 아직 결정되지않은, 이라는 의미를 가지고 있습니다.

**`<Suspense>`는 자식 요소의 렌더링이 지연될 경우, 로딩 중 보여줄 "대체 UI(fallback)"를 지정하는 React 컴포넌트**입니다.

> "이 컴포넌트가 아직 준비되지 않았으면, 그동안 이 UI를 보여줘!" 라고 선언적으로 말할 수 있게 해줍니다.

#### 기본 틀

```tsx
import { Suspense } from "react";

<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>;
```

SomeComponent가 준비되기 전까지 <Loading />을 보여줍니다.
준비되면 자동으로 실제 컴포넌트로 전환됩니다.

### Suspense가 작동하는 경우

Suspense의 핵심 기능은 바로 “아직 준비되지 않은 것”을 기다려주는 것입니다.
근데 뭘 기다리느냐? → 비동기적으로 처리되는 데이터나 컴포넌트

즉, Suspense는 비동기 동작(로딩 중)을 감지하고, 그동안 fallback UI를 보여주는 기능이기 때문에
결국 **"비동기를 감지할 수 있는 상황에서만 작동”** 할 수 있습니다.

비동기를 감지할 수 있는 상황은 다음과 같습니다.

| 케이스                   | 설명                                      |
| ------------------------ | ----------------------------------------- |
| `React.lazy()`           | 컴포넌트를 지연 로딩할 때                 |
| `use(Promise)`           | 서버 컴포넌트에서 비동기 데이터 기다릴 때 |
| `Next.js`, `Relay` 등    | Suspense 지원 프레임워크에서              |
| 서버 렌더링 중 에러 발생 | fallback 처리 가능                        |

❗ 단 Suspense는 컴포넌트 렌더링 중 발생하는 비동기만 감지할 수 있습니다.
❌ useEffect처럼 렌더링 이후의 비동기 작업은 React가 감지할 수 없기 때문에 fallback UI를 자동으로 보여줄 수 없습니다.

## Suspense로 가능한 것들

### 1. 코드 스플리팅

코드 스플리팅이란 앱을 여러 덩어리로 쪼개서 필요한 시점에만 필요한 것을 불러오게 만드는 기술

그것을 도와주는 도구가 **React.lazy()** 입니다.

```tsx
const LazyComponent = React.lazy(() => import("./MyComponent"));

<Suspense fallback={<Loading />}>
  <LazyComponent />
</Suspense>;
```

`/MyComponent` 는 처음 실행될때 안 불러와지고, 실제로 사용될 때(import 시점) 에 따로 로딩됩니다.

### 2. 데이터가 준비될 때까지 기다리기 (React 19)

React 19에선 use(Promise)를 통해 서버 측 데이터도 Suspense로 감쌀 수 있습니다.

#### use(Prmoise)란?

React 19 에서 새롭게 등장한 기능으로, Promise(비동기 결과값)을 컴포넌트 안에서 직접 꺼내 쓸 수 있게 해주는 함수 입니다.

기존 비동기 데이터를 가져오려면 항상 `useEffect + useState` 조합을 썼어야 했지만, 이제는 use(Promise)와 Susepnse를 함께 쓰면 더 깔끔하고 선언적으로 처리할 수 있습니다.

```tsx
const postPromise = fetch('/api/post').then(res => res.json());

function Post() {
  const post = use(postPromise); // 바로 결과 꺼내 쓸 수 있음!
  return <h1>{post.title}</h1>;
}

export default function Page() {
  return (
    <Suspense fallback={<p>로딩 중...</p>}>
      <Post />
    </Suspense>
  );
}
}
```

dataPromise가 아직 완료되지 않았을 때 → React는 fallback을 보여줍니다.
완료되면 → data를 가져와서 바로 렌더링합니다.

### 3. 중첩된 Suspense로 UI 점진적 표시

```tsx
<Suspense fallback={<BigSpinner />}>
  <Biography />
  <Suspense fallback={<AlbumsGlimmer />}>
    <Albums />
  </Suspense>
</Suspense>
```

Biography가 준비되지 않으면 전체를 BigSpinner로 보여줍니다.
Biography가 준비되면 Biography는 보이고, Albums는 따로 AlbumsGlimmer 보여줌
이처럼 점진적으로 콘텐츠가 단계별로 표시되게 할 수 있습니다.

## 📝 마무리 정리

- Suspense는 React에서 **비동기 렌더링을 우아하게 처리할 수 있는 핵심 도구**입니다.
- `lazy()`를 통한 **코드 스플리팅**, `use()`를 통한 **데이터 로딩 대기**, 중첩을 통한 **로딩 단계 제어** 등 다양한 UI 최적화에 활용됩니다.
- React 19부터는 서버 컴포넌트에서도 Suspense가 폭넓게 사용되며, Next.js의 App Router에서도 중요한 역할을 합니다.
