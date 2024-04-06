# **Your First Component**

컴포넌트는 리액트의 핵심 개념 중 하나입니다. 컴포넌트는 UI를 구성하는 기반으로, 리액트를 공부하기 시작하기에 가장 완벽한 시작점입니다.

# **You will learn**

- 컴포넌트가 무엇인지
- 리액트 애플리케이션에서 컴포넌트가 어떤 역할을 하는지
- 첫 리액트 컴포넌트를 작성하는 방법

# **Components: UI building blocks**

웹에서, HTML은 우리가 `<h1>`이나 `<li>` 같은 빌트인 태그를 활용해 풍부한 구조화된 문서를 만들 수 있게 해줍니다.

```html
<article>
  <h1>My First Component</h1>
  <ol>
    <li>Components: UI Building Blocks</li>
    <li>Defining a Component</li>
    <li>Using a Component</li>
  </ol>
</article>
```

위 마크업은 전체 아티클을 `<article>`로, 제목을 `<h1>`로, (약칭) 목차를 순서가 있는 목록인 `<ol>`로 나타내고 있습니다. 스타일을 위한 CSS와 상호작용을 위한 JavaScript, 이 둘과 결합된 위와 같은 마크업은 모든 사이드바, 아바타, 모달, 드랍다운과 같이 웹에서 볼 수 있는 모든 UI 조각의 뒤에 놓여져 있습니다.

> abbreviated - 약칭, 단축된
table of contents - 목차
> 

리액트는 여러분이 여러분의 마크업, CSS, 그리고 JavaScript를, 재사용 가능한 UI 요소인 커스텀 컴포넌트 하나로 묶을 수 있게 해줍니다. 위 마크업에 작성된 목차는 `<TableOfContents />` 컴포넌트로 만들 수 있으며, 여러분은 이를 모든 페이지에 렌더링할 수 있습니다. 내부적으로 이는 여전히 같은 HTML 태그를 사용합니다.

HTML 태그와 같이, 여러분은 전체 페이지를 디자인하기 위해 컴포넌트들을 구성하고, 정렬하고, 종속시킬 수 있습니다. 예를 들어, 여러분이 현재 읽고 있는 이 문서 페이지는 리액트 컴포넌트들로 만들어졌습니다.

```html
<PageLayout>
  <NavigationHeader>
    <SearchBar />
    <Link to="/docs">Docs</Link>
  </NavigationHeader>
  <Sidebar />
  <PageContent>
    <TableOfContents />
    <DocumentationText />
  </PageContent>
</PageLayout>
```

