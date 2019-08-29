# 부스트 캠프 멤버십 4일차 

## week1 front-end day2



### 오늘 공부한거 

- `DOM` : 돔은 web에 올라온 document를 나타내는 수단이다. 브라우저는 web page가 로드될 때 해당 페이지의 DOM을 만든다. 

- `왜?` 브라우저가 해당 page의 content들을 자유롭게 조작하기 위해서이다. 

- `어떤 조작?` DOM은 계층을 가진 Tree 형태이고 최상위에는 Document 가 있다. `document.[접근하고픈 요소]` 식으로 하위 구성 요소에 접근이 가능하다. 즉, JS로 HTML element를 수정할 수 있다는 것이다. CSS style도 수정할 수 있고 지워버릴수도 있다. HTML element 추가 또한 가능하며 HTML event 생성및 react도 가능하다. *Static* 한 HTML page가 *Dynamic* 해질 수 있다.  



### 오늘 개발한거 

- 회원가입 HTML 만들기 : 네이버 회원가입 페이지를 참고하면서 HTML 파일을 만들었다. 초기에는 아래와 같이 단순히 tag들을 나열해서 만들었다. 디자인은 별로지만 내용은 있는 그런 HTML 이었다. CSS를 적용시키기에는 각 요소가 너무 따로 놀고 있었고 왠지 모르게 계층 구조로 잘 구현해보고 싶었다. ( `DOM` 을 공부하니 왜 그렇게 해야하는지 조금 알 것 같다.) 코드를 `div` 로 잘 감싸서 구현하니 CSS를 효율적으로 적용시킬수 있어서 좋았던것 같다. 

```html
<!--before-->
<h3>아이디</h3>
<input type="text" id="id" maxlength="20"/>

<!--after-->
<div id="content">
    <div class="join_content">
        <div class="join_row">
            <h3 class="join_title">
                <label for="id">아이디</label>
            </h3>
            <span class="ps_box">
                <input type="text" id="id" name="id" class="int" title="ID" maxlength="20">
            </span>
        </div>
    </div>
</div>
```



### 오늘 느낀점 

- 아, CSS는 정말 별게 다 있는것 같다. 
- 잘 만들어진 회원가입 페이지를 모티브로 만든건 잘한것 같다.
- `왜 굳이 span tag를 사용한거지? 그냥 input tag사용하면 안되나?` 라는 끝도 없는 불평을 가지긴 했지만 다 이유가 있던것이었다. 내가 만약 `<span class="ps_box">` 를 사용하지 않고 그냥 input tag를 갖다 박았다면 아름다운 입력칸을 만들수 있었을까?  
- class는 CSS를 한번에 짠! 적용시키기 위해 (중복된 코드를 줄이기 위해) 꼭 필요한 요소인것 같다. 
- `div` 로 HTML content 를 계층구조로 아름답게 쌓으면 왠지 브라우저가 좋아할거 같다. 

