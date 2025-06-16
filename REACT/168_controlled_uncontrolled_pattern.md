## Controlled vs Uncontrolled Components

프론트엔드 개발에서 폼(Form) 처리는 피할 수 없는 주제입니다. React에서는 이를 두 가지 패턴으로 관리할 수 있는데, 바로 **Controlled Components** 와 **Uncontrolled Components** 입니다. 이름만 보면 딱딱하게 들릴 수 있지만, 실제 사례와 함께 보면 차이가 분명하고 실무에서 선택 기준도 명확해집니다.

## Controlled Components란?

폼 입력 요소의 값을 **React가 직접 state로 관리** 하는 방식입니다.
React가 값을 통제(control)하기 때문에 이 이름이 붙었습니다.

**특징:**

* 상태의 **진짜 출처(Source of Truth)** 가 React state다.
* 입력 값이 바뀌면 `setState`로 React state를 갱신하고, state가 input의 `value`로 연결된다.
* 유효성 검사, 조건부 렌더링, 다단계 폼 등 복잡한 로직에 적합하다.

**사례 예제:**

```jsx
function ControlledForm() {
  const [name, setName] = useState('');

  return (
    <form>
      <label>
        이름:
        <input 
          type="text" 
          value={name} 
          onChange={e => setName(e.target.value)}
        />
      </label>
      <p>입력된 이름: {name}</p>
    </form>
  );
}
```

**이 코드의 핵심:**

* `value`는 항상 `state`에서 가져온다.
* 사용자가 입력할 때마다 `onChange`로 React가 직접 state를 갱신한다.

## ✅ Uncontrolled Components란?

폼 입력 요소의 값을 **브라우저 DOM이 직접 관리** 하는 방식입니다.
React는 초기값만 주고 이후에는 관여하지 않습니다.

**특징:**

* 값은 DOM에 저장된다.
* `ref`를 사용해 필요할 때 값만 꺼내온다.
* 단순한 폼, 외부 라이브러리 연동, 파일 업로드 등에 적합하다.

**사례 예제:**

```jsx
function UncontrolledForm() {
  const inputRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`입력된 이름: ${inputRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        이름:
        <input type="text" defaultValue="홍길동" ref={inputRef} />
      </label>
      <button type="submit">제출</button>
    </form>
  );
}
```

**이 코드의 핵심:**

* `defaultValue`로 초기값만 지정.
* React는 이후 값을 관리하지 않는다.
* 제출할 때 `ref`로 DOM에서 직접 값을 가져온다.

## 언제 어떤 걸 써야 할까?

| 상황            | Controlled   | Uncontrolled |
| ------------- | ------------ | ------------ |
| 다단계 입력/유효성 검사 | ✅ 선호         | ❌ 부적합        |
| 실시간 렌더링 반영    | ✅ 좋음         | ❌ 직접 처리해야 함  |
| 단일 입력, 단순 폼   | ❌ 코드가 길어짐    | ✅ 더 간단       |
| 파일 업로드        | ❌ 불가 (보안 이유) | ✅ 필수         |

> **Tip:**
> 대부분의 일반 폼은 Controlled로 작성하는 것이 React 철학에 맞고 유지보수가 쉽습니다.
> 다만 파일 업로드(`<input type="file">`)는 반드시 Uncontrolled로만 가능하므로 혼용이 자연스럽습니다.

## 핵심 요약

|        | Controlled    | Uncontrolled   |
| ------ | ------------- | -------------- |
| 상태 저장소 | React state   | DOM            |
| 값 읽기   | state         | ref            |
| 초기값    | `value`       | `defaultValue` |
| 적합한 경우 | 복잡한 폼, 유효성 검사 | 간단한 폼, 파일 업로드  |

## 마무리

Controlled와 Uncontrolled는 둘 다 필요에 따라 유연하게 선택할 수 있는 도구입니다.
React 철학에 맞춘 상태 제어가 필요하면 Controlled, 간단하고 DOM의 기본 동작을 그대로 쓰면 충분하다면 Uncontrolled를 사용하세요.

## 연관 질문으로 더 깊게 이해하기

* 왜 굳이 통제해야 할까요?  
  React의 철학은 데이터 흐름을 단방향으로 유지하는 것입니다. 값이 언제, 왜, 어떻게 바뀌는지 React가 전부 알고 있어야 버그를 줄이고 상태 동기화를 쉽게 할 수 있습니다.

* Uncontrolled가 더 자연스럽지 않나요?  
  맞아요. HTML 기본 동작만 보면 Uncontrolled가 자연스럽습니다. 하지만 React는 UI를 데이터 상태에 맞춰 "계산"하는 방식을 쓰기 때문에 Controlled가 일관성과 유지보수성에서 훨씬 강력합니다.

* Controlled와 Uncontrolled를 혼합할 수 있나요?  
  네! 예를 들어 이름, 이메일은 Controlled로 관리하고, 프로필 이미지 같은 파일 업로드만 Uncontrolled로 관리하는 것이 일반적입니다.

* Controlled가 성능에 불리한가요?  
  대부분은 문제되지 않습니다. 단, 입력 필드가 매우 많으면 state 업데이트 비용이 생기므로 `debounce`나 `useCallback`으로 최적화할 수 있습니다.
