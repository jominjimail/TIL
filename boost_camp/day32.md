# 부스트 캠프 멤버십 32일차 

## week7 back-end day3

### 오늘 공부한거 

jsonwebtoken : https://www.npmjs.com/package/jsonwebtoken

open standard 이다. RFC-7519 (오 대단대단)

토큰 기반 인증 방식이다 토큰을 이용해 유저들의 인증 작업을 처리하는건 좋은 방법이다. 왜 좋은 방법일까? 답부터 말하면 'stateless'한 서버를 만들 수 있기 때문이다. 이게 뭘까? 서버는 클라이언트의 요청을 받을 때 이사람이 로그인을 했는지 관리자인자 일반 사용자인지, 수퍼유저인지, 특정개인인지 구분을 해야한다. 사용자의 권한에 따라 작업을 처리해야하니깐 필요하다.

과거의 인증 시스템은 아래와 같이 작동했다. 

- 서버 기반 인증 : 서버측에서 유저들의 정보를 '**기억**' 하고 있어야 한다. 

  1. 클라이언트가 `/` 을 요청한다.

  1. 서버는 `/` 웹사이트를 제공한다.
  2. 클라이언트가 아이디와 비밀번호를 입력하고 로그인을 요청한다.
  3. 회원가입을 한 사용자가 맞다면 서버는 '**세션**' 을 만들어 어딘가에 **저장** 한 뒤 세션값을 **쿠키**에 넣어(나는 쿠키에 넣는 것 밖에 모른다.) 응답한다.
  4. 클라이언트가 로그인된 사용자만 할 수 있는 특별한 요청을 한다. (예를들어 사용자 메일목록 보기)
  5. 서버는 특별한 요청을 한 사람이 자격이 있는지 확인하기 위해 **서버의 세션 저장소**를 뒤적뒤적한다. 유효하면 요청을 처리한 후 응답한다.

-  서버는 항상 뒤적뒤적 해야한다. 확인이 번거롭다. 왜? 세션은을 어딘가에 기록해야하고 (메모리를 사용해야한다는 소리) 유저가 많을수록 관리가 힘들어지고 만약 로그인중인 유저가 많다면 서버 RAM이 과부하된다. 만약 여러개의 process를 돌리거나 여러대의 서버 컴퓨터를 추가하는 경우를 생각해봐라 session을 어떻게 공유할 건가? 으악 불가능한건 아니지만 과정이 매우매우 복잡하다고 한다. 

다단, 그래서 토큰이 나타났습니다. 이제 더이상 유저의 인증 정보를 서버나 세션에 저장할 필요없다. 이 토큰은 유저가 가지고 있고 유저는 요청을 할 때마다 헤더에 토큰을 넣어 요청을 보내야한다. 

- 토큰 기반 인증 : 서버는 어디상 유저의 정보를 기억할 필요 없다.
  1. 클라이언트가 `/` 을 요청한다.
  2. 서버는 `/` 웹사이트를 제공한다.
  3. 클라이언트가 아이디와 비밀번호를 입력하고 로그인을 요청한다.
  4. 회원가입을 한 사용자가 맞다면 서버는 token 을 발급해 클라이언트에게 보낸다.
  5. 클라이언트가 로그인된 사용자만 할 수 있는 특별한 요청을 한다. (예를들어 사용자 메일목록 보기)
  6. 서버는 특별한 요청을 한 사람이 유효한 token을 가지고 있는지 검사한다. 유효하면 요청을 처리한 후 응답한다.

서버는 토큰을 받아 decode 과정을 거친후 '**유효**' 하다면 요청을 처리한다. 유효한지 안한지는 어떻게 알까? 서버는 토큰을 만들때 **secret** 을 이용해 생성한다. header.payload.signature  로 구성된다.

```javascript
jwt.sign(
    {
        _id: user._id,
        username: user.username,
        admin: user.admin
    }, 
    secret, // 중요한 정보이기 때문에 app.set('jwt-secret', process.env.SECRET) 으로 감춰야 한다.
    {
        expiresIn: '7d',
        issuer: 'velopert.com',
        subject: 'userInfo'
    }, (err, token) => {
        if (err) reject(err)
        resolve(token) 
    })
})
```

마찬가지로 decode를 할 때 동일한 '**secret**' 을 이용해 검증한다.

```javascript
jwt.verify(token, req.app.get('jwt-secret'), (err, decoded) => {
    if(err) reject(err);
    resolve(decoded);
});
```

만약 한 글자라도 틀리면 바로 reject 되어서 인증되지 않는다. 

### 오늘 개발한거 

### 오늘 느낀점 

이제 클라이언트가 토큰을 가지고 있는데 어디에 저장한다는걸까? cookie일까 local storage일까? 둘다 보안상 이슈가 있다는데 아직 잘 모르겠다.