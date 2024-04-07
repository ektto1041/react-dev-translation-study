원문: https://react.dev/learn/describing-the-ui

---

# 사용자 인터페이스 표현하기

리액트는 자바스크립트 라이브러리로서 사용자 인터페이스(UI)를 렌더링한다. UI는 버튼, 텍스트, 이미지와 같은 요소들로 구현되어 있다. 리액트는 해당 UI들을 재사용 가능하고 묶을 수 있는 컴포넌트로 만들어준다. 웹사이트부터 핸드폰 앱까지 화면의 모든 요소들은 컴포넌트로 쪼개질 수 있다. 이 장에서는 이러한 리액트 컴포넌트를 만들고 커스터마이즈 하고 조건에 따라 보여주는 방식에 대해 배울 것이다.

### 이 장에서 배울 내용

- 첫 번째 리액트 컴포넌트 만드는 방법
- 다수의 컴포넌트 파일을 언제, 어떻게 만드는지에 관하여
- 자바스크립트에 JSX 문법으로 마크업 문법을 추가하는 방법
- 컴포넌트에서 자바스크립트에 기능적으로 접근하기 위해 JSX와 함께 중괄호를 사용하는 방법
- props로 컴포넌트를 구성하는 방법
- 조건부 렌더링을 하는 방법
- 한 번에 여러 컴포넌트를 렌더링 하는 방법
- 컴포넌트를 pure하게 유지하여 혼란스러운 버그를 피하는 방법
- UI를 트리로 이해하는 것이 유용한 이유

### 첫 번째 구성 요소

리액트 어플리케이션은 컴포넌트라고 불리는 독립된 UI들로 구성됨. 한 리액트 컴포넌트는 자바스크립트 함수인데, 이는 당신이 마크업 문법으로 표현할 수 있다. 컴포넌트는 버튼처럼 작아질 수도 있고, 전체 페이지처럼 커질 수도 있다. 아래의 코드는 3개의 Profile 컴포넌트를 렌더링하는 Gallery 컴포넌트이다.

```js
function Profile() {
  return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
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

### 컴포넌트 불러오기와 내보내기

- 하나의 파일에 여러 컴포넌트들 선언할 수 있지만, 거대한 파일은 탐색하기 어려울 수 있다. 이를 해결하기 위해서, 구성 요소를 자체 파일로 내보낸 다음 해당 구성 요소를 다른 파일에서 가져올 수 있다.

  ````js
  import Profile from "./Profile.js";

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
  ````

### JSX로 마크업 문법 작성하기

각 리액트 컴포넌트는 자바스크립트 함수인데, 리액트가 브라우저에 렌더링하는 일부 마크업들을 포함할 수 있다. 리액트 컴포넌트는 JSX라는 확장된 문법을 사용하여 마크업을 표현한다. JSX는 마치 HTML처럼 보이지만, 보다 엄격하고 동적인 정보를 표시할 수 있다.

만약 어떤 HTML 마크업을 리액트 컴포넌트에 붙여넣기 한다면, 그건 항상 작동하지는 않을 것이다.

```js
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
```

```js
Error
/src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (5:4)

  3 |     // This doesn't quite work!
  4 |     <h1>Hedy Lamarr's Todos</h1>
> 5 |     <img
    |     ^
  6 |       src="https://i.imgur.com/yXOvdOSs.jpg"
  7 |       alt="Hedy Lamarr"
  8 |       class="photo"
```

이런 HTML이 있을 경우, 아래와 같이 변환기를 사용하여 수정할 수 있다.

```js
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

### 중괄호를 통해 JSX 안에 자바스크립트 표현하기

JSX는 자바스크립트 파일 안에 HTML과 비슷한 마크업을 작성할 수 있게 만들어주는데, 동시에 렌더링 로직과 내부 콘텐츠도 동일하게 유지시켜 준다. 때떄로 마크업 안에서 자바스크립트 로직을 추가하거나, 동적 속성을 참조하고 싶을 수 있다. 이런 경우에는 JSX 내부에서 중괄호를 사용하여 자바스크립트에 대한 문을 열 수 있다.

