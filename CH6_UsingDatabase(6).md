
# CH6-6 MySQL 데이터베이스 사용하기

## 관계형 데이터베이스 간단하게 이해하기.

- 관계형 데이터베이스는 데이터베이스 저장소 안에 테이블(Table)을 만들어서 데이터를 저장한다. 테이블은 엑셀에서 Sheet와 비슷하다. 즉, 엑셀의 시트 탭에 어떤 이름의 데이터가 어떤 타입(type)으로 들어갈지 정의한 후 데이터를 한 줄씩 입력하는 과정과 비슷하다.한 줄에 해당하는 데이터를 레코드(Record) 라고 부르며, 이 레코드가 들어 있는 테이블은 검색할 수 있다. 
[p258 사진]

- 관계형 데이터베이스를 다루는 일은 크게 4 단계로 나뉜다. 

단계 | 설명
-----:|:-----
1단계 : 데이터베이스 연결 | 어떤 데이터베이스에 연결할지 지정하는 정보와 보안 정보를 입력. ex) connectiionLimit: 10 , host : 'localhost' , user : 'root' , password : 'xxxx' , database : 'users' , debug : false
2단계 : 테이블 생성 | 테이블을 만들어 어떤 칼럼에 어떤 타입의 데이터가 들어가는지 정의한다. ex) CREATE TABLE users(id text , name text , age int , password text)
3단계 : 레코드 추가 | 레코드를 한 줄 추가한다. ex) INSERT INTO users(id , name , age , password) VALUES('101' , '김진수' , 20 , '123456')
4단계 : 데이터 조회 | 원하는 조건을 넣어 데이터를 조회한다. ex) SELECT id,name,age,password FROM users WHERE age>10


## MySQL 설치하기

- MySQL을 먼저 설피하고 화면이 있는 관리 도구도 함께 설치한다. 그런 다음 설치한 관리 도구를 사용해서 데이터베이스와 테이블을 만든다. 그리고 노드 서버로 사용자를 추가하거나 데이터베이스에 있는 사용자 정보로 로그인하는 기능을 만들어 보겠다.


- 다음 링크로 접속하여 설치 파일을 다운로드 하자.

http://dev.mysql.com/downloads/


- [Downloads]메뉴 중 Community버전은 무료로 사용할 수 있다. 다운로드 목록 위쪽에 있는 MySQL Community Server링크를 클릭하면 다운로드 페이지로 이동한다. MySQL Installer MSI를 다운로드 한다.

- Download MySQL Installer 페이지가 나타나면 아래쪽의 용도에 맞는 MSI Installer를 설치한다. 여기서는 web에서만 사용하도록 제안된 버전이 아니라 전체 설치가 가능한 Windows(x86.32-bit), MSI Installer를 설치하면 된다.

- 오라클 계정으로 로그인 하거나 새로 회원가입을 진행하라는 페이지가 나타난다. 이 과정은 나중에 진행하고, 하단에 있는 Nothanks, just start my download링크를 클릭하여 설치파일을 다운로드 한다.

- 설치파일을 실행하고 아래쪽에 있는 I accept in license terms 항목을 클릭 후 라이선스에 동의한 후 Next버튼을 누른다. 설치 유형을 선택하는 화면이 나오면 Developer Default 항목을 선택한 상태에서 Next를 누른다.

- MySQL을 설치하게 전에 필요한 다른 제품들을 설치하기를 권하는 Check Requirements 화면이 나타난다. 아래의 Execute버튼을 누른다. 그 후 라이선스 동의를 체크하고 Install버튼을 눌러 설치한다. 설치가 끝난 후 Modify화면이 나타나면 Repair 버튼을 누른 다음 Close버튼을 누르면 설치가 완료 된다.

- 설치 항목이 체크되면 Next를 누른다. 이때 하나 이상의 제품을 요청하면 만족하지 못한 결과를 가져올 수 있다는 경고 창이 나타난다. 우선 예 를 눌러 넘어간다. 

- 설치 항목이 표시된 화면이 보이면 아래쪽의 Execute버튼을 눌러 설치를 진행한다. 설치가 끝난 항목의 Status내용은 Complete로 바뀐다. 모두 끝나면 Next를 누른다.

