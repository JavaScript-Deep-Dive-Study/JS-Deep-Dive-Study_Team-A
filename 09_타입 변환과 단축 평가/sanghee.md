# 타입 변환과 단축 평가

## 타입 변환이란?

자바스크립트의 모든 값은 타입이 있고, 개발자의 의도에 따라 다른 타입으로 변환 가능하다.

개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환(=타입 캐스팅)**,

원시 값은 변경 불가능한 값이므로, 타입 변환은 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다. 즉, 기존 변수 값을 재할당하여 변경하는 것이 아니다.

<br/>

## 암묵적 타입 변환

개발자의 의도와 상관없이 **문맥을 고려해** 표현식을 평가하는 도중에 js 엔진에 의해 암묵적으로 타입이 자동 변환 되는 것을 **암묵적 타입 변환**(=**타입 강제 변환**)이라 한다.

### 문자열 타입으로 변환

문자열 연결 연산자의 피연산자 중에서 **문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입변환** 한다.

```jsx
// 숫자 타입
0 + ''         // -> "0"
-0 + ''        // -> "0"
1 + ''         // -> "1"
-1 + ''        // -> "-1"
NaN + ''       // -> "NaN"
Infinity + ''  // -> "Infinity"
-Infinity + '' // -> "-Infinity"

// 불리언 타입
true + ''  // -> "true"
false + '' // -> "false"

// null 타입
null + '' // -> "null"

// undefined 타입
undefined + '' // -> "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```

### 숫자 타입으로 변환

**산술연산자, 비교 연산자, + 단항연산자**는 피연산자 중 숫자 타입이 아닌 피연산자를 숫자 타입으로 변환한다.

**피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN**이다.

```jsx
1 - "1"; // -> 0
1 * "10"; // -> 10
1 / "one"; // -> NaN
```

```jsx
"1" > 0; // -> true
```

```jsx
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> TypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

**빈 문자열(””), 빈 배열([]), null, false는 0으로, true는 1로 변환된다. 객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다.**

### 불리언 타입으로 변환

if 문이나 for문과 같은 제어문은 불리언 값(참/거짓)으로 평가되어야 하는 표현식이므로 암묵적으로 불리언 타입으로 변환된다.

- false로 평가되는 Falsy 값: false, undefined, null, 0, -0, NaN, “”
- true로 평가되는Truthy 값: Falsy 값 외의 모든 값

<br/>

## 명시적 타입 변환

### 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toString 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법

### 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. 단항 산술 연산자를 이용하는 방법
4. 산술 연산자를 이용하는 방법

### 불리언 타입으로 변환

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. ! 부정 논리 연산자를 두 번 사용하는 방법

<br/>

## 단축 평가

### 논리 연산자를 사용한 단축 평가

논리합(||), 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 한쪽으로 평가된다.

```jsx
"Cat" && "Dog"; // -> "Dog"
```

논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. 두 번째 피연산자 까지 평가해 보아야 표현식을 평가할 수 있고 따라서 두 번째 피연산자가 평가 결과를 결정한다.

```jsx
"Cat" || "Dog"; // -> "Cat"
```

논리합(||) 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true로 반환한다. 따라서 첫 번째 피연산자를 반환한다.

이처럼 논리 연산의 결과는 **결정하는 피연산자를 타입 변환하지 않고 그대로 반환**한다. 이를 **단축 평가**라 한다. 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

| 단축 평가 표현식    | 평가 결과 |
| ------------------- | --------- |
| true \|\| anything  | true      |
| false \|\| anything | anything  |
| true && anything    | anything  |
| false && anything   | false     |

<br/>

단축 평가를 사용하면 if문을 대체할 수 있다. 어떤 조건이 Truthy 값일 때 무언가를 해야한다면 논리곱(&&)으로 대체할 수 있다.

```jsx
var done = true;
var message = "";

if (done) message = "완료";

message = done && "완료";
console.log(message); // 완료
```

조건이 Falsy값일 때 무언가를 해야 한다면 논리합(||) 연산자 표현식으로 대체할 수 있다.

```jsx
var done = false;
var message = "";

if (!done) message = "미완료";

message = done || "미완료";
console.log(message); // 미완료
```

<br/>

### **단축 평가 사용이 유용한 경우**

1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때객체가 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생한다.

```jsx
var elem = null;
var value = elem.value; // TypeError

//단축 평가 사용하면 에러 발생 하지 않음
var elem = null;
var value = elem && elem.value; // null
```

2. 함수 매개변수에 기본값을 설정할 때 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이 때 단축 평가를 사용해 에러를 방지할 수 있다.

```jsx
function getStringLength(str) {
  str = str || "";
  return str.length;
}
```

<br/>

### 옵셔널 체이닝 연산자(?.)

옵셔널 체이닝 연산자 `?.`는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.

```jsx
var elem = null;
var value = elem?.value;
console.log(value); // undefined
```

<br/>

### null 병합 연산자(??)

null 병합 연산자 `??`는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

변수의 기본값을 설정할 때 유용하다.

```jsx
var foo = null ?? "default string";
console.log(foo); // "default string"
```
