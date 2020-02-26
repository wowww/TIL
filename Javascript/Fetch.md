# Fetch

Javascript를 사용하여 AJAX 통신을 사용할 때, 다른 자바스크립트 라이브러리를 많이 사용하였습니다.  
> 대표적으로  jQuery의 ajax()  
> 물론 지금까지도 사용되는 `XMLHttpRequest(XML)`을 사용할 수도 있으나 상대적으로 복잡하고 옵션 값 등을 설정하거나 `Promise()`를 함께 사용해야해서 어렵습니다.

## ES6 - fectch API

Javascript의 ES6이 점점 표준이 되면서 `fetch API`를 사용하는 경우가 많아졌습니다. `fetch API`는 사용하기 간단하고 자체적으로 `Promise` 객체를 반환하여 함께 사용하기도 편리합니다.  
아래는 `fetch API`를 사용하여 서버와 비동기 통신을 어떻게 할 수 있는지 그 방법과 예제에 대한 내용입니다.  

> **장점**
> - 사용이 간단하다.
> - Promise 객체로 값을 return 받는다.
> - Response 타입별로 쉽게 적용이 가능(JSON, Blob 등)

## Fetch API 사용하는 방법
Fetch API는 일반적으로 다음과 같은 모습을 가지고 있습니다.  

``` javascript
fetch(url).then(function(response) {
  // Code ...
});
```
코드는 생각보다 간단합니다. 다른 ajax 함수들과 문법도 비슷하죠. 만약 위에 몇 개의 옵션, 파라미터를 사용하는 경우 아래처럼 사용됩니다.  

``` javascript
fetch(url, {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
}).then(function(response) {
  // Code ...
});
```
메소드(method)나 헤더 값 등을 설정할 때 위와 같이 url 다음 인자 값으로 넘기면 됩니다. 이때 아래와 같은 값들을 설정할 수 있습니다.

|값|설명|
|:--|:--|
|method|사용할 메소드를 선택 (GET, POST, PUT, DELETE 등등)|
|headers|헤더에 전달할 값|
|body|바디에 전달할 값|
|mode|cors 등의 값을 설정|
|cache|캐쉬 사용 여부|

> headers ['Content-Type']  
> mode ['cors', 'no-cors', 'same-origin']  

일반적으로 위와 같은 옵션들을 하나의 객체 변수에 저장하여 많이 사용합니다.  

``` javascript
let optObj = {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  },
  'mode': 'cors'
};
fetch(url, optObj).then(function() {
  ...
});
```

## Fetch() POST, PUT, DELETE 메소드 예제 보기
get이 아닌 다른 메소드를 사용하는 경우에는 다음과 같이 method 값을 post 등으로 바꾸어 사용할 수 있습니다. 만약 POST 메소드라면 아래와 같이 작성합니다.  
``` javascript
fetch(url, {
  method: 'POST'
}
}).then(function(response) { ... });
```
데이터를 전송하는 경우에는 다음처럼 body에 값을 추가합니다. 아래는 sitename으로 `webisfree`라는 값을 서버에 전달합니다.  

``` javascript
fetch(url, {
  method: 'POST',
  body: JSON.stringify({ sitename: 'webisfree' })
}).then(function(response) { ... });
```  
위 예제를 살펴보면 body에 `JSON.stringify()`를 볼 수 있습니다. 이렇게 해주지 않으면 json 타입이 아닌 객체 타입으로 전송되어 에러가 발생할 수 있습니다. 자동으로 변환해서 전해지지 않으니 꼭 필요합니다.  

> fetch에 사용되는 메소드 정보
> blob, json, text, formData, clone

## Request, Response, Headers 인터페이스 지원  

fetch API는 `Request`, `Response`, `Headers`라는 인터페이스를 지원합니다. 이를 사용하면 HTTP 요청시 더 쉽고 간단하고 값을 설정하고 전달 할 수 있습니다.  

### Headers()
헤더 정보에 포함되는 값들을 설정합니다.

``` javascript
var _header = new Headers();
_header.append('Content-Type', 'application/json');
```

append()나 set()을 사용하여 값을 설정할 수 있고 get()으로 값을 가져오거나 delete()로 삭제하는 방법이 매우 간단합니다.  

``` javascrtipt
_header.get('Content-Type'); // 'application/json' 값 출력

_header.delete('Content-Type');
_header.get('Content-Type'); // 삭제되어 null을 출력함
```
### Request()
서버에 요청할 때의 값으로 URL, Body값 등을 설정하여 전달할 수 있습니다.  

> fetch API는 최신 브라우저에서는 정상 동작하나 ES 6를 완벽하게 지원하지 않는 경우 fetch API가 동작하지 않을 수 있습니다. 지원되는 브라우저라면 fetch()를 비롯하여 Request, Response, Headers 등의 API가 사용 가능합니다.  


<br>

참고 사이트: [자바스크립트 Fetch API 알아보기](https://webisfree.com/2019-05-15/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-fetch-api-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)