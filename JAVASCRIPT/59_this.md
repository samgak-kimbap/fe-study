---
layout: post
title: This
author: haeran
date: 2025-05-08 21:07:00 +0900 
categories: [JAVASCRIPT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [this]
---

## this?

### `this`의 어원: 객체지향 언어 전통

자바스크립트는 객체지향 프로그래밍(OOP) 언어인 **Java**, **C++**, **Smalltalk** 등의 영향을 받았는데, 이들 언어에서는 **현재 객체 자신을 가리키는 키워드로 `this`를 사용**합니다. (아무튼 또 따라했다~~~)

```java
// 자바에서의 this
public class Car {
  private String model;

  public Car(String model) {
    this.model = model; // this: 현재 생성 중인 Car 인스턴스
  }
}
```

자바스크립트에서 함수는 보통 어떤 **객체의 메서드로 동작**하게 되며, 그 함수 안에서 `this`는 **그 메서드를 호출한 객체**를 가리킵니다.

즉, `this`는 **“지금 이 순간, 나 자신”**, 다시 말해 \*\*"함수를 호출한 주체"\*\*를 가리키는 역할을 합니다. (this가 가지는 의미인 '이것'도 잘 통했다고 생각합니다. 이 표현은 **자연스럽고 직관적이며**, **다른 언어와도 호환되는 설계**였기 때문에 선택된 이름입니다)

### 다른 언어들과의 비교

| 언어         | 현재 객체를 가리키는 키워드 |
| ---------- | --------------- |
| JavaScript | `this`          |
| Java / C++ | `this`          |
| Python     | `self`          |
| Ruby       | `self`          |
| Swift      | `self`          |

Python은 `self`를 명시적으로 매개변수로 넘기지만, JS는 `this`를 **자동으로 바인딩**합니다 (물론 약간의 혼란이 생기기도...)

`this`는 **자바스크립트에서 실행 컨텍스트(실행되는 환경)에 따라 달라지는 특별한 키워드**입니다. 간단히 말하면 \*\*"누가 이 코드를 호출했는지에 따라 달라지는 객체"\*\*를 가리킵니다.

아래 두 함수는 왜 다르게 호출될까요?

```js
const user = {
  name: "Tom",
  sayHi() {
    console.log(this.name);
  }
};

user.sayHi(); // Tom

const sayHi = user.sayHi;

sayHi(); // undefined (브라우저에서는 window.name)
```

`this`는 **함수를 어떻게 호출했느냐**에 따라 값이 바뀝니다.  
이걸 `this 바인딩`이라고 합니다.

### `this 바인딩`이란?

\*\*바인딩(binding)\*\*이란 `this`가 가리키는 대상을 **"연결한다"**, \*\*"묶는다"\*\*는 뜻입니다.

그렇다면 왜 `this` 바인딩이 발생할까요?

가장 중요한 이유는 **"같은 함수를 여러 객체가 공유해서 쓸 수 있게 하려고"** 입니다.

자바스크립트는 **동적인 객체 지향 언어**이고, **함수는 일급 객체**이기 때문에 **같은 함수를 여러 객체가 재사용하거나, 동적으로 연결하는 일이 매우 흔합니다.**

이때 **함수가 호출될 때마다 “누가 날 부른 거지?”를 알아야** 그에 맞는 행동을 할 수 있어요. 그래서 자바스크립트는 함수 실행 시점에 **`this`라는 특별한 키워드를 바인딩**해서, **"이 함수를 호출한 주체(객체)"가 누군지를 알려주는 메커니즘**을 만든 거죠.

```js
function introduce() {
  console.log(`My name is ${this.name}`);
}

const alice = { name: '혜란', introduce };
const bob = { name: '만두', introduce };

alice.introduce(); // My name is 혜란
bob.introduce();   // My name is 만두
```

**같은 함수 `introduce`를 재사용**했지만, 호출된 **문맥에 따라 this가 달라지고**, 따라서 **출력 결과도 달라집니다**.

이게 가능하려면, `introduce()`라는 함수는 **자신이 속한 객체를 몰라도**, **"나를 호출한 객체가 누구인지" 실행 순간에 알아야** 합니다.

그래서 `this` 바인딩이 꼭 필요해지는 거예요.

| 왜 필요한가?         | 설명                                                    |
| --------------- | ----------------------------------------------------- |
| **재사용성**        | 함수 하나를 여러 객체에 공유해서 쓸 수 있게 하려면, 그때그때 호출한 주체(객체)를 알아야 함 |
| **동적 실행 문맥 반영** | 함수는 정의된 곳이 아니라 \*\*"어떻게 불렸는지"\*\*에 따라 동작이 달라져야 함      |
| **객체지향적 추상화**   | 객체 안에서 메서드를 만들고 `this`를 통해 그 객체의 데이터를 다룰 수 있어야 함      |
| **유연한 구조 설계**   | call, apply, bind 등으로 유연하게 `this`를 조작할 수 있도록 지원       |

자바스크립트에서는 크게 네 가지 방식으로 `this`가 바인딩됩니다.

| 바인딩 방식         | 설명                             |
| -------------- | ------------------------------ |
| **암시적 바인딩**    | `obj.method()` → `this`는 `obj` |
| **명시적 바인딩**    | `call`, `apply`, `bind`로 수동 설정 |
| **new 바인딩**    | 생성자 함수로 호출 시, 새 객체가 `this`     |
| **화살표 함수 바인딩** | 외부 스코프의 `this`를 캡처함 (렉시컬 바인딩)  |

### `this`는 동적으로 바인딩 된다?

일반 함수에서는 `this`가 **동적 바인딩**되며, 호출된 방식에 따라 달라집니다.

| 호출 방식               | `this`가 가리키는 객체                                             |
| ------------------- | ----------------------------------------------------------- |
| 전역 함수 호출            | 브라우저: `window`, Node: `global` (strict mode에서는 `undefined`) |
| 객체의 메서드 호출          | 그 객체 자체                                                     |
| `call` / `apply` 호출 | 넘겨준 첫 번째 인자 (`thisArg`)                                     |
| `new`로 호출           | 새로 생성된 인스턴스                                                 |
| 화살표 함수              | 외부 스코프의 `this` (정적 바인딩)                                     |

```js
function showThis() {
  console.log(this);
}

const obj = {
  show: showThis
};

obj.show(); // obj
showThis(); // window (strict 모드에선 undefined)
```

### 화살표 함수에서 `this`

화살표 함수는 **자기 자신만의 `this`를 가지지 않고**, \*\*함수가 정의될 당시의 외부 스코프(렉시컬 스코프)\*\*에서 `this` 값을 \*\*캡처(기억)\*\*합니다. 이걸 \*\*렉시컬 바인딩(Lexical Binding)\*\*이라고 해요.

```js
function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++; // 화살표 함수는 Person의 this를 참조
  }, 1000);
}

new Person();
```

`setInterval` 안의 함수가 일반 함수였다면 `this`는 `window`가 되었겠지만, 화살표 함수라서 `this`는 **Person 인스턴스**를 계속 가리킵니다.

이 특성 덕분에 `setTimeout`, `map`, `forEach` 같은 **콜백 함수 안에서 `this`가 꼬이지 않게** 사용할 수 있어요.

### 플러스 : `bind()` 메서드를 잘 사용하기

`bind()`는 함수의 `this`를 **강제로 고정**시켜서 **새로운 함수**를 반환합니다.

```js
const user = {
  name: 'Alice',
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

const greetFn = user.greet;
greetFn(); // undefined, this가 window

const boundGreet = user.greet.bind(user);
boundGreet(); // Hello, I'm Alice
```

**언제 유용할까?**

* 객체 메서드를 **이벤트 핸들러나 콜백으로 전달할 때 this가 바뀌지 않도록** 하고 싶을 때
* 클래스 메서드를 `setTimeout`, `Promise.then`, `map` 등으로 넘길 때 this를 고정하고 싶을 때
* React의 클래스 컴포넌트에서 메서드 바인딩할 때
