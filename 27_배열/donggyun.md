# [27장] 배열

## 27.1 배열이란?
> 여러 개의 값을 순차적으로 나열한 자료구조다.

<br/>

```javascript
const arr = ["apple", "banana", "orange"];
```
- 요소(Element) : 배열이 가지고 있는 값  원시값 / 함수 및 객체 등 다양한값들이 요소가 될 수 있다.

- 인덱스(index) : 배열에서 배열의 요소의 위치 / 대괄호 표기법을 통해 요소에 접근 할 수 있다.

<br>

```javascript
arr[0]; // "apple"
arr[1]; // "banana"
arr[2]; // "orange"
```

<br>

- length 프로퍼티 : 배열의 길이를 나타내는 프로퍼티

```javascript
console.log(arr.length); // 3
```

<br>

- 배열은 인덱스와 length 프로퍼티를 갖기 때문에 for문을 통해 순차적으로 요소에 접근할 수 있다.

```javascript
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

- 배열은 객체 타입이다.

```javascript
typeof arr // -> object
```

<br>

- 배열의 생성자 함수는 Array 이며, 프로토타입은 Array.prototype 이다.
- 배열의 생성 방법
  - Array 생성자 함수
  - Array.of
  - Array.from

```javascript
const arr = [1, 2, 3];

arr.constructor === Array; // true
Object.getPrototypeOf(arr) === Array.prototype; // true
```

<br>

| 구분 | 객체 | 배열 |
| --- | --- | --- |
| 구조 | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조 | 프로퍼티 키 | 인덱스 |
| 값의 순서 | X | O |
| length 프로퍼티 | X | O |

<br><br>

## 27.2 자바스크립트 배열은 배열이 아니다
> 배열의 요소는 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접해있다.<br/>
> 이러한 배열을 밀집 배열(Dense Array)이라고 한다.

<br/>

- `(O(1))` : 인덱스를 통해 단 한번의 연산으로 특정 요소에 접근 가능.<br>
- `(O(n))` : 정렬되지 않은 배열의 경우 처음 위치부터 선형적으로 검색해야 한다.
- 또한, 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 한다.


```javascript
// 선형 검색을 통해 배열(array)에 특정 요소(target)가 존재하는지 확인한다.
// 배열에 특정 요소가 존재하면 특정 요소의 인덱스를 반환하고, 존재하지 않으면 -1을 반환한다.
function linearSearch(array, target) {
  const length = array.length;

  for (let i = 0; i < length; i++) {
    if (array[i] === target) return i;
  }

  return -1;
}

console.log(linearSearch([1, 2, 3, 4, 5, 6], 3)); // 2
console.log(linearSearch([1, 2, 3, 4, 5, 6], 0)); // -1
```

<br/>

- 자바스크립트의 배열은 자료구조에서 말하는 일반적인 의미의 배열과 다르다.
- 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 가지지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다.
- 배열의 요소가 연속적으로 이어져 있지 않은 배열을 `희소 배열`이라 한다.

<br/>

- 이처럼 자바스크립트의 배열은 엄밀히 말해 일반적 의미의 배열이 아니다.
- 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.

- 자바스크립트 배열은 인덱스로 배열 요소에 접근하는 경우에는 일반적인 배열보다 느리다.
- 하지만, 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠르다.

<br/><br/>

## 27.3 length 프로퍼티와 희소 배열
- length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖는다.

```javascript 
[].length        // -> 0
[1, 2, 3].length // -> 3
```

<br/>

- length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.

```javascript 
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 요소 추가
arr.push(4);
// 요소를 추가하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 4

// 요소 삭제
arr.pop();
// 요소를 삭제하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 3
```

<br/>

- length 프로퍼티 값은 배열의 길이를 바탕으로 결정되지만, 임의의 숫자 값을 명시적으로 할당할 수도 있다.
- 현재 length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

```javascript 
const arr = [1, 2, 3, 4, 5];

// 현재 `length` 프로퍼티 값인 5보다 작은 숫자 값 3을 `length` 프로퍼티에 할당
arr.length = 3;

// 배열의 길이가 5에서 3으로 줄어든다.
console.log(arr); // [1, 2, 3]
```

- 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우, 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
- 실제 배열에는 아무런 변함이 없다.
- 값 없이 비어 있는 요소를 위해 메모리 공간을 확보하지 않으며 빈 요소를 생성하지도 않는다.

```javascript 
const arr = [1];

// 현재 length 프로퍼티 값인 1보다 큰 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty × 2]
```

<br/>

- 이처럼 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 한다.
- 희소 배열은 length와 배열 요소 개수가 일치하지 않으며, length가 항상 더 크다.
- 배열을 생성할 경우에는 같은 타입의 요소를 연속적으로 위치 시키는 것이 최선이다.

```javascript 
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

<br/><br/>

## 27.4 배열 생성

### 27.4.1 배열 리터럴

> 가장 일반적이고 간편한 배열 생성 방식

<br/>

- 0개 이상의 요소를 쉼표로 구분하여 대괄호로 묶는다.
- 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재한다.

```javascript
const arr = [1, 2, 3];
console.log(arr.length); // 3
```

<br/>

- 배열 리터럴에 요소를 생략하면 희소 배열이 생성된다.

```javascript
const arr = [1, , 3]; // 희소 배열 

// 희소 배열의 length는 배열의 실제 요소 개수보다 언제나 크다
console.log(arr.length); // 3
console.log(arr); // [1, empty, 3]
console.log(arr[1]); // undefined
```

- `arr[1]` 에는 어떤 값도 할당하지 않았기 때문에 `undefined` 가 된다.
- 사실은 객체인 `arr`에 프로퍼티 키가 `1`인 프로퍼티가 존재하지 않기 때문이다.

<br/><br/>