- Product Configuration 화면이 나타나면 Next를 누른다. 네트워크 설정을 어떻게 할 것인지 물어보는 화면이 나온다. Config Type콤보 박스에는 Development Machine이 선택 되어 있고 Port Number입력 상자에는 3306포트가 기본 값으로 설정되어 있다. 기본 값을 그대로 두고 아래쪽의 Next버튼을 누르면 계정 정보를 물어본다. 루트 권한을 가진 계정의 비밀번호는 직접 정하여 두개의 입력 상자에 똑같이 입력한 후 Next를 누른다.

- Windows Service화면이 나타나면 Next를 눌러 넘어간다. 그리고 Plugins and Extensions 화면도 Next를 눌러 넘어간다. 그러면 설정을 적용하는 화면이 나타나는데 여기에서도 Execute버튼을 눌러 진행한다. 각 항목이 하나씩 체크되는 설정 과정이 끝나면 Finish버튼을 눌러 다음 화면으로 넘어간다. 다시 Product Configuration화면이 나타나면 Next버튼을 눌러 넘어간다. Connect To Server 화면에서는 서버 연결을 확인할 수 있다. 아래쪽의 User입력 상자에는 root가 입력되어 있으며, Password입력 상자에는 우리가 정한 비밀번호가 자동으로 등록되어 있다. 아래쪽의 Check버튼을 누르면 MySQL서버에 정상적으로 연결되었다는 메세지가 CHeck버튼 오른쪽에 표시된다.

- Next버튼을 누르면 다시 설정을 적용하는 화면이 나타난다. 아럐쪽의 Excute버튼을 누르면 각 항목이 체크되면서 설정 과정이 진행된다. 모든 설정이 끝나면 아래쪽의 Finish버튼을 눌러 다음 화면으로 넘어간다. 다시 Product Configuration 화면이 보이면 Next버튼을 눌러 넘어간다. 그러면 설치가 완료되었다는 화면이 나타난다. 'Start MySQL Workbench agter Setup'체크 박스를 해제한 후 Finish 버튼을 누른다. MySQL설ㅊ가 끝났으니 MySQL데이터베이스를 사용할 수 있다. 그런데 데이터베이스를 사용할때 일일이 명령을 입력하는 작업은 번거롭다. 명령을 입력하기보다는 화면이 있는 관리 도구를 사용하는 것이 더 편리하다. 그래서 관리 도구중 하나인 HeidiSQL을 설치해보도록 하겠다.

- 다음 링크에 들어가서HeidiSQL을 설치해보도록 하자.

http://www.heidisql.com/download.php

- 설치 파일을 실행하면 Setup - HeidiSQL대화상자가 나타난다. Next버튼을 눌러 진행한 후 설치가 끝나면 Finishi버튼을 누른다.

- 설치 화면이 사라진 후 HeidiSQL의 [세션관리자] 대화상자가 나타난다. 세션은 데이터베이스에 연결하는 하나의 단위이다. 따라서 이 화면에서 연결 정보를 만들면 그 정보로 데이터베이스에 연결할 수 있다. 왼쪽 아래에 '신규'라고 표시된 콤보 박스의 아래모양 버튼을 클릭하면 [루트 폴더 내에 세션 생성] 메뉴를 선택할 수 있다.

- 새로운 세션 정보를 입력하는 화면이 보인다. 다른 항목들은 그대로 두고 사용자 입력 상자에 root, 암호 입력 상자에 우리가 만든 암호를 입력한 후 왼쪽 아래의 [저장] 버튼을 누른다. 그러면 세션 정보가 저장된다. 그다음 오른쪽 아래에 있는 [열기] 버튼을 누르면 데이터베이스에 연결한 후 새로운 창이 뜬다.

- 화면 왼쪽에는 이미 만들어져 있는 데이터베이스들이 보인다. 각각의 데이터베이스는 저장소를 의미하는데 HeidiSQL을 설치하면서 자동으로 여러 개의 데이터베이스가 만들어진다. 우리는 새로운 데이터베이스를 만들어 사용할 것 이다. 왼쪽 영역에서 Unnamed 항목을 더블클릭 한 후 나타는 창에서 Unnamed 항목을 오른쪽 클릭 한다. 빠른 메뉴 중에서 [새로생성 -> 데이터베이스]를 선택 한다. 그러면 [데이터베이스 생성] 대화상자가 보인다. 이름 입력 상자에 test를 입력한 후 [확인]을 누른다.

