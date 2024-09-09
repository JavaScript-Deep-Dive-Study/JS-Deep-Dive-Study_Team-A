### **배열이란?**

```jsx
const arr = [1, 2, 3];
console.log(typeof arr); // object
```

배열은 객체로 평가된다. 그렇지만 객체와 구별되는 특징이 존재한다.

|      구분       |           객체            |     배열      |
| :-------------: | :-----------------------: | :-----------: |
|      구조       | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
|    값의 참조    |        프로퍼티 키        |    인덱스     |
|    값의 순서    |             ✕             |       ○       |
| length 프로퍼티 |             ✕             |       ○       |

<br>

### **자바스크립트 배열은 배열이 아니다**

자바스크립트의 배열은 요소가 메모리 공간에 연속적으로 이어져있지 않은 **희소 배열** 이다.  
자바스크립트 배열은 인덱스 문자열을 키로 가지며 length 프로퍼티를 갖는다.  
해시테이블로 구현되었기 때문에, 인덱스로 접근하면 일반 배열에 비해 속도가 느리지만 삽입과 삭제는 빠르다.

<br>

### **length 프로퍼티와 희소 배열**

length 프로퍼티의 값은 0과 2<sup>32</sup>-1 미만의 양의 정수다.

- push, pop 할 경우 자동으로 갱신된다.
- 명시적으로 할당이 가능하다.

```jsx
const arr = [1, 2, 3, 4, 5];

arr.length = 3;
console.log(arr); // [1, 2, 3]

const arr2 = [1];
arr2.length = 3;
console.log(arr); // [1, empty * 2]
```

`arr2`의 1, 2번째에는 값이 존재하지 않고, 메모리 공간을 확보하지도, 빈 요소를 생성하지도 않는다. 이처럼 일부가 비어있는 배열을 희소 배열이라 한다.  
희소 배열은 일반적인 배열의 개념과 다르기 때문에, 사용을 지양하자.

모던 자바스크립트 엔진은 같은 타입을 갖는 배열의 경우 연속적인 메모리 공간을 할당한다고 한다.

<br>

### **배열 생성**

#### 배열 리터럴

```jsx
const arr = [1, 2, 3];

const sparse = [1, , 3]; // [1, empty, 3]
console.log(sparse[1]); // undefined
```

`sparse` 객체의 프로퍼티에 키가 `'1'`인 프로퍼티가 존재하지 않으므로 `undefined`가 출력된다.

<br/>

#### Array 생성자 함수

Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작한다.

- 전달된 인수가 숫자 1개인 경우 length가 인수인 배열 생성

```jsx
const arr = new Array(10);
console.log(arr, arr.length); // [empty * 10], 10

// 인수의 범위가 배열 크기를 넘어가면 에러 발생
new Array(4294967296); // RangeError: Invalid array length
new Array(-1); // RangeError: Invalid array length
```

- 전달된 인수가 없으면 빈 배열 생성, 배열 리터럴 []와 같다.

- 전달된 요소가 2개 이상이거나 숫자가 아니면 요소로 갖는 배열 생성

```jsx
new Array(1, 2, 3); // [1, 2, 3]
new Array({}); // [{}]

// new 없이 호출해도 new.target을 확인하여 반환한다.
Array({}); // [{}]
```

<br/>

#### 3 Array.of

생성자 함수와 다르게 인수가 숫자 1개여도 요소로 갖는 배열을 생성

```jsx
Array.of(1); // [1]
```

<br/>

#### 4 Array.from

ES6에서 도입, 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 반환

```jsx
// 유사 배열 객체를 받아 배열로 반환
Array.from({ length: 2, 0: 1, 1: 2 }); // [1, 2]

// 이터러블을 변환하여 배열 반환, 문자열: 이터러블
Array.from("Array"); // ['A', 'r', 'r', 'a', 'y']

// 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열 리턴
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

<br>

### **배열 요소의 참조**

대괄호 표기법으로 참조한다.

```jsx
const arr = [1, 2, , 4];

