# 부스트 캠프 멤버십 9일차 

## week2 back-end day2

### 오늘 공부한거 

*참고*

[http response status codes](<https://www.restapitutorial.com/httpstatuscodes.html>)

[유의어 사전집](https://www.thesaurus.com/)

- *읽기 좋은 코드가 좋은코드다* 한빛 미디어 책을 읽었다. week1 마스터 코드리뷰 시간에 마스터님이 조언해준대로 네이밍에 대해 깊이 고민해봐야할것 같아 읽어봤다. 그 중 part1 표면적 수준에서의 개선 부분을 읽고 정리할 계획이다. 

- **가독성의 기본 정리** 은 이 책의 핵심 아이디어이다. 코드는 다른 사람이 그것을 이해하는 데 들이는 시간을 최소화하는 방식으로 작성되어야 한다. 여기서 *이해*는 다른이가 코드를 자유롭게 수정하고, 버그를 짚어내고, 수정된 내용이 다른 부분의 코드와 어떻게 상호작용하는지 알 수 있어야 하는 굉장히 높은 수준이다.

- 상상속에 존재하는 다른 어떤사람이 내 코드를 본다고 계속 생각하라.

- 표면적인 수준에서 개선이란 좋은 이름을 짓고, 좋은 설명을 달고, 코드를 보기 좋게 정렬하는것을 말한다.

- 단어에게 시비를 걸어보자.

  - getPage(url) : 로컬 캐스, 데이터 베이스, 인터넷 중 어디에서 페이지를 가져오는건가? 만약 인터넷에서 가져오는거라면 FetchPage(), DownloadPage() 가 더 의미있는 이름이 될것이다.
  - int Size() :  class BinaryTree{} 안에 저런 함수가 있다고 하자. 트리의 높이를 반환하는건가? 노드의개수? 아니면 트리의 메모리 사용량? Height(), NumNodes(), MemoryBytes() 등이 더 의미 있는 이름이 될것이다.
  - void Stop() : class Thread{} 안에 저런 함수가 있다고 하자. 다시는 되돌릴 수 없는 최종 동작을 수행한다면 Kill()이 더 확실한 의미를 전달할 것이다. 만약 Resume()을 호출해서 다시 돌이킬 수 있는 동작이라면 Pause() 가 더 좋을것 같다.

- send : deliver, dispatch, announce, distribute, route

- find : search, extract, locate, recover

- start : launch, create, begin, open

- make : create, setup, build, generate, compose, add, new

- 루프 반복자에 i,j,k 말고 clu_i, mem_i, user_i 또는 ci, mi, ui를 사용해보자.

- 단위를 포함하는 값들 변수에 _ms 를 추가해주자. 

  - start -> start_ms
  - delay -> delay_secs
  - limit -> max_kbps (kilobits per second)
  - password -> plaintext_password // 암호화가 되야한다로 느낄수 있다.

- new 와 함께 호출되는 함수인 생성자는 대문자로 시작하고 다른 평범한 함수는 소문자로 시작한다.

  - ```javascript
    var x = new DatePicker(); // 생성자구나!
    var y = pageHeight(); // 평범한 함수구나!
    ```

- html 에서 id는 `_` class 는 `-` 를 사용하자.

- 본인이 지은 이름을 '다른 사람들이 다른 의미로 해석할 수 있을까?'라는 질문을 던져보며 철저하게 확인해보자.

  - ```javascript
    filter("year <= 2011"); // 고르는거야 제거하는거야? select? exclude?
    ```

- 양끝이 포함된다는 의미에서 경계를 포함하는 범위에는 first/last가 좋은 선택이다. 경계를 포함하고/ 배제하는 범위에서는 begin/end를 사용하자. 

- 불리언 변수에 이름 붙이기

  - 일반적으로 is, has, can, should 와 같은 단어를 더하면 불리언 값의 의미가 더 명확해진다.
  - SpaceLeft() 같은 함수는 숫자값을 반환할것처럼 보인다 만약 불리언 값을 반환한다면 HasSpaceLeft() 가 더 적합할것이다. 
  - 부정표현보단 긍정표현을 사용하자! disable_ssl -> use_ssl

  

- HTTP 통신은 TCP/IP를 통해 이뤄진다. 통신을 하기 위한 HTTP 계층을 살펴보자. 

  - |       HTTP       | 애플리케이션 layer |
    | :--------------: | :----------------: |
    |       TCP        |     전송 layer     |
    |        IP        |   네크워크 layer   |
    | NetworkInterface |  데이터링크 layer  |

  - 브라우저가(클라이언트)에 http://d2.naver.com:80/news/1234 라고 입력한다.

  - 브라우저가 호스트명에 대한 IP와 포트번호를 얻는다. (from DNS server)

  - 브라우저가 117.52.129.49의 80번 포트로 TCP 커넥션을 생성한다.

  - 브라우저가 서버로 HTTP Request를 보낸다.

  - 서버가 요청을 처리한다.

  - 서버가 브라우저로 응답을 보낸다.

  - **브라우저**가 TCP 커넥션을 끊는다. 

### 오늘 개발한거 

- 사용자가 메인페이지에 들어와 로그인버튼을 클릭해 아이디와 비밀번호를 입력하면 서버가 회원인지 아닌지 판단해 각각 다른 결과창을 띄워준다. 
- 로그인 버튼을 클릭하면 post 로 request가 들어온다. 
- request 의 body 부분에서 html 에서 사용한 name 속성을 이용해 값을 뽑아낼수 있다. 
- db는 임시 DB 이고 select 쿼리를 실행시키는것처럼 보이게 만들었다. 
- 쿼리의 결과에 따라 로그인 성공, 실패여부가 결정된다.

```javascript
const db = require('../js/DBMS');

router.post('/login', function(req, res, next){
	let id = req.body.id;
    let pwd = req.body.pwd;
    let db_query_result = db.select_userTable(id, pwd);

    if(db_query_result == true){
      //로그인 성공
    }else{
      //로그인 실패
    }
})
```

수정된 디렉토리 구조

```javascript
- root
| - DB_TABLE // ADD
	| - userTable.json
| - js // ADD
	| - DBMS.js
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



### 오늘 ~~느낀점~~ 땅파기

-  fs 를 이용해 userTable.json 파일을 읽어올려고 했는데 분명 경로가 맞는데 자꾸 그런 파일없다고 undefined 가 떴다. 

> `npm install` will create a `node_modules` directory in the current path where it is executed. So, while using the *file server system module*, when you declare
>
> ```js
> const fs = require('fs')
> 
> fs.readFile('./DB_TABLE/userTable.json', 'utf8', (err, jsonString) => {
>     if (err) {
>         console.log("File read failed:", err)
>         return
>     }
>     console.log('File data:', jsonString) 
> })
> ```
>
> this locate files from the top level directory of node_modules folder.
>
> 이므로, 내 현재 위치에서 fs의 상대경로를 찍는게 아니라 node_modules folder가 있는 곳을 root 로 생각해 경로를 찍어야 한다.

