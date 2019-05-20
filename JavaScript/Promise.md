# Promise
자바스크립트의 비동기 처리를 해결하기 위해 사용되는 객체.

<br>

## Promise를 사용하는 이유
자바스크립트는 단일 스레드 프로그래밍 언어입니다.  
비동기 코드로 받아온 데이터를 바로 다음 작업을 수행하게 되면  
비동기로 받아온 데이터가 없다는 오류가 발생합니다. 이는 아직 데이터를 받지 못했기 때문에 발생합니다.  
이러한 비동기 코드를 처리하기 위해 사용됩니다.

<br>

## Promise의 상태(States)
* **Pending**  
  (대기) 비동기 처리가 완료되지 않은 상태.
* **Fulfilled**  
  (이행) 비동기 처리가 완료되었고, 프로미스의 결과 값을 반환해준 상태.
* **Rejected**  
  (실패) 비동기 처리에 실패하거나 오류가 발생한 상태.

<br>

## Promise의 장점 #1
프로미스는 ```then``` 메소드를 호출하면 새로운 프로미스를 반환하게 됩니다.  
```then(...).then(...).then(...)``` 와 같이 여러개의 프로미스를 체이닝하여 비동기 처리를 할 수 있습니다.

<br>

## Promise의 장점 #2
기존 Callback으로 비동기 처리를 하였을 때, 체이닝을 이용한 비동기 처리를 통해 콜백지옥을 해결할 수 있습니다.

<br>

## 에러 핸들링
```then``` 메소드의 두 번째 인자로 에러를 핸들링 할 수 있고,
```catch``` 메소드를 이용하여 에러를 핸들링 할 수 있습니다.
```
then(handleResove, handleReject)
...
then(...).catch(...)
```