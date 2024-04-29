컴포넌트는 다른 조건에서 다른 것들을 보여줄 필요가 있을 것입니다. 리액트에서 `if`, `&&` `? :` 연산자와 같은 자바스크립트 문법을 활용하면 조건부로 JSX를 렌더링 할 수 있습니다.

# **You will learn**

- 조건에 따라 다른 JSX를 반환하는 법
- JSX의 일부를 조건부로 포함하거나 제외하는 법
- 여러분이 리액트 코드에서 만날 수 있는 일반적인 조건부 문법 숏컷

# **Conditionally returning JSX**

여러 `Item`을 렌더링하는 `PackingList`를 가지고 있다고 가정해봅시다. `Item`은 패킹되었는지 안되었는지 마킹될 수 있습니다.

```jsx
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

일부 `Item` 컴포넌트는 `isPacked` prop이 `true`로 설정되어 있음에 주목하세요. 여러분은 `isPacked`가 `true`인 `Item`에 체크마크를 추가하길 원합니다.

여러분은 이를 `if else` 문을 활용하여 작성할 수 있습니다.

```jsx
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;
```

만약 `isPacked` prop이 `true`라면, 이 코드는 다른 JSX 트리를 반환합니다. 이 변화와 함께 `Item` 중 일부는 끝부분에 체크마크를 얻게 됩니다.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

여러 케이스에서 어떤 결과가 만들어지는 지 확인해보세요.

리액트에서 제어 흐름은 자바스크립트에 의해 다뤄지기 떄문에, 자바스크립트의 `if`, `return` 문에 대해 잘 알아야 합니다

# **Conditionally returning nothing with `null`**

몇몇 상황에서는 아무 것도 렌더링하지 않기를 원할 것입니다. 예를 들어 패킹된 아이템은 표시하지 않기를 원할 수 있습니다. 컴포넌트는 반드시 무언가를 반환해야하므로, 이 케이스에서는 `null`을 리턴할 수 있습니다.

```jsx
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

`isPacked`가 `true`라면 컴포넌트는 아무것도 반환하지 않을 것입니다.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

실제 상황에서 `null`을 반환하는 것은 일반적이지 않습니다. 그보다 부모 컴포넌트의 JSX에서 조건부로 해당 컴포넌트를 포함할지 아닐지를 선택할 것입니다.

# **Conditionally including JSX**

이전의 예시에서, 여러분은 컴포넌트에 의해 반환될 수 있는 JSX를 제어했습니다. 여러분은 아마 렌더링 결과에 중복된 부분이 있다는 것을 알아차렸을 것입니다.

`<li className="item">{name} ✔</li>`

는 아래와 매우 유사합니다.

`<li className="item">{name}</li>`

두 조건 모두 `<li className="item">...</li>` 를 반환합니다.

이러한 중복이 유해하지 않더라도, 이는 코드를 유지보수하기 힘들게 만들 수 있습니다. `className`을 바꾸기를 원한다면 코드의 두 부분을 바꿔야 합니다. 이러한 상황에서 여러분은 여러분의 코드를 DRY하게 만들기 위해 약간의 JSX를 조건부로 포함할 수 있습니다.

# **Conditional (ternary) operator (`? :`)**

JavaScript has a compact syntax for writing a conditional expression — the [conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) or “ternary operator”.

자바크스립트에는 조건부 표현을 작성하는 짧은 문법이 있습니다. 바로 조건부 연산자 또는 Ternary 연산자입니다. 아래와 같이 작성할 수 있습니다.

```jsx
return (
  <li className="item">
    {isPacked ? name + ' ✔' : name}
  </li>
);
```

위 표현은 `isPacked`가 `true`면 체크마크를 표시합니다.

# **DEEP DIVE Are these two examples fully equivalent? Show Details**

만약 여러분이 객체 지향 프로그래밍 배경에 익숙하다면, 아마 위의 두 예시가 미묘하게 다르다고 생각할 수 있는데, 왜냐면 `<li>`의 다른 두 인스턴스를 생성하기 때문입니다. 그러나 JSX 요소는 내부 상태를 유지하지 않고 실제 DOM 노드가 아니기 때문에 "인스턴스"가 아닙니다. JSX 요소는 설계도에 가깝습니다. 따라서 두 예시는 완벽히 동일합니다. Preserving and Resetting State 챕터에서 자세히 다룹니다.

