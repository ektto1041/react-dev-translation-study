리액트 앱은 서로 종속되는 컴포넌트들로 형성됩니다. 리액트가 어떻게 앱 컴포넌트 구조를 처리할까요?

리액트와 다른 UI 라이브러리 들은 UI를 트리로 모델링합니다. 앱을 트리로 생각하는 것은 컴포넌트 간의 관계를 이해하는 데 유용합니다. 이러한 이해는 성능과 state 관리와 같은 미래의 개념을 디버깅하는 데 도움을 줍니다.

# **You will learn**

- 리액트가 컴포넌트 구조를 파악하는 방법
- 렌더 트리가 무엇이고 어디에 유용한 지
- 모듈 의존성 트리가 무엇이고 어디에 유용한 지

# **Your UI as a tree**

트리는 아이템 사이의 관계 모델이고, UI는 트리 구조로 자주 표현됩니다. 예를 들어, 브라우저는 HTML과 CSS를 모델링하기 위해 트리 구조를 사용합니다. 모바일 플랫폼 또한 뷰 계층을 표현하기 위해 트리를 사용합니다.

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_dom_tree.png&w=1920&q=75

리액트는 컴포넌트들로부터 UI 트리를 만듭니다. 이 예시에서 UI 트리는 DOM을 렌더링하기 위해 사용됩니다.

브라우저와 모바일 플랫폼 같이, 리액트는 컴포넌트 사이의 관계를 모델링하고 관리하기 위해 트리 구조를 사용합니다. 이 트리들은 데이터가 어떻게 흐르는 지를 이해하기 위한, 그리고 앱과 렌더링 사이즈를 최적화하는 방법을 이해하기 위한 유용한 도구입니다.

# **The Render Tree**

컴포넌트의 주 기능은 다른 컴포넌트의 컴포넌트를 구성하는 것입니다. 컴포넌트 종속에서 우리는 부모 컴포넌트와 자식 컴포넌트의 개념에 대해 배웠습니다. 여기서 각 부모 컴포넌트는 그 자체로 다른 컴포넌트의 자식일 수 있습니다.

리액트 앱을 렌더링할 때, 렌더 트리로 알려진 트리로 관계를 모델링할 수 있습니다.

여기 영감을 주는 인용문을 렌더링하는 React 앱이 있습니다.

```jsx
// App.js
import FancyText from './FancyText';
import InspirationGenerator from './InspirationGenerator';
import Copyright from './Copyright';

export default function App() {
  return (
    <>
      <FancyText title text="Get Inspired App" />
      <InspirationGenerator>
        <Copyright year={2004} />
      </InspirationGenerator>
    </>
  );
}

// FancyText.js
export default function FancyText({title, text}) {
  return title
    ? <h1 className='fancy title'>{text}</h1>
    : <h3 className='fancy cursive'>{text}</h3>
}

// Color.js
export default function Color({value}) {
  return <div className="colorbox" style={{backgroundColor: value}} />
}

// InspirationGenerator.js
import * as React from 'react';
import inspirations from './inspirations';
import FancyText from './FancyText';
import Color from './Color';

export default function InspirationGenerator({children}) {
  const [index, setIndex] = React.useState(0);
  const inspiration = inspirations[index];
  const next = () => setIndex((index + 1) % inspirations.length);

  return (
    <>
      <p>Your inspirational {inspiration.type} is:</p>
      {inspiration.type === 'quote'
      ? <FancyText text={inspiration.value} />
      : <Color value={inspiration.value} />}

      <button onClick={next}>Inspire me again</button>
      {children}
    </>
  );
}

// Copyright.js
export default function Copyright({year}) {
  return <p className='small'>©️ {year}</p>;
}

// inspirations.js
export default [
  {type: 'quote', value: "Don’t let yesterday take up too much of today.” — Will Rogers"},
  {type: 'color', value: "#B73636"},
  {type: 'quote', value: "Ambition is putting a ladder against the sky."},
  {type: 'color', value: "#256266"},
  {type: 'quote', value: "A joy that's shared is a joy made double."},
  {type: 'color', value: "#F9F2B4"},
];
```

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Frender_tree.png&w=1080&q=75

