# Describing the UI

React는 유저 인터페이스(UI)를 렌더링하는 자바스크립트 라이브러리입니다. UI는 버튼, 텍스트 그리고 이미지와 같은 작은 단위들로 구성이 됩니다. React는 이러한 작은 단위들을 재사용가능하고, 종속 가능한 컴포넌트 Component 로 묶을 수 있게 해줍니다. 웹 사이트부터 휴대폰 어플리케이션까지, 스크린 속의 모든 것들은 컴포넌트로 쪼개질 수 있습니다. 이 챕터에서 여러분은 React 컴포넌트를 만들고, 커스터마이징하고, 조건부로 보여주는 방법에 대해 배울 것입니다.

# **Your first component**

리액트 애플리케이션은 컴포넌트라고 부르는 UI의 조각들로 이뤄집니다. 리액트 컴포넌트는 마크업도 포함될 수 있는 자바스크립트 함수입니다. 컴포넌트는 버튼처럼 작을 수도 있고 페이지처럼 클 수도 있습니다. 여기 세 Profile 컴포넌트를 렌더링하는 Gallery 컴포넌트가 있습니다.

```jsx
// App.js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

# **Importing and exporting components**

여러분은 많은 컴포넌트를 한 파일에 정의할 수 있지만, 한 파일이 너무 커지면 가독성이 떨어질 수 있습니다. 이를 해결하기 위해 여러분은 파일에서 컴포넌트를 export 할 수 있습니다. 그리고 이를 다른 파일에서 import 할 수 있습니다.

```jsx
// Profile.js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```jsx
// Gallery.js
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

# **Writing markup with JSX**

각각의 리액트 컴포넌트는 마크업을 포함할 수도 있는 브라우저에 렌더링하기 위한 자바스크립트 함수입니다. 리액트 컴포넌트는 마크업을 작성하기 위해 JSX라고 부르는 확장 문법을 사용합니다. JSX는 HTML처럼 생겼지만 조금 더 엄격하고 동적인 정보를 보여줄 수 있습니다.

만약 여러분이 HTML 마크업을 리액트 컴포넌트에 복사한다면 이는 동작하지 않을 것입니다.

```jsx
// App.js
export default function TodoList() {
  return (
    // This doesn't quite work!
    <h1>Hedy Lamarr's Todos</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    >
    <ul>
      <li>Invent new traffic lights
      <li>Rehearse a movie scene
      <li>Improve spectrum technology
    </ul>
  );
}

// 실행 결과
/*
Error
/src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (5:4)

  3 |     // This doesn't quite work!
  4 |     <h1>Hedy Lamarr's Todos</h1>
> 5 |     <img
    |     ^
  6 |       src="https://i.imgur.com/yXOvdOSs.jpg"
  7 |       alt="Hedy Lamarr"
  8 |       class="photo"
*/
```

