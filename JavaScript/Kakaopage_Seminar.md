# JavaScript ES6+ 세미나
책 **'리액트를 다루는 기술'** 에서 내용을 인용하여 작성하였습니다.

## 목차
* JavaScript 란?
* ECMA 란?
* 새롭게 추가된 변수 선언 키워드와 기존 변수 선언 키워드 차이점
* 객체와 배열의 사용성 개선
* 함수의 사용성 개선
* 비동기 프로그래밍을 위한 객체: 프로미스
* 향상된 비동기 프로그래밍을: async & await
* 템플릿 리터럴을 이용한 동적인 문자열 생성하기
* 함수를 중지/재개할 수 있는 제너레이터 함수

<br>

***

<br>

## JavaScript 란?
HTML과 CSS로 그려진 웹페이지를 동적으로 변경할 수 있게 만들어줄 수 있는 언어  
경고창을 띄워주고, 탭 인터페이스, Drag & Drop 등 다양한 동적인 기능 제작

<br>

***

<br>

## ECMA 란?
우리가 알고있는 자바스크립트는 ```ECMA``` + ```BOM``` + ```DOM``` 으로 이루어져
<br><br>

ECMA는 자바스크립트의 표준 규격
<br><br>

```BOM``` 은 Browser Object Model의 약자이며, 브라우저의 기능 요소를 제어/관리  
브라우저의 버튼, url 주소창, 윈도우 크기 등을 제어
<br><br>

```DOM``` 은 Document Object Model의 약자이며, HTML, XML 문서를 파싱한 데이터를 가지는 인터페이스

<br>

***

<br>

## 새롭게 추가된 변수 선언 키워드와 기존 변수 선언 키워드 차이점
ES6 이전에는 ```var``` 키워드를 이용하여 변수를 선언.  
<br>
> ```var``` 키워드를 이용한 변수 사용에 대한 문제점
1. 호이스팅으로 인한 참조에러가 발생하지 않음
2. 블록 스코프가 아닌 함수 스코프
3. ```var``` 키워드를 선언하지 않는다면 전역변수로 할당
4. 블록을 벗어나도 함수를 벗어나지 않는 이상 변수가 생존
5. 재정의가 가능하여 직관적이지 않으며 쉽게 버그가 발생하기 쉬움

<pre>
console.log(foo); // undefined
var foo = 1;
</pre>

<pre>
var foo = 1;
var foo = 2;
console.log(foo); // 2
</pre>

<pre>
function foo() {
    bar = 1;
}
foo();
console.log(bar); // 1
</pre>

<br>

> 이러한 문제점을 해결할 수 있게 나온 것이 ES6 문법에서 지원되는 ```const```, ```let``` 키워드
1. 호이스팅이 발생하지만 일시적 사각지대에 의해 호이스팅이 발생하지 않는 것 처럼 보임
2. 함수 스코프가 아닌, 블록 스코프
3. 재선언이 불가능

<pre>
console.log(foo); // 참조 에러
let foo = 1;
</pre>

<pre>
let foo = 1;
let foo = 2; // 에러 발생
console.log(foo);
</pre>

<pre>
let foo = 1;
foo = 2;
console.log(foo); // 2;
</pre>

<pre>
const foo = 1;
foo = 2; // 에러 발생
console.log(foo);
</pre>

> ```const``` 키워드의 경우 재할당이 불가능하지만 객체/배열의 경우 원소를 추가/수정/삭제가 가능
<pre>
const foo = {};
foo.bar = 1; // 문제 없음
foo.foo = 2; // 문제 없음
console.log(foo);
</pre>

<br>

***

<br>

## 객체와 배열의 사용성 개선
해당 섹션에서는 아래와 같은 항목을 알아볼 것
- ```단축 속성명(Shorthand property names)```
- ```계산된 속성명(Computed property names)```
- ```전개 연산자(Spread Operator)```  
- ```구조 분해 할당(Destructuring assignment) = 비구조화```  

> ```전개 연산자``` / ```구조 분해 할당``` 으로 객체의 속성을 꺼내는 방법이 개선

<br>

> 단축 속성명 (Shorthand property names)  

간편하게 새로운 객체를 만들 목적으로 나온 문법.  
```{ key: value }``` 관계에서 ```key``` 선언부 이름과 ```value``` 변수명이 일치할 경우 성립  
```function``` 키워드 없이 ```() {}``` 문법을 이용하여 간편하게 함수를 선언

