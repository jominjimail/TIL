# 부스트 캠프 멤버십 13일차 

## week3 day1

### 오늘 공부한거 

### Events

- 브라우저가 너무 많은 이벤트 핸들러를 기억하고 있는것은 메모리 낭비이다.
- 만약 ul 안에 li가 100개 있고 각 li마다 event를 달아줘야 한다면? for문을 100번 돌려 100개의 이벤트 핸들러를 만들면 된다. 하지만, 이건 굉장히 비효율적이다.
- 그럼 어떻게 해야할까?
- 해결책을 말하기 전에 이벤트가 브라우저에서 어떻게 동작하는지 알아보자.
- 버블링, 캡처링 2가지 방식으로 동작한다.
- 이 둘은 기본적으로 전파라는 속성을 가지고 있고 차이점은 시작점과 도착점이다. 
- 예를들어 ul > li*3 > img 가 있다고 하자. ul, li, img 각각 동일한 click 이벤트를 등록한 상태이다.
- img를 클릭하면 어떻게 될까? 전파라는 말을 잘 이해해보자
- img -> li -> ul 순으로 모든 이벤트가 트리거 된다. *(브라우저는 기본적으로 버블링 방식을 사용한다)*
- **버블링** : 브라우저는 가장 외각에 있는 HTML element 에 해당 이벤트가 등록되어 있는지 찾아본다. *(있다면 실행 없다면 패스)* 그리고 HTML element 안에 있는 element 로 이동해 해당 이벤트가 등록되어 있는지 찾아본다. 실제로 클릭된 element 를 만날 때까지 반복한다.
- **캡쳐링** : 브라우저는 실제 클릭된 element 가 해당 이벤트가 등록되어 있는지 찾아본다. *(있다면 실행 없다면 패스)* 그리고 next immediate ancestor element 로 이동해 해당 이벤트가 등록되어 있는지 찾아본다. HTML element 를 만날 때까지 반복한다. 
- **해결책** : `event delegation` 이벤트를 임명하는것이다. ul > li*3 > img 가 있다면 ul 에 1개의 이벤트 핸들러를 등록하는것이다. 만약 img 가 클릭됐다면? 버블링 방식에 따라 img (pass) -> li (pass) -> ul (trigger!) 가 될 것이다. li 가 클릭됐다면 li (pass) -> ul (trigger!) 가 된다. 이런식으로 부모 element 에 이벤트를 달아 이벤트를 발생시키는 방식을 이벤트 임명라고 한다. 개개인별로 100개의 event 를 등록하는것보다 부모에 1개의 event 를 등록하는게 더 효율적이다라고 말할 수 있다. stopPropagation() 등을 이용해 적절히 잘 이용하자. 



### 이벤트를 등록하는 방법1

- ```javascript
  var btn = document.querySelector('button');
  
  btn.onclick = function() {
    var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
    document.body.style.backgroundColor = rndCol;
  }
  ```

- 기본적으로 html 과 js 코드는 분리하는게 인지상정이다.

### 이벤트를 등록하는 방법2

- ```javascript
  var btn = document.querySelector('button');
  
  function bgChange() {
    var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
    document.body.style.backgroundColor = rndCol;
  }   
  
  btn.addEventListener('click', bgChange);
  ```

- DOM level 2 event 라는데 이걸 많이 사용한다. 위에 방식보다 좋은점은 상호보완함수인 removeEventListenr() 란 함수가 존재하고 powerful 한 옵션들이 존재하다는 것이다.

- ```javascript
  btn.removeEventListener('click', bgChange);
  ```

- 2개의 파라미터가 필요하다. 이벤트의 이름과 해당 이벤트의 핸들러 이름이다.

```javascript
function bgChange(evt) {
  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  evt.target.style.backgroundColor = rndCol;
  console.log(evt);
}  

btn.addEventListener('click', bgChange);
```

- 핸드러 함수의 파라미터로 event, evt, e 이름으로 많이 사용되는 event object가 있다.

```javascript
btn.addEventListener("click",function(evt) {
    console.log(evt.currentTarget.tagName, evt.target.tagName);
});
```

- evt.target : 이벤트를 트리거 시킨 장본인! Get the element that triggered a specific event . The target event property returns the element that triggered the event.
- evt.currentTarget : 트리거된 이벤트가 등록된 element! Get the element whose event listeners triggered a specific event

> The currentTarget property always refers to the element whose event listener triggered the event, opposed to the [target](https://www.w3schools.com/jsref/event_target.asp) property, which returns the element that triggered the event.
>
> The target property gets the element on which the event originally occurred, opposed to the [currentTarget](https://www.w3schools.com/jsref/event_currenttarget.asp) property, which always refers to the element whose event listener triggered the event.

### 오늘 개발한거 

- 없다.

### 오늘 느낀점 

- 내일이 걱정된다. 공부할게 너무 산더미더미다.