### 27.4.2 Array 생성자 함수
> Array 생성자 함수를 통해 배열을 생성할 수 있다.

<br/>

- 전달된 인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열을 생성한다.
- 이때 생성된 배열은 희소 배열이다. lenght 프로퍼티 값은 0이 아니지만 실제로 배열의 요소는 존재하지 않는다.

```javascript
const arr = new Array(10);

console.log(arr); // [empty × 10]
console.log(arr.length); // 10
```

<br/>

- 전달된 인수가 없는 경우 빈 배열을 생성한다.
- 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성한다.

```javascript
// 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성한다.
new Array(1, 2, 3); // -> [1, 2, 3]

// 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
new Array({}); // -> [{}]
```

<br/><br/>

### 27.4.3 Array.of
> ES6 에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다.

<br/>

- Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.

```javascript
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
Array.of(1); // -> [1]

Array.of(1, 2, 3); // -> [1, 2, 3]

Array.of('string'); // -> ['string']
```

<br/><br/>

### 27.4.4 Array.from
> 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

<br/>

```javascript
Array.from({ length: 2, 0: "a", 1: "b" }); // ['a', 'b']

Array.from("Hello"); // ['H', 'e', 'l', 'l', 'o']
```

- 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있다.
- 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환한다.

```javascript
// Array.from 에 length 만 존재하는 유사 배열 객체를 전달하면 undefined 를 요소로 채운다.
Array.from({ length: 3 }); // [undefined, undefined, undefined]

// Array.from 은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

<br/><br/>

## 27.5 배열 요소의 참조

> 배열의 요소를 참조할 때에는 대괄호 표기법을 사용한다.

<br/>

- 대괄호 안에는 인덱스가 와야 한다. 정수로 평가되는 표현식이라면 인덱스 대신 사용할 수 있다.
- 인덱스는 값을 참조할 수 있다는 의미에서 프로퍼티 키와 같은 역할을 한다.

```javascript
const arr = [1, 2];

// 인덱스가 2인 요소를 참조. 배열 arr에는 인덱스가 2인 요소가 존재하지 않는다.
console.log(arr[2]); // undefined
```

<br/>

- 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 갖는 객체다.
- 따라서 존재하지 않는 요소에 접근하면 undefined가 반환된다.

<br/><br/>

## 27.6 배열 요소의 추가와 갱신
> 객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있다.<br/>
> 존재하지 않는 인덱스를 통해 값을 할당하면 새로운 요소가 추가된다. length 프로퍼티의 값이 자동 갱신된다.

<br/>

```javascript
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [0, 1]
console.log(arr.length); // 2
```

<br/>

- 이미 요소가 존재하는 인덱스에 값을 재할당하면 요소 값이 갱신된다.

```javascript
// 요소값의 갱신
arr[1] = 10;

console.log(arr); // [0, 10, empty * 98 , 100]
```

- 인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수(또는 정수 형태의 문자열)를 사용해야한다.
- 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다.
- 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.

```javascript
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr["1"] = 2;

// 프로퍼티 추가
arr["foo"] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

<br/><br/>

## 27.7 배열 요소의 삭제
> 배열은 객체이기 때문에 특정 요소를 삭제하기 위해 delete 연산자를 사용할 수 있다.

<br/>

```javascript
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
console.log(arr.length); // 3
```

- delete 연산자는 객체의 프로퍼티를 삭제한다.
- 이때 배열은 희소 배열이 되며 length 프로퍼티 값은 변하지 않는다.
- 따라서 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋다.
- 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용하는 것이 좋다.

```javascript
const arr = [1, 2, 3];

// Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1]부터 1개의 요소를 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티가 자동 갱신된다.
console.log(arr.length); // 2
```

<br/><br/>

## 27.8 배열 메서드
> 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.<br/>

```javascript
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경한다.
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat(3);
console.log(arr);    // [1, 2]
console.log(result); // [1, 2, 3]
```

- 원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있다.
- 따라서 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.

<br/><br/>

### 27.8.1 Array.isArray
> 전달된 인수가 배열이면 true, 아니면 false를 반환한다.<br/>

<br/><br/>

###  27.8.2 Array.prototype.indexOf
> 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.<br/>

- 원본 배열에 인수로 전달한 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스를 반환한다.
- 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1을 반환한다.

```javascript
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환한다.
arr.indexOf(2);    // -> 1
// 배열 arr에 요소 4가 없으므로 -1을 반환한다.
arr.indexOf(4);    // -> -1
// 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // -> 2
```

<br/>

- indexOf 메서드는 배열에 특정 요소가 존재하는지 확인할 때 유용하다.

```javascript
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인한다.
if (foods.indexOf('orange') === -1) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가한다.
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

<br/>

- indexOf 메서드 대신 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋다.

```javascript
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인한다.
if (!foods.includes('orange')) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가한다.
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

<br/><br/>

### 27.8.3 Array.prototype.push
> push 메서드는 인수로 전달 받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.<br/>
> push 메서드는 원본 배열을 직접 변경한다.<br/>

```javascript
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열 arr의 마지막 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.push(3, 4);
console.log(result); // 4

// push 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 2, 3, 4]
```

<br/>

- push 메서드는 성능 면에서 좋지 않다.
- push 메서드를 사용하지 않고 length 프로퍼티를 사용하여 배열의 마지막에 요소를 직접 추가할 수도 있다.
- 해당 방법이 push 메서드보다 빠르다.

<br/>

- push 메서드는 원본 배열을 직접 변경하는 부수 효과가 있다.
- 따라서 push 메서드보다는 ES6의 스프레드 문법을 사용하는 편이 좋다.
- 스프레드 문법을 사용하면 함수 호출 없이 표현식으로 마지막에 요소를 추가할 수 있으며 부수 효과도 없다.

