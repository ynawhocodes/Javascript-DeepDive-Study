## 순서

24.1 렉시컬 스코프

24.2 함수 객체의  내부 슬롯 `[[Environment]]`

24.3 클로저와 렉시컬 환경

24.4 클로저의 활용

24.5 캡슐화와 정보 은닉

24.6 자주 발생하는 실수


  
### 클로저 간단 소개

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합이다
> 

```jsx
const x = 1; 

function outerFunc() {
	const x = 10
	function innerFunc() {
		console.log(x);
	}
	innerFunc();
};

outerFunc();
```

```jsx
const x = 1;

function outerFunc() {
	const x = 10
	innerFunc();
};
function innerFunc() {
	console.log(x);
}

outerFunc();
```

만약 innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수의 내부에서 호출한다 하더라도 outerFunc 함수의 변수에 접근할 수 없음 

⇒ 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어기 때문

### 24.1 렉시컬 스코프

> 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적 스코프)라 한다.
> 
- 13장, 스코프
    - 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못한다
    - 함수 상위 스코프는 함수를 정의한 위치에 따라 정적으로 결정되고 변하지 않는다
    - 실행 컨텍스트의 렉시컬 환경이 스코프다

- 23장, 실행 컨텍스트
    - 스코프의 실체는 실행 컨텍스트의 렉시컬 환경이다
    - 이 렉시컬 환경은 자신의 외부 렉시컬 환경에 대한 참조를 통해 상위 렉시컬 환경과 연결된다

- 따라서
    - 함수의 상위 스코프를 결정한다 == 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다
    - 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값 == 상위 렉시컬 환경에 대한 참조
    - 상위 렉시컬 환경에 대한 참조 == 상위 스코프

- 정의
    
    <aside>
    렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값
    즉, 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경에 의해 결정됨
    이것이 바로 렉시컬 스코프
    
    </aside>
    
    

### 24.2 함수 객체의  내부 슬롯 `[[Environment]]`

- 배경
    - 렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과는 상관 없이 자신이 정의된 환경, 즉 상위 스코프를 기억해야함 (상위 스코프 == 함수 정의가 위치하는 곳)
    - 이를 위해 함수는 자신의 내부 슬롯 [[Environment]]에 자신의 정의된 환경, 즉 상위 스코프의 참조를 저장함

- [[Environment]]
    - 함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯에 저장함
    - 즉, 여기에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킴
    
    > 함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다. 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장될 참조값이다. 함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.
    > 

- 예시
    
    ```jsx
    const x = 1;
    
    function foo() {
    	const x = 10;
    	bar();
    }
    
    function bar() {
    	console.log(x);
    }
    
    foo();
    bar();
    ```
    
    
    1. **함수 실행 컨텍스트 생성**
    2. **함수 렉시컬 환경 생성**
        1. **함수 환경 레코드 생성**
        2. **this 바인딩**
        3. **외부 렉시컬 환경에 대한 참조 결정**
        
        → 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당됨
        
    
    함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조는 바로 함수의 상위 스코프를 의미함
    
    이것이 바로 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프의 원리
    

### 24.3 클로저와 렉시컬 환경

- 예제
    
    ```jsx
    const x = 1;
    
    function outer() {
    	const x = 10;
    	const inner = function () { console.log(x); };
    	return inner;
    }
    
    const innerFunc = outer();
    innerFunc(); // 10
    ```
    
    이미 생명 주기가 종료되어 실행 컨텍스트에서 제거된 outer 함수의 지역 변수가 x가 다시 부활이라도 한 듯 동작 중
    
    → 이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있음. 이러한 중첩 함수를 클로저라고 함
    

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합이다
> 
- 그 함수가 선언된 렉시컬 환경
    - 실행 컨텍스트의 렉시컬 환경 (== 상위스코프)

