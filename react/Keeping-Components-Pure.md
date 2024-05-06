# **Keeping Components Pure**

몇몇 자바스크립트 함수는 순수합니다. 순수 함수는 오로지 계산만 수행하고 다른 어떠한 일도 수행하지 않습니다. 컴포넌트를 순수 함수로 작성하면 코드가 성장함에 따라 생기는 예측 불가능한 동작과 버그들을 피할 수 있습니다. 이러한 이점을 얻기 위해 따라야 할 규칙이 있습니다.

# **You will learn**

- 순수함이 무엇이고, 어떻게 버그를 피하는 데 도움을 주는 지
- 렌더링 단계에서 변화를 제외하고 컴포넌트를 순수하게 유지하는 방법
- 컴포넌트에서 실수를 발견하기 위해 Strict Mode를 사용하는 방법

# **Purity: Components as formulas**

컴퓨터 공학에서, 특히 함수형 프로그래밍에서 순수 함수는 다음과 같은 특징을 가진 함수를 말합니다.

- 해당 함수가 호출되기 전에 존재했던 어떤 객체나 변수도 변화시키지 않습니다.
- 같은 인풋이라면 같은 아웃풋을 반환합니다.

수학 공식이 순수 함수의 예시입니다.

다음 식을 생각해봅시다: y = 2x.

If x = 2 이면 y = 4. Always.

If x = 3 이면 y = 6. Always.

만약 y = 2x 이고 x = 3 이라면, y 는 항상 6입니다.

이를 자바스크립트 함수로 만든다면 다음과 같습니다:

`function double(number) { return 2 * number;}`

위 예시에서 `double`은 순수 함수입니다. 3을 전달하면 항상 6을 반환합니다.

리액트는 이 개념을 중심으로 설계되었습니다. 리액트는 모든 컴포넌트를 순수 함수로 가정합니다. 즉, 리액트 컴포넌트는 같은 인풋에 항상 같은 JSX를 반환해야 합니다.

```jsx
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Boil {drinkers} cups of water.</li>
      <li>Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.</li>
      <li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Spiced Chai Recipe</h1>
      <h2>For two</h2>
      <Recipe drinkers={2} />
      <h2>For a gathering</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

마치 수학 공식처럼 `drinkers={2}` 를 `Recipe` 에 전달하면, `2 cups of water` 를 포함하는 JSX를 항상 반환합니다.

You could think of your components as recipes: if you follow them and don’t introduce new ingredients during the cooking process, you will get the same dish every time. That “dish” is the JSX that the component serves to React to [render.](https://react.dev/learn/render-and-commit)

컴포넌트를 레시피라고 생각해도 됩니다. 만약 레시피를 따르면 매번 같은 음식을 얻을 수 있습니다. 음식은 컴포넌트가 만드는 JSX입니다.

!https://react.dev/images/docs/illustrations/i_puritea-recipe.png

Illustrated by [Rachel Lee Nabors](https://nearestnabors.com/)

# **Side Effects: (un)intended consequences**

리액트의 렌더링 프로세스는 항상 순수합니다. 컴포넌트는 JSX를 반환해야하고, 렌더링 이전에 존재했던 어떤 객체나 변수도 변화시키면 안됩니다.

여기 순수하지 않은 컴포넌트가 있습니다:

```jsx
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

이 컴포넌트는 외부에서 선언된 `guest` 변수를 읽고 쓰고 있습니다. 이는 해당 컴포넌트를 여러번 호출하면 다른 JSX를 만들어냄을 의미합니다. 게다가 다른 컴포넌트가 `guest`를 읽는다면 역시 다른 JSX를 만들어 낼 것입니다. 이는 예측이 불가능합니다.

`guest`를 prop으로 전달함으로써 이를 고칠 수 있습니다:

```jsx
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

이제 컴포넌트가 순수해졌습니다.

일반적으로 컴포넌트가 특정한 순서로 렌더링되기를 기대해서는 안 됩니다. y = 5x 이전 또는 이후에 y = 2x를 불러도 상관이 없습니다. 두 공식은 서로 독립적으로 해결됩니다. 같은 방식으로 각 컴포너트는 렌더링하는 동안 다른 컴포넌트와 조정하거나 의존하려고 시도하지 않아야 합니다. 렌더링은 학교 시험과 같습니다. 각 구성 요소는 자체적으로 JSX를 계산해야 합니다.

# **DEEP DIVE Detecting impure calculations with StrictMode Show Details**

아직 여러분이 모두 배우지 않았더라도, 리액트에는 렌더링 동안 읽을 수 있는 세 종류의 인풋이 있습니다: props, state 그리고 context. 여러분은 항상 이 인풋들을 읽기 전용으로 다뤄야 합니다.

유저 인풋에 따라 무언가 변하기를 원한다면, 변수를 수정하기보다 set state를 해야 합니다. 절대로 이미 존재하는 변수나 객체를 수정해서는 안됩니다.

리액트는 개발하는 동안 각 컴포넌트의 함수를 두 번 호출해주는 Strict Mode를 제안합니다. 컴포넌트 함수를 두 번 호출함으로써 Strict Mode는 규칙을 지키지 않는 컴포넌트를 찾아줍니다.

Strict Mode는 프로덕션 버전에서는 어떤 효과도 없습니다. 따라서 어떤 성능 저하도 발생하지 않을 것입니다. Strict Mode를 적용하기 위해서는 루트 컴포넌트를 `<React.StrictMode>`ㄷ로 래핑해야 합니다. 어떤 프레임워크는 이를 기본적으로 적용할 것입니다.

# **Local mutation: Your component’s little secret**

위의 예시에서 컴포넌트가 이미 존재하던 변수를 렌더링 중에 수정한다는 점이 문제였습니다. 이는 mutation이라고 불립니다. 순수 함수는 함수의 스코프 밖에 있는 변수 또는 함수 호출 이전에 만들어진 객체를 mutate하지 않습니다.

However, **it’s completely fine to change variables and objects that you’ve *just* created while rendering.** In this example, you create an `[]` array, assign it to a `cups` variable, and then `push` a dozen cups into it:

하지만, 렌더링 중에 만든 객체나 변수를 수정하는 것은 괜찮습니다. 아래 예시에서, 여러분은 하나의 `[]` 배열을 만들고 이를 `cups` 변수에 할당합니다. 그 후 컵들을 `push` 합니다.

```jsx
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

만약 배열이 TeaGathering 함수 밖에서 만들어졌다면, 이는 큰 문제가 될 것입니다.

하지만 렌더링 중에 배열을 생성했다면 이는 괜찮습니다. 컴포넌트 외부의 코드는 컴포넌트 내부에서 발생하는 일에 대해 모를 것 입니다. 이를 Local Mutation이라고 합니다..

# **Where you *can* cause side effects**

함수형 프로그래밍은 순수성에 의존하지만, 특정 시점, 어딘가에서 어떤 것은 변화해야만 합니다. 그게 바로 프로그래밍의 핵심입니다. 화면 업데이트, 애니메이션 시작, 데이터 변경과 같은 변경 사항을 side effect라고 합니다. 렌더링 중이 아닌 side에서 일어나는 일입니다.

리액트에서 side effect는 일반적으로 이벤트 핸들러 내에 속합니다. 이벤트 핸들러는 여러분이 어떤 액션을 수행했을 때 리액트가 실행하는 함수입니다. 예를 들어, 버튼을 클릭하는 액션이 있을 수 있습니다. 비록 컴포넌트 내에 이벤트 핸들러가 작성된다 하더라도, 그들은 렌더링 중에 실행되지 않습니다. 따라서 이벤트 핸들러는 순수할 필요가 없습니다.

만약 side effect에 적절한 이벤트 핸들러를 찾을 수 없다면 useEffect 호출을 사용하여 반환된 JSX에 연결할 수 있습니다. 이는 리액트에게 렌더링 이후에 실행하라고 알려줍니다. 하지만 이 방식은 최후의 수단이어야 합니다.

가능하다면, 렌더링만으로 논리를 표현해 보세요. 이것이 여러분을 얼마나 멀리 데려갈 수 있는지 놀라게 될 것입니다.

# **Recap**

- A component must be pure, meaning:
- 컴포넌트는 순수해야 합니다.
    - 렌더링 이전에 존재했던 변수나 객체를 수정해서는 안됩니다.
    - **Same inputs, same output.**
- 렌더링은 어떤 시점에서도 발생할 수 있습니다. 따라서 컴포넌트는 다른 렌더링 시퀀스에 의존해서는 안됩니다.
- 컴포넌트의 인풋을 mutate하면 안됩니다. set state 해야합니다.
- 최후의 수단으로 `useEffect`를 활용하고, JSX 내에서 컴포넌트의 로직을 표현하도록 노력하세요
- 순수 함수를 쓰는 것은 약간의 연습이 필요하지만 리액트의 패러다임의 힘을 풀어줍니다.