```javascript
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```

<br/><br/>

### 27.8.4 Array.prototype.pop

> 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.<br/>
> 원본 배열이 빈 배열이면 undefined를 반환한다.<br/>
> 원본 배열을 직접 변경한다.<br/>

```javascript
const arr = [1, 2];

// 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.pop();
console.log(result); // 2

// pop 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1]
```

- pop 메서드와 push 메서드를 사용하면 스택을 쉽게 구현할 수 있다.
- 스택에 데이터를 밀어 넣는 것을 push라 하고 스택에서 데이터를 꺼내는 것을 pop이라 한다.

<br/>

```javascript
const Stack = (function () {
  function Stack(array = []) {
    if (!Array.isArray(array)) {
      // "47. 에러 처리" 참고
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  Stack.prototype = {
    // "19.10.1. 생성자 함수에 의한 프로토타입의 교체" 참고
    constructor: Stack,
    // 스택의 가장 마지막에 데이터를 밀어 넣는다.
    push(value) {
      return this.array.push(value);
    },
    // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
    pop() {
      return this.array.pop();
    },
    // 스택의 복사본 배열을 반환한다.
    entries() {
      return [...this.array];
    }
  };

  return Stack;
}());

const stack = new Stack([1, 2]);
console.log(stack.entries()); // [1, 2]

stack.push(3);
console.log(stack.entries()); // [1, 2, 3]

stack.pop();
console.log(stack.entries()); // [1, 2]
```

<br>

- 스택을 클래스로 구현해보면 다음과 같다.

<br/>

```javascript
class Stack {
  #array; // private class member

  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 스택의 가장 마지막에 데이터를 밀어 넣는다.
  push(value) {
    return this.#array.push(value);
  }

  // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
  pop() {
    return this.#array.pop();
  }

  // 스택의 복사본 배열을 반환한다.
  entries() {
    return [...this.#array];
  }
}

const stack = new Stack([1, 2]);
console.log(stack.entries()); // [1, 2]

stack.push(3);
console.log(stack.entries()); // [1, 2, 3]

stack.pop();
console.log(stack.entries()); // [1, 2]
```

<br><br>

### 27.8.5 Array.prototype.unshift
> 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고, 변경된 length 프로퍼티 값을 반환한다.<br>
> 원본 배열을 직접 변경한다.<br>

```javascript
const arr = [1, 2];

let result = arr.unshift(3, 4);
console.log(result); // 4

// pop 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 4, 1, 2]
```

<br>

- 원본 배열을 직접 변경하는 부수효과가 있어, 해당 메서드보다는 ES6의 스프레드 문법을 사용하는것이 좋다. 

```
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]
```

<br><br>

### 27.8.6 Array.prototype.shift
> 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.<br>
> 원본 배열이 빈 배열이면 undefined를 반환한다.<br>
> 원본 배열을 직접 변경한다.<br>

```javascript
const arr = [1, 2];

// 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1

// shift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [2]
```

<br>

- shift 메서드와 push 메서드를 사용하면 큐를 쉽게 구현할 수 있다.

```javascript
const Queue = (function () {
  function Queue(array = []) {
    if (!Array.isArray(array)) {
      // "47. 에러 처리" 참고
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  Queue.prototype = {
    // "19.10.1. 생성자 함수에 의한 프로토타입의 교체" 참고
    constructor: Queue,
    // 큐의 가장 마지막에 데이터를 밀어 넣는다.
    enqueue(value) {
      return this.array.push(value);
    },
    // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼낸다.
    dequeue() {
      return this.array.shift();
    },
    // 큐의 복사본 배열을 반환한다.
    entries() {
      return [...this.array];
    }
  };

  return Queue;
}());

const queue = new Queue([1, 2]);
console.log(queue.entries()); // [1, 2]

queue.enqueue(3);
console.log(queue.entries()); // [1, 2, 3]

queue.dequeue();
console.log(queue.entries()); // [2, 3]
```

<br>

- 큐를 클래스로 구현해 보면 다음과 같다.

<br/>

```javascript
class Queue {
  #array; // private class member

  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 큐의 가장 마지막에 데이터를 밀어 넣는다.
  enqueue(value) {
    return this.#array.push(value);
  }

  // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼낸다.
  dequeue() {
    return this.#array.shift();
  }

  // 큐의 복사본 배열을 반환한다.
  entries() {
    return [...this.#array];
  }
}

const queue = new Queue([1, 2]);
console.log(queue.entries()); // [1, 2]

queue.enqueue(3);
console.log(queue.entries()); // [1, 2, 3]

queue.dequeue();
console.log(queue.entries()); // [2, 3]
```

<br><br>

### 27.8.7 Array.prototype.concat
> 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.<br>
> 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.<br>
> 원본 배열은 변경되지 않는다.<br>

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
// 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

// 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

<br>

- push와 unshift 메서드는 concat 메서드로 대체할 수 있다.
- push와 unshift 메서드는 concat 메서드와 유사하게 동작하지만 미묘한 차이가 있다.

- push와 unshift 메서드는 원본 배열을 직접 변경하지만 concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다.
- push와 unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 하며 concat 메서드를 사용할 경우 반환값을 반드시 변수에 할당받아야 한다.

```javascript
const arr1 = [3, 4];

// unshift 메서드는 원본 배열을 직접 변경한다.
// 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없다.
arr1.unshift(1, 2);
// unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4]

// push 메서드는 원본 배열을 직접 변경한다.
// 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없다.
arr1.push(5, 6);
// push 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4, 5, 6]

// unshift와 push 메서드는 concat 메서드로 대체할 수 있다.
const arr2 = [3, 4];

// concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다.
// arr1.unshift(1, 2)를 다음과 같이 대체할 수 있다.
let result = [1, 2].concat(arr2);
console.log(result); // [1, 2, 3, 4]

// arr1.push(5, 6)를 다음과 같이 대체할 수 있다.
result = result.concat(5, 6);
console.log(result); // [1, 2, 3, 4, 5, 6]
```