console.log(arr[1]); // 2
console.log(arr[2]); // undefined
```

<br>

### **배열 요소의 추가와 갱신**

객체처럼 배열에도 동적으로 요소를 추가할 수 있다.

```jsx
const arr = [1];

arr[1] = 2;
console.log(arr); // [1, 2]

// length 프로퍼티보다 큰 인덱스로 추가하면 희소 배열이 된다.
arr[3] = 4;
console.log(arr); // [1, 2, empty, 4]

// 이미 존재하는 인덱스에 추가하면 갱신된다.
arr[1] = 100;
console.log(arr); // [1, 100, empty, 4]

// 정수 이외의 값을 추가하면 프로퍼티가 생성, length에는 영향을 주지 않는다.
const arr = [];
arr["hey"] = "hey";
arr.hej = "hej";
arr[0.1] = 0.1;
arr[-1] = -1;
console.log(arr); // [hey: 'hey', hej: 'hej', '0.1': 0.1, '-1': -1]
```

<br>

### **배열 요소의 삭제**

`delete` 연산자를 사용해 삭제한다.

```jsx
const arr = [1, 2, 3];
delete arr[1];
console.log(arr, arr.length); // [1, empty, 3], 3
```

이는 희소 배열을 생성하므로, 대신 Array.prototype.splice 메서드를 사용하자

```jsx
const arr = [1, 2, 3, 4, 5];
arr.splice(1, 3);
console.log(arr, arr.length); // [1, 5], 2
```

<br>

### **배열 메서드**

배열 메서드는 원본 배열을 직접 변경하는 메서드와 원본 배열을 변경하지 않고 새로운 배열을 반환하는 메서드가 있다.

가급적 원본 배열을 변경하지 않는 메서드를 사용하자.

<br/>

#### Array.isArray

전달된 인수가 배열이면 `true`, 아니면 `false`를 반환한다.

유사 배열 또한 `false`를 반환한다.

<br/>

#### Array.prototype.indexOf

인수로 전달된 요소를 검색하여 첫 번째 인덱스를 반환, 존재하지 않으면 -1을 반환

<br/>

#### Array.prototype.push

인수로 받은 값을 배열에 모두 추가한다.

원본 배열을 직접 변경하면서 성능 면에서 좋지 않으므로, 추후 배울 spread 문법을 활용하자.

```jsx
const arr = [1, 2];

const newArr = [...arr, 3, 4]; // [1, 2, 3, 4]
```

<br/>

#### Array.prototype.pop

마지막 요소를 제거, 반환하며, 빈 배열일 경우 `undefined`를 반환한다.

원본 배열을 직접 변경한다.

위의 `push`와 함께 Stack 자료구조를 구현할 수 있다.

<br/>

#### Array.prototype.unshift

push처럼 원본 배열에 인수를 추가, 차이점은 앞에 추가한다.

```jsx
const arr = [1, 2];
arr.unshift(3, 4); // [3, 4, 1, 2]
```

스프레드 문법을 사용하자.

<br/>

#### Array.prototype.shift

맨 앞 요소를 제거, 반환하는 pop 메서드

원본 배열을 직접 변경한다.

위의 `push`와 함께 Queue 자료구조를 구현할 수 있다.

<br/>

#### Array.prototype.concat

인수로 전달된 값들(배열이면 해체)을 합쳐서 새로운 배열로 반환한다.

```jsx
const arr1 = [1, 2];
const arr2 = [3, 4];

let res = arr1.concat(arr2); // [1, 2, 3, 4]
```

이 또한 spread 문법으로 대체가 가능하므로 일관성 있게 spread 문법을 사용하자.

<br/>

#### Array.prototype.splice

원본 배열의 중간에 요소를 추가하거나 제거하는 경우 splice 메서드를 사용한다.

splice 메서드는 3개의 매개변수가 있으며, 원본 배열을 변경한다.

```jsx
const arr = [1, 2, 3, 4];

