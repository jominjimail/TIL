# 부스트 캠프 멤버십 21일차 

## week4 back-end day4

### 오늘 공부한거 

- MySQL을 설치한다.
- mysql2 패키지를 이용해서 구현한다.

라는 조건때문에 기존에 `mysql`로 구현했던 코드를 다 들어냈다.

Node MySQL 2 : https://www.npmjs.com/package/mysql2 npm 문서를 참고했다. 

기존 `mysql` 과 별로 차이가 없는것 같지만 (connect 하고 query문 작성해서 날려 콜백으로 결과값을 받는 형식) 추가된 기능이 있다.



#### 1. ConnectionPool

connection pools을 사용해서 Mysql server 와의 connecting 시간을 줄이고 == query의 대기시간을 줄이고 == 매번 새로운 connection을 할 필요가 없다.

```javascript
// get the client
const mysql = require('mysql2');
 
// Create the connection pool. The pool-specific settings are the defaults
const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  database: 'test',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

// For pool initialization, see above
pool.query("SELECT field FROM atable", function(err, rows, fields) {
   // Connection is automatically released when query resolves
})
```

connect을 조절할 수 있다. 

```javascript
// For pool initialization, see above
pool.getConnection(function(err, conn) {
   // Do something with the connection
   conn.query(/* ... */);
   // Don't forget to release the connection when finished!
   pool.releaseConnection(conn);
})
```



#### 2. Promise Wapper

이게 진짜 좋은것 같다. promise 기반으로 query를 날리니깐 결과값을 자유롭게 다룰수 있다.

```javascript
async function main() {
  // get the client
  const mysql = require('mysql2/promise');
  // create the connection
  const connection = await mysql.createConnection({host:'localhost', user: 'root', database: 'test'});
  // query database
  const [rows, fields] = await connection.execute('SELECT * FROM `table` WHERE `name` = ? AND `age` > ?', ['Morty', 14]);
}
```



### 오늘 개발한거 

connection pool +  promise 기반으로 동작하는 `mysql2/promise` 패키지를 사용했다.

기존 코드와 비교해보겠다.

#### before

```javascript
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: '101.101.163.55',
  user: 'nodejsuser',
  password: 'xxxxx',
  database: 'boostcamp-amazon'
});
..
router.get('/getuserlist', function(req, res, next){
  let sql = `SELECT * FROM ${DB.USER_TABLE}`;
  connection.query(sql, (err, rows) => {
      if (err) throw err;
      res.status(200).send({result : rows});
  });
});
```

#### after

```javascript
const mysql = require('mysql2');
const pool = mysql.createPool({host:'101.101.163.55', user: 'nodejsuser', password: 'xxxxx',
database: 'boostcamp-amazon'});
const promisePool = pool.promise();

select_userTable_by_uId : async (username) => {
  let sql = `SELECT * FROM ${DB.USER_TABLE} WHERE uId = '${username}'`; 
  const [rows,fields] = await promisePool.query(sql);
  return rows;
}
```

query 의 결과가 콜백에 갇히는게 아니라 밖으로 빼낼수 있어서 좋았다. 

### 오늘 느낀점 

fetch 도 그렇고 이번 mysql2도 그렇고 async await 는 정말 괜찮은것 같다. 코드가 더 간결해지고 정리가 잘 되는 느낌이다. 사실 mysql 이랑 별반 다를게 없을거 같아서 그냥 그대로 갈려했는데 바꾸길 잘했다. 