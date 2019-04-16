# React Hooks
React Hooks는 [ReactConf 2018](https://conf.reactjs.org/)에서 발표가 되었습니다.  
class 선언 없이 state를 사용할 수 있도록 하는 새로운 기능이었습니다.

<br>

* [useState](#use-state)
* [useEffect](#use-effect)

<br>

## Hooks는 왜 사용되는걸까?
* 로직이 담긴 컴포넌트와 UI/UX를 담당하는 컴포넌트 간의 차이점이 두드러나지 않습니다.
* 중복된 코드들이 늘어납니다
* HOC(Higher Order Component)를 사용한 wrapper 지옥에 빠질 수 있습니다.

<br>

## useState useEffect
#### #useState
우리가 state를 선언할 때 Hooks는 useState라는 함수를 사용하여 선언할 수 있습니다.  
useState는 반환되는 값이 배열입니다.
* 첫 번째 원소는 state의 값
* 두 번째 원소는 state를 값을 변경해주는 setter 함수


#### #useEffect
React Life Cycles를 useEffect를 이용하여 사용할 수 있습니다.  
useEffect는 아래와 같은 라이프 사이클 함수를 합쳐놓은 것과 같습니다.  
* componentDidMount
* componentDidUpdate
* componentWillUnmount
```
useEffect(() => {
  // componentDidMount, componentDidUpdate
  window.addEventListener('click', handleClick);
  return () => {
    // componentWillUnmount
    window.removeEventListener('click), handleClick;
  };
}, [stateInput]);
```

#### Example)
```
import React, { useState, useEffect } from 'react';

const TodoApp = () => {
  const [todos, setTodos] = useState([]);
  const [newTodo, setNewTodo] = useState();

  const handleInput = e => setNewTodo(e.target.value);
  const handleAddTodo = (e) => {
    e.preventDefault();
    setTodos([...todos, newTodo]);
  };
  
  useEffect(() => {
    // componentDidMount
    // 두 번째 인자에 빈 배열을 넣으면 componentDidMount 처럼
    // 컴포넌트가 마운트된 후 처음에만 실행된다.
  }, []);

  useEffect(() => {
    // componentDidUpdate
    console.log('새롭게 렌더링된 TodoList', todos);
  }, [todos]);

  return (
    <>
      <h1>Todo 어플리케이션</h1>

      <input type="text" onChange={handleInput} />
      <button onClick={handleAddTodo}>Todo 추가하기</button>

      <ul>
        {todos.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </>
  );
};
```

<br>

## useFetch 커스텀 훅 만들기 / 파일 분리(재사용)
API와 통신을 위해, 우리는 간단한 커스텀 훅을 만들것 입니다.  
이는 Fetch API를 사용할 것이며, url과 callBack 함수를 파라미터로 받을 것 입니다.

#### useFetch.js 파일 생성
```useState```를 사용하여 ```loading``` 변수를 만들었습니다. 기본값은 ```false``` 입니다.  
```useEffect```는 처음 mount 될 때에만 실행하기 위하여 두 번째 파라미터에 ```[]``` 빈 배열을 넘겨주었습니다.  
```
import { useState, useEffect } from 'react';

const useFetch = (url = '', callBack = () => {}) => {
  const [loading, setLoading] = useState(false);
  useEffect(() => {
    // 로딩중
    setLoading(true);
    const response = await fetch(url);
    const data = await response.json();
    // 로딩완료
    setLoading(false);
    callBack(data);
  }, []);

  return loading;
};

export default useFetch;
```

