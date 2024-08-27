### 21.1 자바스크립트 객체의 분류

크게 3개로 분류한다

- 표준 빌트인 객체
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/009a30ff-1579-476d-a248-96bf29a9a004/3eed9e1c-dc62-47fd-9068-bda0ac4b014c/image.png)
    
    - 표준 빌트인 객체는 ECMAScript 사양에 정의된 객체들을 말함
    - 예를 들어, `Array`, `Object`, `Function`, `Date`, `Math` 같은 객체들이 이에 해당함
    - 쉽게 말해서 JavaScript 언어에서 기본적으로 제공되는 거임
    - 그래서 특정 코드에서만 사용되는 것이 아니라, 프로그램 어디서든 접근할 수 있음
- 호스트 객체
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/009a30ff-1579-476d-a248-96bf29a9a004/6f74a86e-9d5b-4aa0-9a33-0c5ab194ab02/image.png)
    
    - 정의되어 있지는 않지만 자바스크립트 실행환경에서 제공하는 객체
    - `DOM` : HTML 문서를 구조화하고, 그 구조에 접근하거나 조작할 수 있는 객체 모델
    - `BOM` : 브라우저의 창이나 프레임을 제어할 수 있는 객체 모델
- 사용자 정의 객체
    - 그냥 사용자가 직접 정의한 객체임

### 21.2 표준 빌트인 객체

- 자바 스크립트는 여러가지 객체를 제공함(Object, String, Array 등)
- Math, Reflect, JSON을 제외한 객체는 모두 **인스턴스를 생성할 수 있는** 생성자 함수 객체임
    - 생성자 함수 객체는 프로토타입 메서드와 정적 메서드를 제공
    - 아닌 객체는 정적 메서드만 제공
        
        ```jsx
        // String 생성자 함수에 의한 String 객체 생성
        const strObj = new String('Lee');
        
        // Array 생성자 함수에 의한 Array 객체(배열) 생성
        const arr = new Array(1, 2, 3);
        ```
        
- 생성자 함수 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체임
    - **프로토타입**
        
        모든 JavaScript 객체는 다른 객체로부터 상속을 받으며, 이 상속의 연결고리를 **프로토타입**이라 함
        **프로토타입**은 객체가 다른 객체의 속성과 메서드를 상속받을 수 있도록 함
        
    - **예제**
        
        표준 빌트인 객체인 String을 호출해서 생성한 String 인스턴스의 프로토타입은 String.prototype임
        
- 이런 객체의 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공함
- 또, 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공함
- 말이 너무 어렵다
    - `String` 객체로 만든 모든 문자열 객체는 `String.prototype`에 정의된 기능들을 사용할 수 있다는 뜻임
        
        ```jsx
        const numObj = new Number(1.5);
        
        numObj.isInteger(); 도 가능하지만
        Number.isInteger(); 도 가능함
        
        여기서 isInteger()는 Number의 정적 메서드임
        ```
        

### 21.3 원시값과 래퍼 객체

- 기본 자료형인 `string`, `number`, `boolean`은 원시 값으로 간주되며, 객체와는 달리 메서드나 속성을 가질 수 없음
    - 근데 프로그래밍에서 이러한 기본 자료형에 대해 다양한 조작을 해야 할 경우가 많음
    - `String`, `Number`, `Boolean` 생성자 함수는 이러한 기본 자료형을 객체로 감싸서(`Wrapper Object`) 여러 가지 메서드와 속성을 사용할 수 있게 해줌

```jsx
const str = 'hello';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변화함
str.length -> 가능
str.toUpperCase() -> 가능

// 이처럼 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작함
```

- 이는 원시값에 객체처럼 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문
    - 원시값 → 연관된 객체 생성 → 생성된 객체로 메서드 호출 → 원시값으로 되돌림
- 이처럼 `String`, `Number`, `Boolean` 에 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(`Wrapper Object`)라 함
    - 래퍼 객체의 처리가 종료되면 원시값을 갖도록 되돌리고, 래퍼 객체는 가비지 컬렉션의 대상이 된다
    
    ```jsx
    const str = 'hello';
    str.name = 'Lee';
    console.log(str.name);
    ```
    
- 이 이외의 원시값, 즉 null과 undefined는 래퍼 객체를 생성하지 않는다

### 21.4 전역 객체

- 전역 객체
    - 코드가 실행되기 이전 단계에 어떤 객체보다도 먼저 생성되는 객체
    - 즉, 전역 객체는 계층적 구조상 모든 빌트인 객체의 최상위 객체임
