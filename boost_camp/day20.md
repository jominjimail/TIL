# 부스트 캠프 멤버십 20일차 

## week4 back-end day3

### 오늘 공부한거 

x

### 오늘 개발한거 

관리자가 전체 사용자를 조회하는 기능

- 관리자로 로그인을 하면 관리자 페이지가 띄어진다.
- 관리자 페이지에는 전체 사용자 정보를 조회 할 수 있다.
- 전체 사용자 정보는 ncloud 서버의 mysql 서버의 boostcamp-amazon 스키마의 user_table에 저장되어 있다. 
- 관리자 페이지를 로드할 때 `fetch` 를 사용해서 미리 전체 사용자 정보를 받아온다.
- 받아온 정보를 table tag에 tr, th tag을 사용해 적절히 정보를 채워넣는다.
- 최종적인 관리자 페이지를 만들어 render 한다.

login 기능을 구현했을때 fetch의 콜백안에서 모든걸 해결해야하는줄 알고 로직을 이상하게? 구현했다. 

```javascript
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

하지만, async await 를 사용하면 fetch의 결과를 받을 수 있다.

```javascript
const attachUserlist = (parent) => {
    const userlist = new Userlist(
        parent
    );
    getuserlistItem(userlist).then(
        users => {
            users.forEach( (element) => {
                const user = new User(element);
                userlist.addUserlist(user);
            });
            userlist.render();
        }
    );
    ...
};
```

```javascript
const getuserlistItem = async () => {
    const response = await fetch('/admin/userlist');
    const json = await response.json();
    const users = json.result;
    return users;
};
```

아하!

### 오늘 느낀점 

머리가 너무 아프다. 감기에 걸린것 같다.

같이 스터디 하는 동료가 감기에 걸렸는데 ...........


