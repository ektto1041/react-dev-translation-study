JSX는 자바스크립트 파일 내에 HTML과 같은 마크업을 작성할 수 있게 해주는 자바스크립트 확장 문법입니다. 컴포넌트를 작성하는 다른 방법도 있지만, 대부분의 리액트 개발자들은 JSX를 선호하고, 대부분의 기존 코드 역시 이를 사용합니다.

# **You will learn**

- 리액트가 마크업과 렌더링 로직을 섞는 이유
- JSX가 HTML과 다른 점
- JSX에서 정보를 표시하는 방법

# **JSX: Putting markup into JavaScript**

웹은 HTML, CSS, JS로 구성됩니다. 그동안 웹 개발자들은 내용을 HTML에, 디자인을 CSS에, 그리고 로직을 JS에 담아 구성해왔습니다. 

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fwriting_jsx_html.png&w=750&q=75

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fwriting_jsx_js.png&w=750&q=75

하지만, 웹의 상호작용이 좀 더 늘어나면서 로직이 점점 내용을 결정하게 되었습니다. JS가 HTML를 담당하게 되었습니다. 이것이 리액트에서 렌더링 로직과 마크업을 컴포넌트라고 하는 한 곳에 위치시키는 이유입니다.

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fwriting_jsx_sidebar.png&w=750&q=75

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fwriting_jsx_form.png&w=750&q=75

버튼의 렌더링 로직과 마크업을 함께 위치시키는 것은 모든 편집에서 로직과 마크업이 동기화되는 것을 보장합니다. 반대로, 버튼의 마크업과 사이드바의 마크업과 같이 연관이 없는 디테일은 서로 독립적입니다. 이는 각각을 수정하는 작업을 더 안전하게 만들어줍니다.

각각의 리액트 컴포넌트는 마크업을 포함할 수 있는 JS 함수입니다. 리액트 컴포넌트들은 마크업을 표현하기 위해 JSX라고 부르는 확장 문법을 사용합니다. JSX는 HTML과 같이 생겼지만, 이는 좀 더 엄격하고 동적인 정보를 표현할 수 있습니다. 이를 이해하기 위한 가장 쉬운 방법은 HTML 마크업을 JSX 마크업으로 바꿔보는 것입니다.

> JSX와 리액트는 분리된 개념입니다. 그들은 자주 같이 사용되지만, 여러분은 이들을 독립적으로 사용할 수도 있습니다. 리액트는 자바스크립트 라이브러리인 반면, JSX는 확장 문법입니다.
> 

# **Converting HTML to JSX**

Suppose that you have some (perfectly valid) HTML:

다음과 같은 HTML을 가지고 있다고 가정해보겠습니다:

```html
<h1>Hedy Lamarr's Todos</h1>
<img
   src="https://i.imgur.com/yXOvdOSs.jpg"
   alt="Hedy Lamarr"
   class="photo"
>
 <ul>
     <li>Invent new traffic lights
     <li>Rehearse a movie scene
     <li>Improve the spectrum technology
 </ul>
```

그리고 위 HTML을 컴포넌트를 넣기를 원한다면 어떻게 해야할까요?

```jsx
export default function TodoList() {
  return (
    // ???
  )
}
```

단순히 HTML을 복사, 붙여넣기 한다면 이는 작동하지 않을 것입니다:

```jsx
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
      <li>Improve the spectrum technology
    </ul>
  );
}
```

**Error**

```jsx
/src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (5:4)

  3 |     // This doesn't quite work!
  4 |     <h1>Hedy Lamarr's Todos</h1>
> 5 |     <img 
    |     ^
  6 |       src="https://i.imgur.com/yXOvdOSs.jpg" 
  7 |       alt="Hedy Lamarr" 
  8 |       class="photo"
```

이는 JSX가 HTML보다 더 엄격하고 좀 더 많은 규칙을 가지고 있기 때문입니다. 에러 메시지가 마크업을 수정하기 위한 방법을 알려줄 것입니다.

> 대부분 리액트의 on-screen 메시지는 여러분이 문제를 찾는데 도움을 줍니다. 만약 문제가 생겼다면 이를 읽어보세요.
> 

