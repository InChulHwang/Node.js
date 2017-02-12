# § 인덱스와 메소드 사용하기

> 몽고디비는 데이터베이스이므로 빠른 검색을 위해 각각의 속성에 인덱스를 만들 수 있다.

> ＊ 인덱스 : 데이터베이스 분야에 있어서 테이블에 대한 동작의 속도를 높여주는 자료 구조를 일컫음

> 아래와 같이 스키마를 만들면서 인덱스 정보를 추가할 수 있다.

```shell
var UserSchema = new mongoose.Schema({
  id: {type: String, required: true, unique: true},
  password: {type: String, required: true},
  name: {type: String, index: 'hashed'},
  age: Number,
  created_at: {type: Date, index: {unique: false, expires: '1d'}},
  updated_at: Date
});
```

> 인덱스를 사용하면 검색 속도가 빨라지므로 조회가 필요한 속성에는 인덱스를 만들어 두는 것이 좋다!

> 위치 기반 서비스를 위해 저장되는 경위도 좌표에는 공간 인덱싱을 사용해야 하며 {type: [Number], index: '2d', sparse: true} 와 같은 형태로 객체를 만들어 설정할 수 있다.

메소드 이름 | 설명
-----:|:-----
static(name, fn) | 모델 객체에서 사용할 수 있는 함수 등록. 함수의 이름과 함수 객체를 파라미터로 전달
method(name, fn) | 모델 인스턴스 객체에서 사용할 수 있는 함수 등록. 함수의 이름과 함수 객체를 파라미터로 전달

> 위 표는 mongoose 스키마에서 메소드를 추가하는 두 가지 방법을 말한다.

## 사용자 리스트 조회 기능 추가

