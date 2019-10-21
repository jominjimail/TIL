# 부스트 캠프 멤버십 35일차 

## week8 front-end day1

### 오늘 공부한거 

react tutorial : https://ko.reactjs.org/tutorial/tutorial.html

인상 깊었던말

승자확인을 위해 9개의 사각형의 값을 한 곳에 유지할 것이다. 

- Board가 각각의 Square에게 state를 요청한다.

> 이 방식은 코드를 이해하기 어렵게 만들고 버그에 취약하며 리팩토링이 어렵기 때문에 추천하지 않습니다.

- 자식이 아닌 부모에 state를 저장하는게 좋을 것 같다. 여기서 state를 부모 컴포넌트로 끌어 올린다는 표현을 사용하는데 아직 감이 잘 안온다. 부모로 state를 끌어 올려서 자식이 더이상 state를 유지하지 않으면 자식 컴포넌트는 **제어되는 컴포넌트** 라고 불린다. 

> **여러개의 자식으로부터 데이터를 모으거나 두 개의 자식 컴포넌트들이 서로 통신하게 하려면 부모 컴포넌트에 공유 state를 정의해야 합니다. 부모 컴포넌트는 props를 사용하여 자식 컴포넌트에 state를 다시 전달할 수 있습니다. 이것은 자식 컴포넌트들이 서로 또는 부모 컴포넌트와 동기화 하도록 만듭니다.**



#### 불변성이 왜 중요할까?

일반적으로 데이터를 변경하는 방법은 2가지가 있다. 

1. 데이터의 값을 직접 변경하기
2. 원하는 변경값을 가진 새로운 사본으로 데이터를 교체하기

```javascript
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// 이제 player는 {score: 2, name: 'Jeff'}입니다.
```

```javascript
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// 이제 player는 변하지 않았지만 newPlayer는 {score: 2, name: 'Jeff'}입니다.

// 만약 객체 spread 구문을 사용한다면 이렇게 쓸 수 있습니다.
// var newPlayer = {...player, score: 2};
```

둘 다 최종 결과값은 같다. 하지만, 2번 방법으로 하면 아래와 같은 이점을 얻을 수 있다.

- 직접적인 데이터 변형을 피해므로써 이전의 내역을 가지고 **실행 취소** 와 같은 멋진 일을 할 수 있다.
- 이전의 사본과 지금의 데이터를 비교해 **변화를 쉽게 감지**할 수 있다. 
- React 에서는 이전의 사본과 지금의 데이터를 비교해 **렌더링 하는 시기를 결정**한다. *왠지 이게 리엑트의 핵심인것 같다.*

#### 실수 하기 쉬운거

> Square를 함수 컴포넌트로 수정했을 때 `onClick={() => this.props.onClick()}`을 `onClick={props.onClick}`로 간결하게 작성했습니다. *양쪽* 모두 괄호가 사라진 것에 주목해주세요.

> [JavaScript 클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)에서 하위 클래스의 생성자를 정의할 때 항상 `super`를 호출해야합니다. 모든 React 컴포넌트 클래스는 `생성자`를 가질 때 `super(props)` 호출 구문부터 작성해야 합니다.

> `onClick={() => alert('click')}`이 어떻게 동작하는지 살펴보면 `onClick` prop으로 *함수*를 전달하고 있습니다. React는 클릭했을 때에만 이 함수를 호출할 것입니다. `() =>`을 잊어버리고 `onClick={alert('click')}`이라고 작성하는 것은 자주 발생하는 실수이며 컴포넌트가 다시 렌더링할 때마다 경고 창을 띄울 것입니다.



### 오늘 개발한거 

### 오늘 느낀점 

아 어려브라

데이터 사본을 사용하는 이유를 이제 이해한것 같다 하하