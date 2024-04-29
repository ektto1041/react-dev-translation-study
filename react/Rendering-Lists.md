여러분은 자주 데이터의 집합에서 유사한 여러 컴포넌트를 보여주길 원할 것입니다. 이 때 자바스크립트 배열 메소드를 활용할 수 있습니다. 이 페이지에서 여러분은 데이터의 배열을 컴포넌트의 배열로 전환하기 위해 `filter()`와 `map()`을 사용해볼 것입니다.

# **You will learn**

- 자바스크립트의 `map()`을 사용해서 컴포넌트를 렌더링 하는 방법
- 자바스크립트의 `filter()`를 사용해서 특정한 컴포넌트만 렌더링 하는 방법
- 리액트 키를 언제, 왜 사용하는지

# **Rendering data from arrays**

아래와 같은 컨텐트의 리스트를 가지고 있습니다.

```jsx
<ul>
  <li>Creola Katherine Johnson: mathematician</li>
  <li>Mario José Molina-Pasquel Henríquez: chemist</li>
  <li>Mohammad Abdus Salam: physicist</li>
  <li>Percy Lavon Julian: chemist</li>
  <li>Subrahmanyan Chandrasekhar: astrophysicist</li>
</ul>
```

리스트 아이템 사이의 유일한 차이점은 아이템의 컨텐트밖에 없습니다. 여러분은 인터페이스를 만들 때 다른 데이터를 활용해서 같은 컴포넌트의 여러 인스턴스를 보여줄 필요가 있을 것입니다. 이러한 상황에서 여러분은 데이터를 자바스크립트 객체 또는 배열에 저장할 수 있고, 컴포넌트 리스트를 렌더링하기 위해 `map()` `filter()` 같은 메소드를 활용할 수도 있습니다.

배열로부터 아이템의 리스트를 생성하는 짧은 예시가 있습니다.

1. 데이터를 배열로 옮깁니다

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];
```

1. JSX 노드로 구성된 새 배열 `listItems`에 people의 요소들을 매핑합니다.

```jsx
const listItems = people.map(person => <li>{person}</li>);
```

1. <ul>로 래핑된 listItems를 반환합니다.

```jsx
return <ul>{listItems}</ul>;
```

결과 코드는 아래와 같습니다.

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}

//
Warning: Each child in a list should have a unique "key" prop.

Check the render method of `List`. See https://reactjs.org/link/warning-keys for more information.
    at li
    at List
//
```

콘솔 에러에 주목하세요.

여러분은 이 페이지에서 이 에러를 어떻게 고치는 지 배울 것입니다. 이를 배우기 전에 데이터에 몇몇의 구조를 더해보겠습니다.

# **Filtering arrays of items**

이 데이터는 더 구조화될 수 있습니다.

```jsx
const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',  
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
}];
```

직업이 `chemist`인 사람들만 보여주고 싶다고 해봅시다. 여러분은 그런 사람들만 반환하기 위해 자바스크립트의 `filter()` 메소드를 활용할 수 있습니다. 이 메소드는 배열의 아이템을 일종의 테스트에 전달하고, 해당 테스트를 통과한 아이템들만으로 구성된 새 배열을 반환합니다.

어떻게 하는 지 살펴보겠습니다.

1. `chemist` 사람들을 위한 새 배열 `chemists`를 만들고 `filter()`를 호출하세요. 테스트 조건은 아래와 같습니다.

```jsx
const chemists = people.filter(person =>  person.profession === 'chemist');
```

1. 이제 `chemists`를 매핑합니다.

```jsx
const listItems = chemists.map(person =>
  <li>
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
```

1. 마지막으로, `listItems`를 반환합니다.

```jsx
return <ul>{listItems}</ul>;
```

```jsx
// App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const listItems = chemists.map(person =>
    <li>
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
  return <ul>{listItems}</ul>;
}
```

```jsx
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
```

```jsx
// utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    's.jpg'
  );
}
```

# **Pitfall**

화살표 함수는 암묵적으로 `⇒` 직후의 표현을 반환합니다. 따라서 `return` 문을 작성할 필요가 없습니다.

```jsx
const listItems = chemists.map(person =>
  <li>...</li> // Implicit return!
);
```

하지만 화살표 함수에서 중괄호를 사용한다면 반드시 `return` 문을 작성해야 합니다.

```jsx
const listItems = chemists.map(person => { // Curly brace
  return <li>...</li>;
});
```

중괄호를 포함하는 화살표 함수는 block body를 가졌다고 말합니다. 이는 한 줄의 코드보다 더 많이 적을 수 있게 해주지만, `return` 문을 반드시 작성해야 합니다. 만약 이를 깜빡하면 아무것도 반환하지 않게 됩니다.

# **Keeping list items in order with `key`**

지금까지의 예시들은 모두 콘솔 에러를 발생시킵니다.

```jsx
Console

Warning: Each child in a list should have a unique “key” prop.
```

배열 아이템에 key를 줄 필요가 있습니다. key는 배열에서 아이템을 고유하게 식별하는 문자열 또는 숫자입니다.

```jsx
<li key={person.id}>...</li>
```

