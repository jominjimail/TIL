# 부스트 캠프 멤버십 18일차 

## week4 back-end day1

### 오늘 공부한거 

#### Deview 2017, 그런 REST API로 괜찮은가

- https://tv.naver.com/v/2292653
- http://slides.com/eungjun/rest#/



#### 고민 : **인터넷에서 정보를 어떻게 공유할것인가?**

해결법 : 정보를 하이퍼 텍스트로 연결하자. 

정보를 표현하는 방식으로 **HTML** 을 사용하고 정보를 식별하기 위해 **URI**를 사용하고 정보를 전송하기 위해 **HTTP**를 사용하자!



#### 고민 : HTTP팀에 속해 있던 Roy T.Filding 은 고민에 빠진다. '어떻게 하면 web 호환성을 망가뜨리지 않고 HTTP를 improve 할 수 있을까?'

해결 : **HTTP Object Model**  (2000년도에 REST라는 이름으로 발표된다.)



CMIS(2008) 출시 

- CMS를 위한 표준
- EMC, IBM, Microsoft 등이 함께 작업
- REST 바인딩 지원

> ####  Roy T.Filding 왈 : "NO REST in CMIS"

Microsoft REST API Guildlindes (2016) 출시

- uri는 https://{serviceRoot}/{collection}/{id} 형식이어야한다
- GET, PUT, DELETE, POST, HEAD, PATCH, OPTIONS를 지원해야한다
- API 버저닝은 Major.minor로 하고 uri에 버전 정보를 포함시킨다

> ####  Roy T.Filding 왈 : "이건 REST API가 아니라 HTTP API이다."



#### 사람들은 REST API라고 생각하는데 REST API를 만든 Roy T.Filding 은 아니라고 한다. 왜 일까? 

- REST API는 REST 아키텍처 스타일을 따르는 API이다. 
- REST 는 분산 하이퍼미디어 시스템(ex. web)을 위한 아키텍처 스타일이다.
- 아키텍처 스타일은 제약 조건의 집합이다. 
- REST는 client-server / stateless / cache / **uniform interface** / layered system / code-on-demand(optional) 제약조건을 가진다. 옵션을 제외한 5가지 조건 중 특히 uniform interface 이 가장 많이 안 지켜지고 있다고 한다.
- uniform interface의 세부 조건인  identification of resources / manipulation of resources through representations /  **self-descriptive messages** / **hypermedia as the engine of application state (HATEOAS)** 중 굵은 글씨의 조건을 대게 만족하지 않다고 한다. 
- self-descriptive messages 란 메세지는 스스로를 설명해야한다는 조건이다. 서버나 클라이언트가 변경되더라도 오고가는 메시지는 언제나 self-descriptive 하므로 언제나 해석이 가능하다.
  - Media type으로 만족시키기 : IANA에 미디어 타입을 등록한다. 이 때 만든 문서를 미디어 타입의 명세로 등록한다. 이 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 메시지의 의미를 온전히 해석할 수 있다.
  - Profile 로 만족시키기 : 위와 같이 명세를 등록하고 Link 헤더에 profile relation으로 해당 명세를 링크한다. 이 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 메시지의 의미를 온전히 해석할 수 있다.
- HATEOAS 란 애플리케이션의 상태는 Hyperlink를 이용해 전이(late binding)되어야 한다는 조건이다. 어떤 상태로 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다.
  - ?흐믐나니..?



#### 왜 uniform interface 조건이 필요할까?

- HTTP를 만들때 Roy T.Filding 이 고민했던 **호환성을 망가뜨리지 않고 HTTP를 improve 할 수 있을까?** 는 HTTP의 **독립적인 진화**를 위한 것이었다.
- 독립적인 진화란 
  - ex) 서버기능이 변경됐더라도 클라이언트를 업데이트할 필요가 없다.
  - ex) 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없다.
  - ex)  웹 브라우저를 업데이트했다고 웹 페이지를 변경할 필요도 없다.
  - ex)  HTTP 명세가 변경되어도 웹은 잘 동작한다.
  - ex)  HTML 명세가 변경되어도 웹은 잘 동작한다.
- 즉, 상호운용성(interoperability)에 대한 집착의 결과이다.
- REST는 웹의 독립적 진화에 도움을 주었다.



#### 그럼 REST API는 저 제약조건을 다 지켜야 할까?

> 그렇다고 한다.



#### 그럼 원격 API는 꼭 REST API이어야 하는가?

> 시스템 전체를 통제할 수 있다고 생각하거나, 진화에 관심이 없다면, REST에 대해 따지느라 시간을 낭비하지 마라



#### 그럼 어떻게 해야할까?

> **REST API가 아니지만 REST API라고 부른다. (현재 상태...)**



### 오늘 개발한거 

없다.

### 오늘 느낀점 

우선 자야곘다.

자고 일어났다. REST API가 뭔지는 어느정도 감이 온다. 근데 이걸 어디다 써먹지? 그걸 모르겠다. 


