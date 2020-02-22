# 비동기  

#call-stack #callback-queue #event-loop

## 브라우저의 동작 원리

브라우저는 크게 **렌더링 엔진**과 **자바스크립트 엔진**으로 나뉜다.  
브라우저는 사용자가 참조하고자 하는 페이지를 서버에게 요청 후 그에 대한 응답을 화면에 보여주는 역할을 한다.  

<br>

<img width="500px" alt="브라우저 동작 원리" src="https://poiemaweb.com/img/client-server.png">  


<br />  
<br />

|엔진|설명|
|:--:|:--|
|렌더링 엔진|서버로 부터 받은 HTML, CSS는 브라우저 렌더링 엔진의 HTML 파서, CSS파서에 의해 DOM, CSSOM 트리가 만들어지고 렌더 트리로 결합된다. 렌더 트리를 기반으로 브라우저는 웹 페이지를 표시한다.  |
|자바스크립트 엔진|자바스크립트 처리는 자바스크립트 엔진이 하며, JS로 작성한 코드를 해석하고 실행하는 인터프리터이다. 렌더링 엔진의 HTML 파서가 DOM 생성 프로세스를 하던 중 스크립트 태그를 만나면 자바스크립트 코드를 실행시키기 위해 자바스크립트 엔진에게 제어 권한을 넘겨 주게 된다.  DOM 트리가 다 형성되지 않았는데 자바스크립트에서 해당 DOM을 조작하려고 하면 문제가 발생하기 때문에 `<script>` 태그는 html의 body 태그 제일 아래에 놓는 것이 좋다. |   

> **외부의 자바스크립트 파일을 불러오는 경우에는?**  
> 
> 외부 자바스크립트 파일의 경우 브라우저가 일시 중지하고 디스크, 캐시 또는 원격 서버에서 스크립트를 가져올 때까지 기다려야한다. 이로 인해 주요 렌더링 경로에 수십~수천 밀리초(m/s)의 지연이 추가로 발생할 수 있다.  

## 자바스크립트 엔진
자바스크립트는 싱글 스레드 언어로 한번에 하나의 태스크만 처리할 수 있다. 구글에서 개발한 V8을 비롯해 대부분의 자바스크립트 엔진은 크게 세 영역으로 나뉜다.  
- Call Stack
- Task Queue(Event Queue)
- Heap


## 1. Call Stack
호출 스택은 자바스크립트 프로그램에서 우리가 어디에 있는지 기록하는 데이터 구조이다. 함수를 실행하면 함수에 대한 기록을 스택 제일 위에 추가한다(push). 함수의 결과 값을 반환하면 스택에서 제거된다(pop).

``` javascript
function foo(b) {
  var a = 5;
  return a * b + 10;
}

function bar(x) {
  var y = 3;
  return foo(x * y);
}

console.log(bar(6));

// 결과 값: 100
```

> 1. 먼저 `bar`라는 함수를 호출했으니 `bar`에 해당하는 스택 프레임이 형성되고, 그 안에는 `y`와 같은 local variable과 argument가 함께 생성된다.  
> 2. `bar` 함수는 `foo` 함수를 호출하고 있다. 아직 `bar`라는 값을 반환하지 않은 상태이니 스택에서 pop 되지 않고 호출된 `foo` 함수가 Call Stack에 push 된다.  
> 3. `foo` 함수에서는 `a * b + 10` 이라는 값을 리턴하면서 함수의 역할을 마쳤으므로 Stack에서 pop 된다.   
>4. 다시 `bar` 함수로 돌아와서 `foo` 함수로 받은 값을 리턴하며 함수를 종료하고 스택에서 pop 된다.  

  
  
자바스크립트는 단일 호출 스택을 사용한다. 이 말은 하나의 함수가 실행되고 있으면 이 함수의 실행이 끝날 때까지 다른 테스크들은 수행될 수 없다는 뜻이다. 앞의 예 처럼 호출된 함수가 차례대로 쌓이고 값을 반환하면서 순서대로 pop된다.  

## 2. Heap
메모리 힙은 동적으로 만드어진 객체(인스턴스)가 메모리에 할당되는 곳이다.  

이러한 단일 호출 스택의 문제점은 스택이 한번에 하나의 일만 처리할 수 있기 때문에 만약 그 함수가 복잡한 연산을 수행해야 한다고 하면 다른 함수가 실행되지 못하는 상황이 생긴다.  