여러분의 프로젝트가 성장할수록, 여러분은 여러분의 디자인 중 많은 부분이 이미 작성된 컴포넌트를 재사용함으로써 구성될 수 있음을 알 수 있을 것입니다. 그리고 이는 개발 속도를 빠르게 해줄 것입니다. 우리의 목차는 <TableOfContents /> 컴포넌트를 활용해 어떤 화면에서 추가될 수 있습니다. 심지어 여러분은 수천개의 컴포넌트를 공유하는 리액트 오픈소스 커뮤니티([Chakra UI](https://chakra-ui.com/), [Material UI](https://mui.com/material-ui/))를 활용해 프로젝트를 빠르게 시작할 수도 있습니다.

# **Defining a component**

전통적인 웹 페이지 개발 방식은, 웹 개발자는 그들의 콘텐츠를 마크업한 후 자바스크립트를 뿌림으로써 상호작용을 추가했습니다. 이러한 방식은 상호작용이 있으면 좋은 것일 때 매우 효과적이었습니다. 하지만 현대 웹 개발에서 상호작용은 모든 사이트와 애플리케이션에 기대됩니다. 리액트는 같은 기술을 사용하면서도 상호작용을 추가합니다: **리액트 컴포넌트는 마크업을 뿌릴 수 있는 자바스크립트 함수**입니다. 여기 예시가 있습니다.

> nice-to-have 있으면 좋은
> 

```jsx
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}

```

어떻게 컴포넌트를 작성할 수 있을까요?

# **Step 1: Export the component**

`export default` 접두사는 자바스크립트 표준 문법입니다. 이는 여러분이 파일 내부에서 메인 함수를 표시하고 이를 다른 파일에서 import 할 수 있게 해줍니다. 

# **Step 2: Define the function**

`function Profile() {}` 문으로 여러분은 `Profile` 이라는 이름의 자바스크립트 함수를 정의합니다.

<aside>
💡 리액트 컴포넌트는 일반적인 자바스크립트 함수지만, 컴포넌트의 이름은 반드시 대문자로 시작해야합니다. 그렇지 않으면 동작하지 않을 것입니다.

</aside>

# **Step 3: Add markup**

컴포넌트는 `src`, `alt` 속성과 함께 `<img />` 태그를 반환합니다. `<img />` 태그는 HTML처럼 작성되었지만, 이는 사실 자바스크립트입니다. 이런 문법은 JSX라고 불리며, 이는 여러분이 자바스크립트 내부에 마크업을 포함할 수 있게 해줍니다.

반환문은 한 라인으로도 작성할 수 있습니다.

```jsx
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

하지만 만약 여러분의 마크업이 한 라인이 아니라면 반드시 괄호로 묶어야 합니다.

```jsx
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

<aside>
💡 괄호가 없다면 `return` 문 이후의 코드는 모두 무시될 것입니다.

</aside>

# **Using a component**

이제 `Profile` 컴포넌트를 정의했으므로, 여러분은 이를 다른 컴포넌트의 내부에 종속시킬 수 있습니다. 예를 들어, 여러분은 여러 `Profile` 컴포넌트를 사용하는 `Gallery` 컴포넌트를 export 할 수 있습니다.

```jsx
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

# **What the browser sees**

소문자, 대문자를 사용할 때는 주의해야 합니다:

- `<section>` 은 소문자로 시작합니다. 따라서 리액트는 HTML 태그로 해석합니다.
- `<Profile />` 은 대문자로 시작합니다. 따라서 리액트는 이를 컴포넌트로 해석합니다.

`Profile`은 HTML을 포함하고 있습니다: `<img />`. 최종적으로 브라우저는 다음과 같은 코드를 보게 됩니다.

```jsx
<section>
  <h1>Amazing scientists</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
</section>
```

# **Nesting and organizing components**

컴포넌트는 일반적인 자바스크립트 함수입니다. 따라서 여러분은 여러 컴포넌트를 같은 파일에 작성할 수도 있습니다. 이러한 방식은 컴포넌트가 작거나 다른 컴포넌트와 밀접하게 연관될 때 편리합니다. 만약 이 파일이 너무 복잡해지면, 여러분은 언제나 이를 여러 파일로 분산시킬 수 있습니다. 이에 대한 방법은 Importing and Exporting Components 에서 더 배울 수 있습니다.

`Profile` 컴포넌트는 `Gallery` 컴포넌트 내부에서 렌더링되기 때문에, 우리는 `Gallery` 를 부모 컴포넌트라고 부를 수 있습니다. 이는 리액트의 마법의 일부입니다. 한 번 구성 요소를 정의한 다음 원하는 장소와 횟수만큼 사용할 수 있습니다.

<aside>
💡 컴포넌트는 다른 컴포넌트를 렌더링할 수 있지만, 절대 다른 컴포넌트의 정의를 종속시켜서는 안됩니다. 대신 top-level에 모든 컴포넌트를 정의해야 합니다.

</aside>

# [Deep Dive] Components all the way down

리액트 애플리케이션은 “루트” 컴포넌트에서 시작합니다. 일반적으로 루트 컴포넌트는 새 프로젝트를 시작할 때 자동으로 생성됩니다. 예를 들어, 만약 여러분이 CodeSandbox 또는 Next.js 를 사용한다면, 루트 컴포넌트는 `pages/index.js` 내에 정의되어 있습니다. 

대부분의 리액트 애플리케이션은 use components all the way down. 이는 여러분이 컴포넌트를 재사용가능한 조각으로만 사용하지 않고, 큰 조각인 사이드바, 리스트, 그리고 완성된 페이지로도 사용한다는 것을 의미합니다. 컴포넌트는 UI 코드와 마크업을 구성하는 데 유용한 방법입니다. 일부는 한 번만 사용하더라도 말입니다. 

리액트를 기반으로 하는 프레임워크들은 더 발전했습니다. 빈 HTML 파일을 사용하고 리액트가 자바스크립트를 이용해 페이지를 매니징하게하는 것 대신, 그들은 리액트 컴포넌트로부터 자동적으로 HTML을 생성합니다. 이러한 방식은 자바스크립트 코드가 로딩되기 이전에 몇몇의 콘텐트를 보여줄 수 있게 해줍니다.

여전히 많은 웹사이트는 리액트를 HTML 페이지에 상호작용을 추가하기 위해 사용합니다. 그들은 여러 루트 컴포넌트를 갖비니다. 여러분은 여러분이 필요한 만큼 리액트를 사용할 수 있습니다.

# **Recap**

- 리액트는 컴포넌트를 만들 수 있게 해줍니다. 컴포넌트는 재사용 가능한 UI 요소입니다.
- 리액트 애플리케이션에서 모든 UI 요소는 컴포넌트입니다.
- 리액트 컴포넌트는 일반적인 자바스크립트 함수입니다.
    - 이름은 반드시 대문자로 시작해야 합니다.
    - JSX 마크업을 반환해야 합니다
