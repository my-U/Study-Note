# Function Component

이전에는 Class Component보다 기능도 적고 state와 lifeCycle 기능이 없어 잘 사용되지 않았지만 <br>
React 16.8v 이후 Hook 기능이 추가되면서 state와 lifeCycle 기능을 사용할 수 있게 되어 현재는 주로 사용됨

<br>

<b>기본 선언 방식</b>

- function

      import React from "react";

      function App() {
        return(
            //...
        );
      }

      export default App;

- arrow function

        import React from "react";

        const App = () =>{
          return (
              //...
          )
        };

        export default App;

<br>

<b>특징</b>

- Component의 선언이 편하다<br>
- 메모리 자원을 Class Component보다 덜 사용한다<br>
- Hook을 사용하여 state와 lifeCycle 관련 기능을 사용할 수 있다

<br>

## <b>Hooks</b><br>

React이 v16.8로 업데이트 되면서 추가된 기능으로,<br>
상태 관리를 할 수 있게 해주는 _useState_<br>
랜더링 후에 특정 작업을 수행할 수 있도록 해주는 _useEffect_ 등이 있다<br>

### <b>useState</b><br>

가장 기본적인 Hook으로 Function Component에서도 Class Component처럼 가변적인 state를 가질 수 있게 해준다<br>

    import React, { useStaet } from "react"; // import를 통해 useState 선언

    function App() {
        const [value, setValue] = useState(0) // 배열 비구조화 할당. App Component의 기본 상태값 = 0

        return(
            <div>
                <h3>현재 value 값은 {value}입니다.</h3>
                <button onClick={() => setValue(value + 1)}>+ 1</button>
                <button onClick={() => setValue(value - 1)}>- 1</button>
            </div>
        );
    }

<br>

- <b>비구조화 할당</b><br>
  배열이나 객체의 속성 혹은 값을 해체하여 그 값을 변수에 각각 담아 사용하는 자바스크립트 표현식<br>

  > const [ value, setValue ] = useState(0)

  - value : 원소의 현재 상태 값을 담은 변수
  - setValue : 해당 변수의 상태 값을 변경할 수 있는 함수<br>
    해당 state는 Component가 다시 렌더링 되어도 그대로 유지 된다<br>
    초기값은 첫 번째 렌더링에만 한 번 사용된다

<br>

### <b>useEffect</b><br>

Component가 리렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 기능<br><br>
기본적으로 렌더링 된 직후마다 실행되며, 두 번째 파라미터 배열에 무엇을 넣느냐에 따라 실행되는 조건이 달라진다<br>
Class Component의 componentDidMount와 componentDidUpdate, componentWillUnmount가 합쳐진 형태로 생각하면 된다<br>

<b>기본 형식</b>

    import React, { useState. useEffect } from "react";

    const App() {
        const [ name, setName ] = useState('');
        const [ nickname, setNickname ] = useState('');

        useEffect(() => {
            console.log("렌더링이 완료되었습니다.");
            console.log({
                name,
                nickname
            });
        });

        const onChangeName = e => {
            setName(e.target.value);
        }

        const onChangeNickname = e => {
            setNickname(e.target.value);
        }

        return(
            <div>
                <div>
                    <input value={name} onChange={onChangeName} />
                    <input value={nickname} onChange={onChangeNickname} />
                </div>
                <div>
                    <div>
                        <b>이름:</b> {name}
                    </div>
                    <div>
                        <b>닉네임: </b> {nickname}
                    </div>
                </div>
            </div>
        );
    }

    export default App;

<br>

### <b>마운트 될 때만 실행하고 싶을 경우</b><br>

useEffect에서 설정된 함수가 Component가 화면에 처음 렌더링 될 때에만 실행되길 원할 경우,<br>
선언된 함수의 두 번째 파라미터로 비어있는 배열을 넣어주면 된다

    useEffect(() => {
        console.log('마운트 될 때만 실행됩니다.');
    }. []);

<br>

### <b>특정 값이 업데이트 될 때만 실행하고 싶을 경우</b><br>

특정한 값이 업데이트 될 경우에만 실행되길 원할 경우<br>
배열 안에 useState를 통해 관리하고 있는 상태를 넣거나, props로 전달받은 값을 넣어주면 된다

    useEffect(() => {
        console.log(name);
    }, [name]);

<br>

### <b>Unmount되기 전이나 업데이트 되기 직전에 어떤 작업을 실행하고 싶을 경우</b>

clean-up(뒷정리) 함수를 반환해주면 된다

    import React, { useState } from 'react';

    const App = () => {
    const [visible, setVisible] = useState(false);

    useEffect(() => {
        console.log("effect");
        return function cleanup() {
            console.log("clean-up");
        };
    });

    return (
        <div>
            <button
                onClick={() => {
                setVisible(!visible);
                }}
            >
                {visible ? '숨기기' : '보이기'}
            </button>
        </div>

        );
    };

    export default App;
