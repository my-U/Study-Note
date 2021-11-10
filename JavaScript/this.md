# this

## 메서드를 호출한 객체를 담는 속성

this는 함수가 어떻게 호출되었는지에 따라 this에 바인딩되는 객체가 동적으로 결정된다

<br>

### <b>1. 함수 호출</b><br>

전역 객체(Global Object)는 모든 객체의 유일한 최상위 객체를 의미하며<br>
일반적으로 Browser-side에서는 window, Server-side(Node.js)에서는 global 객체를 의미한다<br>

전역 객체는 전역 스코프(Global Scope)를 갖는 전역 변수(Global variable)를 프로퍼티로 소유한다<br>
이때, 글로벅 영역에 선언한 함수는 전역객체의 프로퍼티로 접근할 수 있는 전역 변수의 메소드이다<br>

    var ga = 'Global variable';

    console.log(ga);
    console.log(window.ga);

    function foo() {
        console.log('invoked!');
    }
    window.foo();

기본적으로 this는 전역 객체에 바인딩되고,<br>
<b>내부함수는 일반 함수, 메소드, 콜백함수 어디에서 선언되었든 관계없이 this에 전역객체를 바인딩한다</b>

- 일반 함수<br>

        function foo() {
            console.log("foo's this: ", this); // window
            function bar() {
                console.log("bar's this: ", this); // window
            }
            bar();
        }
        foo();

- 메소드<br>

        var value = 1;

        var obj = {
            value: 100,
            foo: function() {
                console.log("foo's this: ", this); // obj
                console.log("foo's this.value: ", this.value); // 100
                function bar() {
                    console.log("bar's this: ", this); // window
                    console.log("bar's this.value: ", this.value); // 1
            }
            bar();
            }
        };
        obj.foo();

- 콜백 함수<br>

        var value = 1;

        var obj = {
            value: 100,
            foo: function() {
                setTimeout(function() {
                console.log("callback's this: ",  this);  // window
                console.log("callback's this.value: ",  this.value); // 1
                }, 100);
            }
        };

        obj.foo();

<br>

<b>내부 함수에서 this에 전역객체가 바인딩 되는 것을 막는 방법</b><br>

    var value = 1;

    var obj = {
        value: 100,
        foo: function() {
            var that = this;                                    // Workaround : this === obj

            console.log("foo's this: ",  this);                 // obj
            console.log("foo's this.value: ",  this.value);     // 100
            function bar() { // 내부 함수
                console.log("bar's this: ",  this);             // window
                console.log("bar's this.value: ", this.value);  // 1

                console.log("bar's that: ",  that);             // obj
                console.log("bar's that.value: ", that.value);  // 100
                }
            bar();
        }
    };

    obj.foo();

<br>

### <b>2. 메서드 호출</b><br>

함수가 객체의 프로퍼티 값으로 존재할 때 해당 함수는 메소드로서 호출된다<br>
이때 해당 메소드 내부의 this는 해당 메소드를 소유한 객체, 즉 해당 메소드를 호출한 객체를 바인딩한다<br>

    var obj1 = {
    name: 'Lee',
    sayName: function() {
        console.log(this.name);
    }
    }

    var obj2 = {
    name: 'Kim'
    }

    obj2.sayName = obj1.sayName;

    obj1.sayName();
    obj2.sayName();

 <br>

프로토타입 객체 메소드 내부에서 사용된 this도 일반 메소드 방식과 마찬가지로 해당 메소드를 호출한 객체에 바인딩된다<br>

    function Person(name) {
        this.name = name;
    }

    Person.prototype.getName = function() {
        return this.name;
    }

    var me = new Person('Lee');                 // this.name = Lee
    console.log(me.getName());                  // Lee

    Person.prototype.name = 'Kim';              // this.name = Kim
    console.log(Person.prototype.getName());    // Kim

<br>

### <b>3. 생성자 함수 호출</b><br>

자바스크립트의 생성자 함수는 이름 그대로 객체를 생성하는 역할을 한다<br>
하지만 사용 형식이 정해져 있는 것이 아니라 기존 함수에 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작한다<br>

이는 생성자 함수가 아닌 일반 함수에 new 연산자만 붙이면 생성자 함수로 동작할 수도 있음을 의미하기에 일반적으로 생성자 함수명은 첫 문자를 대문자로 선언한다<br>

    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    var me = new Person('Lee');
    console.log(me); // Person&nbsp;{name: "Lee"}

    // new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수로 동작하지 않는다.
    var you = Person('Kim');
    console.log(you); // undefined

<br>

<br>

### <b>4. apply/call/bind 호출</b><br>

자바스크립트 엔진의 암묵적 this 바인딩(전역 객체) 이외에 this를 특정 객체에 명시적으로 바인딩 하는 방법<br>

    func.apply(thisArg, [argsArray])<br>

> // thisArg: 함수 내부의 this에 바인딩할 객체<br>
> // argsArray: 함수에 전달할 argument의 배열<br>

    var value = 1;

    var obj = {
        value: 100,
        foo: function() {
            console.log("foo's this: ",  this);  // obj
            console.log("foo's this.value: ",  this.value); // 100
            function bar(a, b) {
                console.log("bar's this: ",  this); // obj
                console.log("bar's this.value: ", this.value); // 100
                console.log("bar's arguments: ", arguments); // 1, 2
            }
            bar.apply(obj, [1, 2]);
            bar.call(obj, 1, 2);
            bar.bind(obj)(1, 2);
        }
    };

    obj.foo();

> 기억해야 할 것은 apply() 메소드를 호출하는 주체는 함수이며,<br>
> apply() 메소드는 this를 특정 객체에 바인딩할 뿐 본질적인 기능은 함수 호출이라는 것이다
