# ES6(es2015) 문법
ES6 문법이 들어오고나서부터 우리가 이전에 알고있는 자바스크립트의 개념이 많이 달라졌습니다.  
또한 클래스, 함수, 변수의 선언 / 모듈화에 따른 파일 분리 등 많이 많은 패러다임이 바뀌고 더욱 편리해졌습니다.

<br>

## 목차
* [Classes](#classes)
* [let & const](#let--const)
* [Arrow functions](#arrow-functions)
* [Modules](#modules)
* [Promises](#promises)
* [Template literals](#template-literals)
* [Default Parameters](#default-parameters)
* [Destructuring / Destructuring Assignment](#destructuring-/-destructuring-assignment)
* [Object Literals](#object-literals)
* [Spread Operator](#spread-operator)
* [Fetch API](https://github.com/dwhuh/study/tree/master/JavaScript/FetchAPI)

<br>

## Classes
자바를 접해보았다면 알 수 있습니다. ```OOP``` 개념의 프로그래밍을 자바스크립트에서 할 수 있게되었습니다.  
```constructor``` 메소드를 사용할 수 있게 되었으며, ```extends```를 통해서 클래스를 상속 받을 수도 있습니다.
```
class Parent {
  toString(str) {
    return str.toString();
  }
}

class Child extends Parent {
  constructor() {
    super();
    this.number = 10;
    this.str = super.toString(this.number);
  }
}
```

<br>

## let & const
#### let
기존 자바스크립트에서 변수를 선언하기 위해서는 ```var```를 키워드를 이용하여 변수를 선언하였습니다.  
```var``` 키워드를 사용한 변수는 데이터를 초기화하지 않으면 메모리 누수 문제점이 있을 수 있으며 즉,
```if/else```, ```for``` 구문 등을 사용하여 코드 블럭을 만들어도 변수가 가지고 있는 메모리가 해제되지 않는 문제점이 있었습니다.  
이런 코드 블럭으로 영역을 한정 지을 수 있도록 해주는 키워드인 ```let```이 추가되었습니다.
```
// var 키워드를 사용한 for 문
for (var i = 0; i < 10; i++) {
  ...
}
console.log(i);
> 10

// let 키워드를 사용한 for 문
for (let j = 0; i < 10; j++) {
  ...
}
console.log(j);
> Error: j is not defined
```

#### const
const는 사전적 의미로는 상수 즉, 변경할 수 없는 값 또는 재할당이 필요 없는 경우에 사용한다.
Array 또는 Object의 경우는 데이터의 타입이 변하지 않는 이상 즉,  
재할당이 일어나지 않는 이상 Array 또는 Object를 추가/수정/삭제가 가능합니다.
```
const a = 'hi';
a = 'hello';
> Error: Uncaught TypeError: Assignment to constant variable.

const arr = [];
const obj = {};
arr.push('a');
obj.test = 'a';
console.log(arr);
console.log(obj);
> ['a']
> { test: 'a' }
```

<br>

## Arrow functions
화살표 함수는 더욱 간결해졌습니다.  
```function``` 키워드 없이 함수를 만들 수 있으며, 한 줄로 표현되는 함수의 경우  
```return```을 사용하지 않고 값을 자동 반환시킬 수 있습니다.  
편리할 수도 있고, 
```
const sumOne = (a, b) => a + b;
const sumTwo = a => a.toString();
const sumFur = () => {
  const a = 1;
  const b = sumTwo(a);
  return a + b;
};
sumFur();
> '11'
```

<br>

## Modules
```export```, ```import``` 키워드를 사용하여 다른 파일에서 선언된 함수나 변수를 재사용 가능할 수 있습니다.  
```export```는 외부에서 사용할 수 있도록 내보내는 기능을 합니다.  
```import```는 ```export```된 데이터를 불러오는 기능을 합니다.
#### utils.js
```
export const toString = a => a.toString();
export const toNumber = a => Number(a);
```

#### math.js
```
import { toString, toNumber } from './utils';

console.log(toString(12));
console.log(toNumber('123'));
> '12'
> 123
```

<br>

## Promises
비동기 프로세싱을 다루는 방법인데요.  
비동기 요청에 대한 성공/실패 처리를 단순하게 작성할 수 있습니다.  
프로미스는 아래 세 가지 상태를 가지게 됩니다.
* ```pending``` (대기중)
* ```fulfilled``` (이행됨)
* ```rejected``` (거부됨)
```
const waitPromise = (url) => new Promise((resolve, reject) => {
  fetch(url)
    .then(response => resolve(response))
    .catch(error => reject(error));
});
waitPromise('url은 테스트')
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

<br>

## Template literals
역 따옴표(backTicks)를 사용하여 문자열을 연결, 변수 삽입을 단순하게 사용할 수 있습니다.  
변수를 선언할 때 ```${}```와 같이 사용되어야 합니다.
```
const varA = 'John';
const varB = 18;

console.log(`안녕 나의 이름은 ${varA}이라고해 나이는 ${varB}살이야.`);
> 안녕 나의 이름은 John이라고해 나이는 18살이야.
```

<br>

## Default Parameters
함수를 정의할 때 파라미터의 기본 값을 설정할 수 있게 되었습니다.  
```
const testFunction = (a = 'ABC', b = 'CDE', c = 'EFG') => {
  console.log(a);
  console.log(b);
  console.log(c);
};

testFunction();
> 'ABC'
> 'CDE'
> 'EFG'

testFunction('Hi', 'Huh !!');
> 'Hi'
> 'Huh !!'
> 'EFG'
```

<br>

## Destructuring / Destructuring Assignment
Destructuring(구조 분해)을 사용해 객체 안에 있는 필드 값을 원하는 변수에 대입할 수 있습니다.
```
const points = [10, 21, 32];
const [a, b, c] = points;
const [d, , f] = points;

console.log(x, y, z);
console.log(d, f);
> 10, 21, 32
> 10, 32
```
```
const people = {
  name: 'John',
  age: 18,
  hobby: 'foot ball',
};
const { name, age, hobby } = people;
console.log(name, age, hobby);
> 'John', 18, 'foot ball'
```

<br>

## Object Literals

<br>

## Spread Operator
