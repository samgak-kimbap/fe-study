---
layout: post
title: 웹팩, 모듈, 모듈 번들링
author: haeran
date: 2025-04-16 21:07:00 +0900 
categories: [JAVASCRIPT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [Truthy, falsy]
---

## Truthy와 Falsy

Truthy와 falsy는 자바스크립트에서 boolean을 기대하는 구문에서 각 값이 true와 false 중 어떤 값을 가지냐를 나타내는 값 입니다.

### Truthy

자바스크립트에서, truthy인 값(참 같은 값) Boolean(불리언) 문맥에서 true로 평가되는 값입니다. falsy값으로 정의된 값이 아니면 모두 truthy값으로 평가됩니다.

자바스크립트는 불리언 문맥에서 타입 변환(형 변환)을 사용합니다.

자바스크립트는 Boolean(불리언) 문맥에서 truthy값을 true로 변환하기 때문에 아래의 모든 if 블록을 실행하게 됩니다.

```js
if (true)
if ({})
if ([])
if (42)
if ("0")
if ("false")
if (new Date())
if (-42)
if (12n)
if (3.14)
if (-3.14)
if (Infinity)
if (-Infinity)
```

### Falsy

falsy인 값(거짓 같은 값)은 Boolean 문맥에서 false로 평가되는 값입니다.

| falsy값	| 설명 |
|--------|-----|
|false	|키워드 false|
|0	|Number zero.(0.0, 0x0 등등 또한 해당된다)|
|-0	|Number Negative zero.(-0.0, -0x0 등등 또한 해당된다)|
|0n	|BigInt zero. (0x0n 도 포함) BigInt negative zero는 없음에 유의하자(0n의 negative는 0n이다.)|
|"", '', ``	|빈 문자열 값|
|null	|어떠한 값도 없는 상태|
|undefined|	알수 없음|
|NaN|	Not a Number|
