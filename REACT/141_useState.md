# react-hook(1) - useState
hook은 함수형 컴포넌트에서도 React의 상태 및 생명주기 기능을 연동할 수 있게 하는 기능이다. React에서 제공하는 Hook이 있고, 직접 커스텀 Hook을 정의할 수도 있어요.

##   useState이란?

React에서 가장 자주 사용하는 Hook 중 하나는 바로 `useState`입니다. 함수형 컴포넌트에서 상태(State) 관리를 가능하게 합니다. `useState`는 클래스 컴포넌트, 반복문, 조건문, 중첩된 함수에서 선언할 수 없어요. `useState`는 `null`과 같은 초기 상태 값이 필요해

```javascript
const [state, setState] = useState(null)
```
**구성요소**

| **요소**       | **설명**                            |
| ------------ | --------------------------------- |
| initialValue | 상태의 초기값 (최초 렌더링 시 한 번만 적용)        |
| state        | 현재 상태 값                           |
| setState     | 상태를 업데이트하는 함수. 호출 시 컴포넌트가 다시 렌더링됨 |
### **상태 저장 위치**
- React는 각 함수형 컴포넌트를 렌더링할 때 **컴포넌트 별로 상태를 저장할 공간**을 별도로 갖고 있습니다.
- 이 공간은 **Fiber 구조의 컴포넌트 인스턴스** 내부에 존재합니다.
- useState가 호출되면, 이 저장공간에 상태값을 저장하고 setState 함수를 함께 반환합니다.
### **2. 훅 호출 순서 기반 작동**
- React는 컴포넌트를 렌더링할 때 **useState의 호출 순서대로** 상태를 저장합니다.
- 그래서 useState는 항상 **컴포넌트 최상단에서 고정된 순서로** 호출되어야 하며, **조건문이나 반복문 안에서 호출하면 안 됩니다**.
### **3. 상태 변경 → 재렌더링**
- setState를 호출하면, React는 해당 상태를 새 값으로 업데이트하고, 컴포넌트를 **다시 렌더링**합니다.
- 이때, 이전 상태들과 훅 호출 순서를 기억하고 있어 동일한 위치에 새로운 상태를 반영합니다.

## 내부 동작 원리
```javascript
function Example() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("React");
  ...
}
```
### **React 내부에서는 다음과 같이 처리됩니다:**
1. 컴포넌트가 처음 렌더링되면:
    - useState(0) → count = 0, setCount = 함수A
    - useState("React") → name = "React", setName = 함수B
2. 이후 setCount(1) 호출 시:
    - count가 1로 바뀜
    - React는 해당 컴포넌트를 다시 렌더링함
	- 렌더링 시 이전 순서를 기억하고 count = 1, name = "React" 상태를 유지

#### **useState는 상태를** 기억하는 장치다
- React 함수형 컴포넌트는 렌더링될 때마다 **전체 함수가 다시 실행**됩니다.
- 그럼에도 useState가 값을 “기억”하는 이유는 React가 **훅의 순서를 추적하고 상태 저장소를 유지**하기 때문입니다.

#### useState 동작 구조
```javascript
// 모든 상태값을 저장할 배열 (컴포넌트 단위로 유지된다고 가정)
let stateList = [];

// 현재 상태 위치를 추적하기 위한 인덱스
let index = 0;

// useState 함수 정의
function useState(initialValue) {
  // 현재 이 useState가 몇 번째로 호출되는지 기억
  const currentIndex = index;

  // 만약 아직 이 인덱스에 값이 없다면, 초기값을 설정
  stateList[currentIndex] = stateList[currentIndex] ?? initialValue;

  // 상태를 변경하는 함수 정의
  function setState(newValue) {
    // 새로운 값으로 상태를 업데이트
    stateList[currentIndex] = newValue;

    // 상태가 바뀌었으니 컴포넌트를 다시 렌더링하라고 호출
    render();
  }

  // 다음 useState가 호출될 때를 대비해 인덱스를 1 증가
  index++;

  // 현재 상태값과 변경 함수 반환
  return [stateList[currentIndex], setState];
  //[현재 상태 값, 상태 변경 함수]의 형태로 반환합니다.
  //실제 우리가 사용하는 useState 문과 동일하다.
}
```
