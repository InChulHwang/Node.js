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
