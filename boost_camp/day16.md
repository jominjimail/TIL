# 부스트 캠프 멤버십 16일차 

## week3 front-end day4

### 오늘 공부한거 

### SASS

> [Sass(Syntactically Awesome StyleSheets)](http://sass-lang.com/)는 CSS pre-processor로서 CSS의 한계와 단점을 보완하여 보다 가독성이 높고 코드의 재사용에 유리한 CSS를 생성하기 위한 CSS의 확장(extension)이다.

- pre-processor : 일종의 전처리 프로그램이라고 생각하면 된다. 전처리를 통해 얻은 output은 또 다른 프로그램의 input 이 될 수 있다. 
- CSS의 한계 : 프로젝트의 규모가 커지고 수정이 빈번히 발생함에 따라 쉽게 지저분해지고 유지보수하기 어렵다. 

CSS의 한계를 보완하기 위해 SASS는 아래와 같은 추가기능을 제공한다.

- 변수의 사용
- 조건문과 반복문
- Import
- Nesting
- Mixin
- Extend/Inheritance

#### 설치

브라우저는 SASS 문법을 모른다. 그렇기 때문에 Sass(.scss) 파일을 Css(.css) 파일로 컴파일 해줘야한다. 전처리 프로그램이라는 말이 와닿는 부분이다. 컴파일을 하기 위해선 컴파일 환경이 필요하다. Node.js 는 LibSass를 사용하는데 node-sass를 설치해주면 된다. 

```shell
$npm install -g node-sass
```



#### 파일 세팅

*express-generator 프레임워크를 사용했다.*

 root 에 `sass_project` 디렉토리를 만들어주고 그 안에 .scss 파일을 추가해줬다.

```
.
...
├── public // front-end 로직 
|   ├── javascripts
|   └── stylesheets
|       ├── benefit_carousel.css
|       ├── benefit.css
|       ├── carousel.css
|       ├── layout.css
|       └── mini_carousel.css
├── sass_project
|   ├── benefit_carousel.scss
|   ├── benefit.scss
|   ├── carousel.scss
|   ├── layout.scss
|   └── mini_carousel.scss
...
└── app.js // entry point
```



#### 컴파일 하는 방법

```shell
# 특정 파일을 특정 파일 이름으로 컴파일
$cd sass_project
$node-sass benefit.scss > benefit.css

# 폴더 내의 모든 파일을 컴파일
$cd ..
$node-sass sass_project --output public/stylesheets
```



#### 다양한 컴파일 스타일

```shell
# nested : sass 형식과 유사하게 nested된 css 파일이 생성된다. 기본값으로 옵션을 추가하지 않아도 기본 적용된다.
$node-sass --output-style nested sass_project --output public/stylesheets

# expanded : 표준적인 스타일의 css 파일이 생성된다.
$node-sass --output-style expanded sass_project --output public/stylesheets

# compact : 여러 룰셋을 한줄로 나타내는 스타일의 css 파일이 생성된다.
$node-sass --output-style compact sass_project --output public/stylesheets

# compressed : 가능한 빈공간이 없는 압축된 스타일의 css 파일이 생성된다.
$node-sass --output-style compressed sass_project --output public/stylesheets
```



#### watch

매번 수정할때마다 컴파일하기 귀찮으니깐 SCSS파일의 변경을 감지하여 변경될 때마다 자동으로 SCSS파일을 컴파일해 CSS파일을 업데이트한다. 모니터링은 디렉토리 또는 파일 단위로 가능하다. 

```shell
# 파일 단위 watch
$cd sass_project
$node-sass --watch benefit.scss > benefit.css

# 디렉토리 단위 watch
$cd ..
$node-sass --watch sass_project --output public/stylesheets
```



### 오늘 개발한거 

#### 기준

```scss
// benefit_carousel.scss
.prime-benefit .scroller-container{
  display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-right : 20px;
    width : 50%;
    .a-benefit-carousel{
        width : 360px;
        height: 220px;
        display: flex;
        // border: 1px dotted red;
        justify-content: space-between;
        .carousel-window{
            width: 280px;
            height: 210px;
            .carousel-item-container .carousel-item a img{
                width: 280px;
                height: 210px;
            }
        }
    }
}
```

#### nested

```css
.prime-benefit .scroller-container {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-right: 20px;
  width: 50%; }
  .prime-benefit .scroller-container .a-benefit-carousel {
    width: 360px;
    height: 220px;
    display: flex;
    justify-content: space-between; }
    .prime-benefit .scroller-container .a-benefit-carousel .carousel-window {
      width: 280px;
      height: 210px; }
      .prime-benefit .scroller-container .a-benefit-carousel .carousel-window .carousel-item-container .carousel-item a img {
        width: 280px;
        height: 210px; }

```

#### expanded

```css
.prime-benefit .scroller-container {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-right: 20px;
  width: 50%;
}

.prime-benefit .scroller-container .a-benefit-carousel {
  width: 360px;
  height: 220px;
  display: flex;
  justify-content: space-between;
}

.prime-benefit .scroller-container .a-benefit-carousel .carousel-window {
  width: 280px;
  height: 210px;
}

.prime-benefit .scroller-container .a-benefit-carousel .carousel-window .carousel-item-container .carousel-item a img {
  width: 280px;
  height: 210px;
}
```

#### compact

```css
.prime-benefit .scroller-container { display: flex; align-items: center; justify-content: flex-end; margin-right: 20px; width: 50%; }

.prime-benefit .scroller-container .a-benefit-carousel { width: 360px; height: 220px; display: flex; justify-content: space-between; }

.prime-benefit .scroller-container .a-benefit-carousel .carousel-window { width: 280px; height: 210px; }

.prime-benefit .scroller-container .a-benefit-carousel .carousel-window .carousel-item-container .carousel-item a img { width: 280px; height: 210px; }

```

#### compressed

```css
.prime-benefit .scroller-container{display:flex;align-items:center;justify-content:flex-end;margin-right:20px;width:50%}.prime-benefit .scroller-container .a-benefit-carousel{width:360px;height:220px;display:flex;justify-content:space-between}.prime-benefit .scroller-container .a-benefit-carousel .carousel-window{width:280px;height:210px}.prime-benefit .scroller-container .a-benefit-carousel .carousel-window .carousel-item-container .carousel-item a img{width:280px;height:210px}
```



### 오늘 느낀점 

- 오... 크롬 개발자 도구에서 봤던 css 한 줄 뭉탱이의 정체를 알았다. 이런식으로 다 전처리하는구나
- 가독성은 떨어지지만 브라우져가 좋아할 것 같다. 
- cs에서 pre-processor 는 `프로그램`이라는 것과 `다음 프로그램`의 input을 미리 가공하는것이 전처리임을 알았다. 의미가 애매했었는데 확실히 알았다.
- sass를 제대로 사용하고 있는것 같지는 않지만 확실히 파일을 나누니 관리하기는 용이했다. 