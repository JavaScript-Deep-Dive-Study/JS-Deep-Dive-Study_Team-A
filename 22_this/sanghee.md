# this 키워드

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조이다. 동작을 나타내는 메서드는 자식이 속한 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 한다. 이때, 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```jsx
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter());
```

따라서 자식이 속한 객체 또는 자식이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다. 이를 위해 자바스크립트는 this라는 특수한 식별자를 제공한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수로 이를 통해 프로퍼티나 메서드를 참조할 수 있다.

this는 js 엔진에 의해 암묵적으로 생성되며, 코드 어디에서든 참조할 수 있다. 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달되며 함수 내부에서 지역변수처럼 사용할 수 있다.

this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다. 객체 리터럴의 메서드 내부에서의 this는 메서드를 호출한 객체를 가리키며, 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.

```jsx
// this는 어디서든 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: f}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person("Lee");
```

<br/>

# 함수 호출 방식과 this 바인딩

this는 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

<br/>

## 일반 함수 호출

일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.

메서드 내에서 정의한 중첩 함수, 콜백함수를 일반 함수로 호출한다면 해당 함수 내부의 this에도 전역 객체가 바인딩 된다. 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.

<br/>

### **메서드 내부의 함수의 this 바인딩을 메서드의 this 바인딩을 일치시키기 위한 방법**

```jsx
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
  },
};

obj.foo();
```

그 외에도 this를 명시적으로 바인딩할 수 있는 메서드 제공(apply, call, bind)한다.

화살표 함수를 사용해서 this 바인딩을 일치시킬 수도 있다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(
      function () {
        console.log(this.value); // 100
      }.bind(this),
      100
    );
  },
};

obj.foo();
```

<br/>

## 메서드 호출

메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것에 주의해야한다.

프로토타입 메서드 내부에서 사용된 this도 호출한 객체에 바인딩된다.

<br/>

## 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```jsx
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

<br/>

## Function.prototype.apply / call / bind 메서드에 의한 간접 호출

apply, call, bind 메서드는 Function.prototype의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

```jsx
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

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.

첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

이 둘은 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

```jsx
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

apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.

call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.

대표적인 용도로는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우가 있다. arguments 객체는 배열이 아니기 때문에 배열의 메서들르 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.

```jsx
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

Function.prototype.binde 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않지만, 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
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

bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
```

bind 메서드를 사용하여 this를 일치시킬 수 있다.

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```