【app4.js (cont'd of app3.js)】

```shell
...
// Definition of Schema
UserSchema = mongoose.Schema({
  id: {type : String, required : true, unique : true},
  password: {type : String, required : true},
  name : {type : String, index : 'hashed'},
  age : {type : Number, 'default' : -1},
  created_at : {type : Date, index : {unique : false}, 'default' : Date.now},
  updated_at : {type : Date, index : {unique : false}, 'default' : Date.now}
});

```

> 이후 static() 메소드를 사용하여 스키마에 메소드를 추가한다.

【app4.js (cont'd)】

```shell
// Adding static() on Schema

UserSchema.static('findById', function(id, callback) {
  return this.find({id : id}, callback);
});

UserSchema.static('findAll', function(callback) {
  return this.find({}, callback);
});

console.log('UserSchema 정의함');

// Define UserModel

UserModel = mongoose.model("users2", UserSchema);
console.log('UserModel 정의함');

```

> findById 함수 : 모델 객체에서 호출할 수 있는 함수. id 속성을 전달받아 데이터베이스에서 검색 후 콜백 함수 쪽으로 결과 전달

> findAll 함수 : users2 컬렉션의 모든 문서 데이터를 반환

> showMe 메소드를 method() 함수로 추가하면 모델 인스턴스 객체에서 사용할 수 있다.

【app4.js (cont'd)】

```shell

// 사용자 인증 함수 : 아이디로 먼저 찾고 비밀번호를 비교

var authUser = function(database, id, password, callback) {
  console.log('authUser 호출됨.');

  // 1. 아이디를 사용해 검색
  UserModel.findById(id, function(err, results) {
    if(err) {
      callback(err, null);
      return;
    }
  
  console.log('아이디 [%s]로 사용자 검색 결과', id);
  console.dir(results);

  if(results.length > 0) {
    console.log('아이디와 일치하는 사용자 찾음.');

    // 2. 비밀번호 확인
    if(results[0]._doc.password == password) {
      console.log('비밀번호 일치');
      callback(null, results);
    } else {
      console.log('비밀번호 불일치');
      callback(null, null);
    }
  } else {
    console.log("아이디와 일치하는 사용자를 찾지 못함.");
    callback(null, null);
    }
  });
}
```

> _doc 속성 : 각 문서 객체의 정보를 담고 있어 그 안에 있는 password 속성 값을 확인할 수 있다.

> 아래는 사용자 리스트를 조회하는 메소드를 추가하는 과정이다.

【app4.js (cont'd)】

```shell
...

// User List Function
app.post('/process/listuser', function(req, res) {
  console.log('/process/listuser 호출됨.');
  
  if(database) {
    // 1. 모든 사용자 검색
    UserModel.findAll(function(err, results) {
      if(err) {
        callback(err, null);
        return;
       }
       
       if(results) {
        console.dir(results);
        
        res.writeHead('200', {'Content-Type' : 'text/html;charset=utf8'});
        res.write('<h2>사용자 리스트</h2>');
        res.write('<div><ul>');
        
        for(var i = 0; i < results.length; i++) {
          var curId = results[i]._doc.id;
          var curName = results[i]._doc.name;
          res.write('   <li>#' + i + ' : ' + curId + ' , ' + curName + '</li>');
        }
        
        res.write('</ul></div>');
        res.end();
      } else {
        res.writeHead('200', {'Content-Type' : 'text/html;charset=utf8'});
        res.write('<h2>사용자 리스트 조회 실패</h2>');
        res.end();
        }
    });
  }
});
```

【/public/listuser.html】

```shell
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>사용자 리스트 테스트</title>
  </head>
<body>
  <h1>사용자 리스트</h1>
  <hr>
  <form method="post" action="/process/listuser">
    <table>
      <tr>
        <td><label>아래 [전송] 버튼을 누르세요.</label></td>
      </tr>
    </table>
    <input type="submit" value="전송" name="">
  </form>
</body>
</html>
```

> 이로써 사용자 정보를 우리도 어느 정도 관리를 할 수 있게 되었다!

# § 비밀번호 암호화하여 저장하기

> 우리가 자주 쓰는 비밀번호를 그냥 저장하면 노출되기 쉽다.

> 그렇다면 이 비밀번호를 암호화하려면 어떻게 해야할까?

> Mongoose를 사용하면 비밀번호를 암호화하여 저장을 할 수 있도록 구현할 수 있다!

## virtual 함수 사용하기

> 실무에서는 사용자 정보를 데이터베이스에 저장할 때 비밀번호를 암호화하여 저장하는 경우가 많다.

> 이 때 비밀번호는 단방향 암호화하여 원본 비밀번호 문자열을 알 수 없도록 만든다.

> ＊ 단방향 암호화 : Encryption만 가능, Decryption 불가능

> ＊ 양방향 암호화 : Encrpytion 및 Decryption 가능

> mongoose에서 제공하는 virtual() 함수를 사용하면 비밀번호 암호화 과정이 더 쉽게 처리가 가능하다.

> mongoose에서 쓰이는 virtual() 함수의 특징은 문서 객체에 실제로 저장되는 속성이 아니라 가상의 속성을 지정할 수 있다는 점이다.

> 예를 들어 비밀번호를 저장하기 위해 문서 객체에 만들어지는 속성 이름이 hashed_password 라고 하면,

> 실제로 저장되는 속성은 아니지만 password 속성을 만든 후 이 속성에 값을 넣어 저장할 때 hashed_password 속성으로 저장되도록 만들 수 있다.

## 스키마 객체의 virtual() 함수 사용법

【virtual_test1.js】

```shell

// 모듈 불러들이기 //

var mongodb = require('mongodb');
var mongoose = require('mongoose');

// 데이터베이스 연결 //

var database;
var UserSchema;
var UserModel;

// 데이터베이스 연결 후 응답 객체의 속성으로 db 객체 추가
function connectDB() {
  // 데이터베이스 연결 정보
  var databaseUrl = 'mongodb://localhost:27017/shopping';
  
  // 데이터베이스 연결
  mongoose.connect(databaseUrl);
  database = mongoose.connection;
  
  database.on('error', console.error.bind(console, 'mongoose connection error.'));
  database.on('open', function() {
    console.log('데이터베이스에 연결되었습니다. : ' + databaseUrl);
    
    // user 스키마 및 모델 객체 생성
    createUserSchema();
    
    // test
    doTest();
    
  });
  database.on('disconnected', connectDB);
}

```

【virtual_test1.js (cont'd)】
```shell
...
// user 스키마 및 모델 객체 생성
function createUserSchema() {

  // 스키마 정의
  // password를 hashed_password로 변경, default 속성 모두 추가, salt 속성 추가
  UserSchema = mongoose.Schema({
    id : {type : String, required : true, unique : true},
    name : {type : String, index : 'hashed', 'default' : ''},
    age : {type : Number, 'default' : -1},
    created_at : {type : Date, index : {unique : false}, 'default' : Date.now},
    updated_at : {type : Date, index : {unique : false}, 'default' : Date.now}
  });
  
  // info를 virtual 메소드로 정의
  UserSchema
    .virtual('info')
    .set(function(info) {
      var splitted = info.split(' ');
      this.id = splitted[0];
      this.name = splitted[1];
      console.log('virtual info 설정함 : %s %s', this.id,, this.name);
    })
    .get(function() {return this.id + ' ' + this.name});
    
  console.log('UserSchema 정의함.');
  
  // UserModel 모델 정의
  UserModel = mongoose.model("users4", UserSchema);
  console.log('UserModel 정의함.');
}

```

> split() : 전달된 파라미터를 띄어쓰기 기호로 분리

