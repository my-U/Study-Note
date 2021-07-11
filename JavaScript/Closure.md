## 클로저(Closure)

어떤 함수를 렉시컬 스코프 밖에서 호출해도, 원래 선언되었던 렉시컬 스코프를 기억하고 접근할 수 있도록 하는 특성

<br>

자바스크립트는 함수 안에서 또 다른 함수를 선언할 수 있다. <br>
이때 내부함수는 외부함수의 지역 변수에 접근 할 수 있는데, 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근  할 수 있다.

    function outter(){
      var title = 'coding everybody';  
      
      return function(){        
        alert(title);
      }
    }
    
    inner = outter(); 
    inner(); // title 변수에 접근

> 위의 코드를 보면 함수 outter의 결과가 변수 inner에 담기고 inner가 실행된다. <br>
이때 함수 outter의 실행이 끝났기 때문에 함수 outter의 지역변수는 소멸된다고 생각하는 것이 일반적이다. <br>
하지만 outter의 지역변수는 소멸되지 않고 함수 inner를 실행했을 때 'coding everybody'가 출력되는 것을 알 수 있다.<br>
이처럼 클로저는 외부함수의 지역 변수를 사용하는 내부함수가 소멸될 때까지 외부함수가 소멸되지 않는 특성을 의미한다.


