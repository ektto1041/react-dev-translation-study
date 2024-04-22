리액트 컴포넌트는 다른 컴포넌트와 소통하기 위해 props를 사용합니다. 모든 부모 컴포넌트는 자식 컴포넌트에게 props를 전달함으로써 정보를 전달할 수 있습니다. props는 HTML 속성처럼 보이지만, 어떤 자바스크립트 값도 전달할 수 있습니다.

# **You will learn**

- 컴포넌트에 props를 보내는 방법
- 컴포넌트에서 props를 읽는 법
- props 의 기본값을 지정하는 방법
- 컴포넌트에 JSX를 전달하는 방법
- 시간에 따라 props가 변하는 모습

# **Familiar props**

props는 JSX 태그에 전달하는 정보입니다: 예를 들어, `className`, `src`, `alt`, `width`, 그리고 `height` 는 `<img>` 태그에 전달할 수 있는 props 중 일부입니다.

```jsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

`<img>` 태그에 전달할 수 있는 props는 미리 정의되어 있습니다.(ReactDOM 은 HTML 표준을 따릅니다) 하지만 직접 만든 컴포넌트에는 어떤 props든 전달할 수 있습니다. 어떻게 전달하는 지 살펴봅시다.

# **Passing props to a component**

In this code, the `Profile` component isn’t passing any props to its child component, `Avatar`:

이 코드에서 Profile 컴포넌트는 자식 컴포넌트에 어떤 props도 보내지 않고 있습니다.

```jsx
export default function Profile() {
  return (
    <Avatar />
  );
}
```

두 단계를 거쳐 `Avatar`에 몇몇의 props를 전달할 수 있습니다.

# **Step 1: Pass props to the child component**

First, pass some props to `Avatar`. For example, let’s pass two props: `person` (an object), and `size` (a number):

우선, 몇몇 props를 `Avater`에 전달해야합니다. 이번 예시에선 객체인 `person`과 숫자인 `size`를 전달해보겠습니다.

```jsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

> 만약 이중 중괄호가 헷갈리다면, 그들이 단지 자바스크립트 객체임을 기억하세요.
> 

이제 `Avatar`컴포넌트 내에서 이 props들을 읽을 수 있습니다.

# **Step 2: Read props inside the child component**

`function Avatar` 직후에 있는 `({` 와 `})` 내부에, 쉼표로 구분되는 props의 이름들(`person, size`)을 나열함으로써 props를 읽을 수 있습니다. 이는 마치 변수를 사용하듯 `Avatar` 코드 내에서 그들을 사용할 수 있게 해줍니다. 

```jsx
function Avatar({ person, size }) {
  // person and size are available here
}
```

`person`과 `size`를 사용하는 로직을 `Avatar`에 추가해보세요.

이제 다양한 props로 `Avatar`를 다양한 방식으로 렌더링할 수 있습니다. 값을 조정해 보세요!

```jsx
// App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi', 
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma', 
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{ 
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

```jsx
// utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

props는 부모 컴포넌트와 자식 컴포넌트를 독립적으로 생각할 수 있게 해줍니다. 예를 들어, `person`또는 `size`props를 `Propfile`내에서 `Avatar`가 그들을 어떻게 사용할 지 생각하지 않고 바꿀 수 있습니다. 이와 유사하게 `Avatar`가 그들을 사용하는 방식을 `Profile`을 살펴보지 않고도 바꿀 수 있습니다.

props를 조절할 수 있는 노브로 생각할 수 있습니다. 이들은 함수의 인수와 같은 역할을 수행합니다. 실제로 props는 컴포넌트에 대한 유일한 인수입니다. 리액트 컴포넌트 함수는 `props` 객체 하나만을 받습니다.

```jsx
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

일반적으로는 `props`객체 자체를 필요로 하지는 않습니다. 따라서 이를 개별의 props로 디스트럭쳐링 합니다.

> props를 선언할 때 중괄호를 깜빡하지 마세요
이 문법은 디스트럭쳐링이라고 불리고, 함수 파라미터로부터 프로퍼티를 읽는 것과 유사합니다.
> 

# **Specifying a default value for a prop**

만약 어떤 값도 주어지지 않았을 때 props에 기본값을 주고 싶다면, `=` 기호와 함께 기본값을 입력할 수 있습니다.

```jsx
function Avatar({ person, size = 100 }) {
  // ...
}
```

이제 `size` prop이 전달되지 않으면 `size`는 `100`으로 설정될 것입니다.

기본값은 `size`prop을 전달하지 않았을 때, 또는 `undefined`를 전달했을 때만 사용됩니다. 만약 `null`또는 `0`을 전달한다면 기본값은 사용되지 않습니다.

# **Forwarding props with the JSX spread syntax**

때때로, props를 전달하는 것은 반복적입니다:

```jsx
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

