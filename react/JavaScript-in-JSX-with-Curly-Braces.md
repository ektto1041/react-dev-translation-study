JSX는 자바스크립트 파일 내에서 HTML과 같은 마크업 문법을 작성할 수 있게 해주고, 렌더링 로직과 내용을 한 곳에 모아둘 수 있게 해줍니다. 때때로 여러분은 자바스크립트 로직이나 동적인 프로퍼티를 마크업 내에 넣고 싶어할 것입니다. 이런 상황에서 여러분은 중괄호를 사용할 수 있습니다.

# **You will learn**

- 따옴표로 문자열을 전달하는 법
- 중괄호로 JSX 내에서 자바스크립트 변수를 참조하는 법
- 중괄호로 JSX 내에서 자바스크립트 함수를 호출하는 법
- 중괄호로 JSX 내에서 자바스크립트 객체를 사용하는 법

# **Passing strings with quotes**

JSX에 문자열을 전달하기 원한다면 따옴표 또는 쌍따옴표를 이용할 수 있습니다:

```jsx
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/7vQD0fPs.jpg"
      alt="Gregorio Y. Zara"
    />
  );
}
```

`"https://i.imgur.com/7vQD0fPs.jpg"` 와 `"Gregorio Y. Zara"` 는 문자열로 전달된 값입니다.

`src` 또는 `alt` 프로퍼티의 값을 동적으로 전달하고 싶다면 중괄호와 자바스크립트 변수를 활용하면 됩니다.

```jsx
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

`className=”avatar”`와 `src={avatar}` 사이의 차이점에 주목하세요. 전자는 이미지를 둥글게 만들어주는 CSS 클래스 이름을 가리키고, 후자는 `avatar`라는 자바스크립트 변수의 값을 읽습니다. 중괄호를 사용하면 바로 마크업에서 자바스크립트로 작업할 수 있기 때문입니다!

# **Using curly braces: A window into the JavaScript world**

JSX는 자바스크립트를 작성하는 특별한 방법입니다. 즉, 중괄호를 활용하면 JSX 내에 자바스크립트를 활용하는 것이 가능합니다. 아래의 예시는 과학자의 이름 `name`을 선언하고, 이를 `<h1>` 태그 내에 중괄호와 함께 포함하고 있습니다.

```jsx
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

`name`의 값을 수정하면 리스트 제목이 어떻게 바뀔지 확인해볼까요?

`formatDate()`와 같은 함수 호출을 포함하여, 어떤 자바스크립트 표현도 중괄호 사이에 포함될 수 있습니다.

```jsx
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}
```

# **Where to use curly braces**

여러분은 JSX내에서 두 가지 방법으로 중괄호를 사용할 수 있습니다:

1. JSX 태그 내에서 텍스트로 사용: `<h1>{name}'s To Do List</h1>` 는 작동하지만, `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` 는 작동하지 않습니다.
2. **As attributes** immediately following the `=` sign: `src={avatar}` will read the `avatar` variable, but `src="{avatar}"` will pass the string `"{avatar}"`.
3. = 기호 뒤의 속성으로 사용: `src={avatar}` 는 `avatar` 변수를 읽습니다. 하지만 `src="{avatar}"` 는 문자열 `"{avatar}"` 을 전달합니다.

# **Using “double curlies”: CSS and other objects in JSX**

문자열, 숫자, 그리고 자바스크립트 표현 뿐 아니라 객체도 JSX에 전달할 수 있습니다. 객체 또한  `{ name: "Hedy Lamarr", inventions: 5 }` 와 같이 중괄호로 표시됩니다. 그러므로 자바스크립트 객체를 전달하기 위해서는 반드시 객체를 또 다른 중괄호 쌍으로 묶어야 합니다: `person={{ name: "Hedy Lamarr", inventions: 5 }}`

JSX내에서 인라인 CSS 스타일을 볼 수 있습니다. 리액트에서 인라인 스타일을 사용할 필요는 없습니다.(CSS 클래스가  대부분의 케이스에서 잘 동작합니다.) 하지만 인라인 스타일이 필요하다면 `style` 속성에 객체를 전달해야 합니다.

```jsx
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```

`backgroundColor` 와 `color` 의 값을 바꿔보세요.

아래처럼 작성하면 중괄호 안의 자바스크립트 개체를 실제로 볼 수 있습니다:

```jsx
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
```

다음에 JSX 내의 `{{` 와 `}}` 를 보게 되면, 자바스크립트 객체임에 불과하다는 것을 알아두세요.

> 인라인 `style` 프로퍼티는 카멜 케이스로 쓰여집니다. 예를 들어 `background-color`는 `backgroundColor`로 쓰여져야 합니다.
> 

# **More fun with JavaScript objects and curly brace**

여러분은 한 객체에 여러 표현을 옮기고, 이들을 중괄호를 활용하여 참조할 수 있습니다.

```jsx
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

위 예시에서 `person` 자바스크립트 객체는 문자열 `name`과 `theme`객체를 포함합니다.

컴포넌트는 이 값들을 다음과 같이 사용할 수 있습니다.

```jsx
<div style={person.theme}>
  <h1>{person.name}'s Todos</h1>
```

JSX는 자바스크립트를 사용하여 데이터와 논리를 구성할 수 있기 때문에 템플릿 언어로서 매우 작습니다.

# **Recap**

이제 여러분은 JSX에 대해서 거의 모든 것을 알고 있습니다:

- 따옴표 내에서 문자열로 전달되는 JSX 속성
- 중괄호는 마크업에서 자바스크립트 로직과 변수를 활용할 수 있게 해줍니다.
- 속성에서 `=` 기호 직후, 또는 JSX 태그의 내용에서 동작합니다.
- `{{` 와 `}}` 는 특별한 문법이 아닙니다: 이는 JSX 중괄호 내에 있는 자바스크립트 객체입니다.