// 인덱스 1부터 2개의 요소 제거, 그 자리에 20, 30, 40 삽입
const res = arr.splice(1, 2, 20, 30, 40);

console.log(res); // [2, 3]
console.log(arr); // [1, 20, 30, 40, 4]

//-----------------------------------------------

const arr = [1, 2, 3, 4];

// 인덱스 1부터 0개의 요소 제거, 그 자리에 새로운 요소 삽입
const res = arr.splice(1, 0, 100, 200); // [1, 100, 200, 3, 4]
```

splice 메서드의 세 번째 인수를 전달하지 않으면 제거하기만 한다.

splice 메서드의 두 번째 인수를 생략하면 첫 인수부터 모두 제거한다.

<br>

<br/>

#### Array.prototype.slice

인수로 전달된 범위의 요소들을 복사하여 배열로 반환, 원본 배열을 변경하지 않는다.

```jsx
const arr = [1, 2, 3, 4];

arr.slice(0, 1); // [1]
arr.slice(1, 3); // [2, 3]

// 음수인 경우 끝에서부터 요소를 복사한다.
arr.slice(-1); // [4]
arr.slice(-2); // [3, 4]

// 인수를 생략하면 복사본을 생성해 반환한다. (얕은 복사)
const copy = arr.slice(); // [1, 2, 3, 4]
```

> 💡 얕은 복사와 깊은 복사  
> 얕은 복사는 객체의 경우 한 단계 까지만 복사한다.
> slice, spread, Object.assign 메서드는 모두 얕은 복사를 수행한다.
> 객체에 중첩되어 있는 객체까지 모두 복사하는 깊은 복사를 하고 싶다면 Lodash 라이브러리의 cloneDeep 메서드를 사용하자.

<br/>

#### Array.prototype.join

원본 배열의 모든 요소를 문자열로 변환, 인수를 구분자로 연결한 문자열을 반환한다.

```jsx
const arr = [1, 2, 3, 4];

// 기본 구분자는 콤마(,)
arr.join(); // '1,2,3,4'
arr.join(""); // '1234'
arr.join(":"); // '1:2:3:4'
```

<br/>

#### Array.prototype.reverse

원본 배열의 순서를 반대로 뒤집는다. 원본 배열이 변경된다.

<br/>

#### Array.prototype.fill

인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. 원본 배열이 변경된다.

```jsx
const arr = [1, 2, 3];
arr.fill(0); // [0, 0, 0]

// 두 번째 인수로 시작 인덱스 전달
arr.fill(0, 1); // [1, 0, 0]

// 세 번쨰 인수로 멈출 인덱스 전달
const arr = [1, 2, 3, 4, 5];
arr.fill(0, 1, 3); // [1, 0, 0, 4, 5]
```

<br/>

#### Array.prototype.includes

배열 내에 특정 요소가 포함되어 있는지 확인하여 `true` or `false`를 반환

첫 인수로 검색 대상을 지정한다.

```jsx
const arr = [1, 2, 3];

arr.includes(2); // true
arr.includes(4); // false

// 두 번째 인수로 검색을 시작할 인덱스를 설정
arr.includes(1, 1); // false
arr.includes(3, -1); // true (arr.length - 1 = 2)
```

<br/>

#### Array.prototype.flat

인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```jsx
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]

// 평탄화할 깊이를 인수로 전달할 수 있다.
[1, [2, [3, [4]]]].flat(); // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(2); // [1, 2, 3, [4]]
[1, [2, [3, [4]]]].flat().flat(); // 위와 같다.
[1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4]
```

<br>

### **배열 고차 함수**

고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.

- **불변성을 지향**하는 함수형 프로그래밍 기반
- **조건문과 반복문을 제거**하여 복잡성 해결
- **변수의 사용을 억제**하여 상태 변경을 피함

함수형 프로그래밍은 **순수 함수를 통해 부수 효과를 최대한 억제**하여 안정성을 높인다.

<br/>

#### Array.prototype.sort

배열의 요소를 정렬, 원본 배열을 직접 변경한다. 숫자를 이용한 배열을 정렬할 때 주의가 필요하다.

```jsx
const arr = [40, 100, 1, 5, 2, 25, 10];
arr.sort(); // [1, 10, 100, 2, 25, 40, 5]
```

`sort` 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따른다.  
배열의 요소를 일시적으로 문자열로 변환한 후 정렬하므로, `[2, 10]`을 정렬하면 `[10, 2]`가 된다.  
따라서 숫자 요소를 정렬할 경우 인수로 **정렬 순서를 정의하는 비교 함수**를 전달해야 한다.  
비교 함수는 양수, 음수, 0을 반환해야 한다.

```jsx
const arr = [40, 100, 1, 5, 2, 25, 10];

