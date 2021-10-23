# Scope(유효범위)란

## '변수에 접근할 수 있는 범위'

<br>
Scope는 전역에 선언되어 어디서든지 접근이 가능한 전역스코프(Global Scope)와 해당 지역에서만 접근할 수 있고 지역을 벗어난 곳에선 접근할 수 없는 지역스코프(Local Scope) 2가지 타입이 있음.
<br>
<br>

ES6 이전엔 var만 사용되었고, ES6 이후 let과 const가 추가됨.
<br>

## var 특징

- var 키워드 생략 허용
- 변수 재선언, 재할당 가능

- 변수 호이스팅 가능

- 함수 레벨 스코프(Function-level Scope)

- 전역객체 프로퍼티로 할당됨.
  var a = 10;

      console.log(window.a); // 10

      console.log(a); // 10

## let 특징

- 변수 재선언 불가능, 변수 재할당 가능

- 변수 호이스팅 불가능

- 블록 레벨 스코프(Block-level Scope)

- 전역객체 프로퍼티로 할당되지 않음.
  let a = 10;

      console.log(window.a); // undefined

      console.log(a); // 10

## const 특징

- 변수 재선언 불가능, 변수 재할당 불가능 - const는 상수이기 때문(객체에선 재할당 가능)

- 변수 호이스팅 불가능

- 선언과 대입을 동시에 해야함

- 블록 레벨 스코프(Block-level Scope)

- 전역객체 프로퍼티로 할당되지 않음

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

      function a() {
            var doo = 789; // 함수 내부에서만 유효
      }
      console.log(doo); // Uncaught ReferenceError: doo is not defined

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
   
      let hello;
      if(true) {
            hello = "hahaha"; // hello가 버블 외부에 있기 때문에 가능
      }
      console.log(hello);

<br>
<br>

## 스코프 체인(Scope Chain)

Identifiers(식별자)를 찾는 일련의 과정

      var x = 1;
      function foo() {
            console.log(x);
      }

      console.log(x); // 1
      foo(); // 1

> 식별자를 찾을 때 먼저 자신이 속한 스코프에서 찾고 해당 스코프에 식별자가 없으면 상위 스코프로 찾아 나간다.

<br>
<br>

## 렉시컬 스코프(lexical scope)

<br>

함수 상위의 스코프를 결정하는데에는 두 가지 방법이 있다. 첫 번째는 함수가 어디서 호출이 되는지에 따라 상위 스코프가 결정되는 <b>_동적 스코프_</b>, 두 번째는 함수가 어디서 선언이 되는지에 따라 상위 스코프가 결정되는 <b>_정적 스코프_</b> 이다.

<br>

여기서 렉시컬 스코프는 정적 스코프를 의미하며, 자바스크립트는 <b>_렉시컬 스코프_</b>를 따른다.

      var x = 1;

      function foo() {
            var x = 10;
            bar();  // 호출
      }

      function bar() {  // 선언
            console.log(x);
      }

      foo(); // 1 동적 스코프라면 함수 foo의 실행결과는 10, 정적 스코프라면 1
      bar(); // 1

> bar 함수를 호출한 곳은 foo 함수의 내부였지만, bar 함수가 선언된 곳은 전역이기 때문에 x는 전역에서 선언된 1이 나온다.
