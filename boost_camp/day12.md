# 부스트 캠프 멤버십 12일차 

## week2 back-end day5

### 오늘 공부한거 

- 오늘은 정호영 마스터님의 클래스가 있었다. 그 중 인상깊었던 내용을 정리할것이다.

- 구글의 코드 리뷰 가이드 : https://soojin.ro/review/

  - **CL이 완벽하지 않더라도 전체적인 코드 품질을 증가시키는 상태에 도달했다면 리뷰어는 해당 CL을 승인하는 방향으로 생각한다.**
  - 개선의지 가지기
  - 작성자가 일주일동안 코드를 작성했다면 리뷰어는 5일정도 코드를 읽어봐야 한다. 흠.. fulltime 하루정도는 시간을 투자해라.
  - 완벽한 코드를 만들려고 하지 말고 *더 나은* 코드를 추구해야 한다는 것이다. 완벽함 대신 *지속적인 개선(continuous improvement)* 을 목표로 한다. 
  - 코드 스타일에 관해서는 스타일 가이드를 절대적으로 따른다. 정해진 가이드가 없으면 작성자를 따른다. 정해둔 규칙이 없을 때는 기존 코드와 일관성을 유지하도록 한다.
  - 유닛 테스트, 통합 테스트, E2E 테스트를 작성하도록 한다. [긴급 상황](https://soojin.ro/review/emergencies)이 아닌 일반적인 상황에서는 프로덕션 코드와 테스트 코드가 같은 CL에 들어있어야 한다.
  - 리뷰어의 자격으로 CL의 모든 줄을 리뷰해야한다. 
  - CL을 리뷰하다가 좋은 점을 발견하면 작성자에게 언급한다. 코드 리뷰를 하다보면 실수만 지적할 때가 있는데 좋은 부분에는 칭찬과 고마움을 아끼지 않는다. 멘토링 측면에서보면 잘못한 것 보다 잘한 것을 알려주는 것이 더 값지다.

- 좋은 커밋 메시지 : https://meetup.toast.com/posts/106

  - **git commit -m 명령어 사용하지 말기**
  - 제목과 본문을 한 줄 띄워 분리하기
  - 제목은 영문 기준 50자 이내로 
  - 제목 첫글자를 대문자로
  - 제목 끝에 `.` 금지
  - 제목은 `명령조`로
  - 본문은 영문 기준 72자마다 줄 바꾸기(세모)
  - 본문은 `어떻게`보다 `무엇을`, `왜`에 맞춰 작성하기

- 짝 프로그래밍에서 발견한 리팩토링거리

  - 파일경로를 숨기자! 보안에 안좋다고 한다. 따로 관리하자. 만약 파일 경로가 바뀐다면 하드코딩한 값을 일일히 다 바꿔줘야하니 따로 관리하는게 좋겠다.

  - 문자열와 logic 분리하기.. 

  - js 디렉토리안에 DBMS.js 파일이 있고 DB_TABLE 디렉토리안에 session.json, user.json 파일이 있었다. 짝꿍은 모델이라는 개념을 설명해줬다. 모델이란 DB, API를 통해 데이터를 불러오고 저장하는 역할을 하고 서비스의 비즈니스 로직(도메인)을 담당한다고 한다. 추상적이어서 무슨말인지 와닿지는 않지만 데이터에 접근하고 관리하는게 MODEL이라는걸 알았다. 그래서 model 이라는 디렉토리를 만들어 줬고 DBMS.js, session.json, user.json 파일을 이 디렉토리로 옮겼다. 

  - ```reStructuredText
    ├── model
    |   ├── DBMS.js
    |   ├── session.json // 세션 DB
    |   └── user.json // 사용자 DB
    ```

  - 좀 더 나아가 비즈니스 로직과 프레젠테이션 로직을 분리하는것도 생각했다. 우선 비즈니스 로직이란 유저의 행동에 따라 서비스에서 보여주고자 하는 결과를 나타내기 위해 데이터를 가공하는 로직이다. 프레젠테이션 로직은 가공한 로직을 상황에 맞게 뿌려주는 로직이라고 생각하면 된다. 복잡한 화면일수록 presetation의 코드가 커지고 그럴수록 복잡성이 증가하고 유지보수성이 저하된다. 그래서 분리해서 관리한다. 

  - ```javascript
    //before
    router.get('/', function(req, res, next) {
      if(req.cookies.SESSION_ID){
        const session_id = req.cookies.SESSION_ID;
        const user_index = db.select_sessionTable_id(session_id);
        const result = db.select_userTable_index(user_index);
    
        res.render('user_main',  { user_info : result});
      }else{
        res.render('index', { title: 'not login user' });
      }
    });
    
    //after
    router.get('/', function(req, res, next) {
      if(req.cookies.SESSION_ID){
        const session_id = req.cookies.SESSION_ID;
        const result = service.logined_user(session_id);
        res.render('user_main',  { user_info : result});
      }else{
        res.render('index', { title: 'not login user' });
      }
    });
    ```

  - 

### 오늘 개발한거 

- 아이디 중복검사를 하기 위해 중간에 서버와 통신을 할 수 있는 fetch API를 사용했다. 반환값은 Promise 이다.

- ```javascript
  //아이디 중복 검사 결과에 따라 !
  let content = {id: id};
  fetch('/double',{
      method : 'post',
      headers : {
          'Content-Type': 'application/json'
      },
      body : JSON.stringify(content)
  })
  .then(response => response.json())
  .then(data => {
    staicVali(data.result);
  });
  ```

- 원래는 `const res = fetch()` 이런식으로 결과값을 반환받고 싶었다. async await 로 감싸주면 값을 받을 수 있다. 이걸 생각하지 못해서 이상하게 코드를 짰다. 개선할 부분인것 같다. 

### 오늘 느낀점 

- 생각보다 지켜야 할게  많은것 같다. 