리액트는 렌더링된 컴포넌트로 구성되는 UI 트리인 렌더 트리를 생성합니다.

예시로 든 앱으로부터 우리는 위의 렌더트리를 만들 수 있습니다.

트리는 노드로 구성되는데, 각각의 노드는 컴포넌트를 표현합니다.

리액트 렌더 트리의 루트 노드는 앱의 루트 컴포넌트입니다. 이 예시에서 루트 컴포너트는 `App`이고, 이는 리액트가 렌더링하는 첫 컴포넌트입니다. 각각의 화살표는 부모 컴포넌트에서 자식 컴포넌트로 가리킵니다.

하나의 렌더 트리는 리액트 앱에서의 하나의 렌더 패스를 표현합니다. 조건부 렌더링을 통해 부모 컴포넌트는 아마 다른 자식 컴포넌트를 렌더링 할 것입니다. 

영감을 주는 인용문이나 색상을 조건부로 렌더링하도록 앱을 업데이트할 수 있습니다.

```jsx
// App.js
import FancyText from './FancyText';
import InspirationGenerator from './InspirationGenerator';
import Copyright from './Copyright';

export default function App() {
  return (
    <>
      <FancyText title text="Get Inspired App" />
      <InspirationGenerator>
        <Copyright year={2004} />
      </InspirationGenerator>
    </>
  );
}

// FancyText.js
export default function FancyText({title, text}) {
  return title
    ? <h1 className='fancy title'>{text}</h1>
    : <h3 className='fancy cursive'>{text}</h3>
}

// Color.js
export default function Color({value}) {
  return <div className="colorbox" style={{backgroundColor: value}} />
}

// InspirationGenerator.js
import * as React from 'react';
import inspirations from './inspirations';
import FancyText from './FancyText';
import Color from './Color';

export default function InspirationGenerator({children}) {
  const [index, setIndex] = React.useState(0);
  const inspiration = inspirations[index];
  const next = () => setIndex((index + 1) % inspirations.length);

  return (
    <>
      <p>Your inspirational {inspiration.type} is:</p>
      {inspiration.type === 'quote'
      ? <FancyText text={inspiration.value} />
      : <Color value={inspiration.value} />}

      <button onClick={next}>Inspire me again</button>
      {children}
    </>
  );
}

// Copyright.js
export default function Copyright({year}) {
  return <p className='small'>©️ {year}</p>;
}

// inspirations.js
export default [
  {type: 'quote', value: "Don’t let yesterday take up too much of today.” — Will Rogers"},
  {type: 'color', value: "#B73636"},
  {type: 'quote', value: "Ambition is putting a ladder against the sky."},
  {type: 'color', value: "#256266"},
  {type: 'quote', value: "A joy that's shared is a joy made double."},
  {type: 'color', value: "#F9F2B4"},
];
```

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fconditional_render_tree.png&w=1200&q=75

조건부 렌더링를 통해 렌더 트리는 다른 컴포넌트들을 렌더링 할 것입니다.

이 예시에서, `inspiration.type`이 무엇인지에 따라 우리는 `<FancyText>` 또는 `<Color>`를 렌더링 할 것입니다. 렌더 트리는 각각의 렌더 페스에 따라 달라집니다.

렌더 트리가 렌더 패스에 따라 달라지더라도, 이 트리들은 일반적으로 리액트 앱에 어떤 컴포넌트들이 있는지 파악하는데 유용합니다. 상위 컴포넌트들은 루트 컴포넌트에 가까이 있고, 아래에 있는 컴포넌트들의 렌더링 성능에 영향을 주는 컴포넌트입니다. 하위 컴포넌트는 트리의 바닥에 있고 자식 컴포넌트가 없는, 자주 리렌더링되는 컴포넌트입니다.