<br/>

- 인수로 전달받은 값이 배열인 경우 push와 unshift 메서드는 배열을 그대로 원본 배열의 마지막/첫 번째 요소로 추가한다.
- concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가한다.

```javascript
const arr = [3, 4];

// unshift와 push 메서드는 인수로 전달받은 배열을 그대로 원본 배열의 요소로 추가한다
arr.unshift([1, 2]);
arr.push([5, 6]);
console.log(arr); // [[1, 2], 3, 4,[5, 6]]

// concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 요소로 추가한다
let result = [1, 2].concat([3, 4]);
result = result.concat([5, 6]);

console.log(result); // [1, 2, 3, 4, 5, 6]
```

<br/>

- concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.
- push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.

```javascript
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

// concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.
result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

<br/><br/>

### 27.8.8 Array.prototype.splice
> 원본 배열의 중간에 요소를 추가하거나 제거하는 경우 사용한다.<br/>
> 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.

- `start` : 원본 배열의 요소를 제거하기 시작할 인덱스
- `deleteCount` : start부터 제거할 요소의 개수
- `items` : 제거한 위치에 삽입할 요소의 목록

<br/>

- start가 음수인 경우 배열의 끝에서의 인덱스를 나타낸다.
- start가 `-1`이면 마지막 요소를 가리키고 `-n`이면 마지막에서 n번째 요소를 가리킨다.

<br/>

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

<br/>

- 두 번째 인수인 `deleteCount`에 0을 지정하면 아무런 요소도 제거하지 않고 새로운 요소들을 삽입한다.

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 0개의 요소를 제거하고 그 자리에 새로운 요소 100을 삽입한다.
const result = arr.splice(1, 0, 100);

// 원본 배열이 변경된다.
console.log(arr); // [1, 100, 2, 3, 4]
// 제거한 요소가 배열로 반환된다.
console.log(result); // []
```

<br/>

- 두 번째 인수인 `deleteCount`를 생략하면 첫 번째 인수로 전달된 시작 인덱스부터 모든 요소를 제거한다.

```javascript
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 모든 요소를 제거한다.
const result = arr.splice(1);

// 원본 배열이 변경된다.
console.log(arr); // [1]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3, 4]
```

<br/><br/>

### 27.8.9 Array.prototype.slice
> 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.<br/>
> 원본 배열은 변경되지 않는다.<br/>

- `start` : 복사를 시작할 인덱스 / 음수인 경우 배열의 끝에서의 인덱스
- `end` : 복사를 종료할 인덱스 / 이 인덱스에 해당하는 요소는 복사되지 않는다.

- end는 생략 가능하며 생략 시 기본값은 length 프로퍼티 값이다.

```javascript
const arr = [1, 2, 3];

// arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환한다.
arr.slice(0, 1); // -> [1]

// arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환한다.
arr.slice(1, 2); // -> [2]

// 원본은 변경되지 않는다.
console.log(arr); // [1, 2, 3]
```

<br/>

- 두 번째 인수인 end를 생략하면 첫 번째 인수 start부터 모든 요소를 복사하여 배열로 반환한다.

```javascript
const arr = [1, 2, 3];

// arr[1]부터 이후의 모든 요소를 복사하여 반환한다.
arr.slice(1); // -> [2, 3]
```

<br/>

- 첫 번째 인수인 start가 음수인 경우 배열의 끝에서부터 요소를 복사하여 배열로 반환한다.

```javascript
const arr = [1, 2, 3];

// 배열의 끝에서부터 요소를 한 개 복사하여 반환한다.
arr.slice(-1); // -> [3]

// 배열의 끝에서부터 요소를 두 개 복사하여 반환한다.
arr.slice(-2); // -> [2, 3]
```

<br/>

- 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.

```javascript
const arr = [1, 2, 3];

// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
```

<br/>

- 이때 생성된 복사본은 얕은 복사를 통해 생성된다.

```javascript
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// 얕은 복사(shallow copy)
const _todos = todos.slice();
// const _todos = [...todos];

// _todos와 todos는 참조값이 다른 별개의 객체다.
console.log(_todos === todos); // false

// 배열 요소의 참조값이 같다. 즉, 얕은 복사되었다.
console.log(_todos[0] === todos[0]); // true
```

<br/><br/>

### 27.8.10 Array.prototype.join
> 원본 배열의 요소를 문자열로 변환한 후, 구분자로 연결한 문자열을 반환한다.<br/>
> 구분자는 생략 가능하며 기본 구분자는 `,`이다.<br/>

```javascript
const arr = [1, 2, 3, 4];

// 기본 구분자는 ','이다.
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환한다.
arr.join(); // -> '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈문자열로 연결한 문자열을 반환한다.
arr.join(''); // -> '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환한다.ㄴ
arr.join(':'); // -> '1:2:3:4'
```

<br/><br/>

### 27.8.11 Array.prototype.reverse
> 원본 배열의 순서를 반대로 뒤집으며 원본 배열이 변경된다.<br/>
> 반환 값은 변경된 배열이다.<br/>

```javascript
const arr = [1, 2, 3];
const result = arr.reverse();

// reverse 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]
```

<br/><br/>

### 27.8.12 Array.prototype.fill
> ES6에서 도입되었으며, 인수로 전달받은 값을 배열의 처음부터 끝까지 채운다.<br/>
> 원본 배열이 변경된다.<br/> 

```javascript
const arr = [1, 2, 3];

// 인수로 전달 받은 값 0을 배열의 처음부터 끝까지 요소로 채운다.
arr.fill(0);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [0, 0, 0]
```

