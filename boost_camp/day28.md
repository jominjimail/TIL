# 부스트 캠프 멤버십 28일차 

## week5 front-end day2

### 오늘 공부한거 

### Using Prepared Statements

mysql2 모듈을 사용하려고 https://www.npmjs.com/package/mysql2 가이드를 보다가 prepared statements 라는 단어를 봤다. 

mysql2 는 prepared statements를 지원하고 mysql은 없다고 한다. performance도 더 좋다고 한다. 

#### 왜 사용해야할까?

> https://stackoverflow.com/questions/8263371/how-can-prepared-statements-protect-from-sql-injection-attacks

이유는 간단하다 SQL injection attack을 막을 수 있다고 한다.

#### 왜 막아야 할까?

SQL injection 문제의 근원은 code와 data가 뒤섞여 있다는 점인데, sql query는 규칙이 있는 program 언어이기 때문에 아래와 같이 악용될 수 있다. 

```javascript
const expected_data = 1;
const query = `SELECT * FROM users where id=${expected_data}`;
```

완성된 query문은 아래와 같다. 

```mysql
SELECT * FROM users where id=1
```

그럼, 이렇게도 쓸 수 있겠네.

```javascript
const spoiled_data = "1; DROP TABLE users;"
const query = `SELECT * FROM users where id=${spoiled_data}`;
```

완성된 query문은 아래와 같다. 오오오오오옹....

```mysql
SELECT * FROM users where id=1; DROP TABLE users;
```

실제로 실행된다. 문법상 오류가 없으니깐



prepared statements로 간단하게 위 문제를 해결할 수 있다. 

- server에 program을 **먼저** 보낸다. 변수부분은 `?` 로 처리한다.

```
$db->prepare("SELECT * FROM users where id=?");
```

- 그리고나서  data를 보낸다. 

```
$db->execute($data);
```



### 오늘 개발한거 

#### before

```javascript
select_user_by_userid : async (username) => {
  let sql = `SELECT * FROM ${TABLE.USER} WHERE ${USER_COL.ID} = '${username}'`; 
  const [rows,fields] = await promisePool.query(sql);
  return rows;
}
```

#### after

```javascript
select_user_by_userid : async (username) => {
  let sql = `SELECT * FROM ${TABLE.USER} WHERE ${USER_COL.ID} = ?`; 
  const [rows,fields] = await promisePool.execute(sql,[username]);
  return rows;
}
```



### 오늘 느낀점 

사실 코드상 before after가 큰 차이는 없는데 program과 data를 나눠보내는것으로 sql injection 공격을 막을 수 있다는게 신기했다. 

정확한 원리를 모르겠다. 나눠서 보낸다는건 알겠는데 mysql이를 이를 어떻게 인식하는지 잘 모르겠다. 나중에 보내는 data자체를 query로 인식하는게 아니라 데이터의 변수로써만 인식해서 그런가?

```mysql
c:\temp>java Bind salary
SELECT paymentType, amount FROM employee WHERE name = 'bob' AND paymentType=?
salary 50

c:\temp>java Bind "salary' OR 'a'!='b"
SELECT paymentType, amount FROM employee WHERE name = 'bob' AND paymentType=?
```

위의 예시에서 2번째가 injection공격을 한 예시인데 prepared statment로 구현되어 있어서 결과값이 아예 나오지 않은것을 확인할 수  있다. `?` 를 하나의 **변수** 그 자체로 인식해서 그런것 같다. (query의 string이 아닌)

> the malicious code does not work because there is no paymentType matching that string