# 1. Describing the UI
React는 사용자 인터페이스(UI)를 렌더링하기 위한 JavaScript 라이브러리입니다.

UI는 버튼, 텍스트, 이미지와 같은 작은 단위로 구성됩니다. 
React를 사용하면 이를 재사용 가능하고 중첩 가능한 구성 요소로 결합할 수 있습니다. 

웹사이트부터 모바일 앱까지 화면의 모든 것은 구성 요소로 분해될 수 있습니다. 
이 장에서는 React 구성 요소를 생성, 사용자 정의 및 조건부에 따른 표시하는 방법을 배웁니다.

## [첫 번째 컴포넌트(구성 요소)]
React 애플리케이션은 구성 요소라고 하는 독립된 UI 조각 컴포넌트로 구축됩니다. 

React 컴포넌트는 마크업을 뿌릴 수 있는 JavaScript 함수입니다. 
구성 요소는 버튼만큼 작을 수도 있고 전체 페이지만큼 클 수도 있습니다. 
다음은 세 가지 프로필 구성 요소를 렌더링하는 갤러리 구성 요소입니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/84b4ce05-db5f-43c7-b849-15b3d81746e8)

## [구성요소 가져오기(import) 및 내보내기(export)]
하나의 파일에 많은 구성 요소를 선언할 수 있지만 큰 파일은 탐색하기 어려울 수 있습니다. 
이 문제를 해결하려면 구성 요소를 자체 파일로 내보낸 다음 해당 구성 요소를 다른 파일에서 가져와 사용할 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/f9b7395d-e2f0-4a22-be4d-42df0c017f62)

## [JSX로 마크업 작성]
각 React 구성 요소는 React가 브라우저에 렌더링하는 일부 마크업을 포함할 수 있는 JavaScript 함수입니다. 

React 구성 요소는 JSX라는 구문 확장을 사용하여 해당 마크업을 나타냅니다. 
JSX는 HTML과 매우 비슷해 보이지만 조금 더 엄격하고 동적 정보를 표시할 수 있습니다.

기존 HTML 마크업을 React 구성 요소에 붙여 넣으면 항상 작동하지는 않습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/66903923-fc82-4b56-a07a-a3e21b5afd26)
* JSX 에서는 "class" 속성 지정을 "className" 으로 사용

## [중괄호를 사용하는 JSX방식의 JavaScript]
JSX를 사용하면 JavaScript 파일 내에 HTML과 같은 마크업을 작성하여 
렌더링 논리와 콘텐츠를 동일한 위치에 유지할 수 있습니다. 

때로는 약간의 JavaScript 논리를 추가하거나 해당 마크업 내에서 동적 속성을 참조하고 싶을 수도 있습니다. 
이 상황에서는 JSX에서 중괄호를 사용하여  JavaScript로 "escape back"하여 코드의 일부 변수를 포함하고 사용자에게 표시할 수 있습니다
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/3430123b-4f5e-474e-b505-db4d120e253f)

## [props을 구성 요소에 전달하기]
React 구성 요소는 props를 사용하여 서로 통신합니다. 
모든 상위 구성 요소는 props을 제공하여 하위 구성 요소에 일부 정보를 전달할 수 있습니다. 

Props는 HTML 속성을 연상시킬 수 있지만 
이를 통해 객체, 배열, 함수, JSX를 포함한 모든 JavaScript 값을 전달할 수 있습니다
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/2c5ac019-a8d8-4e16-a80e-bade78d45505)

## [구성 요소를 pure하게 유지(정적 버전)]
일부 JavaScript 함수의 본래 정적 동작은 이렇습니다. 
본래의 기능:
	- 자신의 일을 상기합니다. 호출되기 전에 존재했던 객체나 변수는 변경되지 않습니다.
	- 동일한 입력, 동일한 출력. 동일한 입력이 주어지면 순수 함수는 항상 동일한 결과를 반환해야 합니다.
	
구성 요소를 정적인 성격의 함수로만 작성하면 코드 베이스가 커짐에 따라 당황스러운 버그와 예측할 수 없는 동작을 방지할 수 있습니다. 

다음 예시는 동적인 구성 요소의 예입니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/94cb72c4-8731-412d-810d-66929137ede9)

기존 변수를 수정하는 대신 prop을 전달하여 이 구성 요소를 pure하게 만들 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/04f7ac7f-88ce-4695-be12-b86ce8118d69)

## [리스트 UI 렌더링]
데이터 컬렉션에서 여러 유사한 구성 요소를 표시하려는 경우가 많습니다. 
React와 함께 JavaScript의 filter() 및 map()을 사용하여 데이터 배열을 필터링하고 구성 요소 배열로 변환할 수 있습니다.

각 배열 항목에 대해 key를 지정해야 합니다. 
일반적으로 데이터베이스의 ID를 키로 사용하려고 합니다. 

key를 사용하면 목록이 변경되더라도 React가 목록에서 각 항목의 위치를 ​​추적할 수 있습니다.
![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/3f89adc2-1dab-4a9d-8dd8-1d366609d9ec)

## [UI를 트리구조로 표현]
React는 트리를 사용하여 구성 요소와 모듈 간의 관계를 모델링합니다.

React 렌더링 트리는 구성 요소 간의 상위 및 하위 관계를 표현합니다.![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/2fe8dafe-8e4c-4257-9eaf-7953c25424ad)

트리 상위 , 루트 구성 요소 근처에 있는 구성 요소는 최상위 구성 요소로 간주됩니다. 
하위 구성 요소가 없는 구성 요소는 leaf 구성 요소로 판단합니다. 

이러한 구성 요소 분류는 데이터 흐름 및 렌더링 성능을 이해하는 데 유용합니다.

JavaScript 모듈 간의 관계를 모델링하는 것은 앱을 이해하는 또 다른 유용한 방법입니다. 
우리는 이를 모듈 종속성 트리라고 부릅니다.


종속성 트리는 클라이언트가 다운로드하고 렌더링할 모든 관련 JavaScript 코드를 묶기 위해 
빌드 도구에서 자주 사용됩니다. 

번들 크기가 크면 React 앱의 사용성이 저하됩니다. 
모듈 종속성 트리를 이해하면 이러한 문제를 디버깅하는 데 도움이 됩니다.![image](https://github.com/ektto1041/react-dev-translation-study/assets/165557124/bc330bd1-befd-4209-a9b1-46b163a75e29)

