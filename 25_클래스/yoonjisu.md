내용이 많으니 최대한 간결하게 요약해보기… 예제 코드 위주로…

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- JS는 프로토타입 기반의 객체지향 언어
- 원래 클래스가 필요 없는(class-free) 객체지향 프로그래밍 언어
- 클래스 기반 언어에 익숙한 프로그래머들에게 높은 진입 장벽이 되었음 → 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 매커니즘이 ES6부터 제시됨

  ```jsx
  // ES5 생성자 함수
  var Person = (function () {
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHi = function () {
      console.log("Hi! My name is " + this.name);
    };

    // 생성자 함수 반환
    return Person;
  })();

  // 인스턴스 생성
  var me = new Person("Lee");
  me.sayHi(); // Hi! My name is Lee
  ```

- 사실 클래스는 함수라고 볼 수 있음
- 클래스와 생성자 함수의 차이점
  - 클래스는 new 연산자 없이 호출하면 에러 발생, 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출
  - 클래스는 extends와 super 키워드로 상속 가능, 생성자 함수는 불가능
  - 클래스는 호이스팅이 발생하지 않는 것처럼 동작, 생성자 함수는 호이스팅이 발생
  - 클래스 내부에는 자동으로 strict mode가 지정되며 해제 불가능, 생성자 함수는 그렇지 않음
  - 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 열거 불가능 ([[Enumerable]] ⇒ false)
- 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있는 문법적 설탕(syntactic sugar)

<br>

## 25.2 클래스 정의

- class 키워드를 사용해 정의 가능
- 클래스 이름은 파스칼 케이스를 사용하는 것이 일반적, 사용하지 않는다고 에러가 발생하는 건 아님

  ```jsx
  // 클래스 선언문
  class Person {}

  // 익명 클래스 표현식
  const Person = class {};

  // 기명 클래스 표현식
  const Person = class MyClass {};
  ```

