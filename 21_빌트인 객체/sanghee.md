## 자바스크립트 객체의 분류

자바스크립트 객체는 크게 3개의 객체로 분류한다.

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

<br/>

## 표준 빌트인 객체

자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math 등 40여 개의 표준 빌트인 객체를 제공한다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

- 표준 빌트인 객체
  - 생성자 함수 객체: 프로토타입 메서드, 정적 메서드 제공
  - 생성자 함수 객체가 아닌 것: 정적 메서드 제공

<br/>

## 원시값과 래퍼 객체

원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없다. 하지만 아래 예제는 마치 객체처럼 동작한다.

```jsx
const str = "hello";

console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

원시값을 객체처럼 사용하면 js 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

> 래퍼 객체: 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체

```jsx
const str = "hi";

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

문자열에 대해 마침표 표기법으로 접근하면 그 순간 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다. String 생성자 함수의 인스턴스는 String.prototype 메서드를 상속받아 사용할 수 있다.

그 후 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

문자열, 숫자, 불리언, 심벌 이외의 원시값, 즉 null과 undefined는 래퍼 객체를 생성하지 않는다. 이들을 객체처럼 사용하면 에러가 발생한다.

<br/>

## 전역 객체

전역 객체는 코드가 실행되기 이전 단계에 js 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체다.

> 전역 객체를 지칭하는 이름
>
> - 브라우저 환경: window
> - Node.js 환경: global
> - 통일: globalThis

전역 객체의 프로퍼티는 표준 빌트인 객체, 환경에 따른 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수가 있다. 이는 계층적 구조상 어떤 객체에도 속하지 않은 모든 객체의 빌트인 객체의 최상위 객체다.

### 전역 객체의 특징

- 개발자가 의도적으로 생성할 수 없다.
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)을 생략할 수 있다.
- 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- js 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
  - 브라우저 환경: 클라이언트 사이드 Web API를 호스트 객체로 제공
  - Node.js 환경: Node.js 고유의 API를 호스트 객체로 제공
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.
- let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 이들은 보이지 않는 개념적인 블록 내에 존재하게 된다.
- 브러우저 환경의 모든 js 코드는 하나의 전역 객체 window를 공유하며, 분리되어 있는 js 코드는 하나의 전역을 공유한다.

<br/>

### 빌트인 전역 프로퍼티

빌트인 전역 프로퍼티: 전역 객체의 프로퍼티

#### Infinity

무한대를 나타내는 숫자값 Infinity를 갖는다

```jsx
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3 / 0); // Infinity
// 음의 무한대
console.log(-3 / 0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

#### NaN

숫자가 아님(Not-a-Number)을 나타내는 숫자값 NaN을 갖는다. Number.NaN 프로퍼티와 같다.

```jsx
console.log(window.NaN); // NaN

console.log(Number("xyz")); // NaN
console.log(1 * "string"); // NaN
console.log(typeof NaN); // number
```

#### undefined

원시타입 undefined를 값으로 갖는다.

```jsx
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

<br/>

### 빌트인 전역 함수

빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드이다.

#### eval

기존의 스코프를 런타임에 동적으로 수정한다.

보안에 매우 취약하고 처리 속도가 느리므로 eval 함수의 사용은 금지해야 한다.

#### isFinite

전달받은 인수가 정상적인 유한수인지 검사한다.

유한수이면 true를 반환하고, 무한수면 false를 반환한다.

타입이 숫자가 아닌 경우, 숫자로 타입을 변환한 후 검사한다. 인수가 NaN으로 평가되는 값이라면 false를 반환한다. null은 숫자 타입으로 변환하면 0이다.

```jsx
// 인수가 유한수이면 true를 반환한다.
isFinite(0); // -> true
isFinite(2e64); // -> true
isFinite("10"); // -> true: '10' → 10
isFinite(null); // -> true: null → 0

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity); // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN); // -> false
isFinite("Hello"); // -> false
isFinite("2005/12/12"); // -> false
```

#### isNaN

전달 받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환한다.
타입이 숫자가 아닌 경우 숫자로 타입을 변환한 후 검사를 수행한다.

```jsx
// 숫자
isNaN(NaN); // -> true
isNaN(10); // -> false

// 문자열
isNaN("blabla"); // -> true: 'blabla' => NaN
isNaN("10"); // -> false: '10' => 10
isNaN("10.12"); // -> false: '10.12' => 10.12
isNaN(""); // -> false: '' => 0
isNaN(" "); // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date()); // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```

#### parseFloat

전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.