# **Note**

`map()` 호출 내에 있는 모든 JSX 원소는 항상 `key`를 필요로 합니다.

`Key`는 React에게 각 컴포넌트가 어떤 배열 아이템에 해당하는지 알려줘서 나중에 매칭시킬 수 있도록 해줍니다. 이는 배열 아이템이 움직이거나, 삽입되거나, 삭제될 때 중요합니다. 잘 작성된 `key`는 리액트가 배열에 어떤 변화가 생겼는지 추론하는 것을 도와주고 DOM 트리가 올바르게 변화되도록 도와줍니다.

`key`를 그때그때 생성하기보다, 데이터 안에 이를 포함시켜야 합니다.

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
        <b>{person.name}</b>
          {' ' + person.profession + ' '}
          known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

```jsx
// data.js
export const people = [{
  id: 0, // Used in JSX as a key
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1, // Used in JSX as a key
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2, // Used in JSX as a key
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3, // Used in JSX as a key
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4, // Used in JSX as a key
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```jsx
// utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    's.jpg'
  );
}
```

# **DEEP DIVE Displaying several DOM nodes for each list item Show Details**

각각의 아이템이 여러 DOM 노드로 렌더링되길 원한다면 어떻게 해야 할까요?

짧은 `<> … </>` 프래그먼트 문법은 `key`를 지정할 수 없습니다. 따라서 여러분은 여러 노드를 `<div>` 태그 내에 포함시킬 필요가 있습니다. 또는 `<Fragment>` 문법을 사용할 수도 있습니다.

```jsx
import { Fragment } from 'react';

// ...

const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

프래그먼트는 DOM에서 사라집니다. 따라서 이는 `<h1>, <p>` 의 리스트를 생성합니다.

# **Where to get your `key`**

서로 다른 데이터 소스가 서로 다른 키 소스를 제공합니다.

- 데이터베이스의 데이터: 만약 여러분의 데이터가 데이터베이스에서 왔다면, 데이터베이스의 ID(PK)를 활용할 수 있습니다.
- 로컬에서 생성된 데이터: 데이터가 만약 로컬에서 생성되고 지속된다면, 증가하는 카운터, UUID를 활용할 수 있습니다.

# **Rules of keys**

- Key는 아이템 리스트에서 고유해야 합니다. 하지만, 다른 배열의 JSX 노드에는 같은 key를 사용할 수 있습니다.
- Key는 절대 변하면 안됩니다. 렌더링할때 key를 생성하면 안됩니다.

# **Why does React need keys?**

여러분의 데스크탑에 파일들이 이름을 가지지 않았다고 상상해보세요. 그러면 여러분은 그들의 순서를 통해 그들을 구분해야 합니다. 이는 파일을 삭제하면 복잡해집니다. 두 번째 파일은 첫 번째 파일이 되고, 세 번째 파일은 두 번째 파일이 되고, 계속 이런 처리가 되어야 합니다.

폴더에서의 파일 이름과 배열에서의 JSX key는 유사한 역할을 합니다. 그들은 아이템을 고유하게 식별할 수 있게 해줍니다. 잘 작성된 key는 배열에서 데이터의 위치보다 더 많은 정보를 제공합니다. 아이템의 순서가 바뀌어 위치가 바뀌더라도 key는 리액트가 이를 구분할 수 있게 해줍니다.

# **Pitfall**

여러분은 아마 key에 아이템의 인덱스를 쓰고자 할 것입니다. 사실 이는 key를 명시하지 않았을 때 리액트가 처리하는 방식입니다. 하지만 여러분이 아이템을 렌더링한 순서는 시간에 따라(삽입, 삭제 등에 의해) 바뀔 것입니다. key로 사용되는 인덱스는 자주 버그를 유발합니다.

이와 유사하게, 키를 그때그때 생성하지 마세요(랜덤과 같이). 이는 다시 렌더링 될 때 key가 절대 매칭되지 않게 되어 DOM 트리 전체가 재생성되는 원인이 됩니다. 이는 성능 저하 뿐만 아니라 유저의 인풋 역시 초기화 시킬 수 있습니다. 대신 데이터에 있는 안정적인 ID를 활용하세요.

Note that your components won’t receive `key` as a prop. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: `<Profile key={id} userId={id} />`.

컴포넌트가 prop으로 key를 받지 않음에 주목하세요. 이는 오로지 리액트에 대한 힌트로만 활용됩니다. 만약 여러분의 컴포넌트가 ID를 필요로 한다면 다른 prop으로 전달해야 합니다. `<Profile key={id} userId={id} />`

# **Recap**

이 페이지에서 배운 내용:

- 배열과 객체 같은 데이터 구조에 데이터를 옮기는 법
- 자바스크립트의 `map()`을 활용해 유사한 컴포넌트의 집합을 만드는 방법
- 자바스크립트의 `filter()`를 활용해 필터링된 아이템의 배열을 만드는 방법
- 콜렉션의 각각의 컴포넌트에 key를 왜, 어떻게 설정하는지, 그래서 어떻게 리액트가 아이템의 순서를 유지하는 지