- 데이터베이스가 새로 만들어지면 왼쪽 목록에 test항목이 추가된다. 간단하게 데이터베이스를 다루는 4단계 중 첫번째 단계가 완료되었다. 이제 두 번째 단계인 테이블 생성을 진행해 보겠다. test항목을 오른쪽 클릭 후 [새로 생성 -> 테이블] 메뉴를 선택하자.

- 오른쪽 화면 위쪽에 새로운 [테이블 : Untitled]]탭이 만들어 진다. 화면에는 새로운 테이블의 정보를 입력할 수 있다. 이름 입력 상자에는 users를 입력하고 그 아래에 열 내용으로 칼럼 정보를 추가하자. 4개의 칼럼을 이름은 각각 id, name, age, password로, 데이터 유형은 각각 TEXT, TEXT, INT, TEXT로 추가 해 보자.

- 칼럼 정보를 모두 추가한 후 아래에 있는 [저장] 버튼을 눌러 테이블을 저장하자.

- 테이블이 생성되면 왼쪽의 test데이터베이스 항목 아래에 users테이블이 추가된다. 이 테이블 이름을 선택하고 오른쪽에 있는 탭 중에서 [데이터] 탭을 선택하면 데이터를 조회할 수 있다. 처음에는 데이터가 없기 때문에 레코드가 하나도 보이지 않지만 나중에 데이터를 넣은 후 조회하면 그 데이터를 볼 수 있고 수정할 수도 있다. 이제 2단계까지 진행이 되었다. 





## MySQL을 사용하는 사용자 추가 기능 만들기


다음 단계인 3단계 : 데이터 추가 와 4단계 : 데이터 조회는 익스프레스로 만든 웹 서버에서 진행한다. 노드에서 MySQL데이터베이스에 연결하려면 mysql모듈을 사용하면 된다. 먼저 cmd창에서 모듈을 설치한다.

```shell
npm install mysql --save
```

그 후 다음 파일을 만든다.