- 특징
    - 개발자가 의도적으로 생성할 수 없다(생성자 함수 제공X)
    - 전역 객체 프로퍼티 참조할 때 window 생략 가능
        
        ```jsx
        window.parseInt('F', 16) == parseInt('F', 16)
        ```
        
    - 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있음
    - var 키워드로 선언한 전역 변수, 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역함수는 전역 객체의 프로퍼티가 됨
    - let, const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
    - 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유함

### 21.4.1 빌트인 전역 프로퍼티

- 전역 객체의 프로퍼티
- 어플리케이션 전역에서 사용하는 값 제공

**Infinity**

무한대를 나타내는 숫자값

```jsx
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0);  // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

**NaN**

숫자가 아님을 나타내는 Number

```jsx
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```

**undefined**

원시 타입 undefined를 값으로 가짐

```jsx
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

### 21.4.2 빌트인 전역 함수

- 애플리케이션 전역에서 호출할 수 있는 함수, 전역 객체의 메서드임

**eval**

전달된 문자열을 코드로 해석하고, 그 코드를 실행한다

객체, 함수 리터럴은 반드시 괄호로 둘러싼다

여러 개의 문이라면 모든 문을 실행하고 마지막 결과를 반환

- 표현식이 전달된 경우
    
    ```jsx
    javascriptCopy code
    let x = 10;
    let y = 20;
    
    let result = eval("x + y");  // 표현식 "x + y"가 문자열로 전달됨
    
    console.log(result);  // 30 출력
    ```
    
    여기서 `eval`은 `"x + y"`라는 표현식을 받아서 이 표현식을 실행하여 그 결과인 `30`을 반환
    

- 문이 전달된 경우
    
    ```jsx
    javascriptCopy code
    eval("let z = 50;");  // 변수 z를 선언하고 50으로 초기화하는 문이 실행됨
    console.log(z);       // 50 출력
    ```
    
    여기서는 `eval`이 `"let z = 50;"`이라는 **문**을 받아서 이 문을 실행하고 `z`라는 변수를 생성후 `50`이라는 값을 할당
    
- 일반 모드에서는 `eval`이 호출된 스코프를 직접 수정
    - 함수 안에서 선언됐으면 그 함수의 변수를 수정해줌
- 엄격 모드에서는 `eval`이 자신의 스코프를 만들어 기존 스코프를 건드리지 X
    - 함수고 뭐고 걍 eval 안에 있는 변수만 신경씀
- `let`과 `const`로 선언된 변수는 자동으로 엄격 모드처럼 동작
    - var은 부지런히 자기 변수 수정해주는데 let, const는 좀 게으른 느낌

> **eval 함수 사용은 금지하자**

- 보안에 매우 취약함
- 최적화가 안되니까 코드 실행, 처리 속도가 느림
> 

**isFinite**

유한수면 true
무한수, Nan이면 false

숫자가 아닌 타입이 들어오면 알아서 숫자로 타입 변환해줌

```jsx
// 인수가 유한수이면 true를 반환한다.
isFinite(0);    // -> true
isFinite(2e64); // -> true
isFinite('10'); // -> true: '10' → 10
isFinite(null); // -> true: null → 0

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity);  // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN);     // -> false
isFinite('Hello'); // -> false
isFinite('2005/12/12'); // -> false
```

**isNaN**

NaN 여부를 검사해서 불리언으로 반환해줌

얘도 알아서 숫자로 타입 변환해서 검사해줌

```jsx
// 숫자
isNaN(NaN); // -> true
isNaN(10);  // -> false

// 문자열
isNaN('blabla'); // -> true: 'blabla' => NaN
isNaN('10');     // -> false: '10' => 10
isNaN('10.12');  // -> false: '10.12' => 10.12
isNaN('');       // -> false: '' => 0
isNaN(' ');      // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date());            // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```

**parseFloat**

부동 소수점 숫자, 즉 실수로 해석해서 반환

이거 반올림할 때 많이 사용함 toFixed였나 잘 기억은 안 남

```jsx
// 문자열을 실수로 해석하여 반환한다.
parseFloat('3.14');  // -> 3.14
parseFloat('10.00'); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('34 45 66'); // -> 34
parseFloat('40 years'); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('He was 40'); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(' 60 '); // -> 60
```

**parseInt**