가장 쉬운 해결책은 **비동기 콜백**을 사용하는 것이다. 우리 코드의 일부를 실행하고 나중에 실행될 콜백 함수를 제공한다. 비동기 콜백은 즉시 호출 스택에 쌓이지 않고 **Event Queue**에서 기다렸다가 호출 스택이 비어있는 시점에 실행된다.  

## 3. Event Queue

Javascript의 런타임 환경의 이벤트 큐는 처리할 메세지 목록과 실행할 콜백 함수들의 리스트이다.  
버튼 클릭 같은 이벤트나 DOM 이벤트, http 요청, setTimeout 같은 비동기 함수는 **Web API**를 호출하며 Web API는 콜백 함수를 콜백 큐에 밀어넣는다.  

이벤트 큐는 대기하다가 스택이 비는 시점에 이벤트 루프를 돌려 해당 콜백 함수를 스택에 넣는다. 이벤트 루프의 기본 역할은 큐와 스택 두 부분을 지켜보고 있다가 스택이 비는 시점에 콜백을 실행시켜 주는 것이다.  

웝 브라우저에서는 이벤트가 발생할 때마다 메세지가 추가되고 이벤트 리스너가 첨부된다. 콜백 함수의 호출은 호출 스택의 초기 프레임으로 사용되며 Javascript가 싱글 스레드 이므로 스택에 대한 모든 호출이 반환될 때까지 메세지 폴링 및 처리가 중지된다. 동기식 함수 호출은 이와 반대로 새 호출 프레임을 스택에 추가한다.  

### 이벤트 큐와 이벤트 루프의 동작을 잘 보여주는 setTimeout 예

``` javascript
setTimeout(function() {
  console.log("first");
}, 0);
console.log("secound");

// 실행 순서
// secound
// frist
```
> setTimeout에 0ms를 주었는데 바로 실행되는 것이 아니다.
> setTimeout은 호출 스택에서 실행된 후 Web API의 Timer API를 호출한다. Web API에 의해 setTimeout의 콜백 함수는 이벤트 큐에 enqueue 된다.
> 즉, 0초 뒤에 callback 함수를 실행해라! 가 아닌, 0초 뒤에 callback 함수를 이벤트 큐에 넣어라라는 의미이다.  
> 
> console.log('secound')가 호출 스택에 쌓이고 secound가 실행된 후, 호출 스택이 비었을 때 first가 콘솔 창에 나타나게 되는 것이다.  
>
> ```
> setTimeout의 delay를 0으로 주더라도 0초보다 더 걸리는 이유는 콜 스택에서 모든 프레임이 실행될 때까지 기다려야하기 때문이다.
> ```

setTimeout은 Javascript 내부에서 제공하는 것이 아닌 외부에서 제공하는 Web API이다. 마찬가지로 어떤 버튼이 있다고 하고, 해당 버튼에 click 이벤트 리스너를 붙여놨다고 해보자.   
버튼이 클릭되면 eventHandler가 추가된 후, 호출 스택이 비었을 때 실행된다.  

### element.addEventlistener('click', () => foo(value))

``` javascript
element.document.querySelector('...');
element.addEventlistener('click', foo()); // (1)
element.addEventlistener('click', foo); // (2)
element.addEventlistener('click', () => foo(value)); // (3)
```

(1) 이렇게 핸들러를 바로 호출해버리면 이벤트 발생까지 기다리지 않고 바로 실행됨  
(2) click 될 때 event queue로 넘어가기 위해서 핸들러 이름만 적어줌 -> 인자를 전달 못함  
(3) 이벤트 발생 때 까지 기다렸다가 실행될 수 있고 인자를 전달할 수 있음






## 참고
- [[JS] 자바스크립트 엔진, Event Loop, Event Queue, Call Stack](https://velog.io/@imacoolgirlyo/JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%97%94%EC%A7%84-Event-Loop-Event-Queue-Call-Stack)
- [브라우저 동작 원리](https://poiemaweb.com/js-browser)  
- [Javascript의 엔진, 실행 환경](https://new93helloworld.tistory.com/358)  
- [Javascript의 Event Loop](https://asfirstalways.tistory.com/362)
- [Adding interactivity with javscrtipt](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript?hl=ko)