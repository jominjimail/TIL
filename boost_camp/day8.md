# 부스트 캠프 멤버십 8일차

## week2 back-end day1

### 오늘 공부한거 

- 소켓은 한 번 연결되면 둘 중 하나가 끈지 않은이상 계속 연결된다. 하지만 http는 stateless 속성을 가지고 있기때문에 req 하고 res 보내면 완료가 되서 연결이 끈어지기 때문이다. 서버입장에서 이 connect가 누구랑 한 connect 인지 식별이 불가능하다. 그렇기 때문에 세션이라는 개념이 등장하게 된것이다.

- Node.js 웹 서버를 개발할때 가장 많이 사용하는 모듈은 express 모듈이다.

- event << net << http << express 순으로 점차 기능이 확장됐다.

- http 만으로 개발하면 되지 않나? 왜 express 가 생겼을까?

  - express 모듈을 http 모듈처럼 사용할 수 있지만 훨~씬 많은 기능이 있다. 

- http 모듈과 express 모듈로 만든 서버의 가장 큰 차이점은 무엇일까?

  - ```javascript
    var express = require('express');
    
    var app = express();
    
    app.use(function(request, response){
        response.writeHead(200, {'Content-Type' : 'text/html'});
        response.end('<h1>Hello express</h1>');
    })
    
    app.listen(52273, function(){
        console.log("server running...");
    })
    ```

  - 코드를 보면 express 모듈은 request 이벤트 리스너를 연결할때 use() 메서드를 사용한다. use() 메서드는 여러번 사용할 수 있고 매개변수로 function(request, response, next) {} 형태의 함수를 가지고 있다. next는 바로 다음에 위치한 함수를 의미한다. 

  - 요청의 응답 즉, response.end() 를 만나기전까지 요청 중간중간에 여러가지 일을 할 수 있고 use() 메서드의 매개변수로 입력하는 함수를 미들웨어 middle ware라고 부른다. 

- 미들웨어는 왜 사용해야할까?

  - ```javascript
    var express = require('express');
    
    var app = express();
    
    app.use(function(req, res, next){
        console.log('첫 번째 미들웨어');
        //데이터를 추가함
        req.number = 22;
        res.number = 273;
        next();
    })
    
    app.use(function(req, res, next){
        console.log('두 번째 미들웨어');
        req.sum = req.number + res.number;
        next();
    })
    
    app.use(function(req, res, next){
        console.log('세 번째 미들웨어');
        
        
        res.writeHead(200, {'Content-Type' : 'text/html'});
        res.send('<h1>' +req.number+ + res.number + res.sum +'</h1>');
    })
    
    app.listen(52273, function(){
        console.log("server running...");
    })
    ```

  - 첫 번째 미들웨어에서 req객체에 number 라는 속성을 추가해줬다 req나 res 객체에 속성 또는 메서드를 추가하면 다으멩 위치한 두 번째 미들웨어도 추가한 속성과 메서드를 사용할 수 있다. 

  - 즉, 미들웨어를 사용하면 특정 작업을 수행하는 모듈을 분리해서 만들 수 있다. 

  - 많이 사용되는 미들웨어

    - router : 페이지 라우터를 수행한다. 메인페이지와 (로그인&회원가입) 페이지를 분리할때 사용하면 될것같다.

    - static : 특정 폴더를 서버의 루트 폴더에 올린다. public 파일을 루트 폴더로 올릴때 사용하면 될것같다.

    - morgan : 로그 정보를 출력한다. 개발자 용으로 많이 사용된다고 한다.

    - cookie parser : 쿠키를 분해한다. 과제에서 쿠키를 사용하는데 좀 더 공부해봐야할것 같다.

      - cookie parser 미들웨어는 요청 쿠키를 추출하는 미들웨어이다. cookie parser 미들웨어를 사용하면 request 객체와 response 객체에 cookie 속성과 cookie() 메서드가 부여된다. 

      - 따로 install 해야한다. `npm install cookie-parser`

      - ```javascript
        var express = require('express');
        var cookieParser = requeire('cookie-parser');
        
        var app = express();
        app.use(cookieParser);
        
        app.get('/getCookie', function(req, res){
            //응답
            res.send(req.cookies);
        })
        
        app.get('/setCookie', function(req, res){
            //쿠키 생성
            res.cookie('string', 'cookie');
            res.cookie('json', {
                name : 'cookie',
                property : 'delicious'
            })
            
            res.redirect('/getCookie');
        })
        
        app.listen(52273, function(){
            console.log("server running...");
        })
        ```

      - cookie() 메서드의 세번째 매개변수에는 다음과 같이 옵션 객체를 입력할 수 있다.

        ```javascript
        res.cookie('string', 'cookie', {
            maxAge: 6000,
            secure: true
        })
        ```

        - httpOnly : 클라이언트의 쿠키 접근 권한을 지정한다.
        - secure : sercure 속성을 지정한다.
        - expires : expires 속성을 지정한다.
        - maxAge : 상대적으로 expires 속성을 지정한다.
        - path : path 속성을 지정한다.

    - body parser : POST 요청 매개변수를 추출한다. 회원가입 폼, 로그인 아이디 비번 입력 폼을 POST 로 보내는데 서버에서 바디파서로 해석하면 될것같다.

    - connect-multiparty : POST 요청 매개변수를 추출한다.

    - express-session : 세션 처리를 수행한다. 쿠키를 공부하고 세션도 공부해야할것 같다.

    - csurf : CSURF 보안을 수행한다.

    - error handler : 예외 처리를 수행한다. 좀 더 봐야할것 같다. 404 에러처리를 할때 사용하나?

    - limit : POST 요청의 데이터를 제한한다.

    - vhost : 가상 호스트를 설정한다.

  - 경로를 지정할 때는 대소문자를 무시하는 것이 기본 설정이다!

### 오늘 개발한거 

- week1에서 개발한 프로트 엔트 코드를 마저 리팩토링했다. 백엔드까지 더한 구조는 다음과 같다.

```
- root
| - public
    | - images
    | - javascripts
         | - page.js
         | - singlePage_router.js
    | - stylesheets
         | - main.css
         | - style.css
| - routes
    | - index.js
| - views
    | - index.html
| - app.js ( Start Point )
```



### 오늘 느낀점 

- 백엔드랑 프로트를 엮으려니 코드를 다 뒤엎어야 했다. 의존성을 분리시키는게 왜 중요하지 알것같다. 엄청 고생할것 같다.
- event << net << http << express 순으로 점차 기능이 확장됐다는게 인상깊었다. 거기에 만족하지 못하고 자꾸 자꾸 아 이것도 됐으면 좋겠는데의 결과인것 같다.
- 공식문서를 보자!