반복적인 코드에 잘못된 것은 없지만, 가끔은 간결한 코드를 중시할 수도 있습니다. 몇몇 컴포넌트는 그들의 props 전체를 그들의 자식 컴포넌트에 전달합니다 (위의 예시를 참고하세요). 그들의 props를 직접적으로 사용하지 않기 때문에, 간결한 “스프레드” 문법을 사용하는 것이 더 적절합니다. 

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

props의 이름을 나열하지 않고 `Avatar`에 `Profile`의 모든 props를 전달합니다.

만약 여러분이 다른 모든 컴포넌트에 스프레드 문법을 사용하고 있다면 뭔가 잘못된 것입니다. 종종, 그것은 여러분이 여러분의 컴포넌트를 나누고 아이들을 JSX로 넘겨야 한다는 것을 나타냅니다. 그것에 대해 다음에 더 살펴보겠습니다.

# **Passing JSX as children**

빌트인 브라우저 태그를 종속시키는 것은 일반적입니다:

```jsx
<div>
	<img />
</div>
```

때때로, 여러분은 여러분의 컴포넌트를 같은 방법으로 종속시키기를 원할 것입니다:

```jsx
<Card>
  <Avatar />
</Card>
```

JSX 태그 내에서 내용을 종속시킬 때, 부모 컴포넌트는 `children`이라는 이름의 prop을 통해 해당 내용을 받을 것입니다. 예를 들어, `Card` 컴포넌트는 `<Avatar />`가 할당된 `children` prop을 받을 것입니다. 그리고 이것을 렌더링할 것 입니다.

```jsx
// App.js
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

```jsx
// Avatar.js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

```jsx
// utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

`Card` 컴포넌트가 종속된 내용을 어떻게 래핑할 수 있는 지 살펴려면, `<Avatar>`를 텍스트로 대체해보세요. 그 안에서 무엇이 렌더링 되고 있는 지는 알 필요 없습니다. 이 유연한 패턴은 많은 곳에서 활용됩니다.

`children` prop이 있는 컴포넌트는 임의의 JSX를 가진 부모 컴포넌트에 의해 "채워질" 수 있는 "구멍"을 가지고 있다고 생각할 수 있습니다.

# **How props change over time**

아래의 `Clock` 컴포넌트는 부모 컴포넌트로 부터 두 props를 받습니다: `color`와 `time`. (부모 컴포넌트의 코드는 생략되었습니다.)

셀렉트 박스에서 색을 수정해보세요:

```jsx
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

이 예시는 컴포넌트가 시간에 따라 다른 props를 받을 수 있음을 설명합니다. props는 정적이지 않습니다. `time` props는 매초 변화하고, `color` prop은 다른 색을 선택할 때 마다 바뀝니다. props는 시작 시점에 멈춰있지 않고 특정 시점에서의 컴포넌트의 데이터를 반영합니다. 

하지만 props는 변하지 않습니다. 컴포넌트가 props 값을 수정할 필요가 있을 때, 부모 컴포넌트에 다른 props(다른 객체)를 전달해 줄 것을 요청합니다. 오래된 props는 치워질 것이고, 결국 자바스크립트 엔진은 메모리에서 그들을 지울 것입니다.

props를 변경하려 하지 마세요. 유저의 인풋에 반응해야 한다면, state를 설정해야 합니다. 이는 다른 챕터에서 배울 것입니다.

# **Recap**

- props를 전달하려면 이를 JSX에 추가하세요. HTML 속성을 추가하듯이
- props를 읽으려면 디스트럭쳐링 문법을 활용하세요.
- `size = 100` 과 같이 기본값을 설정할 수 있습니다. 기본값은 prop을 전달하지 않거나 `undefined`로 전달했을 때 사용됩니다.
- 모든 props를 JSX 스프레드 문법을 통해 전달할 수 있습니다. 하지만 남용하지는 마세요.
- 종속된 JSX는 컴포넌트의 `children` prop으로 전달됩니다.
- props는 특정 시점의 수정이 불가능한 스냅샷입니다: 모든 렌더링 결과는 새 props를 받게 됩니다.
- props는 수정할 수 없습니다. 상호작용이 필요하다면 state를 설정해야 합니다.
