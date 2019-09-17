# 부스트 캠프 멤버십 14일차 

## week3 front-end day2

### 오늘 공부한거 

#### 호이스트



호이스트란, 변수의 정의가 범위에 따라 **선언**과 **할당**으로 분리되는 것을 의미한다. 변수가 함수내에 정의되었을 경우 **선언**은 함수내 최상위로, 함수밖에 정의되었을 경우 전역 context의 최상위로 변경된다. 변수의 **선언**이 초기화나 할당시에 발생하는게 아니라 항상 **최상위로 호이스트 된다**는것을 명심해야한다.

```javascript
function showName() {
   console.log("First Name : " + name); //First Name : undefined
   var name = "Ford";
   console.log("Last Name : " + name); //Last Name : Ford
}
showName();
```



위 코드는 JS 엔진에 의해 아래와 같이 해석된다.

```javascript
function showName() {
     var name; // [선언] name 변수는 호이스트 되었습니다. 이 시점에 name의 값은 undefined 입니다.
     console.log("First name : " + name); // First Name : undefined
     name = "Ford"; // [할당] name에 값이 할당 되었습니다.
     console.log("Last Name : " + name); // Last Name : Ford
}
```



함수 선언은 변수 선언을 덮어쓴다.

```javascript
var myName; // string
function myName() {
     console.log("Rich");
}
console.log(typeof myName); // function	
```



하지만, 변수에 값이 할당될 경우에는 반대로 변수가 함수선언을 덮어쓴다.

```javascript
var myName = "Richard";
function myName() {
     console.log("Rich");
}
console.log(typeof myName); //string
```

“strict mode”에서 최초의 선언없이 변수에 값을 할당하려 한다면 오류가 발생한다. 변수에 값을 할당 하려 할때는 항상 미리 선언하는 습관을 들이는것이 좋다.



#### 클로져



클로져란, 외부함수의 변수에 접근할 수 있는 내부함수를 뜻한다. scope chain으로 표현되기도 한다. 

*어려우니 예제 많이*

```javascript
function makeFunc() {
  var name = 'Mozilla';
  // 내부 함수는 외부함수의 변수를 사용할 수 있다.
  function displayName() {
    alert(name); 
  }
  return displayName;
}

var myFunc = makeFunc(); // 외부함수인 makeFunc()가 리턴되어 myFunc 변수에 저장된다.
myFunc(); // 함수가 실행되었을 때 함수는 자신이 생성되었을때와 동일한 스코프 체인을 사용한다.
```

```javascript
function showName(firstName, lastName) {
    var nameIntro = "Your name is ";
    // 내부 함수는 외부함수의 변수뿐만 아니라 파라미터 까지 사용할 수 있다.
    function makeFullName() {
        return nameIntro + firstName + " " + lastName;
    }
    return makeFullName();
}
showName("Michael", "Jackson"); // Your name is Michael Jackson
```

```javascript
function celebrityName(firstName) {
    var nameIntro = "This is celebrity is ";
    // 내부 함수는 외부함수의 변수뿐만 아니라 파라미터 까지 사용할 수 있다.
    function lastName(theLastName) {
        return nameIntro + firstName + " " + theLastName;
    }
    return lastName;
}
var mjName = celebrityName("Michael"); // 외부함수인 celebrityName()가 리턴되어 mjName 변수에 저장된다.
// 외부함수가 위에서 리턴된 후에, 클로저(lastName)가 호출된다.
mjName("Jackson"); // This celebrity is Michael Jackson
```



*함수가 실행되었을때, 함수는 자신이 생성되었을때와 동일한 스코프 체인을 사용한다. 그러므로, 내부 함수를 나중에 호출할 수 있다.*

```javascript
function celebrityID() {
    var celebrityID = 999;
    // 몇 개의 내부 함수를 가진 객체를 리턴 할 것이다.
    // 내부 함수는 외부함수의 변수뿐만 아니라 파라미터 까지 사용할 수 있다.
    return {
      	// 이 내부함수는 celebrityID 변수의 현재값을 리턴한다.
        getID: function() {
            return celebrityID;
        },
     		// 이 내부함수는 외부함수의 값을 언제든지 변경한다.
        setID: function(theNewID) {
            celebrityID = theNewID;
        }
    }
}
var mjID = celebrityID(); // 이 시점에, celebrityID 외부 함수가 리턴된다.
mjID.getID(); // 999
mjID.setID(567); // 외부함수의 변수를 변경한다.
mjID.getID(); // 567; 변경된 celebrityID변수를 리턴한다.
```



*클로저 비꼬기 : 의도치 않은 결과가 나온다.*



이런 결과가 나타나는 이유는 클로저는 외부변수에 대해 값이 아니라 참조로 접근하기 때문이다. 

```javascript
function celebrityIDCreator(theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i=0; i<theCelebrities.length; i++) {
        theCelebrities[i]["id"] = function() {
            return uniqueID + i;
        }
    }
    return theCelebrities;
}
var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
var stalloneID = createIdForActionCelebs[0];
console.log(stalloneID.id); // 103
```

부작용?을 고치기 위해서 “즉시 호출된 함수 표현식(Immediately Invoked Function Expression. IIFE)”를 사용하면 된다.

```javascript
function celebrityIDCreator(theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i=0; i<theCelebrities.length; i++) {
        theCelebrities[i]["id"] = function(j) {
            // j 파라미터는 호출시 즉시 넘겨받은(IIFE) i의 값이 된다.
            return function() {
                // for문이 순환할때마다 현재 i의 값을 넘겨주고, 배열에 저장한다.
                return uniqueID + j;
            } () // 함수의 마지막에 ()를 추가함으로써 함수를 리턴하는 대신 함수를 즉시 실행하고 그 결과값을 리턴한다.
        } (i); // i 변수를 파라미터로 즉시 함수를 호출한다.
    }
    return theCelebrities;
}
var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
var stalloneID = createIdForActionCelebs[0];
console.log(stalloneID.id); // 100
var cruiseID = createIdForActionCelebs[1];
console.log(cruiseID.id); // 101
```



### 오늘 개발한거 

- 공부만 했다!

### 오늘 느낀점 

- 호이스팅이랑 클로져 개념을 분명 챌린지때 공부했었는데 그때랑 지금이랑 이해의 깊이가 다른것 같다. 역시 다시 한번 더 보니 친근해진것 같다. 
- 