이제 다른 HTML태그에 아이템의 텍스트를 래핑하기를 원합니다. <del> 태그를 통해 취소선을 긋는 것과 같이 말입니다. 여러분은 새로운 라인과 괄호를 더 추가하여 각 경우에 더 많은 JSX를 더 쉽게 삽입할 수 있습니다.

```jsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✔'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

이러한 스타일은 간단한 조건에서 잘 동작하지만, 절제해서 사용하세요. 만약 컴포넌트가 너무 많은 조건부 마크업에 의해 더러워진다면, 자식 컴포넌트를 추출하는 방안을 고려해보세요. 리액트에서 마크업은 코드의 일부입니다. 따라서 여러분은 복잡한 표현을 분할하기 위해 변수와 함수같은 도구를 활용할 수 있습니다.

# **Logical AND operator (`&&`)**

또 다른 일반적인 숏컷은 자바스크립트 AND 연산자 `&&` 입니다. 리액트 컴포넌트 내에서 이 연산자는 조건이 `true`일 때 어떤 JSX를 렌더링하길 원하고, `false`일 때 아무것도 렌더링하지 않기를 원할 때 사용됩니다. `&&` 와 함께라면 체크마크를 `isPacked`가 `true`일 때만 렌더링 할 수 있습니다.

```jsx
return (
  <li className="item">
    {name} {isPacked && '✔'}
  </li>
);
```

이는 `isPacked`가 `true`일 때 체크마크를 렌더링하고, 그렇지 않으면 아무것도 렌더링 하지 말라는 뜻입니다.

```jsx
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✔'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

자바스크립트 `&&` 표현은 왼쪽의 조건이 `true`면 오른쪽의 값을 반환합니다. 만약 조건이 `false`라면 전체 표현은 `false`가 됩니다. 리액트는 `false`를 `null`이나 `undefined`와 같은 JSX 트리의 구멍으로 인식합니다. 그리도 아무것도 렌더링하지 않습니다

# **Pitfall**

`&&` 의 조건으로 숫자를 넣지 마세요.

조건을 테스트하기 위해 자바스크립트는 자동으로 왼쪽의 조건을 boolean으로 형변환합니다. 하지만 만약 조건이 `0`이라면, 전체 표현은 값 `0`을 가지게 됩니다. 그리고 리액트는 `0`을 렌더링 할 것입니다.

예를 들어 `messageCount && <p>New messages</p>` 는 예상과 다르게 동작합니다. 이는 `messageCount` 가 `0` 일 때는 아무것도 렌더링하지 말라는 뜻으로 보이겠지만, 실제로는 `0`을 렌더링합니다.

조건을 `messageCount > 0 && <p>New messages</p>` 로 수정하세요.

# **Conditionally assigning JSX to a variable**

단축키가 일반 코드 작성에 방해가 되면 `if` 문과 변수를 사용하세요. `let`을 사용하여 정의된 변수를 재할당할 수 있으므로, 표시할 기본 콘텐츠의 이름을 제공하는 것부터 시작합니다:

`let itemContent = name;`

`if` 문을 사용하여 `isPacked`가 `true`라면 `itemContent`에 JSX 표현을 재할당하세요.

```jsx
if (isPacked) {
  itemContent = name + " ✔";
}

// ...

<li className="item">  {itemContent}</li>
```

이 스타일은 가장 장황하지만 가장 유연합니다.

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

이전과 같이 이는 텍스트와 동작하는 것이 아닙니다. 임의의 JSX 또한 동작합니다.

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✔"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

만약 자바스크립트가 익숙하지 않다면, 이러한 다양한 스타일을 우선 극복해야할 것으로 보일 것입니다. 하지만 이를 배우는 것은 리액트 컴포넌트 뿐만 아니라 자바스크립트 코드를 읽고 쓰는데 도움을 줄 것입니다. 선호하는 스타일을 하나 고르고, 다른 스타일을 잊은 경우 다시 이 문서를 참조하십시오.

# **Recap**

- 리엑트에서는 자바스크립트로 분기 로직을 처리할 수 있습니다.
- `if` 문으로 JSX 표현을 조건부로 반환할 수 있습니다.
- 일부 JSX를 조건부로 변수에 저장할 수 있고, 다른 JSX 내에 중괄호를 활용하여 포함시킬 수 있습니다.
- In JSX, `{cond ? <A /> : <B />}` means *“if `cond`, render `<A />`, otherwise `<B />`”*.
- In JSX, `{cond && <A />}` means *“if `cond`, render `<A />`, otherwise nothing”*.
- 숏컷은 일반적이지만 꼭 사용할 필요는 없습니다.
