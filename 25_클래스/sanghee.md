## 클래스는 프로토타입의 문법적 설탕인가?

ES6에서 도입된 클래스는 **클래스 기반 객체지향 프로그래밍언어와 매우 흡사한 새로운 객체 생성 메커니즘을 제시**한다.  
그렇다고 클래스가 기존 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체 지향 모델을 제공하는 것은 아니다.

클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕이라고 볼 수도 있다.

> **💡 문법적 설탕(syntactic sugar)**  
> 문법적 기능은 그대로인데 그것을 읽는 사람이 직관적으로 쉽게 코드를 읽을 수 있게 만드는 것

클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며 생성자 함수에서 제공하지 않는 기능도 제공한다.

<br/>

### 클래스와 생성자 함수의 차이

1. **클래스는 new 연산자 없이 호출하면 에러가 발생한다.**
   하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. **클래스는 상속을 지원하는 extends와 super 키워드를 제공한다.**
   하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.
3. **클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.**
   하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. **클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다.**
   하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
5. **클래스의 contributer, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumberable]]의 값이 false다. 즉, 열거되지 않는다.**

생성자 함수와 클래스는 **프로토타입 기반의 객체지향을 구현**했다는 점에서 매우 유사하다.

<br/>

## 클래스 정의

클래스는 class 키워드를 사용하여 정의 한다.

클래스 이름은 파스칼 케이스를 쓰는 것이 일반적이다.

일반적이진 않지만 함수와 마찬가지로 표현식으로 클래스를 정의할 수 있다. 이는 값으로 사용할 수 있는 일급 객체라는 것을 의미한다. 더 자세히 말하자면 클래스는 함수다.

```jsx
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("Hello!");
  }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

<br/>

## 클래스 호이스팅

클래스는 함수로 평가된다

```jsx
class Person {}

console.log(typeof Person); // function
```

클래스 선언문으로 정의한 클래스는 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor다.

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하므로 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

클래스는 클래스 정의 이전에 참조할 수 없다.

클래스는 let, const 키워드로 선언한 변수처럼 호이스팅되므로 클래스 선언문 이전에 일시적 사각지대(TDZ)에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

<br/>

## 인스턴스 생성

클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다. 이때, 반드시 new 연산자와 함께 호출해야한다.

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

<br/>

## 메서드

클래스 몸체에서 정의할 수 있는 메서드는 cunstructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

<br/>

### constructor

constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. 내부의 this는 생성한 인스턴스를 가리킨다.

메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.

클래스 내에 최대 한 개만 존재할 수 있으며, 생략할 수 있고 생략하면 클래스에 빈 constructor가 암묵적으로 정의되며 빈 객체를 생성한다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티는 이름이 같지만 직접적인 관련이 없다. 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.

```jsx
class Person {
  constructor() {
    // 고정값으로 인스턴스 초기화
    this.name = "Lee";
    this.address = "Seoul";
  }
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person();
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다.

```jsx
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person("Lee", "Seoul");
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

별도의 반환문을 갖지 않아야 한다. new 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문이다.

만약 this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return문에 명시한 객체가 반환된다.

<br/>

### 프로토타입 메서드

생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 다음과 같이 명시적으로 프로토타입에 메서드를 추가해야 한다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

프로토타입 체인은 기존의 모든 객체 생성 방식 뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용한다. 생성자 함수의 역할을 클래스가 할 뿐이다.

<br/>

### 정적 메소드

정적 메서드는 인스턴스가 생성하지 않아도 호출할 수 있는 메서드를 말한다.

생성자 함수의 경우 정적 메서드를 생성하기 위해서 다음과 같이 명시적으로 생성자 함수에 메서드를 추가해야 한다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 정적 메서드
Person.sayHi = function () {
  console.log("Hi!");
};

// 정적 메서드 호출
Person.sayHi(); // Hi!
```

클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성 과정이 필요 없다. 따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다.

정적 메서드는 인스턴스로 호출할 수 없다. 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에서 존재하지 않기 때문이다. 즉, 인스턴스로 클래스의 메서드를 상속받을 수 없다.

<br/>

### 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

<br/>

### 클래스에서 정의한 메서드들의 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for … in 문이나 Object.keys 메서드 등으로 열거할 수 없ㅂ다. 즉, [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Constructor]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.

<br/>

## 클래스 인스턴스 생성 과정

new 연산자와 함께 클래스를 호출하면 클래스의 내부 메서드 [[Constructor]]가 호출된다.

1. 인스턴스 바인딩과 this 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

<br/>

## 프로퍼티

<br/>

### 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

인스턴스는 언제나 public하다.

```jsx
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public하다.
  }
}

const me = new Person("Lee");

// name은 public하다.
console.log(me.name); // Lee
```

<br/>

### 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값([[Value]] 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

```jsx
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
}

const me = new Person("Ungmo", "Lee");

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
me.fullName = "Heegun Lee";
console.log(me); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

<br/>

### 클래스 필드 정의 제안

클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.

클래스 기반 객체지향 언어의 this는 언제나 클래스가 생성할 인스턴스를 가리킨다.

js의 클래스 몸체에는 메서드만 선언할 수 있다.

```jsx
class Person {
  // 클래스 필드 정의
  name = "Lee";
}

const me = new Person("Lee");
```

최신 브라우저와 최신 Node.js에서는 다음 예제와 같이 클래스 필드를 몸체에 정의할 수 있다.

```jsx
class Person {
  // 클래스 필드 정의
  name = "Lee";
}

const me = new Person();
console.log(me); // Person {name: "Lee"}
```

클래스 필드에 함수를 할당하는 경우, 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다. 따라서 이 방법은 권장하지 않는다.

<br/>

### private 필드 정의 제안

인스턴스 프로퍼티는 언제나 public이다. 하지만 최신 버전부터 private 필드를 정의할 수 있게 됐다.

private 필드의 선두에는 #을 붙여주고, 참조할 때도 #을 붙여줘야 한다

public 필드는 어디서든 참조할 수 있지만 private 필드는 클래스 내부에서만 참조할 수 있다.

클래스 외부에서 private 필드에 접근할 수 있는 방법은 없지만 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.

private 필드는 반드시 클래스 몸체에 정의해야 한다.

```jsx
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

<br/>

### static 필드 정의 제안

최신 버전부터 static public/private 필드를 정의할 수 있게 됐다.

```jsx
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

<br/>

## 상속에 의한 클래스 확장

<br/>

### 클래스 상속과 생성자 함수 상속

상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

코드 재사용 관점에서 매우 유용하다

```jsx
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() {
    return "eat";
  }

