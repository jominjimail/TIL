# 부스트 캠프 멤버십 10일차 

## week2 back-end day3

### 오늘 공부한거 

- 쿠키는 사용자를 *식별*하고 *세션을 유지*하는 방식중에서 현재까지 가장 널리 사용하는 방식이다. 쿠키는 **서버가** 사용자에게 `안녕 내 이름은 …` 라고 적어서 붙이는 스티커와 같다. 사용자가 웹 사이트를 방문하면, 웹사이트는 서버가 사용자에게 붙인 모든 스티커를 읽을 수 있다. 서버가 사용자 추적 용도로 생성한 유일한 단순 식별 번호만 포함된다. 서버는 이 쿠키값으로 DB에서 사용자의 정보를 찾는데 사용할수 있다.

- 브라우저는 서버에서 온 헤더에 있는 쿠키 콘텐츠를 *브라우저 쿠키 DB*에 저장한다. 쿠키의 기본적인 발상은 브라우저가 서버관련정보를 저장하고 사용자가 해당 서버에 접근할때마다 그 정보를 함께 전송하게 하는것이다. 브라우저는 쿠키 정보를 저장할 책임이 있응데 이 시스템을 **클라이언트 측 상태(HTTP State Management Mechanism)** 라고 한다.

- 각 브라우저는 각기 다른 방식으로 쿠키를 저장한다. 구글 크롬은 Cookies라는 SQLite파일에 쿠키를 저장한다. 

- 쿠키 도메인 속성 : 서버는 쿠키를 생성할 때 Set-Cookie 응답헤더에 Domain속성을 기술해서 어떤 사이트가 그 쿠키를 읽을수 있는지 제어할 수 있다. 예를들어 acme.com으로 도메인 속성을 설정했다면, anril.acme.com,shipping.acme.com 에 접속했을때 읽을 수 있고   www.cnn.com 에서는 해당 쿠키를 읽을 수 없다.

- 쿠키 경로 속성 : 웹 사이트 일부에만 쿠키를 적용할 수 도 있다. 해당 경로에 속하는 페이지에만 쿠키를 전달한다. 예를들어 www.air.com 이란 메인 페이지가 있고 www.air.com/reservation 이란 예약 페이지가 있다하자.path 설정을 /reservation/으로 하면 www.air.com/reservation/check/list.html 에서는 쿠키를 읽을 수 있고  www.air.com/shop 에서는 쿠키를 읽을 수 없다. 

- Set-Cookie : name=value [ ; expires=date ] [ ; path=path ] [ ; domain=domain ] [; secure ] 

  - expires는 파기날짜이다 명시되어 있지 않으면 기본값으로 세션이 끝날때까지 유지된다.
  - path의 기본값은 응답을 전달한 URL이다.
  - domain의 기본값은 응답을 생성한 서버의 root 명이다. 

- 클라이언트가 서버에 요청을 보낼때는 Domain, path, secure 필터들이 현재 요청 하려고 하는 사이트에 들어맞으면서 && 아직 파기되지 않은 쿠키들을 함께 보낸다. 모든 쿠키는 Cookie 헤더에 한데 이어 붙어 보낸다. 

  

### 오늘 개발한거 

- http 통신할때 cookie 속성을 추가해주기위해 cookie-parser 모듈을 설치해 사용한다.

```javascript
var cookieParser = require('cookie-parser');

app.use(cookieParser());
```

- 전날 `사용자가 메인페이지에 들어와 로그인버튼을 클릭해 아이디와 비밀번호를 입력하면 서버가 회원인지 아닌지 판단해 각각 다른 결과창을 띄워준다` 을 개발했는데 이를 이어 로그인 성공시 쿠키값을 설정해 브라우저로 보내줬다. 하지만, 이렇게 하면 안된다고 한다. 쿠키값은 탈취가 가능하므로 (언제든지 맘만 먹으면) 사용자의 id와 같이 의미있는 값을 담아 전달하면 안된다고 한다. 보통 로그인을 성공하면 서버가 session_id 라는 고유값을 만든다. 서버는 session_id와 실제 사용자의 id 및 필요한 다른 정보를 매핑한 session DB를 관리하고 있고 브라우저에게는 이 session_id 값만 넘겨주는것이다. 이 값은 중간에 탈취해도 상관없다. 오직 이 서버에 특화된 값이기 때문이다. (과연 그러한가..? 안될거 같은데 머리가 아파온다….. session_id 탈취하면 되는건 아닌가? ) 이런저런 이유로 일단 id 쌩값을 넘겨주는것보단 한번 더 추상된 값을 넘기는게 더 안전할것 같다. 아래 코드는 개선될 것이다.

```javascript
router.post('/login', function(req, res, next){
  let id = req.body.id;
  let pwd = req.body.pwd;
  let db_query_result = db.select_userTable(id, pwd);
  if(db_query_result == true){
    res.cookie('login_success', {
      name : 'jominji',
      id : id
    },{
      maxAge : 6000
    })
    res.redirect('/');
  }else{
    // 메세지 문자열 나중에 다 뺴기 
  }
})
```



### 오늘 느낀점 

- 쿠키가 왜 필요한지 어떤 속성을 먹일수 있는지 알아봤다. 공부하길 잘한 것 같다. 나중에 서비스의 규모가 더 커져  쿠키때문에 성능저하가 일어난다면 path 와 domain 설정을 신경써야할것 같다. 
- 풀리퀘를 못 날렸다. 정신을 어디다 두고 다니는건지 모르겠다. ㅜㅜ