<br/>

- 두 번째 인수로는 fill 을 적용할 시작 인덱스를 정할 수 있다.

```javascript
const arr = [1, 2, 3];

// 인수로 전달받은 값 0을 배열의 인덱스 1부터 끝까지 요소로 채운다.
arr.fill(0, 1);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0]
```

<br/>

- 세 번째 인수로는 fill 을 적용할 마지막 인덱스를 정할 수 있다.

```javascript
const arr = [1, 2, 3, 4, 5];

// 인수로 전달받은 값 0을 배열의 인덱스 1부터 3 이전(인덱스 3 미포함)까지 요소로 채운다.
arr.fill(0, 1, 3);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0, 4, 5]
```

- fill 메서드는 한 가지 값으로만 채울 수 있는 것이 단점이다.
- 하지만, Array.from 을 사용하면 콜백 함수를 인수로 전달 할 수 있어 각각 다른 값으로 채울 수 있음.

```javascript
// 인수로 전달받은 정수만큼 요소를 생성하고 0부터 1씩 증가하면서 요소를 채운다.
const sequences = (length = 0) => Array.from({ length }, (_, i) => i);
// const sequences = (length = 0) => Array.from(new Array(length), (_, i) => i);

console.log(sequences(3)); // [0, 1, 2]
```

<br/><br/>

### 27.8.13 Array.prototype.includes
> 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false 반환한다.<br/>
> 첫 번째 인수로 검색할 대상을 지정한다.<br/>

```javascript
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인한다.
arr.includes(2); // -> true

// 배열에 요소 100이 포함되어 있는지 확인한다.
arr.includes(100); // -> false
```

<br/>

- 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- 두 번째 인수를 생략할 경우 기본값 0이 설정된다.
- 만약 두 번째 인수에 음수를 전달하면 length 프로퍼티 값과 음수 인덱스를 합산하여 검색 시작 인덱스를 설정한다.

```javascript
const arr = [1, 2, 3];
+
// 배열에 요소 1이 포함되어 있는지 인덱스 1부터 확인한다.
arr.includes(1, 1); // -> false

// 배열에 요소 3이 포함되어 있는지 인덱스 2(arr.length - 1)부터 확인한다.
arr.includes(3, -1); // -> true
```

<br/>

- indexOf 메서드를 사용하면 반환값이 `-1`인지 확인해 보아야하고 배열에 `NaN`이 포함되어 있는지 확인할 수 없다는 문제가 있다.

```javascript
[NaN].indexOf(NaN) !== -1; // -> false
[NaN].includes(NaN);       // -> true
```

### 27.8.14 Array.prototype.flat
> 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.<br/>

```javascript
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5];
```

<br/>

- 중첩 배열을 평탄화할 깊이를 인수로 전달할 수 있다.
- 인수를 생략할 경우 기본값은 1이다.
- 인수로 `Infinity`를 전달하면 중첩 배열 모두를 평탄화한다.

<br/>

```javascript
// 중첩 배열을 평탄화하기 위한 깊이 값의 기본값은 1이다.
[1, [2, [3, [4]]]].flat();  // -> [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(1); // -> [1, 2, [3, [4]]]

// 중첩 배열을 평탄화하기 위한 깊이 값을 2로 지정하여 2단계 깊이까지 평탄화한다.
[1, [2, [3, [4]]]].flat(2); // -> [1, 2, 3, [4]]
// 2번 평탄화한 것과 동일하다.
[1, [2, [3, [4]]]].flat().flat(); // -> [1, 2, 3, [4]]

// 중첩 배열을 평탄화하기 위한 깊이 값을 Infinity로 지정하여 중첩 배열 모두를 평탄화한다.
[1, [2, [3, [4]]]].flat(Infinity); // -> [1, 2, 3, 4]
```

<br/><br/>

## 27.9 배열 고차 함수
> 고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.

- 고차 함수는 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다.
- 함수형 프로그래밍은 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다.

<br><br>

### 22.9.1 Array.prototype.sort
> 배열의 요소를 정렬한다. 원본배열을 직접 변경하며 정렬된 배열을 반환한다.
> 기본적으로 오름차순으로 요소를 정렬한다. 

```javascript
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']
```

<br/>

- 한글 문자열인 요소도 오름차순으로 정렬된다.

```javascript
const fruits = ['바나나', '오렌지', '사과'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['바나나', '사과', '오렌지']
```

<br/>

- 내림차순으로 요소를 정렬하려면 sort 메서드를 사용하여 오름차순 정렬한 후, reverse 메서드를 사용하여 요소의 순서를 뒤집는다.

```javascript
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']

// 내림차순(descending) 정렬
fruits.reverse();

// reverse 메서드도 원본 배열을 직접 변경한다.
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

<br/>

- 숫자 요소로 이루어진 배열을 정렬할 때는 주의가 필요하다.

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort();

// 숫자 요소들로 이루어진 배열은 의도한 대로 정렬되지 않는다.
console.log(points); // [1, 10, 100, 2, 25, 40, 5]
```

<br/>

- sort 메서드의 기본 정렬 순서는 유닠코드 코드 포인트의 순서를 따른다.
- 배열의 요소가 숫자 타입이라 할 지라도 배열의 요소를 일시적으로 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬한다.

<br/>

- 따라서 숫자 요소를 정렬할 때는 sort 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.
- 비교 함수의 반환값이 0보다 작으면 비교 함수의 첫 번째 인수를 우선하여 정렬하고, 0이면 정렬하지 않으며, 0보다 크면 두 번째 인수를 우선하여 정렬한다.

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열에서 최소/최대값 취득
console.log(points[0], points[points.length]); // 1

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 크면 b를 우선하여 정렬한다.
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

