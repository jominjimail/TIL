# 부스트 캠프 멤버십 6일차 

## week1 front-end day4

### 오늘 공부한거 & 오늘 개발한거 

- 약관동의란을만들때 custom_modal을 만들었다. flex를 배우고나서 구현하니 금방 만들었다.

- scroller 에 대해 공부했다. scroller 에는 'scroller' 라는 속성으로 이벤트를 추가할수 있다. 사용자가 스크롤을 하면 해당 이벤트가 불려진다. 사용자가 스크롤을 맨 하단까지 내렸다면 '동의' 버튼의 상태가 `disabled=false` 로 전환되서 사용자가 클릭을 할 수 있게 만들었다.

- ```javascript
  scroller.addEventListener('scroll', function(event){
      if(event.target.offsetHeight + event.target.scrollTop == event.target.scrollHeight){
          let agree_button = document.getElementById("agree");
          agree_button.disabled = false;
      }
  })
  ```

- 사용자의 입력값이 조건에 맞는지 확인하기 위해 regular expression을 사용했다. regular 검사를 하는 입력값들이 많아서 ex) 아이디, 비번, 생년월일, 이메일, 전화번호 등 각각 사용하는 정규식만 다르지 결과가 (비어있음, 옳바르지않음, 옳바름) 3가지 상태여서 regular_test() 라는 함수로 로직을 분리시켰다.

- ```javascript
  regular_test : function(regex, value){
          if(value == ""){
              return {state : -1}; // 비어있음
          }
          let result = regex.test(value);
          if(result == true){
              return {state : 1}; // 옳바름
      
          }else{
              return {state : 0}; // 옳바르지 않음
          }
      }
  ```

- 로그인창을 부트스트랩을 사용해 구현해봤다.  기존 css 와 부트스트랩의 css를 한곳에 갖다 붙이니 뒤엉켜 한참 해맸다. 순서가 굉장히 중요하다는걸 느꼈다.

### 오늘 느낀점 

- 오늘은 새로운 무언가를 배운다기보단 기존에 있는 코드의 중복을 줄이는데 시간을 많이 사용했다.
- 약관동의의 시뮬레이션을 생각하면서 차근차근 구현해보았다. 상단히 만족스럽다.
- 사용자 입력 검사와 입력 결과에 따른 경고창 메시지 띄우기가 노가다성이 짙었다.
- css 너무 어렵다.