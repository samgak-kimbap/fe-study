# strict mode 란?
JavaScript의 **strict mode**는 동적 타입 언어의 단점을 일부 보완하려는 시도로 볼 수 있다.
 👉  **자바스크립트의 느슨한 문법과 관용적인(때론 실수하기 쉬운) 동작들을 더 엄격하게 검사해서 오류를 줄이기 위한 기능**이다.

---
## **strict mode란?**
- **ECMAScript 5 (ES5)** 부터 도입된 기능
- JavaScript 코드 실행 시 **더 엄격한 문법 검사와 오류 처리를 강제**
- 코드 상단에 'use strict';를 선언하여 사용
- 전역 혹은 함수 단위로 적용 가능

```javascript
'use strict';
x = 3.14;  // ReferenceError: x is not defined
```

이 예시처럼, 변수 선언 없이 값을 할당하면 **에러를 발생**시킨다.
(엄격하지 않은 모드에선 그냥 전역 변수로 암묵적 선언됨)

---
## **왜 사용하는가?**

1. **암묵적 전역 변수 생성 방지**
    - 실수로 let, const, var 없이 변수를 선언해도 전역 변수가 되지 않음
    - → 스코프 오염 방지, 디버깅 쉬워짐
2. **예약어 사용 방지**
    - 미래의 JavaScript 키워드(interface, package 등)를 변수명으로 사용하는 걸 막음
3. **this의 암묵적 바인딩 방지**
    - 함수 내에서 this가 undefined가 되도록 → 실수로 전역 객체(window) 참조하는 문제 방지

```javascript
'use strict';
function f() { return this; }
console.log(f());  // undefined
```

4. **객체 프로퍼티 중복 정의 금지**
```javascript
'use strict';
const obj = { p: 1, p: 2 };  // SyntaxError
```

5. **묵시적 에러를 명시적 에러로 바꿈**
    - 기존엔 조용히 무시되던 동작들이 **에러를 던지도록 강제**
---
## **요약**
➡️ **strict mode는 JavaScript의 유연함(느슨함)으로 인한 버그 가능성을 줄이고, 더 안전하고 예측 가능한 코드를 작성하도록 도와주는 장치**이다.

동적 타입 언어의 **런타임 오류 발생 가능성을 줄이는 작은 정적 검사 역할**을 한다고 볼 수도 있다.

---

👉 특히 **초보자나 대규모 프로젝트**에선 실수를 줄이고 버그 추적을 쉽게 해주기 때문에 사용하는 게 권장된다.
요즘은 **모듈 시스템(ES Modules)**에선 기본적으로 strict mode가 적용되기 때문에 따로 선언하지 않아도 적용된다!