// 숫자 배열에서 최대값 취득
console.log(points[0]); // 100
```

<br/>

- 객체를 요소로 갖는 배열을 정렬하는 예제는 다음과 같다.

```javascript
const todos = [
  { id: 4, content: 'JavaScript' },
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' }
];

// 비교 함수. 매개변수 key는 프로퍼티 키다.
function compare(key) {
  // 프로퍼티 값이 문자열인 경우 - 산술 연산으로 비교하면 NaN이 나오므로 비교 연산을 사용한다.
  // 비교 함수는 양수/음수/0을 반환하면 되므로 - 산술 연산 대신 비교 연산을 사용할 수 있다.
  return (a, b) => (a[key] > b[key] ? 1 : (a[key] < b[key] ? -1 : 0));
}

// id를 기준으로 오름차순 정렬
todos.sort(compare('id'));
console.log(todos);
/*
[
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
  { id: 4, content: 'JavaScript' }
]
*/

// content를 기준으로 오름차순 정렬
todos.sort(compare('content'));
console.log(todos);
/*
[
  { id: 2, content: 'CSS' },
  { id: 1, content: 'HTML' },
  { id: 4, content: 'JavaScript' }
]
*/
```

<br><br>

### 27.9.2 Array.prototype.forEach
> for 문을 대체할 수 잇는 고차 함수이다.<br/>
> 반복문을 추상화한 고차함수로서 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복적으로 호출한다.<br/>

```javascript
const numbers = [1, 2, 3];
let pows = [];

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
numbers.forEach(item => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

<br/>

- forEach 메서드의 콜백 함수는 forEach 메서드를 호출한 배열의 요소값과 인덱스, forEach 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.

```
// forEach 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].forEach((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

<br/>

- forEach 메서드는 원본 배열을 변경하지 않는다.
- 하지만 콜백 함수를 통해 원본 배열을 변경할 수는 있다.

```javascript
const numbers = [1, 2, 3];

// forEach 메서드는 원본 배열을 변경하지 않지만 콜백 함수를 통해 원본 배열을 변경할 수는 있다.
// 콜백 함수의 세 번째 매개변수 arr은 원본 배열 numbers를 가리킨다.
// 따라서 콜백 함수의 세 번째 매개변수 arr을 직접 변경하면 원본 배열 numbers가 변경된다.
numbers.forEach((item, index, arr) => { arr[index] = item ** 2; });
console.log(numbers); // [1, 4, 9]
```

<br/>

- forEach 메서드의 반환값은 언제나 undefined이다.

```javascript
const result = [1, 2, 3].forEach(console.log);
console.log(result); // undefined
```

<br/>

- forEach 메서드의 두 번째 인수로 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있다.

```javascript
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      this.numberArray.push(item * item);
    }, this); // forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

<br/>

- 화살표는 함수 자체의 this 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프, 즉 multiply 메서드 내부의 this를 그대로 참조한다.

```javascript
[1, 2, 3].forEach(item => {
  console.log(item);
  if (item > 1) break; // SyntaxError: Illegal break statement
});

[1, 2, 3].forEach(item => {
  console.log(item);
  if (item > 1) continue;
  // SyntaxError: Illegal continue statement: no surrounding iteration statement
});
```

<br>

- forEach 메서드는 for 문에 비해 성능이 좋지는 않지만 가독성은 더 좋다.
- 따라서 요소가 대단히 많은 배열을 순회하거나 시간이 많이 걸리는 복잡한 코드 또는 높은 성능이 필요한 경우가 아니라면 for문 대신 forEach 메서드를 사용할 것을 권장한다.

<br><br>

### 27.9.3 Array.prototype.map
> 자신을 호출한 배열을 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.<br/>
> 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.<br/>
> 이때 원본 배열은 변경되지 않는다.<br/>

```javascript
const numbers = [1, 4, 9];

// map 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
const roots = numbers.map(item => Math.sqrt(item));

// 위 코드는 다음과 같다.
// const roots = numbers.map(Math.sqrt);

// map 메서드는 새로운 배열을 반환한다
console.log(roots);   // [ 1, 2, 3 ]
// map 메서드는 원본 배열을 변경하지 않는다
console.log(numbers); // [ 1, 4, 9 ]
```

<br/>

- forEach 메서드와 map 메서드의 공통점은 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출하는 것이다.
- 하지만 forEach 메서드는 언제나 undefined를 반환하고, map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하는 차이가 있다.

<br/>

- forEach 메서드는 단순히 반복문을 대체하기 위한 고차 함수이고, map 메서드는 요소값을 다른 값으로 매핑한 새로운 배열을 생성하기 위한 고차 함수다.
- map 메서드가 생성하여 반환하는 새로운 배열의 length 프로퍼티 값은 map 메서드를 호출한 배열의 length 프로퍼티 값과 반드시 일치한다.
- 즉, map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열을 1:1 매핑한다.

<br/>

- map 메서드의 콜백 함수는 map 메서드를 호출한 배열의 요소값과 인덱스, map 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.

``` javascript
// map 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].map((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
  return item;
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

<br/><br/>

### 27.9.4 Array.prototype.filter
> 자신을 호출한 배열을 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.<br/>
> 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.<br/>
> 이때 원본 배열은 변경되지 않는다.<br/>

```javascript
const numbers = [1, 2, 3, 4, 5];

// filter 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 그리고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.
// 다음의 경우 numbers 배열에서 홀수인 요소만을 필터링한다(1은 true로 평가된다).
const odds = numbers.filter(item => item % 2);
console.log(odds); // [1, 3, 5]
```

<br/>

- forEach 메서드는 언제나 undefined를 반환한다.
- map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
- filter 메서드는 콜백 함수의 반환값이 true인 요소만 추출한 새로운 배열을 반환한다.

<br/>

- filter 메서드는 자신을 호출한 배열에서 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들고 싶을 때 사용한다.
- filter 메서드가 생성하여 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작다.

<br/>

- filter 메서드의 콜백 함수는 filter 메서드를 호출한 배열의 요소값과 인덱스, filter 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.

```javascript
// filter 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].filter((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
  return item % 2;
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

