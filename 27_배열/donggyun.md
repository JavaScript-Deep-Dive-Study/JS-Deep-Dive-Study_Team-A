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