```js
const person = {
  name: "Gregorio Y. Zara",
  theme: {
    backgroundColor: "black",
    color: "pink",
  },
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

### 컴포넌트에 props 전달하기

리액트 컴포넌트는 다른 요소들과 소통하기 위해 props를 사용한다. 모든 부모 컴포넌트는 자식 컴포넌트에게 props를 통해 몇가지 정보를 넘길 수 있다. props는 HTML의 속성들을 떠오르게 하지만, props를 통해 객체, 배열, 함수, 심지어는 JSX를 포함한 모든 자바스크립트 값을 전달할 수 있다.

```js
import { getImageUrl } from "./utils.js";

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: "Katsuko Saruhashi",
          imageId: "YfeOqp2",
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
  return <div className="card">{children}</div>;
}
```

### 조건부 렌더링

컴포넌트는 서로 다른 조건들에 따라 다르게 렌더링 되어야 할 수 있다. 리액트에서는 JSX를 조건부로 렌더링 할 수 있는데, 이를 위해 if 명령문이나 && 및 ? : 연산자와 같은 자바스크립트 구문을 사용할 수 있다.

이번 예제에서는 자바스크립트의 && 연산자가 체크 표시를 조건부로 렌더링 한다.

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && "✔"}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item isPacked={true} name="Space suit" />
        <Item isPacked={true} name="Helmet with a golden leaf" />
        <Item isPacked={false} name="Photo of Tam" />
      </ul>
    </section>
  );
}
```

### 렌더링 목록

데이터 컬렉션에서 여러 유사한 컴포넌트들을 표시하려는 경우도 있다. 이를 위해 리액트에서 자바스크립트의 filter()나 map()를 사용할 수 있는데, 이로써 데이터 배열을 컴포넌트 배열로 필터링하고 변환할 수 있다.

각각의 배열 아이템에 대해서, 특정한 key가 필요하다. 보통 ID를 key로 사용하고자 한다. 이러한 key는 리액트가 각각의 아이템의 배치가 바뀌어도 그 아이템의 위치를 추적할 수 있도록 한다.

```js
import { people } from "./data.js";
import { getImageUrl } from "./utils.js";

export default function List() {
  const listItems = people.map((person) => (
    <li key={person.id}>
      <img src={getImageUrl(person)} alt={person.name} />
      <p>
        <b>{person.name}:</b>
        {" " + person.profession + " "}
        known for {person.accomplishment}
      </p>
    </li>
  ));
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

### 컴포넌트를 pure하게 유지하기

몇몇 자바스크립트 함수는 pure 하다. pure 함수란 아래와 같다.

- 자신의 일을 잘 간직한다. 호출되기 전에 존재했던 객체나 변수는 변경되지 않는다.
- 동일한 입력, 동일한 출력. 동일한 입력이 주어지면, 항상 동일한 결과를 반환해야 한다.

pure 함수들로 엄격하게 컴포넌트를 작성하면, 코드가 길어짐에 따라 당황스러운 버그나 예측할 수 없는 동작들을 방지할 수 있다. pure 컴포넌트의 예시는 아래와 같다.

```js
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

위 컴포넌트를 pure하게 만들기 위해서는 prop을 넘겨주면 되는데, 이는 사전에 존재하는 변수를 수정해주는 과정을 대체한다.

```js
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

### 트리로 표현되는 UI

리액트는 트리를 사용하여 컴포넌트와 모듈들 간의 관계를 모델링한다.

리액트 렌더 트리는 컴포넌트들 간의 부모 자식 관계를 표현한다. 상단 트리 혹은 루트 컴포넌트 근처의 컴포넌트들은 최상단 컴포넌트로 간주된다. 자식 컴포넌트가 없는 컴포넌트들은 말단 컴포넌트로 간주된다. 이러한 분류 체계는 데이터 흐름과 렌더링 성능을 이해하는데 유용하다.

자바스크립트 모듈 간의 관계를 모델링하는 것은 앱을 이해하는 또 다른 유용한 방법이다. 이를 모듈 종속성 트리라고 부른다.

종속성 트리는 빌드 도구들에 의해 자주 사용되는데, 클라이언트가 다운로드하고 렌더링할 모든 관련된 자바스크립트 코드를 묶기 위해 사용된다. 번들 크기가 크면 리액트 앱의 사용자 경험이 저하된다. 모듈 종속성 트리를 이해하면 이러한 문제를 디버깅 하는데 도움이 된다.

### 다음은 뭘까?

이제 '첫 번째 구성요소 챕터'로 이동하여 페이지별로 읽어보자!
또는, 이러한 주제들이 이미 익숙하다면, '상호작용 요소 추가'를 읽어보는건 어때?
