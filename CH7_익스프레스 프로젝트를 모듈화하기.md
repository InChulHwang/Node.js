# 07-1 모듈화 방법 자세히 살펴보기.


지금까지 만든 사용자 기능이나 데이터베이스 저장 기능은 모두 하나의 app.js 파일 안에 들어있다. 따라서 기능이 많아질수록 하나의 파일에 들어가는 코드의 양도 많아질 것 이다. 결국 코드를 분석하기 어려워진다. 이 문제를 해결하려면 기능별로 코드를 구분한 후 독립된 파일로 분리시키는 것이 좋다. 이렇게 별도의 파일로 분리한 것을 모듈 이라고 한다.

익스프레스로 웹 서버를 만드는 과정에서 이미 모듈에 대해 알아보았다. 이 모듈은 구성 방법에 따라 몇 가지 전형적인 코드 패턴을 갖는다. 여기에서는 모듈을 더 자세히 알아본 후 DatabaseExample프로젝트의 app5.js 파일에서 구현한 각 기능을 모듈로 분리하여 구성해 보겠다.

## 다양항 방법으로 모듈 만들기

가장 기본적인 모듈 사용 방법은 자바스크립트 파일을 새로 만드는 것부터 출발한다. 그리고 그 파일 안에서 exports 전역 변수를 사용한다. [p283 사진]

exports 전역 변수는 어디에서나 접근할 수 있게 정의된 것 이고 속성도 추가할 수 있다. 즉, 새로운 파일을 만들고 그 안에서 exports전역 변수에 속성을 추가하면 다른 파일에서도 exports전역 변수의 속성을 참조할 수 있다. 예를들어, 사용자 정보를 확인하려고 만든 기능을 별도의 user1.js 자바스크립트 파일로 분리한 후 모듈로 구성하려면 getUser()함수와 group객체를 exports의 속성으로 추가해야 한다. 이렇게 만든 모듈은 module_test1.js처럼 이 모듈을 사용할 파일에서 require()메소드로 불러들일 수 있다. require()메소드로 불러들인 모듈을 변수에 할당하면 그 변수에는 exports가 속성으로 할당된다. 따라서 속성이 할당된 변수에서는 exports에 추가한 getUser()함수나 group객체를 참조하여 사용할 수 있다.

위와 같은 구조를 만들기 위해 먼저 새로운 프로젝트 ModuleExample을 만든 후 user1.js 파일과 module_test1.js 파일을 만들자. 

user1.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * exports 이용하기
 */

// exports 객체 속성으로 함수 추가
exports.getUser = function() {
   return {id:'test01', name:'소녀시대'};
}

// exports 객체 속성으로 객체 추가
exports.group = {id:'group01', name:'친구'};
```

exports에 속성 두 개가 추가 되었다. 첫 번째 속성은 getUser 속성이며 속성의 값으로 함수가 할당되었다. 이 함수는 id와 name속성을 가진 객체를 반환하는데, 사용자 정보를 객체로 반환받아 확인하려고 만든 함수입니다. 두 번째 속성은 group속성이며 id와 name속성을 가진 객체가 할당되었다. 이것은 그룹의 아이디와 이름을 알 수 있도록 만든 객체이다. 이렇게 만든 모듈 파일은 module_test1.js 파일을 불러들여 사용한다.


module_test1.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * main 함수
 */

// require() 메소드는 exports 객체를 리턴함
var user = require('./user1');

function showUser() {
   return user.getUser().name + ', ' + user.group.name;
}

console.log('사용자 정보 : %s', showUser());
```