이러한 컴포넌트의 분류를 파악하는 것은 데이터 흐름을 이해하고 앱의 성능을 파악하는 데 유용합니다.

# **DEEP DIVE Where are the HTML tags in the render tree? Show Details**

위의 렌더 트리에는 각 컴포넌트가 렌더링하는 HTML 태그에 대한 언급이 없습니다. 왜냐하면 렌더 트리는 React 컴포넌트로만 구성되어 있기 때문입니다.

UI 프레임워크인 리액트는 플랫폼에 구애받지 않습니다. react.dev에서 우리는 HTML 마크업을 UI 프리미티브로 사용하는 웹에 렌더링하는 예를 보여줍니다. 그러나 리액트 앱은 UI 뷰 또는 FrameworkElement와 같은 다양한 UI 프리미티브를 사용할 수 있는 모바일 또는 데스크톱 플랫폼에 렌더링할 수 있습니다.

이러한 플랫폼 UI 프리미티브는 리액트의 일부가 아닙니다. 리액트 렌더 트리는 앱이 어떤 플랫폼을 렌더링하는지에 관계없이 리액트 앱에 대한 통찰력을 제공할 수 있습니다.

# **The Module Dependency Tree**

리액트 앱에서의 또 다른 관계는 앱의 모듈 의존성입니다. 우리는 컴포넌트, 함수 또는 상수를 export할 수 있는 JS 모듈을 만듭니다.

모듈 의존성 트리에서 각각의 노드는 모듈이고, 각각의 연결선은 모듈에서의 `import` 문을 표현합니다.

위의 예시를 예로 들면, 윌는 다음과 같은 모듈 의존성 트리를 만들 수 있습니다.

https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fmodule_dependency_tree.png&w=1920&q=75

트리의 루트 노드는 루트 모듈입니다. 이는 보통 루트 컴포넌트를 포함하고 있는 모듈입니다.

동일한 앱의 렌더 트리와 비교했을 때, 유사한 구조가 있지만 몇 가지 주목할 만한 차이점이 있습니다:

- 노드는 컴포넌트가 아닌 모듈을 나타냅니다.
- 컴포넌트가 아닌 모듈 또한 트리에 표현됩니다.

의존성 트리는 리액트 앱을 실행하는데 필수적인 모듈을 결정하는 데 유용합니다. 프로덕션 리액트 앱을 빌드할 때, 모든 필수적인 자바스크립트를 번들링하는 전형적인 빌드 단계가 있습니다. 이러한 책임을 가진 번들러라고 하는 도구가 있고, 번들러는 의존성 트리를 포함되더야 하는 모듈을 결정하는 데 사용할 것입니다.

앱이 커지면 번들 사이즈도 커집니다. 큰 번들 사이즈는 비용이 많이 듭니다. 큰 번들 사이즈는 UI를 불러오는 시간을 지연시킬 수 있습니다. 앱의 의존성 트리를 이해하는 것은 이러한 이슈를 디버깅하는데 도움을 줍니다.

# **Recap**

- 트리는 엔터티 사이의 관계를 표현하는 일반적인 방법입니다. 이는 보통 UI를 모델링하는데 사용됩니다.
- 렌더 트리는 한 번의 렌더링에서 리액트 컴포넌트 사이의 관계를 표현합니다.
- 조건부 렌더링을 활용하면 렌더 트리는 다른 렌더링마다 변화할 것입니다. 다른 prop, 컴포넌트는 다른 자식 컴포넌트를 렌더링 할 것입니다.
- 렌더 트리는 상위 컴포넌트와 하위 컴포넌트를 파악하는 데 도움을 줍니다.
- 의존성 트리는 모듈 의존성을 표현합니다.
- 의존성 트리는 빌드 도구에 의해 사용됩니다.
- 의존성 트리는 큰 번들 사이즈를 디버깅하는 데 유용합니다.