// 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬
arr.sort((a, b) => a - b); // [1, 2, 5, 10, 25, 40, 100]

// 반환값이 0보다 작으면 b를 우선하여 정렬
arr.sort((a, b) => b - a); // [100, 40, 25, 10, 5, 2, 1]
```

<br/>

#### Array.prototype.forEach

for문을 대체하는 고차 함수로 자신을 호출한 배열을 순회하며, 수행할 처리를 콜백 함수로 전달받아 반복 호출한다. 원본 배열을 변경하지는 않으나, 콜백 함수를 통해 변경할 수는 있다.

```jsx
const nums = [1, 2, 3];
const pows = [];

nums.forEach((x) => pows.push(x ** 2)); // [1, 4, 9]
```

콜백 함수를 호출할 때 3개의 인수(호출한 배열의 요소값, 인덱스, this)를 순차적으로 전달한다.

```jsx
[1, 2, 3].forEach((item, index, arr) => {
  console.log(
    `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`
  );
}); // 요소값: 1, 인덱스: 0, this: [1,2,3] ...
```

forEach 메서드의 두 번째 인수로 this로 사용할 객체를 전달할 수 있다.  
더 나은 ES6의 화살표 함수를 사용하자.

```jsx
class Numbers {
  nums = [];

  multiply(arr) {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    arr.forEach((x) => this.nums.push(x * x));
  }
}
```

희소 배열의 경우 존재하지 않는 요소는 순회 대상에서 제외된다.

<br/>

#### Array.prototype.map

호출한 배열의 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 **콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.** 원본 배열은 변경되지 않는다.

```jsx
// forEach와 같이 3개(요소값, 인덱스, this)의 인수 전달
[1, 2, 3].map((item, index, arr) => {
  console.log(
    `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`
  );
  return item;
});
```

`undefined`를 반환하는 `forEach`와 다르게 새로운 배열을 반환한다.
**반환한 새 배열의 length는 호출한 배열의 length와 반드시 일치한다(1:1 매핑)**
forEach와 같이 화살표 함수로 this를 처리할 수 있다.

```jsx
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map((item) => this.prefix + item);
  }
}
```

<br/>

#### Array.prototype.filter

forEach, map과 같이 콜백 함수를 반복 호출, 콜백 함수의 반환값이 `true`인 요소로 구성된 새로운 배열을 반환한다.
위의 함수들과 같이 3개의 인자 (item, index, this)를 전달할 수 있다.

```jsx
const nums = [1, 2, 3, 4, 5];

const odds = nums.filter((item) => item % 2); // [1, 3, 5]
```

filter 메서드로 특정 요소를 제거할 때, 중복되어 있다면 중복된 요소가 모두 제거된다.
하나만 제거하려면 `indexOf` 메서드로 인덱스를 찾고, `splice` 메서드를 사용한다.

<br/>

#### Array.prototype.reduce

콜백 함수를 반복 호출, 콜백 함수의 반환값을 다음 순회의 콜백 함수의 첫 인자로 전달하여 하나의 결과값을 반환한다.
원본 배열은 변경되지 않는다.

```jsx
const sum = [1, 2, 3, 4].reduce((acc, cur, idx, arr) => acc + cur, 0); // 10
```

위의 코드를 해석해보면 다음과 같다.

|  구분  |    acc     | cur | idx |     arr      | 반환값 |
| :----: | :--------: | :-: | :-: | :----------: | :----: |
| 순회 1 | 0 (초기값) |  1  |  0  | [1, 2, 3, 4] |   1    |
| 순회 2 |     1      |  2  |  1  | [1, 2, 3, 4] |   3    |
| 순회 3 |     3      |  3  |  2  | [1, 2, 3, 4] |   6    |
| 순회 4 |     6      |  4  |  3  | [1, 2, 3, 4] |   10   |

reduce의 활용법을 알아보자

```jsx
// 평균 구하기
const vals = [1, 2, 3, 4, 5, 6];