- 예제로 다시 돌아가서
    
    1. outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고 
    2. 앞서 outer 함수 객체의 [[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 ‘외부 렉시컬 환경에 대한 참조’에 할당
    3. 중첩 함수 inner 평가 (표현식은 런타임에 평가)
    4. 이때 중첩 함수 inner는 자신의 [[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨택스트의 렉시컬 환경 즉 outer 함수의 렉시컬 환경을 상위 스코프로서 저장

    
    1. outer 함수의 실행이 종료하면 inner 함수를 반환하면서 outer 함수의 생명 주기가 종료 (즉 실행 컨텍스트 스택에서 팝)
    
    → 그러나 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아님
    
    - 왜냐면
        
        outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬록에 의해 참조되고 있고
        
        inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 GC의 대상이 아님
        
        
    1. outer 함수가 반환한 inner 함수를 호출하면 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시됨
    2. 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당됨

- 여기서 잠깐
    - 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다
    - 하지만 일반적으로 모든 함수를 클로저라 하진 않는다

- 클로저 재정의

```jsx
const x = 1;

function foo() {
	const x = 10;
	bar();
}

function bar() {
	console.log(x);
}

foo();
bar();
```

1. 상위 스코프의 식별자 참조
2. 곧바로 소멸하지 않음

- 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우 중첩 함수를 클로저하고 함

- 자유 변수
    
    클로저에 의해 참조되는 상위 스코프의 변수
    
    > 클로저(closure)란 함수가 자유 변수에 대해 닫혀있다(closed)라는 의미
    → 자유 변수에 묶여 있는 함수
    > 
    
- 불필요한 메모리 점유를 걱정 안해도 될지?
    - 자바스크립트 엔진은 최적화가 잘 되어 있기에 클로저가 참조하고 있지 않는 식별자는 기억 x

### 24.4 클로저의 활용

1. 상태를 안전하게 변경하고 유지하기 위해 사용
2. 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용
- 실용적 활용
    
    ### 전역 변수의 사용 억제
    
    **전역 변수를 사용하지 않고**도 현재 상태(혹은 값)를 기억하고 변경된 최신 상태를 유지할 수 있다. 어떤 상태를 관리할 때, 클로저가 없다면 (불가피하게)전역 변수를 사용하거나 아예 로컬스토리지를 사용할 수도 있다. 하지만 클로저를 활용하면 전역 변수가 아니라 타이트한 변수 스코핑을 통해 현명하게 상태나 값을 관리할 수 있다.
    
    박스의 색을 토글하는 함수를 전역변수를 사용하지 않고 클로저와 [즉시실행함수(IIFE)](https://developer.mozilla.org/ko/docs/Glossary/IIFE)를 활용하여 구현하였다. 아래 코드에서 즉시실행함수가 반환한 (이름없는) 함수는 렉시컬 환경에 속한 변수를 기억하는 클로저가 된다.
    
    ```html
    <!DOCTYPE html>
    <html>
    	<body>
    		<div class="box" style="width: 100px; height: 100px; background: green;">
    		</div>
    		<script>
    		    // Box Color Toggler
    		    const box = document.querySelector('.box');
    		
    		    const toggleColor = (function () {
    		      let isGreen = true;
    		      // 클로저 반환
    		      return function () {
    		        box.style.background = isGreen ? 'red' : 'green';
    		        // 상태 변경
    		        isGreen = !isGreen;
    		      };
    		    })();
    		    // 박스 클릭 이벤트
    		    box.addEventListener('click', toggleColor);
    		  </script>
    	</body>
    </html>
    ```
    
    예시 코드 처럼 클로저를 사용할 경우 전역 변수를 사용하지 않게 된다. 단순히 `true` , `false` 토글 이외에도 클로저가 포함된 함수의 변수를 private한 변수 인 것처럼 쓸 수 있을 것이다. 이렇게 불필요한 전역 변수를 사용하지 않음으로서 얻는 이득은 상당하다. **의도되지 않은 변경**을 개발자가 걱정할 필요가 없기 때문에 코드가 안정적이게 된다. 필요한 곳에서만 해당 변수에 접근할 수 있게 하고 그 외부에서는 접근하지 못하게 막는 코드는 예상치 못한 부작용을 막을 수 있는 좋은 코드라고 할 수 있다.
    
    - 출처
        
        [클로저와 스코프: 클로저 활용 방법](https://velog.io/@open_h/closure-and-scope)
        

### 24.5 캡슐화와 정보 은닉

1. 캡슐화
    - 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것
    - 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용한다면 정보 은닉
2. 정보 은닉
    - 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개하지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과

### 24.6 자주 발생하는 실수

- 문제
    
    ```jsx
    var func = [];
    for (var i = 0; i < 3; i++) {
      func[i] = function () { return i; };
    }
    
    for (var j = 0; j < 3; j++) {
      console.log(func[j]())
    }
    ```
    
- 해결 방법
    
    ```jsx
    var func = [];
    for (var i = 0; i < 3; i++) {
      func[i] = (function (id) { 
    		return function() { return id; }; 
    	}(i));
    }
    
    for (var j = 0; j < 3; j++) {
      console.log(func[j]())
    }
    ```