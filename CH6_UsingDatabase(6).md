
# CH6-6 MySQL 데이터베이스 사용하기

## 관계형 데이터베이스 간단하게 이해하기.

- 관계형 데이터베이스는 데이터베이스 저장소 안에 테이블(Table)을 만들어서 데이터를 저장한다. 테이블은 엑셀에서 Sheet와 비슷하다. 즉, 엑셀의 시트 탭에 어떤 이름의 데이터가 어떤 타입(type)으로 들어갈지 정의한 후 데이터를 한 줄씩 입력하는 과정과 비슷하다.한 줄에 해당하는 데이터를 레코드(Record) 라고 부르며, 이 레코드가 들어 있는 테이블은 검색할 수 있다. 
[p258 사진]

- 관계형 데이터베이스를 다루는 일은 크게 4 단계로 나뉜다. 

단계 | 설명
-----:|:-----
1단계 : 데이터베이스 연결 | 어떤 데이터베이스에 연결할지 지정하는 정보와 보안 정보를 입력. ex) connectiionLimit:10,host:'localhost',user:'root',password:'xxxx',database:'users',debug:false
2단계 : 테이블 생성 | 테이블을 만들어 어떤 칼럼에 어떤 타입의 데이터가 들어가는지 정의한다. ex) CREATE TABLE users(id text,name text,age int,password text)
3단계 : 레코드 추가 | 레코드를 한 줄 추가한다. ex) INSERT INTO users(id,name,age,password) VALUES('101','김진수',20,'123456')
4단계 : 데이터 조회 | 원하는 조건을 넣어 데이터를 조회한다. ex) SELECT id,name,age,password FROM users WHERE age>10


## MySQL 설치하기

- MySQL을 먼저 설피하고 화면이 있는 관리 도구도 함께 설치한다. 그런 다음 설치한 관리 도구를 사용해서 데이터베이스와 테이블을 만든다. 그리고 노드 서버로 사용자를 추가하거나 데이터베이스에 있는 사용자 정보로 로그인하는 기능을 만들어 보겠다.


- 아래 링크로 접속하여 설치 파일을 다운로드 하자.
http://dev.mysql.com/downloads/
