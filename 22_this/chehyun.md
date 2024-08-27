### 22.1 this 키워드

- 객체는 프로퍼티 + 메서드, 복합적인 자료구조임
- 메서드는 자신이 속한 프로퍼티를 참조하고 변경할 수 있어야 함
- 이를 위해 자신이 속한 객체를 가리키는 **식별자를 참조**할 수 있어야 함
    - 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 함
    - 근데 함수를 정의하는 시점은 아직 **인스턴스를 생성하기 이전**임
    - 그래서 자신이 속한 객체, 인스턴스를 가리키는 특수한 식별자가 필요함
- this는 자신이 속한 객체 또는 생성할 인스턴스의 프로퍼티나 메소드를 참조할 수 있음
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정됨
    - **`this`는 함수를 호출할 때** 그 함수 내부에서 사용되는 특별한 키워드로, **어떤 객체를 가리킬지**를 결정함
    - 근데 어떤 객체를 가리킬지는 함수 호출 방식에 따라 달라짐 (동적 바인딩)
        - 일반 함수 호출: 전역 객체
        - 메서드 호출: 그 메서드를 포함하는 객체
        - 생성자 함수 호출: 새로 생성된 인스턴스 객체
    
    ```jsx
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
    
    → 객체 리터럴 메서드 내부에서의 this는 **circle을 가리킴**
    
    ```jsx
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
    
    → 생성자 함수 내부 this는 생성자가 **생성할 인스턴스를 가리킴**
    
    ```jsx
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
    
- 결론!!!!
    - 자바스크립트 this는 함수 호출 방식에 따라 this 바인딩이 동적으로 결정됨
- strict mode 역시 바인딩에 영향을 줌
    - this는 자기 참조 변수기 때문에 일반적으로 객체의 메서드 또는 생성자 내부에서만 의미 있음
    - 그래서 strict mode에서의 this는 undefined임 → 굳이 일반 함수에서 this를 사용할 필요 없기 때문

### 22.2 함수 호출 방식과 this 바인딩

- 동일한 함수도 다양한 방식으로 호출 가능
    - 일반 함수 호출 - this는 **전역 객체 window를** 가리킴
    - 메서드 호출 - 함수 내부의 this는 **메서드를 호출한 객체**를 가리킴
    - 생성자 함수 호출 - 함수 내부의 this는 **생성자 함수가 생성한 인스턴스**를 가리킴
    - 메서드에 의한 간접 호출 - 함수 내부의 this는 **인수에 의해** 결정됨

### 22.2.1 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩됨
- 위에서도 말했지만 this는 객체의 프로퍼티, 메서드를 참조하기 위한 자기 참조 변수임!!!
    - 일반 함수에서는 객체를 생성 안 하는데 왜 필요하겠음????
- 위에서도 말했지만22 strict mode this는 undefined 바인딩됨
- **!!!!어떤!!!!** 함수라도 **일반 함수**로 호출되면 this에는 전역 객체가 바인딩됨
    - 중첩 함수, 콜백함수 포함 모든 일반함수는 전역 객체가 바인딩
- 근데 이건 문제가 있음
    - 중첩, 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 함
    - 외부 함수의 일부 로직을 대신한다는 말 ㅇㅇ
    - 근데 이 외부 함수인 메서드와 헬퍼 함수의 this가 일치하지 않는다는 것은 헬퍼 함수로 동작하기 어렵게 만듦
- 예제로 이해를 해보자
    - setTimeout 함수에 전달된 콜백함수 this에는 전역 객체가 바인딩됨
    - 그래서 this.value는 obj 객체의 value 프로퍼티가 아닌, window.value를 참조하게 됨
- 그럼 외부 함수랑 헬퍼 함수 this 일치하게 하려면 어떻게 해야됨?
    
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
      }
    };
     
    obj.foo();
    
    var value = 1;
     
    const obj = {
      value: 100,
      foo() {
        // 콜백 함수에 명시적으로 this를 바인딩
        setTimeout(function ()  {
    	    console.log(this.value), 100); // 100
    	  }.bind(this), 100);
    };
     
    obj.foo();
    
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
    
    이 방법 말고도 자바스크립트에서 메서드를 제공한다고 함
    

### 22.2.2 메서드 호출

- 메서드 내부의 this에는 메서드를 호출한 객체가 바인딩 됨
- ⚠️ 주의
    - 메서드 내부의 this는 메서드를 소유한 객체가 아닌, **호출한 객체**에 바인딩됨
    - 메서드는 **프로퍼티에 바인딩된** 함수임
    - 즉, person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 관계가 아니라 아예 **독립적으로 존재하는 객체임**
    - getName 프로퍼티가 함수 객체를 **가리키는 것 뿐임**
- 그래서 프로퍼티가 가리키는 함수 객체는 다른 객체의 메서드가 될 수도 있고 일반 함수로 호출될 수도 있음
    
    ```jsx
    function Person(name) {
      this.name = name;
    }
     
    Person.prototype.getName = function () {
      return this.name;
    };
     
    const me = new Person('Lee');
     
    // getName 메서드를 호출한 객체는 me다.
    console.log(me.getName()); // ① Lee
     
    Person.prototype.name = 'Kim';
     
    // getName 메서드를 호출한 객체는 Person.prototype이다.
    console.log(Person.prototype.getName()); // ② Kim
    ```
    
    - 1번의 경우 getName을 호출한 객체는 me임
        - 그래서 getName 메서드 내부 this는 me를 가리키고, this.name은 ‘Lee’가 됨
    - 2번의 경우 getName을 호출한 객체는 Person.prototype임
        - 그래서 getName 메서드 내부 this는 Person.prototype이고, this.name이 ‘Kim’이 됨
- 객체는 호출 시점에 결정된다!!!

### 22.2.3 생성자 함수 호출

- 생성자 함수 내부 this에는 생성자가 미래에 생성할 인스턴스를 가리킴
- 일반 함수랑 똑같이 생성자를 정의하고, new랑 함께 호출하면 생성자 함수가 되는 거임
    - new 연산자 없이 호출하면 그건 그냥 일반 함수인거임

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call, bind 메서드는 Function.prototype의 메서드임
- 그래서 apply, call, bind 메서드는 모든 함수가 상속받아서 사용 가능함
- apply와 call 메서드 기능은 함수를 호출하는 것
    - 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩함
    - 즉, this로 사용할 객체를 첫 번째 인수에 넣는 것
    
    ```jsx
    function getThisBinding() {
      return this;
    }
     
    // this로 사용할 객체
    const thisArg = { a: 1 };
     
    // apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달
    console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
    // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
    // {a: 1}
     
    // call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
    console.log(getThisBinding.call(thisArg, 1, 2, 3));
    // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
    // {a: 1}
    
    ```
    
- apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달함
- call 메서드는 호출할 함수의 인수를 그냥 쉼표로 구분한 리스트 형식으로 전달함
    - 둘은 방식만 다른거고, 호출한다는 건 동일함
- 이 둘 메서드는 arguments 같은 유사 배열에 배열 메서드를 사용하는 경우임
    - “유사 배열”이기 때문에 배열은 아님
    - 그래서 slice 같은 배열 메서드를 사용할 수 없는데, apply, call 메서드를 이용하면 가능함
- bind 메서드는 메서드의 this와 메서드 내부의 헬퍼 함수의 this가 불일치하는 문제 해결에 쓰임
    
    ```jsx
    const person = {
      name: 'Lee',
      foo(callback) {
        // 1. 
        setTimeout(callback, 100);
      }
    };
     
    person.foo(function () {
      console.log(`Hi! my name is ${this.name}.`); // 2. Hi! my name is .
      // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
      // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
      // Node.js 환경에서 this.name은 undefined다.  
    });
    ```
    
    - person.foo 콜백 함수가 호출되기 전인 1번에서 this는 호출한 person 객체를 가리킴
    - 그런데 person.foo가 일반 함수로서 호출된 2번에서 this는 전역객체 window를 가리킴
    - 이 때 외부 함수와 헬퍼 함수의 this가 상이하게 되어 문맥상 문제가 발생하는 것
    
    ```jsx
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
