# Promise() 생성자

주로 프로미스를 지원하지 않는 함수를 감쌀 때 사용하는 생성자

## <b>구문</b>

    new Promise(executor)

## 매개변수

### <b>executor</b><br>

> resolve 및 reject 인수를 전달할 실행 함수.<br>
> 실행 함수는 프로미스 구현에 의해 resolve와 reject 함수를 받아 즉시 실행됨.<br>
> (실행 함수는 Promise 생성자가 생성한 객체를 반환하기도 전에 호출됨)<br>
> 실행 함수는 보통 어떤 비동기 작업을 시작한 후 모든 작업을 끝내면 resolve를 호출해 프로미스를 이행하고,<br>
> 오류가 발생한 경우 reject를 호출해 거부한다.

    const amISexy = new Promise((resolve, reject) => {
        // Promise를 만들시엔 실행할 수 있는 function을 넣어야 한다 => executor
        resolve("Yes you are");
    });

    console.log(amISexy); // Promise {<resolved>: "Yes you are"}

## <b>_Using Resolve_</b>

    const amISexy = new Promise((resolve, reject) => {
        resolve("Yes you are");
    });

    // value == "Yes you are"
    amISexy.then(value => console.log(value));

\*두 코드는 같은 코드

> amISexy.then(value => console.log(value));

> const thenFn = value => console.log(value);<br>
> amISexy.then(thenFn);<br>

<br>

## <b>_Using Reject_</b>

    const amISexy = new Promise((resolve, reject) => {
        reject("no you aren't");
    });

    amISexy // 순차적 실행 X
        .then(result => console.log(result)) // promise가 resolve된다면 해당 코드가 실행
        .catch(error => console.log(error)); // promise가 reject된다면 해당 코드가 실행

<br>

## <b>Chaining Promises</b>

then은 하나씩 순차적으로 실행되며, 갯수의 상관없이 사용 가능하다

    const amISexy = new Promise((resolve, reject) => {
        resolve(2);
    });

    amISexy
        .then(number => {
            console.log(number * 2);
            return number * 2; // 다음 then이 정상적으로 작동하지 않을 시 return으로 값을 넘겨줌
        })
        .then(otherNumber => { // otherNumber == number * 2
            console.log(otherNumber * 2);
        });

_에러가 발생하는 경우_

    const amISexy = new Promise((resolve, reject) => {
        resolve(2) or reject(2);
    });

    const timesTwo = number => number * 2;

    amISexy
        .then(timesTwo)
        .then(timesTwo)
        .then(timesTwo)
        .then(() => {
            throw Error("Something is wrong"); // resolve일 경우 해당 에러 반환
        })
        .then(lastNumber => console.log(lastNumber))
        .catch(error => console.log(error)); // reject일 경우 error의 값은 2가 되어 2 반환