# **The Rules of JSX**

# **1. Return a single root element**

여러 엘리먼트를 컴포넌트로 부터 리턴하기 위해서는 엘리먼트들을 하나의 부모 태그로 묶어야 합니다.

예를 들어, `<div>` 태그를 활용할 수 있습니다:

```jsx
<div>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</div>
```

만약 추가적인 `<div>`를 추가하고 싶지 않다면, `<>`와 `</>`를 사용할 수도 있습니다:

```jsx
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

이 빈 태그를 Fragment라고 부릅니다. Fragment는 브라우저 HTML 트리에 어떤 흔적도 남기지 않고 엘리먼트들을 그룹화할 수 있게 해줍니다.

### DeepDive: **Why do multiple JSX tags need to be wrapped?**

JSX가 HTML같이 생기긴 했지만, 사실 JSX는 plain JS 객체로 변환됩니다. 배열로 구성하는 것이 아니라면 JS에서 두 객체를 하나의 함수로부터 리턴하는 것은 불가능합니다. 따라서 여러 JSX 태그들은 반드시 하나의 태그 또는 Fragment로 묶여야 합니다.

# **2. Close all the tags**

JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`, and wrapping tags like `<li>oranges` must be written as `<li>oranges</li>`.

JSX는 명시적으로 닫혀야 합니다: `<img>`와 같이 self-closing 태그도 반드시 `<img />`와 같이 작성되어야 합니다. `<li>oranges` 역시 반드시 `<li>oranges</li>`로 작성되어야 합니다.

아래의 예시를 참고하세요:

```jsx
<>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
   />
  <ul>
    <li>Invent new traffic lights</li>
    <li>Rehearse a movie scene</li>
    <li>Improve the spectrum technology</li>
  </ul>
</>
```

# **3. camelCase ~~all~~ most of the things!**

JSX는 JS 객체로 바뀌고, 속성들은 JS 객체의 키로 변환됩니다. 여러분의 컴포넌트에서 여러분은 자주 그들의 속성을 변수로 읽고 싶어할 것입니다. 하지만 JS는 변수 이름에 제한이 있습니다. 예를 들어, 변수의 이름은 `-` 또는 예약된 단어를 사용할 수 없습니다.

이 것은 리액트에서 많은 HTML과 SVG 속성들이 카멜케이스로 쓰여지는 이유입니다. 예를 들어, `stroke-width` 대신 `strokeWidth`를 사용해야합니다. `class`는 예약된 단어이기 때문에 리액트에서는 `className`을 사용해야 합니다. 

```jsx
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

DOM 컴포넌트 props의 리스트에서 모든 속성을 찾을 수 있습니다. 만약 틀려도 리액트가 메시지를 출력해서 알려주니 걱정하지 않아도 됩니다.

> 역사적인 이유로 `aria-*` 와 `data-*` 속성은 `-`와 함께 쓰여집니다.
> 

# **Pro-tip: Use a JSX Converter**

이미 존재하는 마크업에서 모든 속성을 JSX로 변환하는 것은 힘든 작업이 될 수 있습니다. 따라서 존재하는 HTML을 SVG 또는 JSX로 변환하기 위해서 [컨버터](https://transform.tools/html-to-jsx)를 사용하는 것을 추천합니다. 컨버터는 실전에서도 굉장히 유용하지만, 여전히 JSX를 쉽게 작성하기 위해 원리를 이해할 필요할 가치가 있습니다.

여기 마지막 결과가 있습니다:

```jsx
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
        <li>Improve the spectrum technology</li>
      </ul>
    </>
  );
}
```

# **Recap**

이제 여러분은 왜 JSX가 존재하는 지와 이를 컴포넌트에서 어떻게 사용하는 지를 알게 되었습니다:

- 리액트 컴포넌트는 렌더링 로직과 마크업을 묶습니다. (그들이 서로 연관되기 때문에)
- JSX는 HTML과 조금의 차이만 있고 유사합니다. 필요하다면 컨버터를 사용할 수도 있습니다.
- 에러메시지는 마크업을 수정하기 위한 올바른 방향을 알려줍니다.
