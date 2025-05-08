# 클래스가 생기기 이전에 자바스크립트의 OOP 
자바스크립트는 멀티-패러다임 언어로 명령형(imperative), 함수형(functional), 프로토타입 기반(prototype-based) 객체지향 언어다. 
자바스크립트는 본래 프로토타입 기반의 객체지향 언어이기 때문에 클래스가 없어도 객체 생성이 가능하다.

## 1. 객체 리터럴
```javascript
// 객체 리터럴
var obj1 = {};
obj1.name = 'Lee';
```

## 2. Object() 생성자 함수
```javascript
// Object() 생성자 함수
var obj2 = new Object();
obj2.name = 'Lee';
```

## 3. 생성자 함수
```javascript
// 생성자 함수
function F() {}
var obj3 = new F();
obj3.name = 'Lee';
```

클래스를 도입하기 이전에는 이러한 방법으로 객체지향 패턴을 구현하였다.