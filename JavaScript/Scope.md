## Scope(유효범위)란 '변수에 접근할 수 있는 범위'

<br>
Scope는 전역에 선언되어 어디서든지 접근이 가능한 전역스코프(Global Scope)와 해당 지역에서만 접근할 수 있고 지역을 벗어난 곳에선 접근할 수 없는 지역스코프(Local Scope) 2가지 타입이 있음.
<hr>
<br>

ES6 이전엔 var만 사용되었고, ES6 이후 let과 const가 추가됨.
<br>

var 특징
-------------

* var 키워드 생략 허용
   
* 변수 재선언, 재할당 가능

* 변수 호이스팅 가능

* 함수 레벨 스코프(Function-level Scope)

* 전역객체 프로퍼티로 할당됨.
      var a = 10; 

      console.log(window.a); // 10 

      console.log(a); // 10

let 특징
-------

* 변수 재선언 불가능, 변수 재할당 가능

* 변수 호이스팅 불가능

* 블록 레벨 스코프(Block-level Scope)

* 전역객체 프로퍼티로 할당되지 않음.
      let a = 10; 

      console.log(window.a); // undefined 

      console.log(a); // 10


const 특징
---------

* 변수 재선언 불가능, 변수 재할당 불가능

* 변수 호이스팅 불가능

* 선언과 대입을 동시에 해야함

* 블록 레벨 스코프(Block-level Scope)

* 전역객체 프로퍼티로 할당되지 않음

<br>
<hr>
<br>

<b>_함수 레벨 스코프(Function-level Scope)_</b>

> 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없음. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수임.

      var foo = 123; // 전역 변수

      console.log(foo); // 123

      {
         var foo = 456; // 전역 변수. 함수가 아니기 때문
      }

      console.log(foo); // 456

<br>

<b>_블록 레벨 스코프(Block-level scope)_</b>

> 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없음. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수임.

      let foo = 123; // 전역 변수

      { 
         let foo = 456; // 지역 변수
         let bar = 456; // 지역 변수
      }

      console.log(foo); // 123
      console.log(bar); // ReferenceError: bar is not defined
   
<br>





   <!-- 호이스팅
      var 선언문이나 function 선언문 등을 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성
      
      console.log(foo); // undefined
      var foo;

      console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
      let bar;

      호이스팅 3단계
      
      선언 단계(Declaration phase)
      변수를 실행 컨텍스트의 변수 객체(Variable Object)에 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.

      초기화 단계(Initialization phase)
      변수 객체(Variable Object)에 등록된 변수를 위한 공간을 메모리에 확보한다. 이 단계에서 변수는 undefined로 초기화된다.

      할당 단계(Assignment phase)
      undefined로 초기화된 변수에 실제 값을 할당한다. -->