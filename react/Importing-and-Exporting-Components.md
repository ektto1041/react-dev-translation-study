# **Importing and Exporting Components**

컴포넌트의 마법은 재사용성에 있습니다: 여러분은 다른 컴포넌트들로 구성되는 컴포넌트를 만들 수 있습니다. 하지만 많은 컴포넌트를 종속할 수록 이를 다른 파일로 나누는 것이 적절합니다. 이러한 방식은 여러분이 파일의 가독성을 좋게 유지하게 해주고, 컴포넌트를 더 많은 곳에서 재사용할 수 있게 해줍니다.

# **You will learn**

- 루트 컴포넌트 파일이 무엇인지
- 컴포넌트를 어떻게 import/export 하는지
- default / named import와 export를 해야하는 경우
- 한 파일로부터 여러 컴포넌트를 import/export 하는 방법
- 컴포넌트를 여러 파일로 나누는 방법

# **The root component file**

Your First Component 페이지에서 여러분은 `Profile` 컴포넌트와 `Gallery` 컴포넌트를 만들었습니다.

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

이 컴포넌트들은 `App.js` 라고 명명된 루트 컴포넌트 파일 내부에 있습니다. 설정을 통해 여러분의 루트 컴포넌트는 또 다른 파일이 될 수도 있습니다. 만약 여러분이 Next.js와 같은 파일 기반 라우팅을 지원하는 프레임워크를 사용한다면, 여러분의 루트 컴포넌트는 매 페이지마다 다를 것입니다.

# **Exporting and importing a component**

만약 여러분이 미래에 랜딩 스크린을 바꾸고, 과학책의 리스트를 두기를 원한다면 어떻게 해야 할까요? 또는 모든 프로필을 어딘가 다른 곳에 두고 싶다면 어떻게 해야 할까요? 이는 `Gallery`와 `Profile`을 루트 컴포넌트 바깥으로 이동시키면 됩니다. 이 방법은 해당 컴포넌트들을 더 모듈화시키고 다른 파일에서 재사용가능하게 만들 것입니다. 여러분은 컴포넌트를 세 단계를 거쳐 이동시킬 수 있습니다:

1. 컴포넌트를 넣을 새로운 JS 파일을 생성한다.
2. 해당 파일로 부터 컴포넌트를 export 한다. (default 또는 named exports 활용)
3. 컴포넌트를 사용할 파일에서 해당 컴포넌트들을 import 한다.

새 `Gallery.js` 파일에 `Profile`과 `Gallery`를 옮겼습니다. 이제 `App.js` 파일을 `Gallery.js` 파일로부터 `Gallery` 컴포넌트를 import 하게 수정할 수 있습니다.

```jsx
// App.js
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```jsx
// Gallery.js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
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

Notice how this example is broken down into two component files now:

이제 어떻게 이 예시가 두 컴포넌트 파일로 나누어 지는 지 봅시다:

1. `Gallery.js`:
    1. export되지 않는 `Profile` 컴포넌트를 정의합니다.
    2. `Gallery` 컴포넌트를 default export 로 export 합니다.
2. `App.js`:
    1. `Gallery.js` 파일로부터 `Gallery`컴포넌트를 default import 합니다.
    2. 루트 `App`컴포넌트를 default export 합니다.

> 여러분은 아마 .js 확장자가 사라진 것을 확인할 수 있을 것입니다.
`import Gallery from './Gallery';`
'/Gallery.js' 또는 '/Gallery' 둘 다 React와 함께 작동하지만, 전자가 기본 ES Module이 작동하는 방식에 더 가깝습니다.
> 

# **Exporting and importing multiple components from the same file**

`Gallery` 대신 `Profile` 하나만 보여주고 싶다면 어떻게 해야 할까요? `Profile` 컴포넌트도 내보낼 수 있습니다. 하지만 `Gallery.js`에는 이미 default export가 있으며 두 개의 default export를 가질 수는 없습니다. default export과 함께 새 파일을 만들 수도 있고, `Profile`에 대한 named export를 추가할 수도 있습니다. **한 파일에는 default export가 하나만 있을 수 있지만 named export는 여러 개 만들 수 있습니다.**

> default export와 named export 사이의 잠재적인 혼란을 줄이기 위해서, 몇몇 팀은 오직 하나의 스타일만 선택하거나, 한 파일에 섞어 사용하는 것을 금지합니다.
> 

우선, `Profile`을 `Gallery.js`로 부터 named export 합니다.

```jsx
export function Profile() {
	// ...
}
```

그 다음, `Profile`을 `App.js`에서 named import합니다. (중괄호와 함께)

```jsx
import { Profile } from './Gallery.js';
```

마지막으로, `<Profile />`을 `App` 컴포넌트에서 렌더링합니다.

```jsx
export default function App() {
	return <Profile />;
}
```

이제 `Gallery.js` 파일은 두 exports를 포함합니다: default `Gallery` export, 그리고 한 named Profile `export`. `App.js` 파일은 이 둘을 모두 import 합니다. `<Profile />`을 `<Gallery />`로 편집하고 다음 예를 확인하세요:

```jsx
// App.js
import Gallery from './Gallery.js';
import { Profile } from './Gallery.js';

export default function App() {
  return (
    <Profile />
  );
}
```

```jsx
// Gallery.js
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
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

이제 여러분은 default export 와 named export 를 섞어 사용하고 있습니다:

- `Gallery.js`:
    - Exports the `Profile` component as a **named export called `Profile`.**
    - Exports the `Gallery` component as a **default export.**
- `App.js`:
    - Imports `Profile` as a **named import called `Profile`** from `Gallery.js`.
    - Imports `Gallery` as a **default import** from `Gallery.js`.
    - Exports the root `App` component as a **default export.**

# **Recap**

이런 내용을 배웠습니다:

- 루트 컴포넌트 파일이 무엇인지
- 컴포넌트를 어떻게 import/export 하는지
- default / named import와 export를 해야하는 경우
- 한 파일로부터 여러 컴포넌트를 import/export 하는 방법
