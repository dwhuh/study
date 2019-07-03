# JavaScript ES6+ 세미나
책 **'리액트를 다루는 기술'** 에서 내용을 인용하여 작성하였습니다.

## 목차
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
1. 호이스팅이 발생하지만 일시적 사각지대에 의해 호이스팅이 발생하지 않는 것 처럼 보이게 됨.
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

> ```const``` 키워드의 경우 재할당이 불가능하지만 객체나배열의 경우 원소를 추가 수정 삭제가 가능

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
해당 섹션에서는 아래와 같은 항목을 알아볼 것 입니다.
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
```{ key: value } ``` 관계에서 ```key``` 의 선언부의 명칭을 변수의 값으로 할당하기에 어려움이 있습니다.  

<pre>
onClick = index => {
    const key = `state_${index}`;
    this.setState({ [key]: this.state[key] + 1 });
}
</pre>

> 전개 연산자 (Spread Operator)

객체나 배열의 하나 이상의 원소를 풀어서 사용할 수 있는 문법  
이 문법은 새로운 변수에 할당하면 새로운 객체나 배열을 만듬.  
대신, 해당 문법은 ```Deep Copy```가 아닌 ```Shallow Copy```로 데이터를 복사하기 때문에,  
내부 데이터가 객체나 배열로 이루어져 있는 경우에는 값이 참조됩니다.

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
const [a1, , ...b1] = arr1;

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
> ```undefined``` 는 할당이 되지 않은 것을 의미하며, 값이 ```undefined``` 가 아닌 모든건 기본 값을 할당할 수 없음

<pre>
const obj1 = { age: undefined, name: null };
const { age = 23, name = 'root' } = obj1;
console.log(age, name); // 23, null
</pre>

> 객체와 배열이 중첩된 경우의 구조 분해 할당

<pre>
const obj = {
    age: 23,
    name: 'root',
    mother: {
        name: 'kim',
    },
};
const {
    name,
    mother: { name: 'my mother' },
} = obj;

console.log(name); // 'root'
console.log(motherName); // 'my mother'
console.log(mother); // 참조 에러
</pre>

<pre>
const [{ a: x } = { a: 123 }] = [];
const [{ a: y } = { a: 123 }] = [{}];

console.log(x); // 123
console.log(y); // undefined
</pre>

> 별칭을 이용해 다른 객체와 베열의 원소 할당

<pre>
const obj = {};
const arr = [];
const temp = { a: 123, b: true };

({ a: obj.prop, b: arr[0] } = temp); // page. 67 ①
console.log(obj); // { prop: 123 }
console.log(arr); // [true]
</pre>

<br>

***

<br>

## 함수의 사용성 개선
ES6 에서는 함수의 기능이 강력해졌습니다.  
매개변수에 기본 값을 할당 할 수 있게 되었으며, 화살표 함수(Arrow function)가 추가되어  
코드가 간결해졌으며, this 바인딩에 대한 고민을 덜 수 있게 되었습니다.  

<br>

일반 함수의 경우 동적으로 ```this```, ```arguments```가 바인딩되지만,  
화살표 함수는 정적으로 바인딩 됩니다.  
또한 ```this```, ```arguments```는 상위 스코프를 가리키게 됩니다.  
사용에 따라 ```arguments``` 가 필요로 하게 되는 상황이 오는데,  
이때에는 ```나머지 매개변수(rest parameter)```를 사용하면 됩니다.

> 매개변수의 기본 값 할당

<pre>
function func1(a = 1, b = {}, d = [], e = [0, 1, 2]) {
    ...
}
</pre>

> 화살표 함수

<pre>
const func1 = () => { ... };

// 매개 변수가 하나인 경우
const func2 = a => { ... };

const func3 = (a, b) => { ... };

const func4 = (a = 1, b = []) => { ... };

// 중괄호를 사용하지 않을 경우에는 return 키워드를 생략된 것과 같이 값을 반환하는 함수로 만들 수 있습니다.
const func5 = (a, b) => a + b;

// 소괄호를 이용하여 코드가 길면 한 줄 내려 가독성을 높일 수 있습니다.
const func6 = (a, b) => (
    (a * b) + (a + b)
);
</pre>

> 나머지 매개변수 (Rest parameter)

<pre>
const func = (a, ...b) => console.log(a, b);

func(1, 2, 3, 4);
// 1, [2, 3, 4]

func(1, [2, 3, 4, 5, 6], 7, 8, 9);
// 1, [2, 3, 4, 5, 6], 7, 8, 9
</pre>

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
```템플릿 리터럴(Template literals)```은 변수를 이용해 동적으로 문자열을 생성할 수 있는 문법입니다.  
기존에는 ```더하기(+)```, ```따옴표(')``` 기호를 반복적으로 조합하여 하나의 문자열을 생성하였습니다.  
이런 코드는 가독성이 떨어지며, 작성하는데에 시간도 오래 걸립니다.  
아래의 코드는 ```템플릿 리터럴```을 이용한 동적으로 문자열을 생성한 문법입니다.

> ```백틱(`)```을 이용하여 템플릿 리터럴을 사용할 수 있다.  
> 변수의 사용은 ```${변수명}``` 와 같이 사용할 수 있다.

<pre>
const age = 23;
const name = 'root';
const introduce = `Name: ${name}, Age: ${age}`;
console.log(introduce); // 'Name: root, Age: 23'
</pre>

> 여러줄을 사용하는 경우

<pre>
const age = 23;
const name = 'root';
const introduce = `Name: ${name}
Age: ${age}
`;
console.log(introduce);
=>
Name: root
Age: 23
</pre>

> 태그된 템플릿 리터럴 (Tagged Template Literal)

템플릿 리터럴의 확장 기능입니다.  
함수로써 정의되며, 사용할 때에는 함수명과 템플릿 리터럴을 붙여서 사용할 수 있습니다.  

<pre>
const v1 = 10;
const v2 = 20;

function tag(a, ...b) {
    console.log(a); // 분할된 문자열의 배열 (사용된 변수보다 항상 길이가 하나 더 많다)
    console.log(b); // 사용된 변수들
    return 1;
}

// 표현식(v1, v2)을 기준으로 문자열을 분할한다.
tag`a-${v1}-b-${v2}-c`;
=>
['a-', '-b-', '-c']
[10, 20]

// 왼쪽 또는 오른쪽이 표현식이면 빈 문자열이 배열에 담아진다.
tag`${v1}-${v2}`;
=>
['', '-', '']
[10, 20]
</pre>

<br>

***

<br>

## 함수를 중지/재개할 수 있는 제너레이터 함수