<br/>

- filter 메서드는 자신을 호출한 배열에서 특정 요소를 제거하기 위해 사용할 수도 있다.

```javascript
class Users {
  constructor() {
    this.users = [
      { id: 1, name: 'Lee' },
      { id: 2, name: 'Kim' }
    ];
  }

  // 요소 추출
  findById(id) {
    // id가 일치하는 사용자만 반환한다.
    return this.users.filter(user => user.id === id);
  }

  // 요소 제거
  remove(id) {
    // id가 일치하지 않는 사용자를 제거한다.
    this.users = this.users.filter(user => user.id !== id);
  }
}

const users = new Users();

let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Lee' }]

// id가 1인 사용자를 제거한다.
users.remove(1);

user = users.findById(1);
console.log(user); // []
```

<br/><br/>

### 27.9.5 Array.prototype.reduce
> 자신을 호출한 배열을 순회하면서 인수로 전달받은 콜백 함수를 호출한다.<br>
> 그리고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다.<br/>
> 이때 원본 배열은 변경되지 않는다.<br/>

- reduce 메서드는 첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값을 전달받는다.
- reduce 메서드의 콜백 함수에는 4개의 인수, 초기값 또는 콜백 함수의 이전 반환값, reduce 메서드를 호출한 배열의 요소값과 인덱스, reduce 메서드를 호출한 배열 자체, 즉 this가 전달된다.

```javascript
// [1, 2, 3, 4]의 모든 요소의 누적을 구한다.
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);

console.log(sum); // 10
```

<br/>

