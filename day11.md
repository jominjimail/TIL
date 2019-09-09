# 부스트 캠프 멤버십 11일차 

## week2 back-end day4

### 오늘 공부한거 

- 로그아웃을 구현할때 공부했던거다. 쿠키를 설정하는법은 서버가 브라우저에게 *쿠키를 만들라* 고 set-cookies 헤더를 넣어서 response를 보낸다. 그럼 브라우저는 헤더를 보고 쿠키값을 설정한다. 예) `SESSION-ID : 1234` 그리고 만약 사용자가 로그아웃을 요청하면 가지고 있던 쿠키값을 들고 서버에게 요청을 보낸다. 서버는 `SESSION-ID` 값으로 session DB를 탐색해 일치하는 row 을 지운다. 서버쪽 정보를 지웠다면 브라우저쪽 정보도 지워야하니깐 `how to delete cookies in express` 라고 검색을 하니 내가 원하는 cookies.distory() 함수같은건 없었다. 검색을 통해 쿠키는 삭제하는것이 아니라는 결론을 얻었다. 생각해보니 브라우저 DB에 서버가 맘대로 접근해 값을 삭제하는건 이상하고 말이 안되는거였다. 그럼 어떻게 해야할까? 다행히 쿠키 option 속성중에 overwrite 를 사용하면 된다. 기본값은 true 이다. 즉, 같은 이름으로 쿠키를 다시 설정해주면 된다. 또는 expire date 를 이미 지나게 하면된다. `res.clearCookie()` 를 사용하면 된다. 

```javascript
router.get('/logout', function(req, res, next){

  const session_id = req.cookies.SESSION_ID;
  db.delete_sessionTable(session_id);
  res.cookie('SESSION_ID', session_id,{
    maxAge : 0
  })
  res.redirect('/');
})
```

- 3xx: Redirection - Further action must be taken in order to complete the request
  리다이렉션 - 요청이 완수되기 위해서 추가적인 동작이 이뤄져야 함

  - 300 : Multiple Choices
  - 301 : Moved Permanently
  - 302 : Found
  - 303 : See Other
  - **304** : Not Modified
  - 305 : Use Proxy
  - 307 : Temporary Redirect

- 304 : HTTP Get 의 특별한 타입으로 요청 메시지에 다음 필드가 있다면 HTTP Conditional Get 으로 변경한다. *※ 대부분의 브라우저는 HTTP conditional request를 사용하여 자동 캐시 기능을 지원한다.*

  - If-Modified-Since
  - If-Unmodified-Since
  - If-Match
  - If-None-Match
  - If-Range header fields

- 클라이언트가 조건부 GET 요청을 실행하고 접근이 허용되었지만 문서가 수정되지 않았다면, 서버는 304 상태코드로 응답한다. 304 응답은 메시지-바디 를 절대 포함하면 안된다. 그래서 이것은 항상 헤더 필드후에 처음 공백라인으로 종료된다.

- 예시

  - ```
    GET /sample.html HTTP/1.1
    Host: example.com
    ```

  - ```
    HTTP/1.x 200 OK
    Via: The-proxy-name
    Content-Length: 32859
    Expires: Tue, 27 Dec 2005 11:25:11 GMT
    Date: Tue, 27 Dec 2005 05:25:11 GMT
    Content-Type: text/html; charset=iso-8859-1
    Server: Apache/1.3.33 (Unix) PHP/4.3.10
    Cache-Control: max-age=21600
    Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
    Etag: “4135cda4”
    ```

  - **Cache-Control**: It tells the client the maximum time in seconds to cache the document.

  - **Last-Modified**: 문서가 마지막으로 수정된 시간

  - **Etag**: 문서를 구별할수 있는 유일한 hash 값

  - 클라이언트는 일단 서버의 응답을 받으면 문서를 캐시하고 일정시간동안 저장한다. 위와 같은 상황은 21600 초이다.

  - ```
    GET /sample.html HTTP/1.1
    Host: example.com
    If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT
    If-None-Match: “4135cda4”
    ```

  - 다음에 사용자가 같은 문서를 cache time 안에 또 요청하면 (/sample.html) 브라우저(클라이언트)는  conditional get request 를 만들것이고 서버에게 Etag 의 문서가 혹시 변경됐는지 물어볼것이다. 

  - ```
    HTTP/1.x 304 Not Modified
    Via: The-proxy-server
    Expires: Tue, 27 Dec 2005 11:25:19 GMT
    Date: Tue, 27 Dec 2005 05:25:19 GMT
    Server: Apache/1.3.33 (Unix) PHP/4.3.10
    Keep-Alive: timeout=2, max=99
    Etag: “4135cda4”
    Cache-Control: max-age=21600
    ```

  - 서버는 해당 문서가 변경됐는지 안됐는지 확인하고 만약 변경이 안됐다면 304 Not Modified header를 보내다. 

참고 : https://ruturajv.wordpress.com/2005/12/27/conditional-get-request/

### 오늘 개발한거 

- 클라이언트 쿠키에는 서버가 식별할수 있는 session-id 값만 있어야 된다고 해서 간단하게 만들었다. 

- ```javascript
  //before
  res.cookie('login_success', {
    name : 'jominji',
    id : id
  },{
    maxAge : 6000
  })
  
  //after
  res.cookie('SESSION_ID',  session_id,{
    maxAge : 10000
  })
  ```

- session DB 에는 유일하게 식별가능한 session_id 값과 이에 맵핑된 user DB의 index값이 있다. session이 생성된 시간도 추가해줬다. 

- ```json
  {
    "user_index" : 0,
    "session_id":"a099c300-d31b-11e9-82dc-73693cf35adf",
    "time_stamp":1568045122351
  }
  ```

### 오늘 느낀점 

- jest 와 supertest로 test 코드를 짜봤는데 그린이 되기 위해 어떤것을 expect 로 검증해야하는지 아직 잘 모르겟다. 흠........ 많이 해봐야 알것같다. 그저 console,log()를 찍어서 해당값이 보이는지 안보이는지 확인만 했다. 입력과 결과에 대해 명확하게 설계할 필요를 느꼈다. 

- ```javascript
  describe('로그인 성공시, 메인화면 가기전에 set-cookie가 있는지 확인하기 /login', () => {
      test('responds with json', (done) => {
          request(app)
          .post('/login')
          .send({id:'abc1234', pwd:'1234'})
          .expect(302)
          .expect('set-cookie', /SESSION_ID/)
          .then(res =>{
              //console.log(res.header);????????
              done();
          })
      })
  })
  ```

- 깃 project 칸반을 사용해봤다. 생각보다 재밌었다. 기능하나 구현할때 마다 git commti 남기는걸 자꾸 까먹었었는데 이걸 사용하니깐 좀 관리가 되는 느낌이었다. 앞으로 애용해야겠다. 