# 부스트 캠프 멤버십 15일차 

## week3 front-end day3

### 오늘 공부한거 

#### export

```javascript
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
export const MODULES_BECAME_STANDARD_YEAR = 2015;
export class User {
  constructor(name) {
    this.name = name;
  }
} // no ; at the end
```

주의 : class또는 function을 export 할 때 마지막에 **semicolons** 을 붙이지 않는다.



#### Export apart from declarations

```javascript
// say.js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

export {sayHi, sayBye}; // a list of exported variables
```



#### import

```javascript
import {sayHi, sayBye} from './say.js';

sayHi('John'); // Hello, John!
sayBye('John'); // Bye, John!
```



#### import *

```javascript
import * as say from './say.js';

say.sayHi('John');
say.sayBye('John');
```

`*` 을 사용해 say.js 에서 export 한 모든걸 가져올 수 있다.

그럼 그냥 `*` 로 다 써버리면 되지 않을까? 왜 명시적으로 { 리스트안에 } import하는게 필요할까?

- webpack같은 build tool을 살펴보면 (자동으로 export, import 해주는듯) 로딩 속도를 최적화하기 위해 사용하지 않은건 지워버린다. 이런걸 tree-shacking 이라고 한다. 

  > Then the optimizer will see that and remove the other functions from the bundled code, thus making the build smaller. That is called “tree-shaking”.

- { sayHi } 라고 리스트안에 명시적으로 표현하면 say.sayHi() 대신 sayHi() 로 바로 사용할 수 있다.

- 가독성이 좋다. 어떤 함수를 가져다 사용하는지 쉽게 알 수 있다.

#### as

```javascript
import {sayHi as hi, sayBye as bye} from './say.js';

hi('John'); // Hello, John!
bye('John'); // Bye, John!
```

as로 별명을 붙여줄 수 있다.

```javascript
export {sayHi as hi, sayBye as bye};
```

export 할 때도 as로 별명을 붙여줄 수 있다.



------



#### export default

가만히 살펴보면 2가지 타입의 모듈이 있다. 

- Module that contains a library, pack of functions
- Module that declares a single entity

*대게 2번째 타입을 선호한다. single entity를 하게되면 파일수가 많아지긴 하지만... 그건 문제가 되지 않는다고 한다! 파일명만 잘 주면 훨씬 더 읽기 쉬워진다고 한다! 아하!*

**export default == one thing per module**

```javascript
export default class User { // just add "default"
  constructor(name) {
    this.name = name;
  }
}
```

각 파일에는 한 개의 export default 만 있어야 한다.

```javascript
import User from './user.js'; // not {User}, just User
new User('John');
```

그럼 { 리스트 } 를 사용할 필요 없이 바로 갖다 사용할 수 있다.



### 오늘 개발한거 

```javascript
// index.js
import { _$, } from './util.js'
import attachMiniCarouselTo from './mini_carousel.js';

const root = _$('#root');

attachMiniCarouselTo(root);
```

index.js 파일은 attachMiniCarouselTo 를 import 받았다. {} 가 없는거 보니 export default 인 것 같다.

```javascript
// mini_carousel.js
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
  
function attachCarouselButton(parent , carousel){
  ...
}
  
function rotateRightInterval(carousel) {
	...
}
```

mini_carousel.js 파일을 보니 attachMainCarouselTo() 함수 앞에만 `export default` 키워드가 있고 나머지 함수는 없다.

### 오늘 느낀점 

export 와 export default 를 뒤엉켜서 여태까지 개발했던것 같다... 진작 공부할걸 생각이 들었다. webpack 같은 번들 툴은 export 와 import 를 알아서 최적화 해준다는걸 알았다. 너무 신기했다. 

캠퍼들 코드를 보면 index.html에 script 와 css 를 이어주는 `<script>` , `<link>` 태그가 없던데 webpack이 이런것도 알아서 해주나? 나중에 한 번 써봐야겠다. 