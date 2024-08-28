# [22장] this

## 22.1 this 키워드

> 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야한다.<br/>
> `this` 키워드가 메서드 자신이 속한 객체를 가리키고 있기에 가능하다.

<br/>

- `circle` 내부에서 `circle` 을 참조하는 형태
- 이런 재귀적인 코드가 가능한 이유는, `getDiameter()` 메서드가 실행되기 전에 circle 객체가 생성되어 있기 때문이다.
- 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지도 않다.

```javascript
const circle = {
  radius: 5, // 프로퍼티: 객체 고유의 상태 데이터

  getDiameter() {
    // 메서드: 프로퍼티를 참조하고 조작하는 동작
    // 자신이 속한 객체인 circle을 참조 할 수 있어야 한다. 
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

<br/>

- 생성자 함수를 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.<br>
- 그러므로 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다.

```javascript
function Circle(radius) {
// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 
    ????.radius = radius;
}

// 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 
Circle.prototype.getDiameter = function () {
    return 2 * ????.radius;
}

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다. 
const circle = new Circle(5);
```

<br/>

- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조 할 수 있다.
- this를 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다. 

```javascript
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

<br/>

```javascript
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

<br/>

- 자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, this 바인딩이 동적으로 결정된다. (상황에 따라 가리키는 대상이 다르다.)
- this는 코드 어디서든 참조 가능하다.

```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

<br/>

- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이다.
- 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
- strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다. 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

<br/><br/>

## 22.2 함수 호출 방식과 this 바인딩

> `this` 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

<br/>

- 함수를 호출하는 다양한 방식
  - 일반 함수 호출
  - 메서드 호출
  - 생성자 함수 호출
  - Function.prototype.apply/call/bind 메서드에 의한 간접 호출

``` javascript
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```   

<br/><br/>

### 22.2.1 일반 함수 호출

> 일반 함수로 호출하면, 기본적으로 this에는 전역 객체가 바인딩된다.

<br/>

- 일반 함수로 호출하는 경우 기본적으로 this에는 전역 객체가 바인딩된다.

```javascript
function foo() {
  console.log("foo's this: ", this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
});
```

<br/>

- strict mode에서는 this에 undefined가 바인딩된다.

```javascript
function foo() {
  'use strict';

  console.log("foo's this: ", this);  // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

<br>

- 매서드 내의 중첩 함수라도 일반 함수로 호출되면 함수 내부의 this에는 전역 객체가 바인딩된다. 

```javascript
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this);  // {value: 100, foo: ƒ}
    console.log("foo's this.value: ", this.value); // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  }
};

obj.foo();
```

<br/>

- 메서드 내부의 중첨 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법

```javascript
// that 사용 
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  }
};

obj.foo();
```

<br/>

```javascript
// Function.prototype.apply/call/bind메서드 활용
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(function () {
      console.log(this.value); // 100
    }.bind(this), 100);
  }
};

obj.foo();
```

<br/>

```javascript
// 화살표 함수 활용
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  }
};

obj.foo();
```

<br/><br/>

### 22.2.2 메서드 호출

> 메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.

<br/>

```javascript
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

- `getName` 메서드는 `person` 객체의 메서드로 정의되었다. 메서드는 프로퍼티에 바인딩된 함수다.
- 즉, `person` 객체의 `getName` 프로퍼티가 함수 객체를 가리키고 있을 뿐이다. `getName` 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

<br/>

- `getName` 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```javascript
const person = {
  name: 'Lee',
  getName() {
    return this.name;
  }
};

const anotherPerson = {
  name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

<br/>

- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // Lee
//  getName메서드를 호출한 객체는 me이다. 따라서 getName메서드 내부의 this는 me를 가리키며 this.name값은 Lee다.

Person.prototype.name = 'Kim';
// getName메서드를 호출한 객체는 Person.prototype이고, 객체이므로 직접 메서드를 호출할 수 있다. 
// 따라서 getName메서드 내부의 this는 Person.prototype을 가리키며 this.name값은 Kim이다.

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // Kim
```

<br/><br/>

### 22.2.3  생성자 함수 호출

> 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

<br/>

``` javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

```

<br/><br/>

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

> 위 메서드들은 `Function.prototype` 의 메서드로  모든 함수가 상속받아 사용할 수 있다.<br/>
> `apply()` 와 `call()` 메서드는 함수가 `this binding`을 진행할 수 있게 해준다.

```javascript
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, agr2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

<br/>

- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

<br/>
  
- apply와 call 메서드의 본질적인 기능은 동일하다. 다만 인수를 전달하는 방식만 다르다.
- apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
- call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.

```javascript
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```

<br/>

- apply와 call 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 베서드를 사용하는 경우다.
- arguments 객체는 배열이 아니기 때문에 `Array.prototype.slice` 같은 배열의 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.

```javascript
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체를 배열로 변환
  // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성한다.
  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

<br/>

- `Function.prototype.bind` 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다.
- 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

<br/>

- bind 메서드는 메서드의 this와 메서드 내부의 중첨 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```javascript
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
```

- `1`의 시점에서 this는 `foo` 메서드를 호출한 객체인 `person` 객체를 가리킨다.
- 그러나 `person.foo`의 콜백 함수가 일반 함수로서 호출된 `2`의 시점에서 this는 전역 객체 `window`를 가리킨다.
- 따라서 `person.foo`의 콜백 함수 내부에서 `this.name`은 `window.name`과 같다.

<br/>

- `person.foo`의 콜백 함수는 외부 함수 `person.foo`를 돕는 헬퍼 함수 역할을 한다.
- 외부 함수 `person.foo` 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 발생한다.
- 이때 bind 메서드를 사용하여 this를 일치시킬 수 있다.

```javascript
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

<br/><br/>

| 함수호출방식                                            | this 바인딩                                                        |
| ------------------------------------------------------- | ------------------------------------------------------------------ |
| 일반 함수 호출                                           | 전역객체                                                            |
| 메서드 호출                                              | 메서드를 호출한 객체                                                |
| 생성자 함수 호출                                         | 생성자 함수가 (미래에) 생성할 인스턴스                                |
| Function.prototype.apply/call/bind 메서드에 의한 간접호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |
