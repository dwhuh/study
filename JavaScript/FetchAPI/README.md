# Fetch API
우리는 그 동안 비동기로 요청하기 위해서는 XHR(XML HTTP Request) 객체를 사용했습니다.  
이런 XHR 객체는 잘 디자인되어 있는 API가 아니었는데요, 요청의 상태/변경을 하려면  
Event를 등록하여 response를 받아야 했고 요청의 성공/실패 여부에 따라 처리하는 로직이 불편하였습니다.

<br>

이를 보완하기 위해서 Fetch API를 도입하였는데요.
이는 HTTP 요청에 최적화 되어 있고 상태도 잘 추상화되어 있습니다.  
Promise 기반이라 상태에 따른 로직을 추가하고 처리하는데에도 최적화 되어 있습니다.

<br>

## 일반적인 사용방법
```
fetch('URL')
  .then(response => response.json())
  .then(json => json.filter(item => item.firstAttr))
```

<br>

## 기본사항
Fetch API는 3개의 인터페이스를 도입하였습니다.  
이는 곧 HTTP의 개념과 대응되는 인터페이스 입니다.  
아래와 같습니다.

#### Headers
HTTP Header와 대응되는 객체입니다.  
```append()``` 메소드를 이용하여 HTTP Request Header 정보를 추가할 수 있습니다.  
```
const headers = new Headers();
const content = 'My name is dong wook huh';
headers.append('Content-Type', 'application/json'));
headers.append('Content-Length', content.length.toString());
```
또는 Object 리터럴을 이용하여 정의할 수도 있습니다.
```
const content = 'My name is dong wook huh';
const headers = new Headers({
  'Content-Type': 'application/json',
  'Content-Length': content.length.toString(),
});
```

<br>

### Request
HTTP 요청을 통해 자원을 가져오는 인터페이스입니다.  
Request는 URL, Header, Body가 필요합니다.  
그리고 Request에 대한 mode 제한과 certificate 관련 설정도 추가할 수 있습니다.  
Request 객체를 생성한다고해서 HTTP Request 정보를 보낼 수 있는 것은 아닙니다.  
Request 두 번째 인자에 정보를 입력할 수 있는 것은 아래와 같습니다.
* **method**        (default: 'GET')
* **headers**
* **body**          (new FormData 객체도 보낼 수 있습니다. 즉, 파일전송도 가능합니다.)
* **mode**          (default: 'no-cors')
  * same-origin
  * no-cors
  * cors
* cache
* credentials
* redirect
* referrer
* integrity
```
const request = new Request('URL', {
  method: 'GET',
  headers: new headers({ ... }),
  body: {
    name: 'My name is dong wook huh',
    pass: '1234',
  },
});
```
**Request 객체는 스펙을 정의할 뿐** 데이터를 Fetching 하는것은 Fetch Method 를 사용해야 합니다.
```
fetch(request)
  .then(response => response.json())
  .then(json => console.log(json));
```

<br>

### Response
Response는 fetch를 호출하면 가져올 수 있는 객체인데요,  
ServiceWorker가 아니면 생성해서 쓰는것이 크게 의미가 없습니다.
* **status**  
  HTTP Response Code를 담고 있으며, 일반적으로 성공했다면 200이 됩니다.
* **statusText**  
  기본값은 **'ok'** 이고 상황에 따라 다른 Message가 담길 수 있습니다.
* **ok**  
  Status의 200299의 값을 추상화한 boolean 값인데 200299사이의 status이면 true를 가지게 됩니다.
* **headers**  
  Response Headers 인데, headers의 guard 속성은 response로 되어 있습니다.
* **type**  
  type은 Response 객체의 type을 의미합니다.
  * **basic**  
    가장 기본 적인 속성이고 Request 객체의 mode 값이 cors가 아니라면 basic이 올 것입니다.  
    가능한 모든 Headers 속성에 접근할 수 있습니다.
  * **cors**  
    Request 객체의 mode 값이 cors일때 나오는 값 입니다.  
    이 때는 Headers 일부 속성에 접근이 제한됩니다.
  * **opaque**  
    Request 객체의 mode 값이 no-cors일때 나오는 값 입니다.
  * **error**  
    Response.error()를 호출했을때 나오는 type입니다.