user1.js 파일로 만든 모듈을 사용하기 위해 require() 메소드를 호출한다. require()메소드로 불러들인 모듈은 user1 변수에 할당된다. require()메소드는 exports 객체를 반환하기 때문에 user1변수는 exports 객체를 참조한다. 이제 user1변수는 exports객체와 같다고 볼 수 있으므로 user1변수에 들어 있는getUser()함수를 호출할 수 있다. showUser()함수는 콘솔 창에 로그를 출력하는 간단한 기능을 수행한다. showUser()함수 안에 들어 있는 코드는 user변수의getUser()메소드를 호출하여 반환받은 객체의 name속성을 참조하여 사용자의 이름을 알아낸다. 그리고 group객체의 name속성을 참조하여 그룹 이름을 알아낸 후 두 개의 문자열을 나란히 붙여서 반환한다. 

module_test1.js 를 실행해 보자. 그 결과는 user1변수가 exports객체를 참조하여 출력한 것 이다.



## exports에 객체 저장하기

export에 객체를 할당할 수 있는지 확인해 보기 위해 user2.js파일을 만들어보자.

user2.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * exports 이용하기
 */

// user1.js의 코드는 exports에 객체를 할당하는 것과 같으나, exports에는 속성만 추가할 수 있고 객체를 할당할 수는 없음
// Reason : exports는 속성으로, exports에 속성을 추가하면 모듈에서 접근하지만,
//          exports에 객체를 할당하면 자바스크립트에서 새로운 변수로 처리함
exports = {
   getUser: function() {
      return {id:'test01', name:'소녀시대'};
   },
   group: {id:'group01', name:'친구'}
}
```
이렇게 코드를 입력하면 getUser()함수와 group객체를 속성으로 가진 객체를 만들어 exports에 할당하게 된다. 따라서 exports속성으로 getUser() 함수와 group객체를 추가한 것과 같은 효과를 낸다. 일반적인 자바스크립트 코드에서는 user1.js파일의 코드와 user2.js파일의 코드가 같은 것처럼 보이는 것이다. 그럼 이 코드가 정상적으로 작동하는지 확인하기 위해 module_test2.js를 만들어 보자.


module_test2.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * main 함수
 */

// users2는 {} 로 리턴됨
// 오류 발생함
// Reason : user2.js 파일에서 exports에 객체를 할당하였으므로,
//          require() 호출 시 자바스크립트에서 새로운 변수로 처리함
//          결국 아무 속성도 없는 {} 객체가 리턴됨
var user = require('./user2');

console.dir(user);

function showUser() {
   return user.getUser().name + ', ' + user.group.name;
}

console.log('사용자 정보 : %s', showUser());
```

module_test1.js파일에 입력한 코드와 같지만 require()메소드로 불러들이는 모듈 파일의 이름을 ./user2로 하였다. 그리고 user변수에 할당한 객체의 속성 값을 확인하기 위해 console.dir()메소드를 호출하였다. 그런데 module_test2.js파일을 실행시켜보면 오류가 표시되면서 정상적으로 실행되지 않는다.

잘 보면 user변수에는 객체가 할당되었지만 어떤 속성도 들어 있지 않다. 이 때문에 user변수의 getUser() 메소드를 호출할 때 오류가 발생한 것이다. 왜 모듈을 불러와도 모듈 파일 안에서 exports에 할당한 객체가 보이지 않는 것 일까?

노드는 모듈을 처리할 때 exports를 속성으로 인식한다. 이 속성에 함수나 객체를 속성으로 추가하면 모듈을 불러들인 쪽에서 exports에 추가된 속성들을 참조할 수 있다. 그러나 exports에 객체를 할당하면 모듈 파일 안에서 선언한 exports는 모듈 시스템에서 처리할 수 있는 전역 변수가 아닌 단순 변수로 인식된다. 이 때문에 모듈을 불러들인 쪽에서 exports를 참조할 수 없게 되고 결과적으로 모듈 파일을 불러들일 때 그 결과 객체에는 아무것도 들어있지 않게 된다. [p287 사진]