app8.js
```shell

/**
 * 데이터베이스 사용하기
 * 
 * 데이터베이스 열고 로그인 화면에 붙이기
 * 
 */

//===== 모듈 불러들이기 =====//
var express = require('express')
  , http = require('http')
  , path = require('path');

var bodyParser = require('body-parser');
var cookieParser = require('cookie-parser');
var expressSession = require('express-session');
var expressErrorHandler = require('express-error-handler');


//===== MySQL 데이터베이스를 사용할 수 있도록 하는 mysql 모듈 불러오기 =====//
var mysql = require('mysql');

//===== MySQL 데이터베이스 연결 설정 =====//
var pool      =    mysql.createPool({
    connectionLimit : 10, 
    host     : 'localhost',
    user     : 'root',
    password : 'admin',
    database : 'test',
    debug    :  false
});



//===== Express 서버 객체 만들기 =====//
var app = express();


//===== 서버 변수 설정 및 static으로 public 폴더 설정  =====//
app.set('port', process.env.PORT || 3000);
app.use('/public', express.static(path.join(__dirname, 'public')));

//===== body-parser, cookie-parser, express-session 사용 설정 =====//
app.use(bodyParser.urlencoded({extended: true}));

app.use(cookieParser());
app.use(expressSession({
   secret:'my key',
   resave:true,
   saveUninitialized:true
}));

//===== 라우터 미들웨어 사용 =====//


//사용자 추가 함수
app.post('/process/adduser', function(req, res) {
   console.log('/process/adduser 호출됨.');

   var paramId = req.param('id');
   var paramName = req.param('name');
   var paramAge = req.param('age');
   var paramPassword = req.param('password');
   
   if (당구) {
      addUser(paramId, paramName, paramAge, paramPassword, function(err, result) {
         if (err) {throw err;}
         
         if (result) {
            console.dir(result);

            console.log('inserted ' + result.affectedRows + ' rows');
              
              var insertId = result.insertId;
              console.log('추가한 레코드의 아이디 : ' + insertId);
              
            res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
            res.write('<h2>사용자 추가 성공</h2>');
            res.end();
         } else {
            res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
            res.write('<h2>사용자 추가  실패</h2>');
            res.end();
         }
      });
   } else {
      res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
      res.write('<h2>데이터베이스 연결 실패</h2>');
      res.end();
   }
   
});

//사용자를 등록하는 함수
var addUser = function(id, name, age, password, callback) {
   console.log('addUser 호출됨.');
   
   // 커넥션 풀에서 연결 객체를 가져옵니다.
   pool.getConnection(function(err, conn) {
        if (err) {
           conn.release();  // 반드시 해제해야 합니다.
          return;
        }   
        console.log('데이터베이스 연결 스레드 아이디 : ' + conn.threadId);

       // 데이터를 객체로 만듭니다.
       var data = {id:id, name:name, age:age, password:password};
       
        // SQL 문을 실행합니다.
        var exec = conn.query('insert into users set ?', data, function(err, result) {
           conn.release();  // 반드시 해제해야 합니다.
           console.log('실행 대상 SQL : ' + exec.sql);
           
           if (err) {
              console.log('SQL 실행 시 에러 발생함.');
              console.dir(err);
              
              callback(err, null);
              
              return;
           }
           
           callback(null, result);
           
        });
        
        conn.on('error', function(err) {      
              console.log('데이터베이스 연결 시 에러 발생함.');
              console.dir(err);
              
              callback(err, null);
        });
    });
   
}



// 로그인 처리 함수
app.post('/process/login', function(req, res) {
   console.log('/process/login 호출됨.');

   var paramId = req.param('id');
   var paramPassword = req.param('password');
   
   if (당구) {
      authUser(paramId, paramPassword, function(err, rows) {
         if (err) {throw err;}
         
         if (rows) {
            console.dir(rows);

            var username = rows[0].name;
            
            res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
            res.write('<h1>로그인 성공</h1>');
            res.write('<div><p>사용자 아이디 : ' + paramId + '</p></div>');
            res.write('<div><p>사용자 이름 : ' + username + '</p></div>');
            res.write("<br><br><a href='/public/login2.html'>다시 로그인하기</a>");
            res.end();
         
         } else {
            res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
            res.write('<h1>로그인  실패</h1>');
            res.write('<div><p>아이디와 패스워드를 다시 확인하십시오.</p></div>');
            res.write("<br><br><a href='/public/login2.html'>다시 로그인하기</a>");
            res.end();
         }
      });
   } else {
      res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
      res.write('<h2>데이터베이스 연결 실패</h2>');
      res.write('<div><p>데이터베이스에 연결하지 못했습니다.</p></div>');
      res.end();
   }
   
});


// 사용자를 인증하는 함수
var authUser = function(id, password, callback) {
   console.log('authUser 호출됨.');
   
   // 커넥션 풀에서 연결 객체를 가져옵니다.
   pool.getConnection(function(err, conn) {
        if (err) {
           conn.release();  // 반드시 해제해야 합니다.
          return;
        }   
        console.log('데이터베이스 연결 스레드 아이디 : ' + conn.threadId);
          
        var columns = ['id', 'name', 'age'];
        var tablename = 'users';
 
        // SQL 문을 실행합니다.
        var exec = conn.query("select ?? from ?? where id = ? and password = ?", [columns, tablename, id, password], function(err, rows) {
            conn.release();  // 반드시 해제해야 합니다.
            console.log('실행 대상 SQL : ' + exec.sql);
            
            if (rows.length > 0) {
              console.log('아이디 [%s], 패스워드 [%s] 가 일치하는 사용자 찾음.', id, password);
              callback(null, rows);
            } else {
               console.log("일치하는 사용자를 찾지 못함.");
              callback(null, null);
            }
        });

        conn.on('error', function(err) {      
            console.log('데이터베이스 연결 시 에러 발생함.');
            console.dir(err);
            
            callback(err, null);
      });
    });
   
}


//===== 404 에러 페이지 처리 =====//
var errorHandler = expressErrorHandler({
 static: {
   '404': './public/404.html'
 }
});

app.use( expressErrorHandler.httpError(404) );
app.use( errorHandler );


//===== 서버 시작 =====//
http.createServer(app).listen(app.get('port'), function(){
  console.log('서버가 시작되었습니다. 포트 : ' + app.get('port'));
});
```