[컨버터](https://transform.tools/html-to-jsx)를 사용해서 수정할 수 있습니다.

```jsx
// App.js
export default function TodoList() {
  return (
    <>
      <h1>Hedy Lamarr's Todos</h1>
      <img
        src="https://i.imgur.com/yXOvdOSs.jpg"
        alt="Hedy Lamarr"
        className="photo"
      />
      <ul>
        <li>Invent new traffic lights</li>
        <li>Rehearse a movie scene</li>
        <li>Improve spectrum technology</li>
      </ul>
    </>
  );
}

```

# **JavaScript in JSX with curly braces**

JSX는 자바스크립트 파일 안에서 렌더링 로직과 렌더링 내용을 같은 파일 내에서 유지함과 동시에 HTML과 비슷한 마크업 문법을 작성할 수 있게 해줍니다. 때때로 당신은 자바스크립트 로직을 조금 추가하거나 마크업 내부에서 동적인 속성을 참조하기를 원할 것입니다. 이 상황에서 여러분은 중괄호를 JSX에 사용하여 자바스크립트를 활용할 수 있습니다.

```jsx
// App.js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

# **Passing props to a component**

리액트 컴포넌트는 다른 컴포넌트와 소통하기 위해 props를 사용합니다. 모든 부모 컴포넌트는 자식 컴포넌트에게 props를 전달함으로써 정보를 전달할 수 있습니다. props는 HTML 속성처럼 보일 수도 있지만, 객체, 배열, 함수, 심지어 JSX를 포함한 모든 자바스크립트 값을 전달할 수 있습니다.

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

// App.js
import { getImageUrl } from './utils.js'

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

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

```

# **Conditional rendering**

여러분의 컴포넌트는 여러 상태에 따라 다른 것들을 보여줄 필요가 있습니다. 리액트에서 여러분은 `if` 문, `&&` 연산자, `?` 연산자와 같은 자바스크립트 문법을 활용하여 여러 조건에 맞게 다른 JSX를 렌더링할 수 있습니다.

아래 예에서, 자바스크립트 `&&` 연산자는 체크마크를 조건에 따라 렌더링하기 위해 사용됩니다.

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

# **Rendering lists**

여러분은 데이터의 집합을 여러 유사한 컴포넌트를 통해 보여주려고 할 것입니다. 여러분은 데이터 배열을 컴포넌트의 배열로 변환하기 위해 자바스크립트의 `filter()`, `map()` 과 같은 함수를 활용할 수 있습니다.

각각의 배열 아이템에는 `key`를 명시할 필요가 있습니다. 일반적으로 여러분은 데이터베이스에서 사용하는 ID를 키로 사용하길 원할 것입니다. `Key`는 배열이 변하더라도 리액트가 각각의 아이템의 위치를 기억할 수 있도록 해줍니다.

```jsx
// App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}

// data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];

// utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    's.jpg'
  );
}

```

# **Keeping components pure**

몇몇 자바스크립트 함수는 순수합니다. 순수 함수란:

- **자신의 역할에만 신경을 씁니다.** 순수 함수는 해당 함수가 호출되기 전 존재하던 어떤 객체나 변수도 바꾸지 않습니다.
- **input이 같으면 output도 항상 같습니다.**

여러분의 함수를 순수 함수로 작성함으로써 여러분은 코드가 커지면서 생기는 예상치 못한 동작이나 버그를 피할 수 있습니다. 여기 순수하지않은 컴포넌트의 예가 있습니다.

```jsx
// App.js
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

여러분은 이미 존재하는 변수를 수정하는 대신 prop을 전달함으로써 해당 컴포넌트를 순수하게 만들 수 있습니다.

```jsx
// App.js
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

# **Your UI as a tree**

리액트는 컴포넌트와 모듈 사이의 관계를 모델링하기 위해 트리를 사용합니다.

리액트 렌더트리는 컴포넌트 사이의 부모 자식 관계를 표현하기 위한 트리입니다.

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fgeneric_render_tree.png&w=1080&q=75

리액트 렌더트리의 예시입니다.

트리의 상위에 위치한 컴포넌트들은 top-level 컴포넌트입니다. 자식이 없는 컴포넌트는 리프 컴포넌트입니다. 컴포넌트의 분류화는 데이터의 흐름과 렌더링 성능을 이해하는 데 유용합니다.

자바스크립트 모듈 사이의 관계를 모델링하는 것은 여러분의 애플리케이션을 이해하는 또 다른 유용한 방법입니다. 이를 모듈 의존성 트리라고 합니다.

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fgeneric_dependency_tree.png&w=1080&q=75

모듈 의존성 트리의 예시입니다.

의존성 트리는 모든 연관된 자바스크립트 코드를 번들링하기 위해 빌드 툴에 의해 사용됩니다. 큰 번들 사이즈는 리액트 애플리케이션에서의 유저 경험을 저해시킵니다. 모듈 의존성 트리를 이해하는 것은 이러한 이슈를 디버깅할 때 유용합니다.