![image](https://github.com/user-attachments/assets/1d0b46bf-63e5-49a2-88e8-72401306a8be)

<br/>

- reduce 메서드는 초기값과 배열의 첫 번째 요소값을 콜백 함수에게 인수로 전달하면서 호출하고 다음 순회에는 콜백 함수의 반환값과 두 번째 요소값을 콜백 함수의 인수로 전달하면서 호출한다.
- 이러한 과정을 반복하여 reduce 메서드는 하나의 결과값을 반환한다.

<br/>

#### 🐽 평균 구하기

```javascript
const values = [1, 2, 3, 4, 5, 6];

const average = values.reduce((acc, cur, i, { length }) => {
  // 마지막 순회가 아니면 누적값을 반환하고 마지막 순회면 누적값으로 평균을 구해 반환한다.
  return i === length - 1 ? (acc + cur) / length : acc + cur;
}, 0);

console.log(average); // 3.5
```

<br/>

#### 🐽 최대값 구하기

```javascript
const values = [1, 2, 3, 4, 5];

const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);
console.log(max); // 5
```

<br/>

#### 🐽 요소의 중복 횟수 구하기

```javascript
const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];

const count = fruits.reduce((acc, cur) => {
// 첫 번째 순회 시 acc는 초기값인 {}이고 cur은 첫 번째 요소인 'banana'다.
// 초기값으로 전달받은 빈 객체에 요소값인 cur을 프로퍼티 키로, 요소의 개수를 프로퍼티 값으로
// 할당한다. 만약 프로퍼티 값이 undefined(처음 등장하는 요소)이면 프로퍼티 값을 1로 초기화한다.
acc[cur] = (acc[cur] || 0) + 1;
return acc;
}, {});

// 콜백 함수는 총 5번 호출되고 다음과 같이 결과값을 반환한다.
/_
{banana: 1} => {banana: 1, apple: 1} => {banana: 1, apple: 1, orange: 1}
=> {banana: 1, apple: 1, orange: 2} => {banana: 1, apple: 2, orange: 2}
_/

console.log(count); // { banana: 1, apple: 2, orange: 2 }
```

<br/>

#### 🐽 중첩 배열 평탄화

```javascript
const values = [1, [2, 3], 4, [5, 6]];

const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
// [1] => [1, 2, 3] => [1, 2, 3, 4] => [1, 2, 3, 4, 5, 6]

console.log(flatten); // [1, 2, 3, 4, 5, 6]
[1, [2, 3, 4, 5]].flat(); // -> [1, 2, 3, 4, 5]

// 인수 2는 중첩 배열을 평탄화하기 위한 깊이 값이다.
[1, [2, 3, [4, 5]]].flat(2); // -> [1, 2, 3, 4, 5]
```

<br/>

#### 🐽 중복 요소 제거

```javascript
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = values.reduce(
  (unique, val, i, _values) =>
    // 현재 순회 중인 요소의 인덱스 i가 val의 인덱스와 같다면 val은 처음 순회하는 요소다.
    // 현재 순회 중인 요소의 인덱스 i가 val의 인덱스와 다르다면 val은 중복된 요소다.
    // 처음 순회하는 요소만 초기값 []가 전달된 unique 배열에 담아 반환하면 중복된 요소는 제거된다.
    _values.indexOf(val) === i ? [...unique, val] : unique,
  []
);

console.log(result); // [1, 2, 3, 5, 4]
```

<br/>

- 중복 요소를 제거할 때는 reduce 메서드보다 filter 메서드를 사용하는 방법이 더 직관적이다.

```javascript
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

// 현재 순회 중인 요소의 인덱스 i가 val의 인덱스와 같다면 val은 처음 순회하는 요소다. 이 요소만 필터링한다.
const result = values.filter((val, i, _values) => _values.indexOf(val) === i);
console.log(result); // [1, 2, 3, 5, 4]
```

<br/>

- 또는 중복되지 않는 유일한 값들의 집합인 Set을 사용할 수도 있다.

```javascript
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

// 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.
const result = [...new Set(values)];
console.log(result); // [1, 2, 3, 5, 4]
```

<br/>

- reduce 메서드의 두 번째 인수로 전달하는 초기값은 생략할 수 있다.
- 하지만 reduce 메서드를 호출할 때는 언제나 초기값을 전달하는 것이 안전하다.

```javascript
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 }
];

// 1번째 순회 시 acc는 { id: 1, price: 100 }, cur은 { id: 2, price: 200 }이고
// 2번째 순회 시 acc는 300, cur은 { id: 3, price: 300 }이다.
// 2번째 순회 시 acc에 함수에 객체가 아닌 숫자값이 전달된다. 이때 acc.price는 undefined다.
const priceSum = products.reduce((acc, cur) => acc.price + cur.price);

console.log(priceSum); // NaN
```

<br/><br/>

### 27.9.6 Array.prototype.some
> 자신을 호출한 배열을 순회하면서 인수로 전달된 콜백 함수를 호출한다.<br/>
> 콜백 함수 반환값이 단 한 번이라도 참이면 true, 거짓이면 false를 반환한다.<br/>
> 즉, 배열의 요소 중에 콜백 함수를 통해 정의한 조건을 만족하는 요소가 1개 이상 존재하는지 확인하여 그 결과를 불리언 타입으로 반환한다.<br/>
> 단, some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환하므로 주의해야 한다.<br/>

```javascript
// 배열의 요소 중에 10보다 큰 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item > 10); // -> true

// 배열의 요소 중에 0보다 작은 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item < 0); // -> false

// 배열의 요소 중에 'banana'가 1개 이상 존재하는지 확인
["apple", "banana", "mango"].some((item) => item === "banana"); // -> true

// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.
[].some((item) => item > 3); // -> false
```

<br/><br/>

### 27.9.7 Array.prototype.every
> 자신을 호출한 배열을 순회하면서 인수로 전달된 콜백 함수를 호출한다.<br>
> 콜백 함수 반환값이 모두 참이면 true, 거짓이면 false를 반환한다.
> 즉, 배열의 요소 중에 콜백 함수를 통해 정의한 조건을 모두 만족하는지 확인하여 그 결과를 불리언 타입으로 반환한다.<br/>
> 단, every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환하므로 주의해야 한다.<br/>

```javascript
// 배열의 모든 요소가 3보다 큰지 확인
[5, 10, 15].every((item) => item > 3); // -> true

// 배열의 모든 요소가 10보다 큰지 확인
[5, 10, 15].every((item) => item > 10); // -> false

// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다.
[].every((item) => item > 3); // -> true
```

<br/><br/>

### 27.9.8 Array.prototype.find
> 자신을 호출한 배열을 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다.<br/>
> 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined를 반환한다.<br/>

```javascript
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 2, name: "Choi" },
  { id: 3, name: "Park" },
];

// id가 2인 첫 번째 요소를 반환한다. find 메서드는 배열이 아니라 요소를 반환한다.
users.find((user) => user.id === 2); // -> {id: 2, name: 'Kim'}
// Array#filter는 배열을 반환한다.
[1, 2, 2, 3].filter((item) => item === 2); // -> [2, 2]

// Array#find는 요소를 반환한다.
[1, 2, 2, 3].find((item) => item === 2); // -> 2
```

<br/><br/>

### 27.9.9 Array.prototype.findIndex
> 자신을 호출한 배열을 순회하면서 인수로 전달된 콜백 함수를 호출하면서 반환값이 true인 첫 번째 요소의 인덱스를 반환한다.<br/>
> 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 `-1`을 반환한다.<br/>

```javascript
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 2, name: "Choi" },
  { id: 3, name: "Park" },
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex((user) => user.id === 2); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex((user) => user.name === "Park"); // -> 3

// 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우
// 다음과 같이 콜백 함수를 추상화할 수 있다.
function predicate(key, value) {
  // key와 value를 기억하는 클로저를 반환
  return (item) => item[key] === value;
}

// id가 2인 요소의 인덱스를 구한다.
users.findIndex(predicate("id", 2)); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(predicate("name", "Park")); // -> 3
```

<br/><br/>

### 27.9.10 Array.prototype.flatMap
> map 메서드를 통해 생성된 새로운 배열을 평탄화한다.<br/>
> 즉, map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.<br/>

```javascript
const arr = ['hello', 'world'];

// map과 flat을 순차적으로 실행
arr.map(x => x.split('')).flat();
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

// flatMap은 map을 통해 생성된 새로운 배열을 평탄화한다.
arr.flatMap(x => x.split(''));
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```

<br/>

- 단 flatMap 메서드는 flat 메서드처럼 인수를 전달하여 평탄화 깊이를 지정할 수는 없고 1단게만 평탄화한다.
- map 메서드를 통해 생성된 중첨 배열의 평탄화 깊이를 지정해야 하면 flatMap 메서드를 사용하지 말고 map 메서드와 flat 메서드를 각각 호출한다.

```javascript
const arr = ['hello', 'world'];

// flatMap은 1단계만 평탄화한다.
arr.flatMap((str, index) => [index, [str, str.length]]);
// -> [[0, ['hello', 5]], [1, ['world', 5]]] => [0, ['hello', 5], 1, ['world', 5]]

// 평탄화 깊이를 지정해야 하면 flatMap 메서드를 사용하지 말고 map 메서드와 flat 메서드를 각각 호출한다.
arr.map((str, index) => [index, [str, str.length]]).flat(2);
// -> [[0, ['hello', 5]], [1, ['world', 5]]] => [0, 'hello', 5, 1, 'world', 5]
```