이 코드는 몽고디비에 연결할 때 사용한 app.js파일에서 몽고디비를 연결할 때 추가한 코드를 모두 삭제 한 후 수정한 것 이다. 

[20~31] 코드 윗부분에 myql모듈을 불러오는 코드를 추가한 후 그 아래에 MySQL데이터베이스에 연결하는 코드를 추가한다.

관계형 데이터베이스에 연결할 때는 보통 커넥션 풀(Connextion Poll)을 사용한다. 이것은 데이터베이스 연결 객체가 너무 많이 만들어지는 것을 막고 한번 만든 연결을 다시 사용할 수 있게 한다. 데이터베이스에 연결하면 메모리 리소스를 많이 차지하므로 한번 만든 연결 객체는 커넥션 풀에 넣어 두고 다음번 요청이 있을 때 다시 사용한다. 이때 너무 많은 연결이 만들어지지 않도록 커넥션 풀의 최대 크기를 설정한다. 커넥션 풀은 연결 개수를 제한하므로 연결을 사용한 후에는 반드시 다시 풀어주어야 하는 제약이 있다. 커넥션 풀을 만들려면 mysql모듈의 createPool메소드를 호출해서 옵션 정보가 있는 객체를 넣어 준다. 객체 안에는 connectionLimit, host, port등의 연결 정보가 속성으로 들어간다.

속성 | 설명 
-----:|:-----
connectionLimit | 커넥션 풀에서 만들 수 있는 최대 연결 개수를 설정한다.
host | 연결할 호스트 이름을 설정한다. 내 컴퓨터인 경우 localhost또는 127.0.0.1을 입력할 수 있다.
port | 데이터베이스가 사용하는 포트 번호를 설정한다. MySQL의 디폴트 포트는 3306이다.
user | 데이터베이스 사용자 아이디를 설정한다. MySQL에서 루트 권한을 가진 디폴트 사용자 아이디는 root이다.
password | 데이터베이스 사용자의 비밀번호를 설정한다.
databse | 데이터베이스 이름을 설정한다.
debug | 데이터베이스 처리 과정을 로그로 남길 것인지 설정한다.

createPool 메소드를 호출하면 데이터베이스에 연결해서 커넥션 풀을 만들게 된다. 따라서 그 다음부터는 pool객체에서 연결 객체를 가져와 사용할 수 있다.

[94~135] MySQL데이터베이스에 연결하는 과정이 끝났으니 사용자 추가 기능을 넣어보자. 

addUser함수에는 다섯 개의 파라미터를 전달한다. id, name, age, password 파라미터는 웹 브라우저에서 요청할 때 전달한 요청 파라미터이며 callback은 결과를 처리할 콜백 함수이다. pool객체의 getConnection 메소드를 호출하면 커넥션 풀에서 연결 객체를 하나 가져올 수 있다. 연결 객체를 성공적으로 가져오면 콜백 함수가 호출되면서 conn파라미터로 연결 객체가 전달된다. 연결 객체에는 query메소드가 있어 SQL문을 실행할 수 있다. 여기에서는 데이터를 추가하는 INSERT 문을 사용했다. SQL문을 만들어 실행할 때는 SQL문 안에 ? 기호를 넣을 수 있는 이 기호는 query메소드를 호출할 때 전달하는 추가 파라미터를 사용해 대체한 후 실행된다. 즉, data변수에 {id : id , name : name , age : age , password : password } 객체를 할당한 후 query메소드를 호출하면서 파라미터로 전달하면 SQL문은 다음과 같은 형태로 만들어 진 후 실행된다.

```shell
insert into users set id='test01', name='김준수',age=20, password='123456'
```

SQL문이 실행되면 콜백 함수가 호출되면서 결과가 result파라미터로 전달된다. SQL문을 실행한 후에는 연결 객체의 release메소드를 호출하여 연결 객체를 커넥션 풀로 반환해야 한다. 실행 결과는 콜백 함수 쪽에서 처리할 수 있도록 callback(null,result)코드를 넣어 콜백 함수를 실행한다.