const avg = vals.reduce((acc, cur, i, { length }) => {
  // 마지막 순회가 아니면 누적값 반환, 마지막 순회면 평균 반환
  return i === length - 1 ? (acc + cur) / length : acc + cur;
}, 0); // 3.5

// 최대값 구하기
// Math.max를 쓰자
const max = vals.reduce((acc, cur) => (acc > cur ? acc : cur), 0); // 6

// 중복 횟수 구하기
const alphas = ["a", "b", "c", "c", "a"];
const count = alphas.reduce((acc, cur) => {
  acc[cur] = (acc[cur] || 0) + 1;
  return acc;
}, {}); // { a: 2, b: 1, c: 2 }

// 중첩 배열 평탄화
// flat을 쓰자
const vals = [1, [2, 3], 4, [5, 6]];
const flats = vals.reduce((acc, cur) => acc.concat(cur), []); // [1, 2, 3, 4, 5, 6]

// 중복 요소 제거
// filter를 쓰자
const vals = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
const res = vals.reduce(
  (unique, val, i, _values) =>
    _values.indexOf(val) === i ? [...unique, val] : unique,
  []
); // [1, 2, 3, 5, 4]

const res = vals.filter((val, i, _values) => _values.indexOf(val) === i);
```

reduce 메서드의 두 번째 인수로 전달하는 초기값은 생략이 가능하다.  
하지만 언제나 초기값을 전달하는 것이 안전하므로 초기값을 명시적으로 작성하자.

<br/>

#### Array.prototype.some

배열을 순회하며 콜백 함수를 호출하고, 반환값이 한 번이라도 참이면 `true`, 모두 거짓이면 `false` 반환하고, 빈 배열은 언제나 `false`를 반환한다.

```jsx
[5, 10, 15].some((x) => x > 10); // true
[5, 10, 15].some((x) => x < 0); // false
```

<br/>

#### Array.prototype.every

배열을 순회하며 콜백 함수를 호출하고, 반환값이 모두 참이면 `true`, 한 번이라도 거짓이면 `false` 반환하고,
빈 배열은 언제나 `true`를 반환한다.

```jsx
[5, 10, 15].every((x) => x > 3); // true
[5, 10, 15].every((x) => x > 10); // false
```

<br/>

#### Array.prototype.find

배열을 순회하며 콜백 함수를 호출하고, 반환값이 `true`인 첫 번째 요소를 반환, 없다면 `undefined`를 반환한다.

```jsx
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 3, name: "Seo" },
  { id: 4, name: "Park" },
];

user.find((user) => user.id === 2); // {id: 2, name: 'Kim'}
```

<br/>

#### Array.prototype.findIndex

배열을 순회하며 콜백 함수를 호출하고, 반환값이 `true`인 첫 번째 요소의 인덱스를 반환, 없다면 -1을 반환한다.

<br/>

#### Array.prototype.flatMap

map 메서드를 통해 생성된 새로운 배열을 평탄화한다. (map -> flat 실행)

```jsx
const arr = ["hey", "hej"];

arr.flatMap((x) => x.split("")); // ['h', 'e', 'y', 'h', 'e', 'j']
```

단, flatMap은 1단계만 평탄화 가능하므로, 평탄화 깊이를 지정해야하면 map과 flat을 각각 호출한다.
