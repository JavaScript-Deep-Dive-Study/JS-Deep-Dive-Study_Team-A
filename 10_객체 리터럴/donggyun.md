# 객체

### 🐽 객체

---

- 자바스크립트는 객체 기반의 프로그래밍 언어
- 원시 값을 제외한 나머지 값은 모두 객체
- 0개 이상의 프로퍼티로 구성된 집합

<br/>

### 🐽 객체 타입

---

- 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조
- 원시 값은 변경 불가능한 값이지만 객체는 변경 가능한 값

<br/><br/>

# 객체 리터럴에 의한 객체 생성

### 🐽 객체 생성

---

- 클래스 기반 객체지향 언어
    - 클래스를 사전에 정의하고, 필요한 시점에 `new`연산자와 함께 생성자를 호출하여 인스턴스를 생성
- 자바스크립트 ( 프로토타입 기반 객체지향 언어 )
    - 다양한 객체 생성 방법을 지원

<br/>

### 🐽 객체 리터럴

---

- 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석하여 객체를 생성

```jsx
var person = {
  name: 'Lee',
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}
```

<br/>

- 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성됨

```jsx
var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

<br/>

- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않음
- 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요 없음
- 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가 가능

<br/><br/>

# 프로퍼티

### 🐽 프로퍼티

---

- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성됨

```jsx
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: 'Lee',
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20
};
```

- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

<br/>

- 프로퍼티 키는 식별자 역할을 하지만 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아님
- 가급적 식별자 네이밍 규칙을 준수하는 프로퍼티 키를 사용할 것을 권장

```jsx
var person = {
  firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```

<br/>

- 문자열  또는 문자열로 평가할 수 있는 표현식을 사용하여 프로퍼티 키를 동적으로 생성 가능
- 이때, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 함

```jsx
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' };

console.log(obj); // {hello: "world"}
```

<br/>

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 됨

```jsx
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

<br/>

- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 덮어씀

```jsx
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```

<br/><br/>

# 메서드

### 🐽 메서드

---

- 자바스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있음
- 함수도 프로퍼티 값으로 사용 가능
- 프로퍼티 값이 함수일 경우 메서드라 부름

```jsx
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

<br/><br/>

# 프로퍼티 접근

### 🐽 접근 방법

---

- 마침표 표기법 / 대괄호 표기법

```jsx
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

<br/><br/>

# 프로퍼티 변경

### 🐽 값 갱신

---

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신됨

```jsx
var person = {
  name: 'Lee'
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```

<br/>

### 🐽 동적 생성

---

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당됨

```jsx
var person = {
  name: 'Lee'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
// 따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

<br/>

### 🐽 삭제

---

- delete 연산자는 객체의 프로퍼티를 삭제함
- delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 함
- 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시됨

```jsx
var person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}
```

<br/><br/>

# ES6 객체 리터럴 확정 기능

### 🐽 프로퍼티 축약 표현

---

- 프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름일 때, 프로퍼티 키를 생략할 수 있음
- 프로퍼티 키는 변수 이름으로 자동 생성됨

```jsx
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

<br/>

### 🐽 계산된 프로퍼티 이름

---

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용하여 프로퍼티 키를 동적으로 생성할 수 있음
- 단, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 함
- 계산된 프로퍼티 이름이라 함

- ES5에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호 표기법을 사용해야 함

```jsx
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

<br/>

- ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성 가능

```jsx
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

<br/>

### 🐽 메서드 축약 표현

---

- ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당함

```jsx
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

<br/>

- ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현 사용 가능

```jsx
// ES6
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```
