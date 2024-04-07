원문: https://react.dev/learn/your-first-component

---

### 첫 번째 컴포넌트 만들어보기

컴포넌트는 리액트의 핵심 개념 중 하나이다. 이는 사용자 인터페이스를 구축하는 기반이며, 이는 리액트 여정의 시작을 완벽하게 해준다.

### 이 장에서 배울 것

- 컴포넌트가 무엇인지
- 리액트 앱에서 컴포넌트가 어떤 역할을 하는지
- 리액트 컴포넌트를 어떻게 만드는지

### 컴포넌트: UI 구축을 위한 블럭 요소

웹에서는 HTML을 사용하면 H1이나 li같은 내장된 태그를 사용하여 많은 구조화된 문서를 만들 수 있다.

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

위 마크업은 article 태그로 기사를 표시하고, h1 태그로 제목을 표시하며 ol 태그와 같이 순서가 지정된 목록을 (축약된)목차로 나타낸다. 이러한 마크업은 스타일을 위한 CSS와 상호작용을 위한 자바스크립트와 결합되어 모든 사이드바, 아바타, 모달, 드롭다운 뒤에 있다. - 웹에서 보는 모든 UI들

리액트는 마크업, css, 자바스크립트를 커스텀 컴포넌트로 결합해주는데, 이는 앱의 재사용 가능한 UI요소이다. 위에서 본 목차 코드는 모든 페이지에서 렌더링할 수 있는 TableOfContents 컴포넌트로 바뀔 수 있다. 내부적으로는 여전히 article, h1 등과 같은 동일한 HTML 태그를 사용한다.

HTML 태그와 마찬가지로 구성 요소를 구성, 정렬 및 중첩하여 전체 페이지를 디자인할 수 있다. 예를 들어, 읽고 있는 문서 페이지는 React 구성 요소로 구성되어 있다.

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

프로젝트가 성장함에 따라, 이미 작성된 컴포넌트를 재사용하여 많은 디자인을 구성함으로써 개발 속도를 높일 수 있다는 것을 알게 될 것이다. 위의 목차는 TableOfContents을 사용하여 어떤 화면에도 추가할 수 있다. Chakra UI 및 Material UI 와 같은 React 오픈 소스 커뮤니티에서 공유하는 수천 개의 구성 요소를 사용하여 프로젝트를 시작할 수도 있다.

### 컴포넌트 정의

전통적으로 웹 페이지를 만들 때 웹 개발자는 콘텐츠를 표시한 다음 일부 JavaScript를 뿌려 상호 작용을 추가했다. 이는 웹에서 상호 작용이 있으면 좋을 때 훌륭하게 작동했다. 이제 많은 사이트와 모든 앱에서 그러한 작동을 기대한다. React는 동일한 기술을 사용하면서도 상호작용을 최우선으로 생각힌다. React 컴포넌트는 마크업을 뿌릴 수 있는 JavaScript 함수이다. 그 모습은 다음과 같다(아래 예를 편집할 수 있음).

```js
export default function Profile() {
  return <img src="https://i.imgur.com/MK3eW3Am.jpg" alt="Katherine Johnson" />;
}
```

구성요소를 빌드하는 방법은 다음과 같다.

### 1단계: 컴포넌트 내보내기

export default 접두사는 표준 자바스크립트 문법이다.(리액트에만 국한된 것이 아님.) 이를 통해 나중에 다른 파일에서 가져올 수 있도록 파일의 주요 기능을 표시할 수 있다. (컴포넌트 가져오기 및 내보내기 파트에서 더욱 자세히 알아보기!)

### 2단계: 함수 정의하기

function Profile() { } 을 통해 Profile 라는 이름의 자바스크립트 함수를 정의한다.

- 주의!
  - React 구성 요소는 일반 JavaScript 함수이지만 이름은 대문자로 시작해야 하며 그렇지 않으면 작동하지 않는다!

### 3단계: 마크업 추가하기

구성 요소는 src와 alt 속성이 포함된 img태그를 반환한다. img 태그는 HTML처럼 작성되었지만 실제로는 내부적으로는 JavaScript 이다! 이 구문을 JSX 라고 하며 이를 사용하면 JavaScript 내에 마크업을 삽입할 수 있다.

리턴문은 다음과 같이 한 줄에 모두 작성될 수 있다.

```js
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

하지만 마크업이 return 키워드와 같이 모두 같은 줄에 있지 않으면, 마크업을 한 쌍의 괄호로 묶어야 한다.

```js
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

- 주의!
  - 괄호가 없으면 return 다음 줄의 코드는 모두 무시된다!

### 컴포넌트 사용하기

### 브라우저가 보는 것

### 컴포넌트 중첩 및 구성하기

### 요약
