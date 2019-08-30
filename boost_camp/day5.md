# 부스트 캠프 멤버십 5일차 

## week1 front-end day3



### 오늘 공부한거 

- **flex** : css 에서 아이템을 배치할때 block, fixed, relative, float, hidden… 등 이해하기 힘든 키워드를 사용한다. `display : block` 을 `display: flex` 로 모두 변경하고 직관적인 *flex-direction: column, justify-content: center* 등을 이용해 아이템들을 정렬, 배치했다. flex는 *flex container*  안에 있는 아이템들을 말그대로 *flexible* 하게 다룰수 있는 속성이다. 
  - 크롬 브라우져 기준으로 `h1` 태그는 `Support YES` 이다. 
  - `header` 태그는 `Support 6.0` 약 2010년이다. 
  - 그럼 fles는? `Support 29.0` 약 2013년이다. 
  - 현재 chrome version 은 `76.0.3809.100` 이다. 
- **class 와 id 를 둘다 준 이유는 무엇일까?** : id는 js 코드에서 `getElementId()` 함수를 이용해 다이렉트로 접근이 가능하다. 하지만 중첩된 모든 tag에 고유한 id 값을 주는건 마치 변수를 너무 많이 둔 코드와 비슷한 느낌이 었다. 다른 방법이 없을까? `addEventlistner()`가 실행될때 **누가** 이벤트를 불렀는지 callback() 함수의 `event`파라미터로 알아낼수 있다. `event.target` 이라는 속성을 사용하면 된다. 타겟의 부모가 누군지 자식이 누군지 이웃형제가 누군지 상대적으로 접근이 가능하다. 아래코드는 부모의 부모의 자식중 5번째 자식을 찾는건데 해당 코드는 이웃형제를 찾는 형식으로 개선될 예정이다. 

```javascript
const user_id = document.getElementById('id');

user_id.addEventListener('blur', function(event){
    ...
    check_id(event, user_id);
})

function check_id(event, id){
    ...
    //const user_id_msg = document.getElementById('idMsg'); // id로 다이렉트 접근
    const user_id_msg = event.target.parentNode.parentNode.childNodes[5];
}
```



### 오늘 개발한거 

- layout 뒤엎기 flex 짱
- 사용자가 id 입력란을 클릭하고 `focus` —> 값을 입력하고 —> id 입력란 외 다른곳을 클릭했을때 `blur` 입력된 id 값의 유효성을 검사해 결과에 따른 메세지를 하단에 띄어준다. 비밀번호, 재입력, 이름, 생년월일, 성별, 이메일, 휴대전화도 아래와 같은 logic을 가진다. 

```javascript
function check_id(event, id){
    ...
    const user_id_msg = event.target.parentNode.parentNode.childNodes[5];
    const regex_id = /^[a-z0-9][a-z0-9_\-]{4,19}$/;
	...
    let result = regex_id.test(id.value);
    
    if(result == true){
        user_id_msg.style = "display:block";
        user_id_msg.innerHTML="멋진 아이디네요.";
        idFlag =true;
        return true;
    }else{
        user_id_msg.style = "display:block; color:red";
        user_id_msg.innerHTML="5~20자의 영문 소문자, 숫자와 특수기호(_),(-)만 사용 가능합니다.";
        return false;
    }
}
```



### 오늘 느낀점 

- 사실 기존에 잘 돌아가는 코드를 뒤엎기란 쉽지 않다. 새로운것을 학습하고 이를 대체하는데 시간이 걸리고 수정하다 막히면 *~~아.. ctrl+z 할까..~~* 라는 생각이 들었지만.. 다행히 잘 해결됐다. 코드가 전보다 훨씬 더 직관적이게 짜졌고 내가 이해하기가 쉬웠다. 잘했다. 
- html 을 너무 붙잡고 있어서 왠지 모르게 주말에도 코딩을 하고 있을것 같다. 
- 하단 `초기화` `가입하기` 버튼을 멋있게 구현하고 싶었다. 절대적인 위치값을 주는게 아닌 상대적으로 구현하고 싶었다. 이런 욕심을 부려도 되는건지 안되는건지 잘 모르겠다. 
- 개구리게임을 하면서 flex의 사용법을 익혔다. 획실히 놀이는 새로운것을 습득하는데 좋은 수단이다. 

### 참고

[flex 문서](<https://css-tricks.com/snippets/css/a-guide-to-flexbox/>)

[flex 개구리게임](<https://flexboxfroggy.com/#ko>)

