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

<b>_에러가 발생하는 경우_</b>

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

<br>

## <b>Promise.all</b>

여러 개의 Promise를 사용하는 경우 주어진 모든 Promise를 실행한 후 이행된 Promise들의 값을 반환한다

    const p1 = new Promise(resolve => {
        setTimeout(resolve, 5000, "First");
    })

    const p2 = new Promise(resolve => {
        setTimeout(resolve, 1000, "Second");
    })

    const p3 = new Promise(resolve => {
        setTimeout(resolve, 3000, "Third");
    })

    const motherPromise = Promise.all([p1, p2, p3]);

    motherPromise.then(values => console.log(values));  // ["First", "Second", "Third"]

> Promise가 완료가 되는 순서는 "Second", "Third", "First" 순이지만 완료되는 시간 상관없이 all()에 들어오는 순서대로 값을 반환한다

<br>

<b>_오류가 발생했을 경우_</b>

    const p1 = new Promise(resolve => {
        setTimeout(resolve, 5000, "First");
    })

    const p2 = new Promise(resolve, reject => {
        setTimeout(reject, 1000, "I hate JS"); // reject
    })

    const p3 = new Promise(resolve => {
        setTimeout(resolve, 3000, "Third");
    })

    const motherPromise = Promise.all([p1, p2, p3]);

    motherPromise
        .then(values => console.log(values))
        .catch(error => console.log(error));  // I hate JS

Promise.all에 제공된 Promise들 중에 하나라도 reject되면 다른 Promise들도 reject 처리가 된다<br>
이는 Promise가 제대로 작동하고 있는지 확인할 때 유용하다

<br>

## <b>Promise.race</b>

Promise.all()과 사용법은 같지만 <br>
가장 빨리 완료된 Promise의 결과만 반환한다

    const p1 = new Promise(resolve => {
        setTimeout(resolve, 10000, "First");
    })

    const p2 = new Promise(resolve, reject => {
        setTimeout(reject, 5000, "I hate JS"); // reject
    })

    const p3 = new Promise(resolve => {
        setTimeout(resolve, 3000, "Third");
    })

    const motherPromise = Promise.race([p1, p2, p3]);

    motherPromise
        .then(values => console.log(values))  // Third
        .catch(error => console.log(error));

> p3가 3초로 가장 빠르게 완료되어 결과값을 반환했다<br>
> 이처럼 Promise.ract()는 제공된 Promise들 중 어느 것이 먼저 반환되는지 상관 없을 경우에 사용된다

<br>

    Promise.all([p1, p2, p3])
        .then(values => console.log(values))
        .catch(error => console.log(error));

> 해당 코드로도 사용 가능

<br>

## <b>finally</b>

Promise가 성공적으로 수행되었는지 거절되었는지에 관계없이 Promise가 처리되면 지정된 콜백함수를 반환한다

    const p1 = new Promise(resolve => {
        setTimeout(resolve, 5000, "First);
    })
      .then(value => console.log(value))
      .finally(() => console.log("I'm done"));

      // 결과
         First
         I'm done
