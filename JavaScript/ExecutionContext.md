# 실행 컨텍스트(Execution Context)

실행 가능한 코드를 형상화하고 구분하는 추상적인 개념으로, 실행 가능한 코드들이 실행되기 위해 필요한 환경을 의미한다.<br>
실행 컨텍스트는 논리적 스택(Stack) 구조를 갖는다.

<br>

### 실행 컨텍스트 스택

<br>

<img src="https://jong-hui.github.io/assets/img/posts/execution-context/1.png" width="800px"  height="150px"><br>
코드를 실행하면 실행 컨텍스트 스택이 생성되고 실행 컨텍스트가 쌓인다.<br>
현재 실행 중인 컨텍스트에서 해당 컨텍스트와 관련이 없는 코드(다른 함수 등)가 실행되면 새로운 컨텍스트가 생성되어 스택에 쌓이고 컨트롤(제어권)이 이동한다.

<br>
<br>

### 실행 가능한 코드

- 전역 컨텍스트(Global Context)

- 함수 컨텍스트(Function Context)

- Eval 컨텍스트

> 이 중 Eval 컨텍스트는 수 많은 취약점이 있어 사용하지 않는다.

<br>

실행 컨텍스트는 `실행에 필요한 정보`를 형상화하고 구분할 때 물리적으로 객체의 형태를 가진다.

<br>

### 실행 컨텍스트 구조

<br>
<img src="https://jong-hui.github.io/assets/img/posts/execution-context/2.png" width="300px" height="300px">

<br>
<br>

### 실행에 필요한 정보

- 변수 객체 (전역변수, 지역변수, 매개변수, 객체의 프로퍼티)

- 스코프 체인(Scope Chain)

- this

<br>
<hr>
<br>

## 변수 객체(Variable Object/VO)

실행에 필요한 여러 정보들을 담고 있는 객체
코드가 실행될 때 엔진에 의해 참조되며 코드에서는 접근할 수 없다.

<br>

### 변수 객체가 담는 정보

- 변수

- 매개변수(parameter)와 전달인자(arguments)

- 함수 선언(함수 표현식은 제외)

<br>

> <b>전역 컨텍스트일 경우</b><br>
> VO는 모든 전역 변수와 전역 함수 등을 포함하는 전역 객체(Global Object/GO)를 가리킨다.

<br>

> <b>함수 컨텍스트일 경우</b><br>
> VO는 지역 변수와 내부 함수 등을 포함하는 활성 객체(Activation Object/AO)를 가리키며 AO는 GO와 달리 매개변수와 전달인자들의 정보를 배열의 형태로 담고 있는 Arguments Object가 추가된다.

<br>

    Arguments 객체는 함수 컨텍스트 내부에 암묵적으로 생성되는 객체로, 함수에 전달된 인수가 담겨있는 Array 형태의 객체이다.

<br>

## 스코프 체인(Scope Chain)

변수, 함수 선언 등의 정보를 담고 있는 전역 객체나 활성 객체들의 스코프를 단계별로 저장한 리스트

<br>

함수를 실행하면 생성되는 Execution Context에는 Lexical Environment와 Variable Environment라는 컴포넌트가 있다.

<br>

### Variable Environment

- Environment Record : 현재 실행 컨텍스트 내에서 호이스팅이 발생하는 변수, 함수선언문 등을 저장

- OuterLexicalEnvironmentReference : outer Environment를 참조

<br>

### Lexical Environment

- Environment Record : let, const로 선언된 변수, 함수표현식 등을 저장

- OuterLexicalEnvironmentReference : Variable Environment를 참조

        OuterLexicalEnvironmentReference는 또 다른 Lexical Environment를 참조하는 포인터 덕분에 연결된 Lexical Environment의 내부정보를 탐색할 수 있다.

<br>
<br>

    function foo(){
        var name='kyle';
        let gender='male';

        if(name==='kyle'){
            let city='seoul';
            var varTest = 'varTest';
            console.log(name,gender,city);
        }

    foo();
    }

<br>
<img src="https://media.vlpt.us/images/proshy/post/b6a65236-a471-4eef-8f52-71eaf20ed7b6/image.png" height="400px" width="500px">

<br>
<br>
<br>

## this 바인딩

this의 값이 여기서 할당된다.<br>
전역 실행 컨텍스트에서 this는 전역 객체(window)이다.

<br>

이때, 함수가 호출되는 방식에 따라 this의 값은 달라진다.<br>
함수가 객체 참조에 의해 호출되면 this는 해당 객체를 참조하고, <br>
그렇지 않을 경우 this는 `전역 객체(window)`를 가리키거나 strict mode에서는 `undefined`를 가르킨다.