```jsx
// 문자열을 실수로 해석하여 반환한다.
parseFloat("3.14"); // -> 3.14
parseFloat("10.00"); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat("34 45 66"); // -> 34
parseFloat("40 years"); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat("He was 40"); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(" 60 "); // -> 60
```

#### parseInt

전달받은 문자열 인수를 정수로 해석하여 반환한다.

문자열이 아니면 문자열로 변환한 다음, 정수로 해석하여 반환한다.

두 번째 인수로 진법을 나타내는 기수(2~36)을 전달할 수 있다. 기수를 지정하면 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석하여 반환한다. 반환값은 언제나 10진수다. 기수를 생략하면 10진수로 해석하여 반환한다.

```jsx
// 문자열을 정수로 해석하여 반환한다.
parseInt("10"); // -> 10
parseInt("10.123"); // -> 10

parseInt(10); // -> 10
parseInt(10.123); // -> 10

// 10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10"); // -> 10
// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 2); // -> 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 8); // -> 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 16); // -> 16
```

기수를 지정하여 10진수 숫자를 해당 기수의 문자열로 변환하여 반환하고 싶을 때는 Number.prototype.toString 메서드 사용한다.

```jsx
const x = 15;

// 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
x.toString(2); // -> '1111'
// 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt(x.toString(2), 2); // -> 15
```

첫 번째 인수로 전달한 문자열의 첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN을 반환한다.

```jsx
// 'A'는 10진수로 해석할 수 없다.
parseInt("A0"); // -> NaN
// '2'는 2진수로 해석할 수 없다.
parseInt("20", 2); // -> NaN
```

첫 번째 인수로 전달한 문자열의 두 번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자와 마주치면 이 문자와 계속되는 문자들은 전부 무시되며 해석된 정수값만 반환한다.

```jsx
// 10진수로 해석할 수 없는 'A' 이후의 문자는 모두 무시된다.
parseInt("1A0"); // -> 1
// 2진수로 해석할 수 없는 '2' 이후의 문자는 모두 무시된다.
parseInt("102", 2); // -> 2
// 8진수로 해석할 수 없는 '8' 이후의 문자는 모두 무시된다.
parseInt("58", 8); // -> 5
// 16진수로 해석할 수 없는 'G' 이후의 문자는 모두 무시된다.
parseInt("FG", 16); // -> 15
```

첫 번째 인수로 전달한 문자열에 공백이 있다면 첫 번째 문자열만 해석하여 반환하며 앞뒤 공백은 무시된다. 만일 첫 번째 문자열을 숫자로 해석할 수 없는 경우 NaN을 반환한다.

```jsx
// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseInt("34 45 66"); // -> 34
parseInt("40 years"); // -> 40
// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseInt("He was 40"); // -> NaN
// 앞뒤 공백은 무시된다.
parseInt(" 60 "); // -> 60
```

#### encodeURI / decodeURI

encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

URI는 인터넷에 있는 자원을 나타내는 유일한 주소를 말한다. 하위 개념으로 URL, URN이 있다.

인코딩은 URI의 문자들을 이스케이프 처리하는 것이다.

이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

URL은 아스키 문자 셋으로만 구성되어야 한다.

```jsx
// 완전한 URI
const uri = "http://example.com?name=이웅모&job=programmer&teacher";

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
```

decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

```jsx
const uri = "http://example.com?name=이웅모&job=programmer&teacher";

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

// decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
const dec = decodeURI(enc);
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```

#### encodeURIComponent / decodeURIComponent

encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩한다.

인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다. 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.

decodeURIComponent 함수는 매개변수로 전달된 URI 구성 요소를 디코딩한다.

매개변수로 전달된 문자열을 완전한 URI 전체라고 간주한다. 쿼리 스트링 구분자로 사용되는 =, ?m. &은 인코딩하지 않는다.

```jsx
// URI의 쿼리 스트링
const uriComp = "name=이웅모&job=programmer&teacher";

// encodeURIComponent 함수는 인수로 전달받은 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc);
// name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// 이웅모&job=programmer&teacher

// encodeURI 함수는 인수로 전달받은 문자열을 완전한 URI로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &를 인코딩하지 않는다.
enc = encodeURI(uriComp);
console.log(enc);
// name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

dec = decodeURI(enc);
console.log(dec);
// name=이웅모&job=programmer&teacher
```

## 암묵적 전역

```jsx
var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

암묵적 전역은 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작하는 것을 말한다.
전역 객체의 프로퍼티로 추가되었을 뿐이므로 변수는 아니다. 변수가 아니므로 변수 호이스팅이 발생하지 않는다. 프로퍼티이므로 delete 연산자로 삭제할 수 있다.
