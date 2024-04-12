# 컴포넌트 import & export 하기
컴포넌트의 특수성은 재사용 가능성에 있습니다. 
즉, 다른 컴포넌트로 구성된 또 하나의 컴포넌트를 만들 수 있습니다. 
그러나 점점 더 많은 컴포넌트를 중첩한 형태는 차라리 구성 요소를 따로 다른 파일로 분할하는 것이 오히려 합리적일 때가 많습니다. 
파일로 분할하는 것은 파일을 관리하기에 편리하고, 더 많은 위치에서 구성 요소를 재사용할 수 있습니다.

## root 컴포넌트 파일
앞서 Your First Component 단계에서 Profile 컴포넌트와 이를 렌더링에 사용한 Gallery 컴포넌트를 만들었습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/d0fceb3a-3156-4698-9215-234cd5adcc19)
이 예제에서는 현재 App.js라는 루트 구성 요소 파일에 있습니다. 
하지만 설정에 따라 루트 구성 요소가 다른 파일에 있을 수도 있습니다. 
Next.js와 같은 파일 기반 라우팅이 포함된 프레임워크를 사용하는 경우 루트 구성 요소 위치는 페이지마다 다릅니다.

## 컴포넌트 import & export 하기
나중에 랜딩된 화면을 바꿔 거기에 과학도서 목록을 넣고 싶다면? 아니면 모든 Profile을 다른 곳에 배치하시겠습니까? 
루트 구성 요소 파일에서 Gallery와 Profile을 따로 이동하는 것이 좋습니다. 
이렇게 하면 더 모듈화되고 다른 파일에서 재사용이 가능해집니다. 다음 세 단계로 구성 요소를 이동할 수 있습니다.

1. 구성요소를 넣을 새 JS 파일을 만듭니다.
2. 해당 파일에서 함수 구성 요소를 내보냅니다(default export 또는 named export 사용)
3. 구성 요소를 사용할 파일에서 가져옵니다(default or named import/export 기술 사용).

* 내보내기에는 두 종류, named과 default 내보내기가 있습니다.
* 모듈 하나에서, named export는 여러 개 존재할 수 있지만 default export는 하나만 가능합니다. 각 종류는 위의 구문 중 하나와 대응합니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/2c13e43e-d4d8-49d3-aebf-a7e0f6a11d7d)
* named 내보내기는 여러 값을 내보낼 때 유용합니다. 가져갈 때는 내보낸 이름과 동일한 이름을 사용해야 합니다.

* 반면 기본 내보내기는 어떤 이름으로도 가져올 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/c877d98e-9f72-4b59-b9e8-36f05d6eacad)

다시 본론으로 돌아와서
여기에서는 Profile과 Gallery가 모두 App.js에서 Gallery.js라는 새 파일로 이동되었습니다. 이제 App.js를 변경하여 Gallery.js에서 Gallery 컴포넌트를 가져올 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/964afd2b-107d-4eb9-83aa-c02cebf96e39)
이제 이 예제가 두 개의 구성 요소 파일로 어떻게 구분되는지 확인하세요.

1. Gallery.js:
- 동일한 파일 내에서만 사용되며, 내보내지지 않는 Profile 구성요소를 정의합니다.
- Gallery 구성요소를 default export로 내보냅니다.

2. App.js:
- Gallery.js에서 default import로 Gallery를 가져옵니다.
- default export로 root App 구성 요소를 내보냅니다. -> *export default function App() { ... }

* './Gallery.js' 또는 './Gallery'는 React에서 작동하지만 전자는 기본 ES 모듈 작동 방식에 더 가깝습니다.

### Default vs named exports 
JavaScript를 사용하여 값을 내보내는 두 가지 주요 방법은 default 내보내기와 named 내보내기입니다. 
지금까지 예제에서는 default 내보내기만 사용했습니다. 
그러나 동일한 파일에서 둘 중 하나 또는 둘 다를 사용할 수 있습니다. 
파일에는 dafault 내보내기가 하나만 있을 수 있지만 named(이름이 지정된) 내보내기는 원하는 만큼 많이 가질 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/fb6d68ec-1a0e-495b-8ac9-e52126302564)

구성 요소를 내보내는 방법에 따라 가져오는 방법이 결정됩니다. 
named 내보내기와 동일한 방식으로 default 내보내기를 가져오려고 하면 오류가 발생합니다! 이 차트는 다음을 추적하는 데 도움이 될 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/be181832-3272-41f3-8929-9970f3278425)

default 가져오기를 작성할 때 가져오기 후에 원하는 이름으로 변경 입력할 수 있습니다. 
예를 들어, 대신 import Banana from './Button.js'라고 쓸 수 있으며 여전히 동일한 default내보내기가 제공됩니다. 
대조적으로, named 가져오기의 경우 이름이 양쪽에서 일치해야 합니다. 그래서 named(명시된 이름 그대로 사용) imports이라고 부르죠!

파일이 하나의 구성 요소만 내보내는 경우 사람들은 종종 default export를 사용하고, 여러 구성 요소와 값을 내보내는 경우 named exports를 사용합니다. 
선호하는 코딩 스타일에 관계없이 항상 구성요소 기능과 이를 포함하는 파일에 의미 있는 이름을 지정하세요. 
export default () => {}와 같이 이름이 없는 구성 요소는 디버깅을 더 어렵게 만들기 때문에 권장되지 않습니다.

## 동일한 파일에서 여러 구성요소 내보내기 및 가져오기
Gallery 대신 하나의 Profile만 표시하려면 어떻게 해야 합니까? Profile 구성 요소도 내보낼 수 있습니다. 
하지만 Gallery.js에는 이미 기본 내보내기가 있으므로 두 개의 export default를 가질 수는 없습니다. 
export default를 사용하여 새 파일을 생성하거나 Profile에 대한 named exports를 추가할 수 있습니다. 
파일에는 export default가 하나만 있을 수 있지만 named exports는 여러 개 있을 수 있습니다!

먼저 named exports(default 키워드 없음)를 사용하여 Gallery.js에서 Profile을 내보냅니다.
export function Profile() {
  // ...
}

그런 다음 named exports(중괄호 포함)를 사용하여 Gallery.js에서 App.js로 Profile을 가져옵니다.
import { Profile } from './Gallery.js';

마지막으로 App 구성 요소에서 <Profile />을 렌더링합니다.
export default function App() {
  return <Profile />;
}

이제 Gallery.js에는 a default Gallery export와 a named Profile export라는 두 가지 exports가 포함되어 있습니다. 
App.js는 둘 다 가져옵니다. 다음 예에서는 <Profile />을 <Gallery />로 편집하고 다시 되돌려 보십시오.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/d4899eb3-f633-416a-82e7-82f0a468787c)

이제 export default와 named exports를 혼합하여 사용하고 있습니다.

Gallery.js:
Profile 구성 요소를 Profile이라는 named exports로 내보냅니다.
Gallery 구성요소를 export default로 내보냅니다.

App.js:
Profile from Gallery.js라는 named exports로 Profile을 가져옵니다.
Gallery.js에서 export default로 Gallery를 가져옵니다.
export default로 root App 구성 요소를 export합니다.
