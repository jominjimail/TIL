# 부스트 캠프 멤버십 30일차 

## week7 back-end day1

### 오늘 공부한거 

#### Sequelize, Express 에 ORM 적용

링크 : https://sequelize.org/master/manual

왜 필요한가? 그냥 query 문 작성하면 안될려나? 

sql query 자체를 객체화 시켜서 관리한다는 소리인데 '객체화한다'에 꼭 따라 붙는 장점이 있다. 유지 보수성이 높아지고 개발 생산성이 높아진다는 소리이다. 와닿지 않는다. 왠지 굉장히 복잡하고 이해하기 어려워서 '아 이거그냥 query 한 줄이면 끝나는데'하고 아쉬워할것 같다. 

> 개발 현장마다 생산성과 품질을 향상시켜야 한다는 목소리는 높지만 이러한 문제들을 대폭 개선시킬 수 있는 ORM에 대해서는 의심만 쌓여 간다. 네모 바퀴 수레를 미느라 힘들어하면서 둥근 바퀴를 쓰라고 주면 정작 바퀴를 갈 시간이 없다고 거부하는 형국입니다.
>
> 박성철 /SK 플래닛 개발팀 그룹장

아... 동그란 바퀴라...  여태까지 mysql2 를 이용해 직접 query를 작성했는데 (prepared statements 사용) 얼만큼 잘 굴러가는지? 사용해봐야겠다.

*ORM은 사용하기 쉽다? sql을 몰라도 사용할 수 있다?* 아니다. 틀렸다. ORM을 '**잘**' 사용하기 위해서는 **DB, sequlize**뿐만아니라 **object** 까지 알아야 한다. 굉장히 어려운거다. 그러니 정신을 똑바로 차려야 한다.

#### instll

```shell
$npm install sequilize sequlize-cli
$npm install --save sqlite3
```

#### quick start

```javascript
const { Sequelize, Model, DataTypes } = require('sequelize');
const sequelize = new Sequelize('sqlite::memory:');

class User extends Model {}
User.init({
  userid: { type: Sequelize.STRING, allowNull: false },
  userpwd: { type: Sequelize.STRING, allowNull: false },
  username: { type: Sequelize.STRING, allowNull: false },
  userrank: { type: Sequelize.BOOLEAN, allowNull: false, defaultValue: 'USER' }
}, { sequelize, modelName: 'user' });

sequelize.sync({ force: true })
  .then(() => User.create({
    userid: 'new1234',
    userpwd: '1234',
    username: 'janedoe',
    userrank: 'ADMIN'
  }))
  .then(jane => {
    console.log(jane.toJSON());
  });
```

Model을 extends한 User class를 만든다. init를 이용해 db columns를 정의해주고 규칙도 주고, 이름도 줬다. 아직까지는 별 다른 차이점을 못 느끼겠다. insert를 할때는 User.create method를 이용한다. 

```
{ id: 1,
  userid: 'new1234',
  userpwd: '1234',
  username: 'janedoe',
  userrank: 'ADMIN',
  updatedAt: 2019-10-14T14:50:17.222Z,
  createdAt: 2019-10-14T14:50:17.222Z }
```

기본적으로 모든 model마다 id라는 column 값이 있다. 전에는 auto increments속성을 줘서 직접 정의해줬는데 ORM은 알아서 해준다. 특이하게 createdAt, updatedAt 이라는 속성이 자동으로 추가된다. 이 2개의 속성은 나중에 migration pool이라는 (실행, 되돌리기) 기능을 사용할텐데 그때 꼭 필요한 속성이라고 한다.

> By default, Sequelize will add the attributes `createdAt` and `updatedAt` to your model so you will be able to know when the database entry went into the db and when it was updated last.
>
> Note that if you are using Sequelize migrations you will need to add the `createdAt` and `updatedAt` fields to your migration definition

#### foreign key

```javascript
class Reservation extends Model {}
Reservation.init({ 
  user_id: {
    type: Sequelize.INTEGER,
    references: {
      model: User,
      key: 'id'
    }
  },
  ...
```

fk를 만드는 방법이 너무 간편했다. 굉장히 직관적이었다. Reservation의 칼럼인 user_id는 integer 타입을 가졌고 이것은 User( 위에 만든 class )의 'id' 칼럼을 참조한다. 

```javascript
class Employee extends Model {}
Employee.init({
  name: {
    type: Sequelize.STRING,
    allowNull: false,
    get() {
      const title = this.getDataValue('title'); // this's magic
      return this.getDataValue('name') + ' (' + title + ')';
    },
  },
  title: {
    type: Sequelize.STRING,
    allowNull: false,
    set(val) {
      this.setDataValue('title', val.toUpperCase());
    }
  }
}, { sequelize, modelName: 'employee' });

Employee
  .create({ name: 'John Doe', title: 'senior engineer' })
  .then(employee => {
    console.log(employee.get('name')); // John Doe (SENIOR ENGINEER)
    console.log(employee.get('title')); // SENIOR ENGINEER
  })
```

대박.... 단순 query 한줄이 아닌 그 이상의 동작을 하고 있다. 객체다. 객체

아래는 우리가 평소에 사용하는 class 형식과 비슷한 ORM의 모습이다. 오호....

```javascript
class Foo extends Model {
  get fullName() {
    return this.firstname + ' ' + this.lastname;
  }

  set fullName(value) {
    const names = value.split(' ');
    this.setDataValue('firstname', names.slice(0, -1).join(' '));
    this.setDataValue('lastname', names.slice(-1).join(' '));
  }
}
Foo.init({
  firstname: Sequelize.STRING,
  lastname: Sequelize.STRING
}, {
  sequelize,
  modelName: 'foo'
});
```

validation 부분을 참고하면 우리가 front 단에서 검사했던 사용자의 입력 (ex. 아이디는 영소문자, 숫자로 이뤄진 8~16자리) 들을 검사할 수 있다. 오! error을 throw 한다.

import도 가능하고

```javascript
const Project = sequelize.import(__dirname + "/path/to/models/project")
```

오....

### 오늘 개발한거 

### 오늘 느낀점 

forign key 설정하는 방법중 직접 reference 정의하는 방법과 association을 이용하는 방법이 있는데 언능 공부해서 변경시켜야겠다.