![287](https://github.com/SKKUMathcom/Node.js/blob/master/image/287.png)

결국 user2.js파일에서 exports에 객체를 할당하면 전역 변수 exports와 무관한 모듈 파일 안의 변수가 되고 이 모듈을 불러들인 쪽에서는 비어 있는 exports 전역 변수만 참조하게 된다.


## module.exports를 사용해서 객체를 그대로 할당하기

자바스크립트 코드를 만들 때 객체 안에 속성을 넣어 두는 경우가 많다. 따라서 모듈 파일에서 이 모듈을 불러들인 쪽으로 객체를 전달하는 것이 훨씬 편할 수 있다. 그렇다면 객체를 그대로 할당하는 방법을 없을까? exports가 아니라 module.exports를 사용하면 객체를 그대로 할당할 수 있다. module.exports를 사용해 보기 위해 user3.js 만들자.

user3.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * module.exports 이용하기
 */

// 객체를 그대로 할당할 수 있음
var user = {
   getUser: function() {
      return {id:'test01', name:'소녀시대'};
   },
   group:{id:'group01', name:'친구'}
}
   
module.exports = user;
```
getUser()메소드와 group객체를 속성으로 갖는 객체를 새로 만들고 user변수에 할당한다. 이 user변수는 다시 module.exports에 그대로 할당 되었다. 이제 user3.js 모듈을 사용하는 module_test3.js를 만들자.

module_test3.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * main 함수
 */

// require() 메소드는 객체를 리턴함
var user = require('./user3');

function showUser() {
   return user.getUser().name + ', ' + user.group.name;
}

console.log('사용자 정보 : %s', showUser());
```

module_test3.js 코드는 module_test1.js파일의 코드와 같으며 require()메소드에 전달되는 파일 이름만 ./user3로 변경되었다. 이 파일을 실행하면 module_test1.js파일을 실행했을 때와 똑같은 결과가 콘솔 창에 표시된다. 모듈을 만들 때 module.exports에 객체를 그대로 할당하는 방식으로 맏르었다는 점과 이 모듈을 require()메소드로 불러들이면 그 객체가 그대로 반환된다는 점을 잘 이해하자. [p288 사진]

exports에 객체를 그대로 할당하면 모듈 파일을 불려들인 쪽에서 그 객체를 참조할 수 없지만, module.exports에 객체를 그대로 할당하면 모듈 파일 안에서 할당한 객체를 참조할 수 있다. 이 때문에 아무런 오류가 발생하지 않는다.


## module.exports에 함수만 할당하기

module.exports에는 함수도 할당할 수 있다. 함수도 객체이기 때문이다. user4.js를 만들자.

user4.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * module.exports 이용하기
 */

// 인터페이스(함수 객체)를 그대로 할당할 수 있음
 
module.exports = function() {
   return {id:'test01', name:'소녀시대'};
};
```

이 파일에서는 익명 함수를 만들어 module.exports에 할당한다. 익명 함수에 들어 있는 코드는 앞에서 만든 getUser 함수의 코드와 같다. 그리고 나서 이 모듈 파일을 불러들여 사용하는 기능을 module_test4.js 파일에 입력한다.


module_test4.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * main 함수
 */

// require() 메소드는 함수를 리턴함
var user = require('./user4');

function showUser() {
   return user().name + ', ' + 'No Group';
}

console.log('사용자 정보 : %s', showUser());
```

require()메소드를 호출하면 모듈 파일 안에서 module.exports에 할당한 익명 함수가 반환된다. 따라서 이 익명 함수를 user변수에 할당했다면 user()와 같이 소괄호를 붙여 함수를 실행할 수 있다. showUser()함수 안에서 suer변수에 할당한 함수를 실행하면 id와 name속성을 가진 객체가 반환되므로 그 객체의 name속성을 참조할 수 있다. 

module_test4.js를 실행하면 콘솔에서 확인할 수 있는 것처럼, 모듈 파일 안에서 module.exports에 함수를 할당하면 모듈을 불러들인 쪽에서 그 함수를 그대로 참조할 수 있다. 따라서 반환된 함수에 소괄호만 붙여 주면 함수를 그대로 실행할 수 있다. user변수에 할당된 함수를 실행하면 사용자 정보가 들어 있는 객체가 반환되므로 그 객체의 name속성을 사용할 수 있다. [p290 사진]


## exports와 module.exports를 함께 사용하기

exports와 module.exports를 함께 사용하면 어떻게 될까? 이 두 가지를 함께 사용하면 module.exports가 우선으로 적용된다. 즉 모듈을 불러오는 쪽에서는 module.exports에 할당된 객체나 속성을 참조할 수 있으며 exports 전역 변수는 무시된다.

- exports를 사용하면 모듈을 불러들인 쪽으로 객체를 전달할 수도 없을뿐더러 module.exports에 의해 무시되는 상황이 발생할 수 있다는 점이 주의하자. 따라서 실무에서는 module.exports사용을 권장한다.


user5.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * module.exports 이용하기
 */

// module.exports가 사용되면 exports는 무시됨

module.exports = {
   getUser: function() {
      return {id:'test01', name:'소녀시대'};
   },
   group:{id:'group01', name:'친구'}
}

exports.group = {id:'group02', name:'가족'};
```

module.exports에 객체를 그대로 할당했는데 그 객체에는 group속성이 들어 있다. exports에도 group속성을 추가하고 그룹의 이름에 '가족'이라는 정보를 넣었다.

module_test5.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * main 함수
 */

// require() 메소드는 export가 아닌 module.exports로 설정된 속성을 리턴함
var user = require('./user5');

function showUser() {
   return user.getUser().name + ', ' + user.group.name;
}

console.log('사용자 정보 : %s', showUser());
```


이 파일을 실행하면 그룹의 name 속성에 '친구'라는 글자가 들어 있고 모듈 파일 안에서 exports.group 으로 할당한 '가족'이라는 그룹 이름은 적용되지 않았음을 확인할 수 있다.

이제 require()메소드를 호출했을 때 모듈을 어떻게 불러오고 불러온 모듈을 어떻게 사용할 수 있는지 어느정도 이해가 되었는가? 그렇다면 require()메소드를 호출했을 때 모듈 파일에서 설정한 객체나 함수를 그대로 반환받을 수 있다는 것도 이해할 수 있을 것 이다.


## require()메소드의 동작 방식 이해하기

require()메소드는 다른 모듈 파일에서 만든 객체를 참조할 수 있다. 이 require()메소드를 더 쉽게 이해하려면 가상의 함수를 만들어 보는 것도 좋은 방법이다. module_test6.js를 만들어 보자.

module_test6.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * require() 함수를 만들어보기
 */

// 가상으로 require() 함수를 정의해 보면 require() 함수가 내부적으로 처리되는 방식을 이해할 수 있음
var require = function(path) {
   var exports = {
      getUser: function() {
         return {id:'test01', name:'소녀시대'};
      },
      group:{id:'group01', name:'친구'}
   }
   
   return exports;
}

var user = require('...');

function showUser() {
   return user.getUser().name + ', ' + user.group.name;
}

console.log('사용자 정보 : %s', showUser());
```

여기에서는 모듈을 불러올 때 사용한 require()함수가 아닌, 우리가 직접 require()함수를 새로 만들었다. 이 require()함수는 한 개의 파라미터를 입력받으며, 이 파라미터를 모듈 파일의 이름처럼 전달한다. require()함수 안에서는 exports 변수를 만들어 객체를 할당했는데, 함수의 마지막 부분에서 exports변수를 반환하도록 만들면 require()메소드를 호출했을 때 이 객체를 반환받게 된다.

이제 그 아랫부분에서 require() 메소드를 호출한 후 반환받은 객체의 메소드를 호출하거나 속성을 참조하도록 반든다. 이 부분은 모듈 파일을 불러올 때 사용한 코드와 똑같이 구성한다. 따라서 이 module_test6.js파일을 실행하면 모듈 파일을 불러와 사용했을 때와 똑같은 결과를 얻을 수 있다.

이렇게 함수를 직접 만들어 실행해 보면 모듈 파일을 불러올 때 사용한 require()함수가 어떻게 동작하는지 이해할 수 있다. 그리고 어떤 함수나 객체가 있을 때 이것을 별도의 모듈 파일로 분리시키는 방법이 아주 자연스러운 자바스크립트의 코드 사용 패턴 중 하나라는 것을 알게 될 것이다.

책의 p293의 사진은 모듈 파일을 만들어 불러들이는 경우와 조금 전 직접 만든 코드를 비교해서 보여준다.

별도의 모듈 파일로 분리하면 module.exports에 객체를 할당해야 하고 이 모듈 파일을 require()메소드로 불러와서 변수에 할당하는 과정이 추가된다. 그러나 exports를 사용하지 않고 module.exports에 객체를 할당하는 방식으로만 모듈을 구성한다면, 모듈을 분리하는 과정이 생각보다 복잡하지는 않다.


## 모듈을 분리할 때 사용하는 전형적인 코드 패턴

이제 모듈로 분리하여 구성할 때 사용하는 전형적인 코드 패턴 세 가지를 살펴보겠다. 첫 번째 패턴은 이미 살펴본 것처럼 모듈 파일 안에서 함수를 할당한다. 그리고 두 번째 패턴은 모듈 파일 안에서 인스턴스 객체를 할당하며, 세 번째 패턴은 모듈 파일 안에서 프로토타입 객체를 할당한다. 

코드 패턴 | 설명
-----:|:-----
함수를 할당하는 경우 | 모듈 안에서 함수를 만들어 할당한다. 모듈을 불러온 후 소괄호를 붙여 모듈을 실행한다.
인스턴스 객체를 할당하는 경우 | 모듈 안에서 인스턴스 객체를 만들어 할당한다. 모듈을 불러온 후 해당 객체의 메소드를 호출하거나 속성을 사용할 수 있다.
프로토타입 객체를 할당하는 경우 | 모듈 안에서 프로토타입 객체를 만들어 할당한다. 모듈을 불러온 후 new 연산자로 인스턴스 객체를 만들어 사용할 수 있다.

모듈을 구성할 때 사용하는 세 가지 코드 패턴은 다시 exports전역 변수에 속성으로 추가하는 경우와 module.exports에 할당하는 경우로 나눌 수 있다. 다만 앞에서 설명한 것 처럼 module.exports 사용을 권장한다.

## 함수를 할당하는 코드 패턴

그러면 먼저 첫 번째 코드 패턴을 만들어 보겠다. 

user7.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports에 속성으로 추가된 함수 객체를 그대로 참조한 후 호출함

exports.printUser = function() {
   console.log('user 이름은 소녀시대입니다.');
};
```

exports전역 변수에 printUser속성을 추가했으며 이 속성에 함수를 할당하였다. 이 패턴은 앞에서도 만들어 보았으므로 쉽게 이해될 것 이다. 

이 묘듈 파일을 사용하는 module_test7.js를 만들자

module_test7.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports에 속성으로 추가된 함수 객체를 그대로 참조한 후 호출함

var printUser = require('./user7').printUser;

printUser();
```

require()메소드로 모듈을 불러들이면 exports자체가 참조된다. exports의 printUser속성이 함수이므로 모듈을 불러들이면서 그 속성을 바로 참조하면 다음과 같은 코드를 구성할 수 있다.

```shell
require('./user7').printUser;
```
exports가 아닌 exports의 printUser 속성을 변수에 할당하였다. 여기에서 printUser속성이 함수이므로 함수를 실행하려면 printUser()처럼 변수 이름 뒤에 소괄호를 붙여준다. [p295 사진]

module.exports 에는 객체를 직접 할당할 수 있으므로 exports가 아니라 module.exports에 객체를 할당하고 그 안에 다시 함수를 속성으로 추가하는 경우에도 모듈을 불러와 사용하는 쪽의 코드 패턴은 위 사진의 구조와 같다.


## 인스턴스 객체를 할당하는 코드 패턴

두 번째 패턴은 모듈을 위해 만든 파일 안에서 인스턴스 객체를 만들어 할당한다. 먼저 인스턴스 객체를 만들어 module.exports에 직접 할당하는 예를 만들어 보겠다.

user8.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : module.exports에 인스턴스 객체를 만들어 할당함

// 생성자 함수
function User(id, name) {
   this.id = id;
   this.name = name;
}

User.prototype.getUser = function() {
   return {id:this.id, name:this.name};
}

User.prototype.group = {id:'group1',name:'친구'};

User.prototype.printUser = function() {
   console.log('user 이름 : %s, group 이름 : %s', this.name, this.group.name);
}

module.exports = new User('test01', '소녀시대');
```
자바스크립트에서는 함수를 생성자로 지정하여 객체를 정의하고, 그 객체를 사용해 인스턴스 객체를 만들 수 있다. 만약 User객체를 만들고 싶다면 User라는 이름의 함수를 먼저 정의한다. 이 함수에는 id와 name속성이 파라미터로 전달되며, 이 둘을 User객체의 속성으로 만들기 위해 다음 코드를 사용한다.

```shell
this.id=id;
```
이렇게 하면 전달받은 파라미터가 this객체의 속성으로 추가된다. User객체의 속성으로 함수나 값을 추가하려면 User.prototype 객체에 속성으로 추가하면 된다. 이렇게 정의한 User객체에서 new 연산자를 사용하여 새로운 인스턴스를 객체를 만든 후 module.exports에 직접 할당한다. 이렇게 하면 모듈을 불러오는 쪽에서 인스턴스 객체를 바로 참조할 수 있다.

module_test8.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : module.exports 에 객체로부터 new 연산자로 생성된 인스턴스 객체를 할당한 후 그 인스턴스 객체의 함수 호출함

var user = require('./user8');

user.printUser();
```

require()메소드를 사용하여 user8.js파일에 정의된 모듈을 불러오면 new User()로 만든 인스턴스 객체가 반환된다. User의 인스턴스 객체에는 printUser()메소드가 추가되어 있으므로 user.printUser()와 같은 형태로 메소드를 호출할 수 있다. [p296 사진]

그런데 모듈을 만들 때 exports객체의 속성으로 인스턴스 객체를 추가했다면 이 객체를 어떻게 사용할까? 모듈의 속성으로 exports에 인스턴스 객체를 추가한 경우에는 모듈을 불러들이는 쪽에서 require()메소드를 호출한 후 반환되는 객체 속성으로 인스턴스 객체를 참조할 수 있다.

user9.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports의 속성 이름을 주면서 추가하되 인스턴스 객체를 만들어 할당함

// 생성자 함수
function User(id, name) {
   this.id = id;
   this.name = name;
}

User.prototype.getUser = function() {
   return {id:this.id, name:this.name};
}

User.prototype.group = {id:'group1',name:'친구'};

User.prototype.printUser = function() {
   console.log('user 이름 : %s, group 이름 : %s', this.name, this.group.name);
}

exports.user = new User('test01', '소녀시대');
```
User객체를 정의하는 코드는 user8.js와 같다. 다만 마지막에 있는 module.exports에 인스턴스 객체를 직접 할당했던 방식을 바꾸어 여기에서는 exports.user속성으로 인스턴스 객체를 추가 한다. 

module_test9.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports의 속성 이름을 주면서 추가하되 인스턴스 객체를 만들어 할당함

var user = require('./user9').user;

user.printUser();
```
require()메소드를 사용해서 user9.js에 정의한 모듈을 불러들이고, 곧바로 반환된 객체의 user속성을 참조한다. user속성은 User에서 만든 인스턴스 객체를 의미하므로 user.printUser()와 같은 코드를 사용하면 User안에 정의도니 함수를 호출할 수 있다.


## 프로토타입 객체를 할당하는 코드 패턴

세 번째 패턴은 User 객체를 새로 정의한 후 module.exprots에 직접 할당한다. 다시 말해, 앞에서 만든 User객체로 인스턴스 객체를 만든 후 module.exports에 할당하는 것이 아니라 User객체 자체를 module.exports에 할당한다. 이렇게 하면 User객체를 정의하는 부분만 별도의 모듈 파일로 분리할 수 있으니 다른ㅌ 파일에서 필요할 때마다 직접 인스턴스 객체를 만들어 사용할 수 있다는 장점이 있다. 

user10.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : module.exports에 프로토타입 객체를 정의한 후 할당함

// 생성자 함수
function User(id, name) {
   this.id = id;
   this.name = name;
}

User.prototype.getUser = function() {
   return {id:this.id, name:this.name};
}

User.prototype.group = {id:'group1',name:'친구'};

User.prototype.printUser = function() {
   console.log('user 이름 : %s, group 이름 : %s', this.name, this.group.name);
}

module.exports = User;
```
모듈 파일의 나머지 부분은 user9.js와 같으며 마지막 부분에서만 module.exports에 객체 User를 직접 할당한다.

module_test10.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : module.exports에 프로토타입 객체를 정의한 후 할당함

var User = require('./user10');
var user = new User('test01', '소녀시대');

user.printUser();
```

require()메소드를 호출하여 모듈을 불러들이면 User객체가 반환된다. 프로토타입 객체로 만든 User객체를 참조했으므로 new연산자를 사용해 인스턴스 객체를 만든 후 그 안에 정의된 함수를 호출할 수 있다. [p298 사진]

이 코드 패턴도 모듈 안에서 module.exports에 객체를 할당하는 방식 대신 exports객체에 속성을 추가한 후 프로토타입 객체를 할당하는 방식을 사용할 수 있다. 


user11.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports의 속성 이름을 주면서 추가하되 프로토타입 객체를 정의한 후 할당함

// 생성자 함수
function User(id, name) {
   this.id = id;
   this.name = name;
}

User.prototype.getUser = function() {
   return {id:this.id, name:this.name};
}

User.prototype.group = {id:'group1',name:'친구'};

User.prototype.printUser = function() {
   console.log('user 이름 : %s, group 이름 : %s', this.name, this.group.name);
}

exports.User = User;
```

module_test11.js
```shell
/**
 * 모듈에 대해 알아보기
 * 
 * 모듈 사용 패턴
 */

// 사용 패턴 : exports의 속성 이름을 주면서 추가하되 프로토타입 객체를 정의한 후 할당함

var User = require('./user11').User;
var user = new User('test01', '소녀시대');

user.printUser();
```
require() 메소드로 모듈을 불러왔을 때 반환되는 객체는 exports이므로 그 안에 추가한 User객체를 참조한 후 new 연산자로 인스턴스 객체를 만들 수 있다.

지금까지 모듈을 정의하는 방법과 대표적인 코드 사용 패턴 몇 가지를 알아보았다. 코드 패턴이 다양해 보일 수 있지만 어떤 경우이든 별도의 모듈 파일에서 module.exports에 객체를 할당하면 된다고 생각하면 이해하기 쉽다. 그리고 module.exports에 할당한 객체나 exports에 속성으로 추가한 객체는 모듈을 불러왔을 때 그대로 참조할 수 있다는 것만 알아두면 된다. 그러면 다른 사람이 만든 모듈이더라도 쉽게 이해하고 사용할 수 있다.