- 정수로 해석하여 반환함
    
    ```jsx
    // 문자열을 정수로 해석하여 반환한다.
    parseInt('10');     // -> 10
    parseInt('10.123'); // -> 10
    
    // '10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt('10'); // -> 10
    // '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt('10', 2); // -> 2
    // '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt('10', 8); // -> 8
    // '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt('10', 16); // -> 16
    ```
    
- 10진수를 특정 진수의 문자열로 변환하려면 (ex. 10진수 15를 2진수로 변환해보자!) toString() 사용하자
    
    ```jsx
    // 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
    x.toString(2); // -> '1111'
    // 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
    parseInt(x.toString(2), 2); // -> 15
    ```
    
- 16진수는 따로 16 적지 않아도 0x, 0X를 보고 알아서 16진수로 해석해주지만, 2진수와 8진수는 꼭 명시해줘야 한다
- 첫 번째 인수로 전달한 문자열 **첫 번째 문자**가 해당 지수로 변환될 수 없으면 NaN 반환한다
- 만약 첫 번째 문자는 해석됐는데 두 번째 문자부터 해석이 안된다면?
    
    → 첫 번째 문자만 해석해서 반환해주고 두 번째 문자 이후의 문자는 **모두 무시한다**
    
- 문자열에 공백이 있으면 **첫 번째 문자열만** 반환하고 뒤 문자는 **전부 무시한다**
    
    → 첫 번째 문자열이 해석 불가능하면 그냥 NaN 반환한다
    

**encodeURI/decodeURI**

- `encodeURI` : 완전한 URI를 문자열로 전달 받아 이스케이프 처리를 위한 인코딩
- 인코딩 URI의 문자들을 이스케이프 처리하는 과정
- `이스케이프 처리` : 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있도록 아스키 문자 셋으로 변환하는 것
    
    → URL에서 의미를 갖는 문자나 URL에 올 수 없는 문자 등을 이스케이프 처리
    
    → 야기될 수 있는 문제를 예방한다
    
- `decodeURI` : 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩.
    
    ```jsx
    const uri = 'http://example.com?name=이웅모&job=programmer&teacher';
    
    // encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
    const enc = encodeURI(uri);
    console.log(enc);
    // http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
    
    // decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
    const dec = decodeURI(enc);
    console.log(dec);
    // http://example.com?name=이웅모&job=programmer&teacher
    ```
    

**encodeURIComponent/decodeURIComponent**

- encodeURIComponent URI 구성 요소를 인수로 전달받아 인코딩
- decodeURIComponent URI 구성 요소를 디코딩

<aside>
🍀 이 함수는 전달된 문자열을 URI 구성요소인 쿼리 스트링의 일부로 간주함

- 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩함

반면 encodeURI는 문자열을 그냥 완전한 URI라고 간주함

- 그래서 쿼리 스트링 구분자는 인코딩 안 함
</aside>

### 21.4.3 암묵적 전역

먼저 코드를 보쟈

```jsx
var x = 10; // 전역 변수

function foo () {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

- foo 함수 내의 y는 선언하지 않은 식별자임
    - 근데 이게 왠걸, 마치 선언된 전역 변수처럼 동작한다
    - 이건 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문이다
- foo 함수가 호출되면 y 변수에 값을 할당하기 위해 **선언된 변수인가?** 확인한다
    - 오잉 근데 선언이 안되어 있음 → 참조 에러가 발생한다
    - 근데 자바스크립트 엔진은 y=20을 window가 생략된 window.y=20으로 해석
    - 그래서 동적으로 프로퍼티를 생성하게 됨
    - 이런 현상이 **!!!!암묵적 전역!!!!**
- 하지만 y는 그저 객체 프로퍼티로 추가되었을 뿐, 변수는 아님
    - 그래서 호이스팅은 발생 안 함
        
        ```jsx
        // 전역 변수 x는 호이스팅이 발생한다.
        console.log(x); // undefined
        // 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
        console.log(y); // ReferenceError: y is not defined
        ```
        
- 그래서 변수가 아니고 단순 프로퍼티인 y는 delete로 삭제 가능함
    - 전역 변수는 프로퍼티는 맞지만 변수이기 때문에 삭제 불가능함
        
        ```jsx
        delete x; // 전역 변수는 삭제되지 않는다.
        delete y; // 프로퍼티는 삭제된다.
        
        console.log(window.x); // 10
        console.log(window.y); // undefined
        ```
