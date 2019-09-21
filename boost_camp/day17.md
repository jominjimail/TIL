# 부스트 캠프 멤버십 17일차 

## week3 front-end day5

### 오늘 공부한거 & 오늘 개발한거 

front end 를 개발할때 index.html, index.js 는 패턴이 있는 것 같다. 

```html
<!DOCTYPE html>
<html>
  <head></head>
  <body>
      <div id="root"></div>
  </body>
</html>
```

Index.html 에는 <body> 안에 오직 한 개의 <div>  태크만 있다. 

```javascript
import attachMiniCarouselTo from './mini_carousel.js';

const root = document.querySelector('#root');

attachMiniCarouselTo(root);
```

index.js 에는 querySeletor 로 html의 root 를 찾아 다음 함수로 넘겨준다. 

```javascript
export default function attachMainCarouselTo(parent) {
  const miniCarouselRoot = document.createElement('div');
  parent.appendChild(miniCarouselRoot);

  const carousel = new Carousel(
    miniCarouselRoot,
    MINI_CAROUSEL.WIDTH,
    MINI_CAROUSEL.HEIGHT
  )
    ...
}
```

다음 함수인 attachMiniCarouselTo() 를 살펴보면 `miniCarouselRoot` <div> 태그를 새로 만들어 `parent` 파라미터로 받은 root <div> 밑으로 append 한다. `miniCarouselRoot` 는 Carousel 생성자의 인자로 들어간다.

```javascript
export class Carousel {
  constructor(parent, width = 3, height = 3) {
    this.parent = parent;
    ...
    this.self = document.createElement('div');
    this.self.classList.add(CLASS_NAME.CAROUSEL_CONTAINER); //carousel
    ...
    this.cardContainer = document.createElement('div');
    this.cardContainer.classList.add(CLASS_NAME.CARD_CONTAINER); //circular-queue
    ...
    this.self.appendChild(this.cardContainer);
    this.parent.appendChild(this.self);
  }	
}
```

Carousel 클래스의 생성자를 살펴보면 `this.self` <div> 태그와 `this.cardContainer` <div> 태그를 새로 만들어 `this.cardContainer` 을 `this.self` 밑으로 append 하고 parent 파라미터로 받은 `miniCarouselRoot` <div> 밑으로 `this.self` 를 append 한다. 

```html
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <div id="root">
      <div>
        <div class="carousel" >
          <div class="circular-queue"></div>
        </div>
      </div>
    </div>
  </body>
</html>
```

parnet 를 넘겨받아 필요한 tag를 생성해 append 하고 그 안에 추가할게 있다면 다시 parent 로 넘겨준다. 이렇게 하면 querySeletor를 한번만 호출한다. js로 html 을 만든다는게 이런거 같다. 이렇게 짜니깐 코드가 간결하고 읽기 쉬웠다. 

### 오늘 느낀점 

carousel version2 를 만들어 보았다. ss21님과 ss07님의 코드를 참고했다. 두분의 공통점은 `오늘 공부한거` 의 패턴이었다. top-down 방식으로 개발하는 느낌이었다. 확실히 이렇게 하니깐 부모가 무슨일을 해야하는지 자식들에게 어떤 정보를 넘겨줘야하는지 부모 자식관계가 뚜렷해서 좋았다. 