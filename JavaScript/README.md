
# ES6+ 문법

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