  move() {
    return "move";
  }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() {
    return "fly";
  }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드가 기본적으로 제공된다.

<br/>

### extends 키워드

상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

```jsx
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

상속을 통해 확장된 클래스는 서브클래스라 부르고, 서브클래스에게 상속된 클래스를 수퍼클래스라 부른다.

extends 키워드의 역할은 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다. 클래스도 프로토타입을 통해 상속 관계를 구현한다.

수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.

<br/>

### 동적 상속

extends 키워드는 클래스 뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수 있다.

단, extends 키워드 앞에는 반드시 클래스가 와야한다.

뒤에는 클래스 뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

```jsx
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

<br/>

### 서브클래스의 constructor

클래스에서 constructor를 생략하면 클래스에 다음과 같이 비어있는 constructor가 암묵적으로 정의된다.

```jsx
constructor() {}
```

서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다.

args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.

```jsx
constructor(...args) { super(...args); }
```

super()는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성한다.

다음 예제를 보면 수퍼클래스와 서브클래스 모두 constructor를 생략했다.

```jsx
// 수퍼클래스
class Base {}

// 서브클래스
class Derived extends Base {}
```

위 예제의 클래스에는 다음과 같이 암묵적으로 constructor가 정의된다.

```jsx
// 수퍼클래스
class Base {
  constructor() {}
}

// 서브클래스
class Derived extends Base {
  constructor() {
    super();
  }
}

const derived = new Derived();
console.log(derived); // Derived {}
```

위 예제와 같이 수퍼클래스와 서브클래스 모두 constructor를 생략하면 빈 객체가 생성된다.

프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 한다.

<br/>

### super 키워드

super 키워드는 함수처럼 호출할 수 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

**super 호출**

super을 호출하면 수퍼클래스의 constructor(super-consturctor)를 호출한다.

```jsx
// 수퍼클래스
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  // 다음과 같이 암묵적으로 constructor가 정의된다.
  // constructor(...args) { super(...args); }
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 없다.

인스턴스 초기화를 위해 전달한 인수는 수퍼클래스와 서브클래스에 배분되고 상속 관계의 두 클래스는 서로 협력하여 인스턴스를 생성한다.

**super을 호출할 때 주의사항**

1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super을 호출해야 한다.
2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
3. super는 반드시 서브클래스의 constructor에서만 호출한다.

<br/>

**super 참조**

메서드 내에서 super을 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

super 참조가 동작하기 위해서는 super를 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 찾을 수 있어야 한다. 이를 위해 메서드는 내부 슬롯 [[HomeObject]]를 가지며, 자신을 바인딩하고 있는 객체를 가리킨다.

[[HomeObject]]를 가지는 함수만이 super 참조를 할 수 있으며, ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다.

super 참조는 클래스의 전유물은 아니며, 객체 리터럴에서도 사용할 수 있다.

<br/>

### 상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 super 호출
2. 수퍼클래스의 인스턴스 생성과 this 바인딩
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 constructor로의 복귀와 this 바인딩
5. 서브클래스의 인스턴스 초기화
6. 인스턴스 반환

<br/>

### 표준 빌트인 생성자 함수 확장

extends 키워드 다음에는 클래스 뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.

```jsx
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```
