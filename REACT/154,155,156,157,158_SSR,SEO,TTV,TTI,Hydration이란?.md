---
layout: post
title: SSR,SEO,TTV,TTI,Hydration
author: balancelife99
date: 2025-06-12 18:00:00 +0900
categories: [REACT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [SSR]
---

SSR이 뭔가요?
SEO가 뭔가요?
TTV, TTI 뭔가요?
서버사이드 렌더링을 지원하기 위한 리액트 API를 알고 있나요 ?
하이드레이션에 대해 알고 있나요?

질문에 대해 답하기 위해 공부해보겠습니다.

---

# SSR 이 뭘까요?

최근 프론트엔드 개발에서 사실상의 표준으로 자리매김 하는 기술이 있다고 합니다.

바로 SSR, Server Side Rendering인데요. Next.js를 비롯한 다양한 프레임워크에서 SSR을 손쉽게 구축할 수 있는 솔루션을 제공하고 있습니다.

SSR(Server Side Rendering)은 **브라우저가 아닌 서버에서 HTML을 렌더링**한 뒤, 완성된 HTML을 클라이언트에게 전달하는 방식입니다.

현대의 SSR은 “첫 HTML 렌더링을 서버에서 처리하고, 이후의 렌더링 사이클은 클라이언트에서 처리”하는 하이브리드 형태의 SSR을 가리킵니다.

Next.js, Astro 등의 현대적인 웹 프레임워크는 기본적으로 제공하는 아키텍처입니다. Static Site Generation이나 Dynamic SSR처럼 다양한 방식이 있습니다.

SSR은 “첫 HTML 렌더링을 서버에서 처리”하기 때문에, 사용자의 화면에 컨텐츠가 그려지는데 걸리는 시간(FCP, First Contentful Paint)가 더 짧습니다.

출처 : https://toss.tech/article/ssr-server

#### 기존 CSR 방식의 흐름

![](https://velog.velcdn.com/images/balancelife99/post/918ba378-d921-493c-88db-5bfcd2b25d23/image.png)

사용자의 화면에 JavaScript 번들이 모두 다운로드된 다음 첫 렌더링을 실행하면서 인증, 데이터 요청 등의 과정을 거치다보니 화면이 렌더링되는 시간이 상대적으로 길었습니다.

#### SSR 방식의 흐름

![](https://velog.velcdn.com/images/balancelife99/post/d83116fb-d97f-467c-9802-dc2b08f04acf/image.png)
인증과 데이터 처리의 첫 과정이 서버에서 먼저 모두 이루어진 다음 사용자는 완성된 HTML을 받아보면서 로딩 속도를 상당히 감축할 수 있습니다.

1. 사용자가 URL에 접속
2. 서버에서 해당 URL에 맞는 HTML을 생성 (React, Vue 등의 프레임워크가 서버에서 실행됨)
3. 완성된 HTML이 브라우저에 전달됨
4. 브라우저는 HTML을 먼저 렌더링한 뒤 JS 파일을 받아 실행
5. React 등은 HTML과 연결(Hydration)하여 인터랙션을 제공

---

### SSR의 장단점

#### 장점

- **SEO에 유리**: 검색엔진이 HTML 컨텐츠를 바로 읽을 수 있습니다.
- **빠른 FCP(First Contentful Paint)**: 첫 로딩 시 사용자에게 빠르게 콘텐츠를 제공합니다.
- SNS 공유 시 메타데이터가 잘 노출됩니다.

#### 단점

- **서버 부하 증가**: 모든 요청마다 HTML을 새로 생성해야 함
- **복잡한 구성**: CSR에 비해 초기 설정, 빌드, 배포 구조가 복잡
- **상태 관리가 까다로움**: 클라이언트와 서버 간의 상태 동기화 필요

### 여기서 잠깐 SEO란?

SEO는 Search Engine Optimization 의 약자로 검색 엔진 최적화를 말합니다.

#### 왜 SSR 방식을 사용하면 SEO에 유리할까?

-> 검색 엔진(예: Google)은 웹사이트를 크롤링(crawling) 하고
그 내용을 인덱싱(indexing) 해서 검색 결과에 노출시켜줍니다.

- 크롤링: HTML 문서를 읽고 링크를 따라가며 사이트를 탐색
- 인덱싱: 수집한 정보를 데이터베이스에 저장해서 검색할 수 있도록 함

그런데 CSR은 자바스크립트로 HTML을 동적으로 만들기 때문에, 즉
초기 로딩 시 브라우저에 빈 HTML 틀만 먼저 전달하고
내용은 자바스크립트가 실행된 후에야 채워집니다.

```js
<!-- 사용자가 처음 받는 CSR HTML -->
<body>
  <div id="root"></div>
  <script src="/main.js"></script>
</body>
```

이 시점에서 #root 안에는 아무 내용도 없습니다.

브라우저가 JS 파일을 실행하고 나서야 내용이 들어가기 때문에 일부 똑똑한 검색엔진(Google)은 JS 실행까지 기다려주지만,모든 크롤러가 그렇진 않습니다.

그러나 SSR의 경우에는 완성된 HTML을 브라우저에 주니까 크롤러가 바로 콘텐츠를 읽고 인덱싱이 가능하고 결과적으로 검색 노출(SEO) 이 잘됩니다.

---

#### TTV, TTI는 무엇인가요?

SSR과 Hydration을 이야기할 때 중요한 성능 지표가 있습니다.
바로 TTV(Time To Visually Ready), **TTI(Time To Interactive)**입니다.

#### TTV (Time To Visually Ready) : 사용자가 콘텐츠를 눈으로 볼 수 있게 되는 시간

서버가 HTML을 미리 만들어 주기 때문에 콘텐츠가 빠르게 눈에 보입니다.

SSR의 장점 중 하나입니다.

#### TTI (Time To Interactive) : 콘텐츠가 실제로 동작 가능해지는 시간

콘텐츠는 보이지만, 자바스크립트를 통해 이벤트 리스너나 상태 등을 연결하는 Hydration 과정이 끝나야 실제로 동작합니다.

이 때문에 사용자가 클릭했을 때 반응이 없는 것처럼 느껴질 수 있습니다.

특히 저사양 기기에서는 Hydration 비용이 커져 TTI가 더욱 늦어집니다.

---

### 여기서 또 잠깐 하이드레이션(Hydration)란?

서버에서 미리 렌더링한 HTML을 클라이언트에서 자바스크립트를 통해 동적으로 활성화하는 과정을 말합니다.

SSR(서버 사이드 렌더링) 또는 SSG(정적 사이트 생성)를 통해 서버에서 생성된 HTML은 브라우저에서 **시각적으로 완성된 페이지**로 보이지만, 이 HTML은 단순히 보이는 것만 가능하며 **동작하지는 않습니다.**

이후 React 같은 프론트엔드 프레임워크가 자바스크립트를 실행하여, 서버에서 생성된 HTML 구조와 연결하고 이벤트 리스너 등을 부여해서 살아있는 React앱을 만드는 과정이 필요한데, 이를 수분을 충전시켜서 살아나게 만드는 것처럼 보인다해서 **Hydration(하이드레이션)**이라고 합니다.

서버에서 응답한 HTML (SSR 결과):

```html
<div id="root">
  <button>클릭</button>
</div>
```

이 시점에서는 버튼이 보일 뿐이며, 실제로 클릭해도 아무 동작도 하지 않습니다.

클라이언트 측에서 JS가 실행되어 하이드레이션 수행:

```js
ReactDOM.hydrate(<App />, document.getElementById("root"));
```

이후 버튼 클릭 시 이벤트가 작동하고, 상태 변화가 반영되는 완전한 React 앱으로 동작하게 됩니다.

---

### 언제 SSR을 사용하는가?

- 검색 엔진 최적화가 중요한 페이지 (ex. 쇼핑몰, 블로그)
- 페이지 로딩 속도가 사용자 경험에 큰 영향을 주는 서비스
- SNS 공유 등에서 메타태그가 필요한 경우에 사용하면 유용할 수 있다.

---

## SSR을 지원하기 위한 리액트 API

SSR을 할 때 React에서 HTML을 문자열로 미리 렌더링해서 서버에서 응답할 수 있도록 도와주는 API들이 있다. 이는 React 18 전후로 크게 바뀌었다.

#### React 18 이전 SSR 지원 API

### 1. `renderToString`

- 인수로 전달된 React 컴포넌트를 HTML 문자열로 렌더링합니다.
- **서버사이드 렌더링의 핵심 함수**로, 초기 페이지를 HTML로 반환하는 역할을 합니다.
- `useEffect`, `onClick` 등의 클라이언트 이벤트는 이 결과물에 포함되지 않습니다.
- React 전용 속성(data-reactroot 등)을 포함하여, 클라이언트에서 Hydration이 가능하도록 도와줍니다.

### 📌 deep dive: React-specific 속성

React-specific 속성이란?
React가 클라이언트에서 Hydration(하이드레이션) 을 정확히 수행하기 위해
HTML 태그에 추가로 삽입하는 속성들을 말합니다.

이 속성들은 브라우저에 직접 보여지는 콘텐츠에는 영향을 주지 않지만,
React 내부에서 DOM과 가상 DOM을 연결하고 이벤트를 붙이기 위한 중요한 힌트 역할을 합니다.

- 서버에서 생성한 HTML을 클라이언트에서 재사용하기 위해, React는 DOM 구조를 정확히 이해해야 합니다.
- 이때 `data-reactroot` 등의 속성을 통해 **가상 DOM과 실제 DOM을 연결**합니다.

#### 2) 주요 속성 설명

| 속성             | 설명                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------ |
| `data-reactroot` | 컴포넌트의 루트 엘리먼트를 식별하며, Hydration 시 기준이 되는 DOM입니다.             |
| `data-reactid`   | (React 15 이하) DOM 요소 식별용 내부 ID입니다. React 16 이후 제거됨.                 |
| `__reactProps$`  | (React 18) 이벤트 핸들러 등 Hydration 정보가 임시로 저장됨. Hydration 후 제거됩니다. |

---

### 2. `renderToStaticMarkup`

- `renderToString`과 매우 유사하지만, **React-specific 속성 없이** HTML을 반환합니다.
- Hydration을 고려하지 않기 때문에, **JS 이벤트 핸들러 연결이 필요 없는 페이지**(예: 약관, 정적 콘텐츠)에 적합합니다.
- 결과 HTML의 크기를 줄이는 데도 유리합니다.

---

### 3. `renderToNodeStream` (React 17 이하)

- Node.js 환경에서만 동작하며, React 컴포넌트를 **UTF-8로 인코딩된 바이트 스트림**으로 변환합니다.
- **큰 HTML을 청크 단위로 스트리밍**하여 서버 메모리 사용을 줄일 수 있습니다.
- 대표적인 SSR 프레임워크들이 채택했던 방식입니다.
- 브라우저에서 직접 사용은 불가능합니다.

---

### 4. `renderToStaticNodeStream`

- `renderToNodeStream`과 동일한 스트리밍 기반이지만,
- `renderToStaticMarkup`처럼 **React-specific 속성이 제거된 HTML 스트림**을 반환합니다.
- Hydration을 하지 않는, 순수 HTML이 필요한 정적 페이지에 적합합니다.

---

### 5. `hydrate` (브라우저에서 실행)

- SSR 또는 SSG로 렌더링된 HTML을 브라우저에서 활성화하여, **클릭, 입력 등 인터랙션이 가능하도록** 만드는 함수입니다.
- `renderToString` 등의 결과물 위에 이벤트 핸들러와 상태를 연결합니다.

```js
import { hydrate } from "react-dom";

hydrate(<App />, document.getElementById("root"));
```

브라우저에서만 사용되며, 기존 HTML과 React 상태를 연결해 완전한 SPA로 동작하게 만듭니다.

### 6. React 18 이후: Streaming SSR API

#### renderToPipeableStream (Node.js 환경 전용)

```js
import { renderToPipeableStream } from "react-dom/server";

const { pipe } = renderToPipeableStream(<App />, {
  onShellReady() {
    pipe(response); // 스트리밍 전송 시작
  },
  onError(err) {
    console.error("SSR Error:", err);
  },
});
```

- Node.js 환경에서만 사용 가능
- HTML을 청크 단위로 스트리밍 전송
- Suspense와 함께 작동하며, 초기 콘텐츠를 빠르게 렌더링 가능
- 성능 최적화, 대규모 페이지, 점진적 데이터 주입에 적합

```ts
type RenderToPipeableStreamOptions = {
  onShellReady?: () => void;
  onAllReady?: () => void;
  onError?: (error: unknown) => void;
  bootstrapScripts?: string[];
};
```

`onShellReady?: () => void`

- HTML의 기본 틀(Shell)이 준비됐을 때 호출되는 콜백 함수
- Hydration이 가능한 HTML을 클라이언트에 바로 스트리밍 전송할 수 있음
- 데이터가 다 안 와도 빠른 TTV(Time to Visually Ready)를 구현할 수 있게 도와줌

`onAllReady?: () => void`

- 모든 React 컴포넌트와 Suspense 내부까지 다 렌더링이 완료됐을 때 호출
- 이 콜백이 호출된 시점에서는 전체 페이지가 완성되어 Hydration 없이 바로 사용 가능
- 보통 클라이언트에서 자바스크립트를 실행해야 하는 시점과 연결됨

`onError?: (error: unknown) => void`

- SSR 렌더링 도중 에러가 발생했을 때 호출되는 함수
- 에러 로깅이나 fallback 응답 처리를 할 수 있음

`bootstrapScripts?: string[]`

- 브라우저에 먼저 삽입할 JS 파일 경로들을 미리 정의
- `<script src="...">` 형식으로 클라이언트에 삽입됨
- Hydration이나 React 앱 로딩에 필요한 클라이언트 번들을 여기에 지정함

```ts
bootstrapScripts: ["/static/client.bundle.js"]
// 이는 서버에서 HTML을 보낼 때 아래처럼 자동으로 삽입됨:
<script src="/static/client.bundle.js" async></script>
```

`RenderToPipeableStreamOptions`는 React 18의 Streaming SSR에서
스트리밍 시작 시점, 전체 완료 시점, 에러 처리, 초기 스크립트 삽입을 제어할 수 있도록 도와주는 옵션 객체

#### renderToReadableStream (Web Streams API 지원 환경)

```js
import { renderToReadableStream } from "react-dom/server";

async function render() {
  const stream = await renderToReadableStream(<div>Hello</div>);
  const reader = stream.getReader();
  const result = await reader.read();
  console.log(new TextDecoder().decode(result.value));
}
render();
```

- Edge 환경이나 Web Streams API 지원 브라우저/서버에서 사용 가능
- renderToPipeableStream과 동일하게 HTML을 스트리밍하되, Web 표준 기반
- 클라우드 환경 (예: Cloudflare Workers)에서 SSR을 수행할 때 유용

```ts
type RenderToReadableStreamOptions = {
  bootstrapScripts?: string[];
  bootstrapModules?: string[];
  onError?: (error: unknown) => void;
  signal?: AbortSignal;
  nonce?: string;
};
```

- `bootstrapScripts` : 클라이언트에서 사용할 JS 파일 경로 (기본 - `<script src="...">` 형태 삽입됨)
- `bootstrapModules` : ESModules 형태로 삽입할 JS (type="module")
- `onError` : 렌더링 중 오류 발생 시 실행될 콜백
- `signal` : **AbortController**와 연결하여 렌더링도중 중단(abort)할 수 있음

```ts
const controller = new AbortController();
const stream = await renderToReadableStream(<App />, {
  signal: controller.signal,
});
// 일정 조건에서 취소하고 싶을 때
controller.abort(); // SSR 중단!
```

- `nonce` : CSP(Content Security Policy) 대응을 위한 `nonce` 속성 추가용

````plain
CSP (Content Security Policy) 라는 웹 보안 기능과 관련된 옵션이야.
서버에서 <script> 태그를 만들 때 nonce="abc123"처럼 값을 붙이면,
브라우저는 CSP 정책에 따라 nonce가 일치하는 스크립트만 실행해.=
React SSR이 <script> 태그를 자동 삽입할 때 이 값을 추가해서 보안 정책을 통과할 수 있게 도와주는 거야.
nonce는 특정 <script> 태그가 신뢰된 코드인지 확인하는 값으로 사용돼.
	```

renderToReadableStream()에서는
스트리밍 시작 시점을 직접 제어하지 않기 때문에, onShellReady, onAllReady 같은 콜백은 존재하지 않습니다.

이 API는 Promise 기반이고, .then() 또는 await으로 스트림을 받을 수 있습니다.

---
#### 표 요약
| API                        | 특징                                 | 사용 환경         |
| -------------------------- | ---------------------------------- | ------------- |
| `renderToString`           | React-specific 속성이 포함된 HTML 문자열 반환 | 모든 SSR        |
| `renderToStaticMarkup`     | React 속성이 없는 순수 HTML 반환            | 정적 콘텐츠        |
| `renderToNodeStream`       | HTML을 스트리밍 형태로 반환 (Node.js 전용)     | React 17 이하   |
| `renderToStaticNodeStream` | React 속성 없는 스트리밍 HTML 반환           | 정적 스트리밍       |
| `hydrate`                  | 클라이언트에서 HTML에 React 기능을 연결         | 브라우저          |
| `renderToPipeableStream`   | React 18의 SSR 스트리밍 API             | Node.js       |
| `renderToReadableStream`   | Web Streams API 기반 SSR             | Edge / Web 환경 |


### 요약
React에서 SSR을 구현할 때는 `renderToString`, `renderToPipeableStream` 등의 API로 HTML을 만들고,
클라이언트에서 hydrate를 통해 이를 인터랙티브한 React 앱으로 연결합니다.
React 18 이후에는 스트리밍 기반 SSR이 핵심 트렌드로 자리잡고 있습니다.
````