- 클래스는 표현식으로 정의 가능하며 값으로 사용할 수 있는 일급 객체
- 클래스의 특징

  - 무명 리터럴로 생성 가능 = 런타임에 생성 가능
  - 변수나 자료 구조에 저장 가능
  - 함수의 매개변수에게 전달 가능
  - 함수의 반환값으로 사용 가능

    ```jsx
    // 클래스 선언문
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name; // name 프로퍼티는 public함
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

  - 클래스 정의 방식 vs 생성자 함수 정의 방식

    ```jsx
    // 생성자 함수
    var person = (function() {
    	function Person(name) {
    		this.name = name;
    	}
    ...

    // 클래스의 생성자
    class Person {
    	constructor(name) {
    		this.name = name;
    	}
    ...
    ```

    ```jsx
    // 생성자 함수 프로토타입 메서드
    ...
    	Person.prototype.sayHi = function() {
    		console.log('Hi! My name is ' + this.name);
    	};
    ...

    // 클래스의 프로토타입 메서드
    ...
    sayHi() {
    	console.log('Hi! My name is ' + this.name);
    };
    ...
    ```

    ```jsx
    // 생성자 함수의 정적 메서드 및 함수 반환
    ...
    	Person.sayHello = function() {
    		console.log('Hello!');
    	};

    	return Person;
    }());

    // 클래스의 정적 메서드
    ...
    static sayHello() {
    	console.log('Hello!');
    }
    ...
    ```

<br>

## 25.3 클래스 호이스팅

- 클래스 선언문으로 정의된 클래스는 런타임 이전에 먼저 평가되어 함수 객체를 생성 → 이때 생성되는 함수 객체는 contructor 생성자 함수
- 생성자 함수로서 호출할 수 있는 함수는, 함수 객체가 생성되는 시점에 프로토타입도 더불어 생성

  ```jsx
  // 클래스 선언문
  class Person {}

  console.log(typeof Person); // function
  ```

  ```tsx
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
  ```

  ```jsx
  const Person = "";

  {
    // 호이스팅이 발생하지 않는다면 ''이 출력되어야 함
    console.log(Person);
    // ReferenceError: Cannot access 'Person' before initialization

    // 클래스 선언문
    class Person {}
  }
  ```

  - 클래스는 let, const 변수처럼 호이스팅 발생
  - 따라서 선언문 이전에 TDZ에 들어가서 접근 불가능

<br>

## 25.4 인스턴스 생성

- 클래스는 생성자 함수, new 연산자와 함께 호출되어 인스턴스를 생성

  ```jsx
  class Person {}

  // 인스턴스 생성
  const me = new Person();
  console.log(me); // Person {}
  ```

- 클래스의 유일한 존재 이유: 인스턴스를 생성하기 → new 연산자와 함께 호출해야 함

  ```jsx
  class Person {}

  // 클래스를 new 연산자 없이 호출하면 타입 에러 발생
  const me = Person();
  // TypeError: Class constructor Person cannot be invoked without 'new'

  const Person = class MyClass {};

  // 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 함
  const me = new Person();

  // 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자
  console.log(MyClass); // ReferenceError: MyClass is not defined

  const you = new MyClass(); // ReferenceError: MyClass is not defined
  ```

<br>

## 25.5 메서드

- 클래스의 몸체에는 0개 이상의 메서드만 선언 가능
- 클래스 몸체에서 정의할 수 있는 메서드: constructor, 프로토타입 메서드, 정적 메서드
- constructor

  - 인스턴스를 생성하고 초기화하기 위한 특수한 메서드
    ```jsx
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    }
    ```
    ```jsx
    // 클래스는 함수
    console.log(typeof Person); // function
    console.dir(Person);
    ```
    - 클래스 역시 함수와 동일하게 프로토타입과 연결됨
    - 자신의 스코프 체인을 구성
  - 모든 함수 객체가 갖고 있는, prototype 프로퍼티가 가리키는, 프로토타입 객체의 constructor 프로퍼티는, 클래스 자신

    ```jsx
    // 인스턴스 생성
    const me = new Person("Lee");
    console.log(me);

    // constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 됨

    // 클래스
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
      }
    }

    // 생성자 함수
    function Person(name) {
      // 인스턴스 생성 및 초기화
      this.name = name;
    }
    ```

  - constructor는 하나의 메서드로 해석되는 것 X, 클래스가 평가되어 생성한 함수 객체 코드의 일부가 됨
  - 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성됨
  - constructor는 클래스 내에 최대 한 개만 존재

    ```jsx
    class Person {
      constructor() {}
      constructor() {}
    }
    // SyntaxError: A class may only have one constructor

    // constructor를 작성하지 않으면 암묵적으로 빈 constructor 객체가 생성

    class Person {}

    class Person {
      // constructor를 생략하면 다음과 같이 빈 constructor가 암묵적으로 정의됨
      constructor() {}
    }

    // 빈 객체 생성
    const me = new Person();
    console.log(me); // Person {}
    ```

  - constructor 내부의 this에 인스턴스 프로퍼티를 추가하여 인스턴스 초기화 가능

    ```jsx
    class Person {
      constructor() {
        // 고정값으로 인스턴스 초기화
        this.name = "Lee";
        this.address = "Seoul";
      }
    }

    // 인스턴스 프로퍼티가 추가됨
    const me = new Person();
    console.log(me); // Person {name: "Lee", address: "Seoul"}
    ```

  - 클래스 외부에서 인스턴스 프로퍼티 초기값을 전달하려면 매개 변수로 받아올 수 있음

    ```jsx
    class Person {
      constructor(name, address) {
        // 인수로 인스턴스 초기화
        this.name = name;
        this.address = address;
      }
    }

    // 인수로 초기값을 전달, 초기값은 constructor에 전달
    const me = new Person("Lee", "Seoul");
    console.log(me); // Person {name: "Lee", address: "Seoul"}
    ```

  - new 연산자와 함께 클래스가 호출되면, 생성자 함수와 동일하게 암묵적으로 this 인스턴스가 반환

    ```jsx
    class Person {
      constructor(name) {
        this.name = name;

        // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시됨
        return {};
      }
    }

    // constructor에서 명시적으로 반환한 빈 객체가 반환됨
    const me = new Person("Lee");
    console.log(me); // {}

    class Person {
      constructor(name) {
        this.name = name;

        // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환됨
        return 100;
      }
    }

    const me = new Person("Lee");
    console.log(me); // Person { name: "Lee" }

    // 클래스에서 this가 아닌 다른 값을 반환하는 것은 클래스의 기본 동작을 해치는 행위
    ```

- 프로토타입 메서드

  - 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨

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

  - 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 됨

    ```jsx
    // me 객체의 프로토타입은 Person.prototype
    Object.getPrototypeOf(me) === Person.prototype; // -> true
    me instanceof Person; // -> true

    // Person.prototype의 프로토타입은 Object.prototype
    Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
    me instanceof Object; // -> true

    // me 객체의 constructor는 Person 클래스
    me.constructor === Person; // -> true
    ```

  - 프로토타입 체인은 클래스에 의해 생성된 인스턴스에도 동일하게 적용

- 정적 메서드

  - 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

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

  - static 키워드를 붙이면 정적 메서드 선언 가능

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

  - 정적 메서드는 클래스에 바인딩된 메서드가 됨
  - 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있음
  - 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출 가능

    ```jsx
    // 정적 메서드는 클래스로 호출
    // 정적 메서드는 인스턴스 없이도 호출 가능
    Person.sayHi(); // Hi!

    // 인스턴스 생성
    const me = new Person("Lee");
    me.sayHi(); // TypeError: me.sayHi is not a function
    ```

- 정적 메서드와 프로토타입 메서드의 차이
  - 정적 메서드와 프로토타입 메서드는 프로토타입 체인이 다름
  - 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출
  - 정적 메서드는 인스턴스 프로퍼티 참조 불가능, 프로토타입 메서드는 가능
- 클래스에서 정의한 메서드의 특징
  - function 키워드를 생략한 메서드 축약 표현 사용
  - 클래스에서 메서드를 정의할 때는 콤마 불필요
  - 암묵적으로 strict mode로 실행
  - for … in 또는 Object.keys 로 열거 불가능
  - 내부 메서드 [[Contructor]]를 갖지 않는 non-constructor, new 연산자와 함께 호출 불가능

<br>

## 25.6 클래스의 인스턴스 생성 과정

- 1: 인스턴스 생성과 this 바인딩
  - new 연산자와 함께 호출하면 먼저 암묵적으로 빈 객체가 인스턴스로 생성됨
  - 이 인스턴스의 프로토타입은 클래스의 prototype 프로퍼티가 가리키는 객체로 설정
  - 즉, 인스턴스는 this에 바인딩
- 2: constructor 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화
  - this에 바인딩되어 있는 인스턴스에 프로퍼티 추가
  - constructor에 인수로 전달된 초기값으로 인스턴스 프로퍼티를 초기화
- 3: 인스턴스 반환
  - 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

<br>

## 25.7 프로퍼티

- 인스턴스 프로퍼티

  - 인스턴스 프로퍼티는 constructor 내부에서 정의

    ```jsx
    class Person {
      constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name;
      }
    }

    const me = new Person("Lee");
    console.log(me); // Person {name: "Lee"}
    ```

    - constructor 코드가 실행되기 전에는 암묵적으로 생성된 빈 객체가 인스턴스로서 존재함
    - constructor 코드가 실행되어 인스턴스에 프로퍼티를 추가하면서 초기화

      ```jsx
      class Person {
        constructor(name) {
          // 인스턴스 프로퍼티
          this.name = name; // name 프로퍼티는 public함
        }
      }

      const me = new Person("Lee");

      // name은 public함
      console.log(me.name); // Lee
      ```

- 접근자 프로퍼티

  - 접근자 프로퍼티는 자체적으로는 값을 갖지 않음
  - 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티

    ```jsx
    const person = {
      // 데이터 프로퍼티
      firstName: "Ungmo",
      lastName: "Lee",

      // fullName은 접근자 함수로 구성된 접근자 프로퍼티
      // getter 함수
      get fullName() {
        return `${this.firstName} ${this.lastName}`;
      },
      // setter 함수
      set fullName(name) {
        // 배열 디스트럭처링 할당: "36.1. 배열 디스트럭처링 할당" 참고
        [this.firstName, this.lastName] = name.split(" ");
      },
    };

    // 데이터 프로퍼티를 통한 프로퍼티 값의 참조
    console.log(`${person.firstName} ${person.lastName}`); // Ungmo Lee

    // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
    // 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출됨
    person.fullName = "Heegun Lee";
    console.log(person); // {firstName: "Heegun", lastName: "Lee"}

    // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
    // 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출
    console.log(person.fullName); // Heegun Lee

    // fullName은 접근자 프로퍼티
    // 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 가짐
    console.log(Object.getOwnPropertyDescriptor(person, "fullName"));
    // {get: ƒ, set: ƒ, enumerable: true, configurable: true}
    ```

    ```jsx
    class Person {
      constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
      }

      // fullName은 접근자 함수로 구성된 접근자 프로퍼티
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

    // 데이터 프로퍼티를 통한 프로퍼티 값의 참조
    console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

    // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
    // 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출
    me.fullName = "Heegun Lee";
    console.log(me); // {firstName: "Heegun", lastName: "Lee"}

    // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
    // 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출
    console.log(me.fullName); // Heegun Lee

    // fullName은 접근자 프로퍼티
    // 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 가짐
    console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
    // {get: ƒ, set: ƒ, enumerable: false, configurable: true}
    ```

    - getter: 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
    - setter : 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용

- 클래스 필드 정의 제안

  - 클래스 필드(class field)
    - 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티
  - 클래스 몸체에서 클래스 필드를 정의하려면 this에 바인딩 X, this는 constructor와 메서드 내에서만 유효

    ```jsx
    class Person {
      // this에 클래스 필드를 바인딩해서는 안 됨
      this.name = ''; // SyntaxError: Unexpected token '.'
    }

    // this를 반드시 사용

    class Person {
      // 클래스 필드
      name = 'Lee';

      constructor() {
        console.log(name); // ReferenceError: name is not defined
      }
    }

    new Person();

    // 초기값을 할당하지 않으면 undefined

    class Person {
      // 클래스 필드를 초기화하지 않으면 undefined를 가짐
      name;
    }

    const me = new Person();
    console.log(me); // Person {name: undefined}

    // constructor 내부에서만 초기화

    class Person {
      name;

      constructor(name) {
        // 클래스 필드 초기화
        this.name = name;
      }
    }

    const me = new Person('Lee');
    console.log(me); // Person {name: "Lee"}
    ```

  - 클래스 필드에 함수를 할당하면 인스턴스의 메서드가 됨 → 모든 클래스 필드는 인스턴스 프로퍼티이기 때문 → 따라서 클래스 필드에 함수 할당은 비권장사항

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;

            // increase 메서드를 이벤트 핸들러로 등록
            // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킴
            // 하지만 increase는 화살표 함수로 정의되어 있으므로
            // increase 내부의 this는 인스턴스를 가리킴
            this.$button.onclick = this.increase;

            // 만약 increase가 화살표 함수가 아니라면 bind 메서드를 사용해야 함
            // $button.onclick = this.increase.bind(this);
          }

          // 인스턴스 메서드
          // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this를 가리킴
          increase = () => this.$button.textContent = ++this.count;
        }
        new App();
      </script>
    </body>
    </html>
    ```

