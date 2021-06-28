# 면접 단골 질문!!

## 호이스팅

var 선언문이나 function 선언문 등이 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성으로, 초기화가 아닌 선언부'만'을 제일 위로 끌어올림

    console.log(foo); // undefined
    var foo;

    console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
    let bar;
<br>

## 호이스팅 3단계
<br>

_선언 단계(Declaration phase)_

변수를 실행 컨텍스트의 변수 객체(Variable Object)에 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.

<br>

_초기화 단계(Initialization phase)_

변수 객체(Variable Object)에 등록된 변수를 위한 공간을 메모리에 확보한다. 이 단계에서 변수는 undefined로 초기화된다.

<br>

_할당 단계(Assignment phase)_

undefined로 초기화된 변수에 실제 값을 할당한다.

<br>
<br>

    console.log(name) // undefined
    var name = '샘'

    console.log(name) // print: ReferenceError
    let name = '샘'

> 초기화되지 않으면 undefined로 시작되는 var과 다르게 let 변수는 선언코드가 실행되기 전까지 초기화되지 않는다. let 변수에 Hoisting이 발생하여 선언부가 실행된 것 같지만 실제 코드가 실행된 것은 아니기 때문에 let 변수는 초기화가 진행되지 않아 TDZ(Temporal Dead Zone)에 들어가게 된다. 때문에 ReferenceError가 발생하게 된다.
const 변수는 선언과 초기화가 동시에 일어나야하기 때문에 let과 같은 오류가 발생한다.

<b>TDZ(Temporal Dead Zone)</b>

초기화되지 않은 변수가 있는 곳<br>
변수가 초기화되는 순간 TDZ에서 나오게 되어 사용할 수 있게 됨

<br>

    let letValue = 'out Scope';
    function hoistedLet() {
        // letValue가 TDZ에 영향을 받는 순간
        console.log('letValue', letValue); // ReferenceError
        let letValue = 'inner scope'; // letValue가 초기화 됨으로써, TDZ가 끝나게 됩니다.
    };
    hoistedLet()

> hoistedLet 함수 내부로 들어가면 함수 내부스코프에서 우선적으로 변수를 찾아 Hoisting이 일어나기 때문에 let 변수에 Hoisting이 발생하여 ReferenceError가 나타난다.<br>
Hoisting이 일어나지 않았다면 letValue는 ReferenceError가 아닌 outScope가 나왔을 것이다.

### 이는 초기화가 진행되지 않은 바인딩에 접근할 때, 초기화를 진행하라 알려준다.