# var 키워드로 선언한 변수의 문제점

### 🐽 변수 중복 선언 허용

---

- var 키워드로 선언한 변수는 중복 선언이 가능함

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

- var 키워드로 선얺나 변수를 중복 선언하면 초기화문 유무에 따라 다르게 동작함
- 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시됨
- 이때 에러는 발생하지 않음
- 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생함

<br/>

### 🐽 함수 레벨 스코프

---

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정함
- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됨

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

<br/>

- for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 됨

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

- 함수 레벨 스코프는 전역 변수를 남발할 가능성을 높임
- 이로 인해 의도치 않게 전역 변수가 중복 선언되는 경우가 발생함

<br/>

### 🐽 변수 호이스팅

---

- 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있음
- 할당문 이전에 변수를 참조하면 언제나 undefined를 반환함

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

<br/><br/>

# let 키워드

### 🐽 변수 중복 선언 금지

---

- let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생함

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

<br/>

### 🐽 블록 레벨 스코프

---

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따름
- let 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따름

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

<br/>

### 🐽 변수 호이스팅

---

- var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작함

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

- let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생함
- let 키워드로 선언한 변수는 “선언 단계”와 “초기화 단계”가 분리되어 진행됨
- 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행됨

<br/>

- 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생함
- let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없음
- 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**라 함

```jsx
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

![image](https://github.com/user-attachments/assets/ce6cc1c0-7998-40fe-ac69-f7fba3089039)

<br/>

- let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보임

```jsx
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

- 하지만, 만약 호이스팅이 발생하지 않는다면 전역 변수 foo의 값을 출력해야함
- 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생함

<br/>

### 🐽 전역 객체와 let

---

- var 키워드로 선언한 전역 변수와 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 됨
- 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있음

```jsx
// 이 예제는 브라우저 환경에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}
```

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
- let 전역 변수는 보이지 않는 개념적인 블록 내에 존재함

```jsx
// 이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```

<br/><br/>

# const 키워드

### 🐽 선언과 초기화

---

- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함
- 그렇지 않으면 문법 에러가 발생함

```jsx
const foo; // SyntaxError: Missing initializer in const declaration
```

<br/>

- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작함

```jsx
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

<br/>

### 🐽 재할당 금지

---

- const 키워드로 선언한 변수는 재할당이 금지됨

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

<br/>

### 🐽 상수

---

- const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없음
- 원시 값은 변경 불가능한 값이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문

<br/>

- 상수는 재할당이 금지된 변수를 말함
- 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 함

```jsx
// 세전 가격
let preTaxPrice = 100;

// 세후 가격
// 0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let afterTaxPrice = preTaxPrice + (preTaxPrice * 0.1);

console.log(afterTaxPrice); // 110
```

<br/>

- 일반적으로 상수의 이름은 대문자로 선언하여 상수임을 명확히 나타냄
- 일반적으로, 여러 단어로 이뤄진 경우에는 언더스코어로 구분하여 스네이크 케이스로 표현함

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

<br/>

### 🐽 const 키워드와 객체

---

- const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음
- 객체는 재할당 없이도 직접 변경이 가능하기 떄문

```jsx
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

- const 키워드는 재할당을 금지할 뿐, “불변”을 의미하지 않음