- private 필드 정의 제안

  - 인스턴스 프로퍼티는 클래스 외부에서 접근이 가능한 public

    ```jsx
    class Person {
      constructor(name) {
        this.name = name; // 인스턴스 프로퍼티는 기본적으로 public함
      }
    }

    // 인스턴스 생성
    const me = new Person("Lee");
    console.log(me.name); // Lee
    ```

  - #을 사용해 private을 정의할 수 있는 표준 사양이 TC39 프로세스의 stage 3에서 제안됨

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

    // private 필드 #name은 클래스 외부에서 참조 불가능
    console.log(me.#name);
    // SyntaxError: Private field '#name' must be declared in an enclosing class
    ```

- static 필드 정의 제안

  - TC39 프로세스의 stage 3에서 “Static class features”가 제안됨
  - static public 필드, static private 필드, static private 메서드를 정의할 수 있음

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

<br>

## 25.8 상속에 의한 클래스 확장

- 클래스 상속과 생성자 함수 상속

  - 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장해 정의하는 것

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

- extends 키워드
  - 서브 클래스(sub-class): 상속을 통해 확장된 클래스, 파생 클래스 또는 자식 클래스
  - 수퍼 클래스(super-class): 서브 클래스에게 상속된 클래스, 베이스 클래스 또는 부모 클래스
  - 수퍼 클래스와 서브 클래스는 extends 키워드를 통해 상속 관계를 결정
  - 동시에 클래스 간의 프로토타입 체인도 생성하여 프로토타입 메서드, 정적 메서드 모두 상속 가능
- 동적 상속

  - extends 키워드 앞에는 반드시 클래스가 와야 함

    ```jsx
    // 생성자 함수
    function Base(a) {
      this.a = a;
    }

    // 생성자 함수를 상속받는 서브 클래스
    class Derived extends Base {}

    const derived = new Derived(1);
    console.log(derived); // Derived {a: 1}
    ```

  - extends 키워드 다음에는 클래스 뿐만 아니라 [[Constructor]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식이 올 수 있음

    ```jsx
    function Base1() {}

    class Base2 {}

    let condition = true;

    // 조건에 따라 동적으로 상속 대상을 결정하는 서브 클래스
    class Derived extends (condition ? Base1 : Base2) {}

    const derived = new Derived();
    console.log(derived); // Derived {}

    console.log(derived instanceof Base1); // true
    console.log(derived instanceof Base2); // false
    ```

- 서브 클래스의 constructor
  - 서브 클래스의 constructor 역시 생략하면 빈 객체가 생성됨
  - 프로퍼티가 있는 인스턴스를 생성하려면 서브 클래스 내부의 constructor에서 프로퍼티를 추가해야 함
- super 키워드

  - super 호출 시 수퍼 클래스의 constructor(super-constructor)를 호출

    ```jsx
    // 수퍼 클래스
    class Base {
      constructor(a, b) {
        this.a = a;
        this.b = b;
      }
    }

    // 서브 클래스
    class Derived extends Base {
      // 다음과 같이 암묵적으로 constructor가 정의됨
      // constructor(...args) { super(...args); }
    }

    const derived = new Derived(1, 2);
    console.log(derived); // Derived {a: 1, b: 2}
    ```

  - 서브 클래스에서 constructor를 사용하려면 반드시 super를 호출해야 함

    ```jsx
    class Base {}

    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        console.log("constructor call");
      }
    }

    const derived = new Derived();
    ```

  - 서브 클래스의 constructor에서 super를 호출하기 이전에는 this 참조 불가능

    ```jsx
    class Base {}

    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        this.a = 1;
        super();
      }
    }

    const derived = new Derived(1);
    ```

  - super는 반드시 서브 클래스의 constructor에서만 호출

    ```jsx
    class Base {
      constructor() {
        super(); // SyntaxError: 'super' keyword unexpected here
      }
    }

    function Foo() {
      super(); // SyntaxError: 'super' keyword unexpected here
    }
    ```

  - super 참조: 메서드 내에서 super를 참조하면 수퍼 클래스의 메서드 호출 가능

    ```jsx
    // super 참조를 통해 수퍼 클래스의 메서드를 참조하려면
    // super가 수퍼 클래스의 prototype 프로퍼티에 바인딩된
    // 프로토타입을 참조할 수 있어야 함

    // 수퍼 클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        return `Hi! ${this.name}`;
      }
    }

    class Derived extends Base {
      sayHi() {
        // __super는 Base.prototype을 가리킴
        const __super = Object.getPrototypeOf(Derived.prototype);
        return `${__super.sayHi.call(this)} how are you doing?`;
      }
    }

    /*
    [[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킴
    [[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있음
    ex) Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩됨
    따라서 Derived 클래스의 sayHi 메서드의 [[HomeObject]]는 Derived.prototype이고
    이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정
    따라서 super.sayHi는 Base.prototype.sayHi를 가리키게 됨
    */
    super = Object.getPrototypeOf([[HomeObject]])
    ```

  - ES6의 메서드 축약 표현으로 정의된 함수만이 super 참조 가능

    ```jsx
    const obj = {
      // foo는 ES6의 메서드 축약 표현으로 정의한 메서드다, 따라서 [[HomeObject]]를 가짐
      foo() {},
      // bar는 ES6의 메서드 축약 표현으로 정의한 메서드가 아니라 일반 함수
      // 따라서 [[HomeObject]]를 갖지 않음
      bar: function () {},
    };
    ```

    ```jsx
    const base = {
      name: "Lee",
      sayHi() {
        return `Hi! ${this.name}`;
      },
    };

    const derived = {
      __proto__: base,
      // ES6 메서드 축약 표현으로 정의한 메서드, 따라서 [[HomeObject]]를 가짐
      sayHi() {
        return `${super.sayHi()}. how are you doing?`;
      },
    };

    console.log(derived.sayHi()); // Hi! Lee. how are you doing?
    ```

    ```jsx
    // 수퍼 클래스
    class Base {
      static sayHi() {
        return "Hi!";
      }
    }

    // 서브 클래스
    class Derived extends Base {
      static sayHi() {
        // super.sayHi는 수퍼 클래스의 정적 메서드를 가리킴
        return `${super.sayHi()} how are you doing?`;
      }
    }

    console.log(Derived.sayHi()); // Hi! how are you doing?
    ```

- 상속 클래스의 인스턴스 생성 과정

  - 1: 서브 클래스의 super 호출
    - 자바스크립트 엔진은 수퍼 클래스와 서브 클래스 구분을 위해 내부 슬롯 [[ConstructorKind]]를 가짐
    - 아무것도 상속받지 않는 클래스는 “base”로, 상속을 받는 클래스는 “derived”로 설정됨
  - 2: 수퍼 클래스의 인스턴스 생성과 this 바인딩

    - super를 통해 수퍼 클래스의 인스턴스가 생성
    - new 연산자와 함께 호출된 클래스는 서브 클래스, 따라서 인스턴스는 서브 클래스가 생성한 것으로 처리됨

      ```jsx
      // 수퍼 클래스
      class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩
          console.log(this); // ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle
          console.log(new.target); // ColorRectangle
      ...
      ```

      ```jsx
      // 수퍼 클래스
      class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩
          console.log(this); // ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle
          console.log(new.target); // ColorRectangle

          // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정
          console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
          console.log(this instanceof ColorRectangle); // true
          console.log(this instanceof Rectangle); // true
      ...
      ```

  - 3: 수퍼 클래스의 인스턴스 초기화

    - 수퍼 클래스의 constructor가 실행되어 this에 바인딩된 인스턴스 프로퍼티를 초기화

      ```jsx
      // 수퍼 클래스
      class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩
          console.log(this); // ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle
          console.log(new.target); // ColorRectangle

          // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정
          console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
          console.log(this instanceof ColorRectangle); // true
          console.log(this instanceof Rectangle); // true

          // 인스턴스 초기화
          this.width = width;
          this.height = height;

          console.log(this); // ColorRectangle {width: 2, height: 4}
        }
      ...
      ```

  - 4: 서브 클래스 constructor로의 복귀와 this 바인딩

    - super가 반환한 인스턴스가 this에 바인딩, 서브 클래스는 이를 그대로 사용

      ```jsx
      // 서브 클래스
      class ColorRectangle extends Rectangle {
        constructor(width, height, color) {
          super(width, height);

          // super가 반환한 인스턴스가 this에 바인딩
          console.log(this); // ColorRectangle {width: 2, height: 4}
      ...

      // 즉, super가 호출되지 않으면 this 바인딩이 불가능
      ```

  - 5: 서브 클래스의 인스턴스 초기화
    - super 호출 이후 서브 클래스의 constructor 쪽의 인스턴스가 초기화
  - 6: 인스턴스 반환
    - 모든 처리가 끝난 후 인스턴스가 바인딩된 this가 암묵적으로 반환

- 표준 빌트인 생성자 함수 확장

  - String, Number, Array와 같은 표준 빌트인 객체 역시 [[Construct]] 내부 메서드를 갖는 생성자 함수 → extends 키워드를 통한 확장 가능

    ```jsx
    // Array 생성자 함수를 상속받아 확장한 MyArray
    class MyArray extends Array {
      // 중복된 배열 요소를 제거하고 반환: [1, 1, 2, 3] => [1, 2, 3]
      uniq() {
        return this.filter((v, i, self) => self.indexOf(v) === i);
      }

      // 모든 배열 요소의 평균을 구함: [1, 2, 3] => 2
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