<pre>
const name = 'root';
const age = 23;
const obj = {
    name, // { name: name }
    age, // { age: age }
    getName() {
        return this.name; // 'root'
    },
    getAge() {
        return this.age; // 23
    },
};
</pre>

> 계산된 속성명 (Computed property names)

객체의 속성을 동적으로 결정하기 위해 나온 문법  
```{ key: value } ``` 관계에서 ```key``` 의 선언부의 명칭을 변수의 값으로 할당하기에 어려움  

<pre>
onClick = index => {
    const key = `state_${index}`;
    this.setState({ [key]: this.state[key] + 1 });
}
</pre>

> 전개 연산자 (Spread Operator)

객체나 배열의 하나 이상의 원소를 풀어서 사용할 수 있는 문법  
이 문법은 새로운 변수에 할당하면 새로운 객체나 배열을 만듬  
대신, 해당 문법은 ```Deep Copy```가 아닌 ```Shallow Copy```로 데이터를 복사하기 때문에,  
내부 데이터가 객체나 배열로 이루어져 있는 경우에는 값이 참조됨

<pre>
const arr1 = [1, 2, 3];
const obj1 = { age: 23, name: 'root', data: {} };

const arr2 = [...arr1];
const obj2 = { ...obj1 };

obj1.data.foo = 1;

console.log(arr1 === arr2); // false
console.log(obj1 === obj2); // false

console.log(obj1.data.foo); // 1
console.log(obj2.data.foo); // 1

console.log(obj1.data === obj2.data); // true
</pre>

<pre>
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

const obj3 = { ...obj1, ...obj2 };
console.log(obj3); // { a: 1, b: 2, c: 3, d: 4 }
</pre>

> 전개 연산자는 순서에 따라서도 값의 할당이 달라짐
<pre>
const obj1 = { a: 1, b: 2 };
const obj2 = { a: -1, c: 3, d: 4 };

const obj3 = { ...obj1, ...obj2 };
const obj4 = { ...obj2, ...obj1 };

const obj5 = { ...obj1, ...obj2, a: 'a', d: 'd' };

console.log(obj3); // { a: -1, b: 2, c: 3, d: 4 }
console.log(obj4); // { a: 1, b: 2, c: 3, d: 4 }
console.log(obj5); // { a: 'a', b: 3, c: 3, d: 'd' }
</pre>

> 구조 분해 할당 (Destructuring assignment)

객체나 배열의 원소를 변수로 쉽게 할당할 수 있는 문법  
<pre>
const arr1 = [1, 2];
const [a, b] = arr1;
console.log(a, b); // 1, 2
</pre>

<pre>
let a, b;
[a, b] = [1, 2];
console.log(a, b); // 1, 2

[a, b] = [b, a];
console.log(a, b); // 2, 1
</pre>

<pre>
const arr1 = [1, 2, 3, 4, 5];
const [a, b, , , e] = arr1;
const [a1, , ...b1] = arr1; // rest parameter

console.log(a, b, e); // 1, 2, 5
console.log(a1, b1); // 1, [3, 4, 5]
</pre>

<pre>
const obj1 = { age: 23, name: 'root' };
const { age, name } = obj1;
console.log(age, name); // 23, 'root'
</pre>

> 별칭 사용하기
<pre>
const obj1 = { age: 23, name: 'root' };
const { age: theAge, name } = obj1;
console.log(theAge, name); // 23, 'root'
</pre>

> 기본 값 할당하기
> ```undefined``` 는 할당이 되지 않은 것을 의미하며 값이 ```undefined``` 가 아닌 모든건 기본 값을 할당이 불가능
<pre>
const obj1 = { age: undefined, name: null };
const { age = 23, name = 'root' } = obj1;
console.log(age, name); // 23, null
</pre>

<br>

***

<br>

## 함수의 사용성 개선

<br>

***

<br>

## 비동기 프로그래밍을 위한 객체: 프로미스

<br>

***

<br>

## 향상된 비동기 프로그래밍을: async & await

<br>

***

<br>

## 템플릿 리터럴을 이용한 동적인 문자열 생성하기

<br>

***

<br>

## 함수를 중지/재개할 수 있는 제너레이터 함수
