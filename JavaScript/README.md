
# ECMA

## 배열의 중복되는 값을 제거하는 방법
### 1. Set 객체를 이용
```
const removeDuplicatedArray = (collection = []) => ([
  ...new Set(collection)
]);
```

### 2. filter() 와 indexOf() 를 이용
```
const removeDuplicatedArray = (collection = []) => ([
  collection.filter((item, index) => collection.indexOf(item) === index)
]);
```

<br>

## Rest 파라미터
정해지지 않은 파라미터를 배열로 나타낼 수 있게 해줍니다.
```
const sum = (...nums) => (
  nums.reduce((previousNum, currentNum) => (
    previousNum + currentNum
  ))
);

sum(1, 2, 3);           // 6
sum(1, 2, 3, 4, 5, 6);  // 21
```
### Rest 파라미터 해체 ex. 1)
```
const sum = (...[a, b, c]) => a + b + c;

sum(1);           // NaN (b, c는 undefined 이다)
sum(1, 2, 3);     // 6
sum(1, 2, 3, 4);  // 6 (4번째 파라미터 값인 '4'는 해체되지 않았습니다)
```

### Rest 파라미터 해체 ex. 2)
```
const sum = (a, b, ...nums) => a + b + nums.reduce((prevNum, curNum) => (
  prevNum + curNum
));

sum(1, 2, 3);             // 6
sum(1, 2, 3, 4, 5);       // 15
sum(1, 2, 3, 4, 5, 10);   // 25
```

<br>

## 연사자로 문자열을 숫자로
```
+'11'         // 11
+'11.111112'  // 11.111112
-'11'         // -11
-'11.111112'  // -11.111112
```
