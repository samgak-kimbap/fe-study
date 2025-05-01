---
layout: post
title: Map,Set 그리고 Lookup Table
author: balancelife
date: 2025-05-01 16:43:00 +0900
categories: [JAVASCRIPT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [Map, Set, Lookup-Table]
---

오늘은 Map, Set 그리고 Lookup Table에 대해 공부해보겠다.

우선 Map 부터.

> # Map이 뭐지?

처음엔 객체를 순회하며 함수를 돌리는 .map() 함수인 줄 알았다.
물론 map() 함수도 있지만 set, Lookup Table 과는 어울리지 않는 map이었다.

그럼 map이 무엇을 의미하나??

## 1. “Map이 뭐야? 자료구조야? 함수야?”

여기서 의미하는 Map 은 **자료구조**이다.

자바스크립트에서는 이걸 Map 클래스로 구현했다.

- key → value 형태로 데이터를 저장하고, 키를 기준으로 빠르게 값을 찾는 구조

```js
// 자료구조로서 Map
const m = new Map();
m.set("a", 1); // key , value 형태로 데이터를 저장
m.get("a"); // 1 , key로 value를 찾음.
```

## 2. “뭐야, 딕셔너리랑 뭐가 다른 거야?

**Map**과 **딕셔너리(dictionary)** 는 거의 같은 역할을 한다.
하지만 언어나 구현 방식에 따라 다르게 부르기도 하고, 실제로는 세부적인 차이도 있다.

**딕셔너리란?**
**파이썬 같은 언어에서 사용하는 용어**

{"apple": 3, "banana": 5} 처럼 key-value 쌍 저장

자바스크립트에선 전통적으로 **Object**가 이 역할을 했다.

그런데 왜 Map이 따로 나왔냐?
→ 자바스크립트의 Object는 한계가 많았다.

## 3. 맞네 그냥 늘 쓰던 객체가 키-밸류 형태였지. 그럼 Map은 왜 있는거야

### Object vs Map

| 항목                     | `Object`                                           | `Map`                                                |
| ------------------------ | -------------------------------------------------- | ---------------------------------------------------- |
| 🔑 **Key(키) 타입**      | 문자열(`"foo"`) 또는 심볼만 가능                   | ✅ **모든 타입 가능** (문자, 숫자, 객체, 함수 등)    |
| 🧾 **삽입 순서 유지**    | ❌ 보장 안 됨 (ES6부터 일부 보장)                  | ✅ **삽입 순서 유지**                                |
| 🔄 **반복 및 순회**      | `for...in`, `Object.keys()` 등 필요                | ✅ `for...of` + `.entries()` 등 바로 사용 가능       |
| 🧠 **내장 메서드**       | 별로 없음 (직접 구현해야 함)                       | ✅ `.set()`, `.get()`, `.has()`, `.delete()` 등 풍부 |
| 🧪 **성능**              | 적은 데이터엔 빠르지만 대량/빈번 시 느려질 수 있음 | ✅ **많은 데이터 처리에 더 안정적**                  |
| 📦 **직렬화(JSON 변환)** | ✅ JSON.stringify 가능                             | ❌ `Map`은 직접 변환 필요                            |
| 🤯 **설계 목적**         | 일반적인 객체 표현 (데이터 구조보단 구조적 의미)   | ✅ **데이터 저장과 조작**을 위한 구조                |

### 키 타이 차입

Object 에서는 키로 "문자열" 타입 또는 "심볼" 타입만 가능하다.
반면
Map 자료구조는 모든 타입을 키로 사용할 수 있다.

#### 1. 문자열

````js
const obj = {};
const map = new Map();

obj["name"] = "민준";
map.set("name", "민준");

console.log(obj["name"]); // "민준"
console.log(map.get("name")); // "민준"```
````

둘 다 문자열 키를 잘 받는다.

#### 2. 숫자 키

```js
const obj = {};
const map = new Map();

obj[123] = "숫자키";
map.set(123, "숫자키");

console.log(obj[123]); // 1
console.log(obj["123"]); // 2

console.log(map.get(123)); // 3
console.log(map.get("123")); // 4
```

1,2,3,4의 결과가 어떻게 될까?

1의 경우, obj[123] 을 키로 넣었으니 "숫자키" 가 잘 출력됨이 당연하다.
2의 경우, "123" 로 123을 문자열로 바꾸어 키로 집어넣어도 "숫자키" 라는 키가 출력된다.
왜일까?

> ❗ Object는 숫자 키(123)도 문자열("123")로 변환해서 저장하기 때문이다.
> → 123 이든 "123" 이든 같은 key로 취급함

반면 map의 경우 123과 "123" 을 구분한다. 그래서 3은 잘 나오지만
문자열 "123"을 키로 넣으면 undefined가 출력된다.

#### 3. 객체 키

```js
const obj = {};
const map = new Map();

const key1 = { id: 1 };

obj[key1] = "객체키"; // key1이 문자열로 변환됨 → "[object Object]"
map.set(key1, "객체키"); // key1 그대로 객체로 저장됨

console.log(obj);
// 🔸 출력: { "[object Object]": "객체키" }

console.log(map);
// 🔸 출력: Map(1) { { id: 1 } => "객체키" }
```

이번엔 key1 이라는 객체를 선언하고 이를 키로 전달했다.
그리고 obj와 map을 각각 찍어보면
map의 경우 key1 이 잘 객체로 저장되지만,
obj의 경우 key1이 "[object Object]" 이라는 문자열로 변환 되어 저장된다.
즉 key1 말고 key2 = { id: 2 } 여도 ["object Object]" 로 저장되어 둘의 키가 일치하게 된다.

#### 4. 함수 키

```js
const obj = {};
const map = new Map();

const keyFn = () => {};

obj[keyFn] = "함수키";
map.set(keyFn, "함수키");

console.log(obj); // { "[object Function]": "함수키" }
console.log(map.get(keyFn)); // "함수키"
```

함수도 동일하다. Object의 경우 어떤 함수이던 "[object Function]" 이라는 문자열로 변환되어 저장된다.

#### 5. 배열 키

```js
const obj = {};
const map = new Map();

const keyArr = [1, 2];

obj[keyArr] = "배열키";
map.set(keyArr, "배열키");

console.log(obj); // { "1,2": "배열키" } ← 배열이 문자열 "1,2"로 바뀜
console.log(map); // {Array(2) => '배열키'}
```

배열도 동일하다. Object의 경우 '[1, 2]' 라는 배열이 "1,2" 라는 문자열로 변환되어 저장되는 반면 map의 경우 배열의 형태를 잘 유지한다.

### 삽입 순서

#### Object

```js
const obj = {};
obj[3] = "셋";
obj[1] = "하나";
obj[2] = "둘";

console.log(Object.keys(obj));
// 👉 ["1", "2", "3"] ← 숫자 키는 강제로 **오름차순 정렬**
```

3, 셋 | 1, 하나 | 2, 둘 | 순으로 저장했지만 저장된 순서는 1,2,3 이 된다.

단, 문자열 키는 순서 보장이 된다.

#### Map

```js
const map = new Map();
map.set(3, "셋");
map.set(1, "하나");
map.set(2, "둘");

console.log([...map.keys()]);
// 👉 [3, 1, 2] ← **삽입 순서 그대로!**
```

삽입 순서가 보장된다.

---

### Object vs Map 내장 메서드 비교

| 기능         | Object 방식                                 | Map 방식              |
| ------------ | ------------------------------------------- | --------------------- |
| 값 저장      | `obj[key] = value`                          | `map.set(key, value)` |
| 값 조회      | `obj[key]`                                  | `map.get(key)`        |
| 키 존재 확인 | `'key' in obj` or `obj.hasOwnProperty(key)` | `map.has(key)`        |
| 키 삭제      | `delete obj[key]`                           | `map.delete(key)`     |
| 전체 삭제    | ❌ (직접 반복하면서 해야 함)                | `map.clear()`         |
| 전체 개수    | `Object.keys(obj).length`                   | `map.size` (속성)     |

---

### 성능 차이 – 대량 데이터 처리

```js
// Object
const obj = {};
for (let i = 0; i < 1_000_000; i++) {
  obj[i] = i;
}
console.log(obj[999999]); // 999999

// Map
const map = new Map();
for (let i = 0; i < 1_000_000; i++) {
  map.set(i, i);
}
console.log(map.get(999999)); // 999999
```

실제로 브라우저 콘솔에서 돌려보면, Map이 해시 테이블 기반 검색으로 조회 성능이 더 일정하고 빠름
Object는 키가 많아질수록 조회가 상대적으로 느려질 수 있음

근데 사실 Object 도 해시테이블 기반이다.

> ### 해시테이블이란 ?
>
> **해시 테이블은 키를 숫자로 변환해 빠르게 저장하고 꺼내는 자료구조**
> Map, Object, Set 등도 내부적으로 이걸 기반으로 동작한다.

**""Object도 해시 테이블 기반인데, 왜 Map이 더 성능이 좋다고 하냐?""**
-> Object도 해시 테이블 기반이 맞지만, Map은 "순수한 해시 테이블"로 설계되었고, Object는 그렇지 않기 때문이라고 한다.

> ### 비유: Object vs Map

#### Object는 "다기능 만능 가방"

문서, 필통, 책, 물통 다 넣을 수 있어. 하지만 너무 무겁고 복잡해서 "진짜 빠르게 꺼내 쓰기엔 불편해"

#### Map은 "정리된 서랍장"

키 하나당 정확한 칸이 있고, 꺼내고 넣기 빠름. 다른 기능은 없지만 데이터 저장과 꺼내기는 최적화돼 있음

라고 한다. 필자는 일단 여기까지만 이해하고 넘어가겠다.

---

### 직렬화 (JSON 변환)

▶ Object는 바로 직렬화 가능

```js
// Object
const obj = { name: "민준", age: 25 };
console.log(JSON.stringify(obj));
// {"name":"민준","age":25}
```

▶ Map은 JSON으로 직렬화하면 null 또는 {} 나옴

```js
// Map
const map = new Map([
  ["name", "민준"],
  ["age", 25],
]);
console.log(JSON.stringify(map));
// "{}"

// Map을 JSON으로 변환하려면 수동 처리
const jsonObj = Object.fromEntries(map);
console.log(JSON.stringify(jsonObj));
// {"name":"민준","age":25}
```

Object.fromEntries의 뜻
**"엔트리(Entry) 배열을 일반 객체로 변환해준다"**는 뜻.
엔트리 = 키 -> 값의 구조

즉 map 을 일반 객체로 변환해줘야 JSON.stringify 가능

---

### 설계 목적 차이

#### Object : 객체를 표현하기 위한 기본 단위 (속성/설정 값 저장)

```js
// Object는 "구조" 표현에 적합
const user = {
  name: "민준",
  age: 25,
  sayHi() {
    console.log("안녕!");
  },
};
```

#### Map : 데이터를 저장하고 조작하기 위한 전용 자료구조

```js
// Map은 "데이터 컨테이너"로 적합
const settings = new Map();
settings.set("darkMode", true);
settings.set("fontSize", 16);
```

그래서:
"사용자 정보", "게시물 내용" → Object
"설정값", "데이터 캐시", "단어 빈도수 계산" → Map

이런식으로 사용한다고 한다. Map 자료구조 끝

---

# Set 자료구조

## Set – 중복 없는 값의 집합

### 설계 목적

> **Set은 "중복 없는 값들"을 저장하기 위한 전용 자료구조다.**

- 수학의 "집합" 개념과 유사함
- 값만 저장하고, 키는 없음 (즉, Key-Value 구조 ❌)
- 순서 보장, 반복 가능
- 중복 제거, 존재 여부 확인에 특화됨

---

### 기본 사용법

```js
const mySet = new Set();

mySet.add("apple");
mySet.add("banana");
mySet.add("apple"); // 중복은 무시됨

console.log(mySet); // Set(2) { 'apple', 'banana' }
console.log(mySet.has("apple")); // true
```

- Set() 로 선언한다.
- add(요소) 로 자료형에 넣는다.
- Set.has() 로 요소의 존재여부를 불리안값으로 확인할 수 있다.

---

### 중복 제거 예시 (Array → Set)

```js
const arr = ["a", "b", "a", "c"];
const unique = [...new Set(arr)];

console.log(unique); // ['a', 'b', 'c']
```

---

### Set 사용 시점

| 언제 쓰면 좋을까?                            | 예시                                       |
| -------------------------------------------- | ------------------------------------------ |
| 배열에서 중복 제거                           | `[...new Set(arr)]`                        |
| 값이 있는지 빠르게 확인                      | `set.has(value)`                           |
| 교집합/합집합/차집합 연산 (수학적 집합 처리) | `new Set([...a].filter(x => b.has(x)))` 등 |

---

### Set의 수학적 연산 예시 (교집합, 합집합, 차집합)

#### 기본 개념

| 연산   | 설명                             | 기호  |
| ------ | -------------------------------- | ----- |
| 교집합 | 두 집합에 **모두 있는 값**만     | A ∩ B |
| 합집합 | 두 집합의 **모든 값(중복 제거)** | A ∪ B |
| 차집합 | A에는 있지만 B에는 **없는 값**   | A - B |

---

### 데이터 예시

```js
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);
```

---

### 교집합 (A ∩ B)

```js
const intersection = new Set([...setA].filter((x) => setB.has(x)));
console.log(intersection); // Set(2) { 3, 4 }
```

- `filter()`로 A의 요소 중 **B에도 있는 것만** 남긴다

---

### ➕ 합집합 (A ∪ B)

```js
const union = new Set([...setA, ...setB]);
console.log(union); // Set(6) { 1, 2, 3, 4, 5, 6 }
```

- 두 집합을 펼쳐서(...spread) Set으로 감싸면 **중복 자동 제거**

---

### ➖ 차집합 (A - B)

```js
const difference = new Set([...setA].filter((x) => !setB.has(x)));
console.log(difference); // Set(2) { 1, 2 }
```

- `setA`의 값 중에서 **`setB`에 없는 것만** 남긴다

---

- python에는 setA & setB / setA | setB / setA - setB ← 이런 연산자 지원하는데 JavaScript에는 그런 게 없고 직접 구현해야 한다는 것을 공부하며 알았다.

### 정리 한 줄

> `Set`은 "중복을 없애고, 빠르게 포함 여부를 확인하고 싶은 값들"을 저장할 때 쓰는 자료형이다.

---

# Lookup Table

### 정의

> **Lookup Table은 "빠른 참조를 위해 미리 값을 매핑해 둔 테이블"**  
> 자바스크립트에선 주로 **Object**나 **Map**을 이용해 구현한다.

룩업 테입르 = 어떤 값을 키로 "찾아보기 위해 미리 정리해 둔 표"

---

### 왜 쓰는가?

- `if-else`나 `switch` 없이 값을 빠르게 찾고 싶을 때
- 반복적인 계산을 미리 저장해두고 재사용하고 싶을 때
- 어떤 값을 기준으로 → 다른 값을 빠르게 매칭시킬 때

---

### Object 기반 Lookup Table

```js
const asciiTable = {
  A: 65,
  B: 66,
  C: 67,
};

console.log(asciiTable["B"]); // 66
```

- 문자 → 숫자 매핑
- 키는 문자열이어야 함

---

### Map 기반 Lookup Table

```js
const roleTable = new Map([
  ["admin", "관리자"],
  ["user", "일반 사용자"],
  ["guest", "게스트"],
]);

console.log(roleTable.get("admin")); // "관리자"
```

- `Map`을 쓰면 **객체, 숫자, 함수 등도 키로 가능**

---

### 📌 언제 유용할까?

| 상황           | 예시                                           |
| -------------- | ---------------------------------------------- |
| 상태 코드 해석 | `{ 200: "OK", 404: "Not Found" }`              |
| 등급 매핑      | `{ A: "우수", B: "보통", C: "미흡" }`          |
| 단축 키 처리   | `{ ArrowUp: "moveUp", ArrowDown: "moveDown" }` |
| 계산 캐싱      | `{ 5: 120 } // 5! = 120 처럼`                  |

---

### 🔄 vs if-else

```js
// 비효율적인 방식
function getRoleText(role) {
  if (role === "admin") return "관리자";
  if (role === "user") return "일반 사용자";
  if (role === "guest") return "게스트";
  return "알 수 없음";
}

// Lookup Table로 개선
const roleText = {
  admin: "관리자",
  user: "일반 사용자",
  guest: "게스트",
};

console.log(roleText["user"]); // "일반 사용자"
```

- 훨씬 **깔끔하고 확장 가능**

---

## Map & Set & Lookup Table

| 항목             | 목적                          | 내부 구조            | 시간 복잡도    |
| ---------------- | ----------------------------- | -------------------- | -------------- |
| **Map**          | key → value 저장 및 조회      | 해시 테이블          | 조회/삽입 O(1) |
| **Set**          | 중복 없는 값 저장             | 해시 테이블          | 존재 확인 O(1) |
| **Lookup Table** | 키 → 값 매핑을 통한 빠른 참조 | Map 또는 Object 기반 | 조회 O(1)      |

- **전부 “찾기, 구분, 저장”이라는 공통된 패턴**을 처리함
- `Map`과 `Set`은 자료구조의 형태고,
- `Lookup Table`은 **그 자료구조를 활용하는 방식(패턴)**이다.
