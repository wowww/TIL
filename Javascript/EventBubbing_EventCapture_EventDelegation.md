> 이 글은 [CAPTAIN PANGYO - 이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/) 글을 기반으로 작성되었습니다. 

# 이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임

## 이벤트 등록  

본문에 들어가기 앞서 가장 기본적으로 이해하고 있어야 하는 내용은 `이벤트 등록(addEventListener)` 입니다.  
여기서 이벤트 등록이란 웹 애플리케이션에서 사용자의 입력을 받기 위해 필요한 기능입니다. 아래와 같은 코드를 의미합니다.  
``` html
<button>add one item</button>
```


``` javascript
const button = document.querySelector('button');
button.addEventListener('click', addItem);

function addItem(event) {
  console.log(event);
}
```

`add one item`이라는 간단한![Uploading file..._kbt2artdb]()
 버튼을 만들어 클릭했을 때 `addItem()`이라는 함수를 실행시키는 코드입니다.  
버튼을 클릭하고 나면 `addItem`함수가 실행되고 `addItem` 함수에 `event` 인자가 넘어옵니다. `event` 인자를 콘솔에 출력해보면 이벤트와 관련된 정보를 확인할 수 있습니다.  

이처럼 `addEventListener()` 웹 API는 웹 개발자들이 화면에 동적인 기능을 추가하기 위해 자연스럽게 접하게 되는 기본적인 기능입니다. 사용자의 입력에 따라 추가 동작을 구현할 수 있는 방법이죠.  
여기서 브라우저는 어떻게 이벤트의 발생을 감지했을까요?  
브라우저가 이벤트를 감지하는 방식 2가지를 아래에서 알아보겠습니다.  

## 이벤트 버블링(Event Bubbing)
이벤트 버블링은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미합니다.  
아래의 그림처럼요!  

<img src="https://joshua1988.github.io/images/posts/web/javascript/event/event-bubble.png" />  

> 하위의 클릭 이벤트가 상위로 전달되어 가는 그림
 
```
상위의 화면 요소란?
HTML 요소는 기본적으로 트리구조를 갖습니다. 여기서는 트리 구조상으로 한 단계 위에 있는 요소를 상위요소라고 하며, body 태그를 최상위 요소라고 부르겠습니다.
```

``` html
<body>
	<div class="one">
		<div class="two">
			<div class="three">
			</div>
		</div>
	</div>
</body>
```

``` javascript
const divs = document.querySelectorAll('div');
divs.forEach(div => {
  div.addEventListener('click', logEvent);
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```
위 코드는 세 개의 div 태그에 모두 클릭 이벤트를 등록하고 클릭했을 때, `logEvent` 함수를 실행시키는 코드입니다.  
여기서 위 그림대로 최하위 div 태그 `<div class="three"></div>`를 클릭하면 아래와 같은 결과가 실행됩니다.  



<img width="300" src="https://i.imgur.com/zzUJ9IE.png" />  

> three 클래스를 갖는 div 태그를 클릭했을 때의 결과 


div 태그를 한 개만 클릭했을 뿐인데 왜 3개의 이벤트가 발생되는 걸까요? 그 이유는 브라우저가 이벤트를 감지하는 방식 때문입니다.  

브라우즈는 특정 화면 요소에서 이벤트가 발생했을 때, 그 이벤틀르 최상위에 있는 화면 요소까지 이벤트를 전파시킵니다.  
따라서 클래스 명 `three` -> `two` -> `one` 순으로 클릭 이벤트가 동작하겠죠.  

여기서 주의해야 할 점은 각 태그마다 이벤트가 등록되어 있기 때문에 상위 요소로 이벤트가 전달되는 것을 확인할 수 있습니다. 만약 이벤트가 특정 div 태그에만 달려 있다면 위와 같은 동작 결과는 확인할 수 없습니다.  

이와 같은 하위에서 상위 요소로의 이벤트 전파 방식을 **이벤트 버블링**(**Event Bubbling**)이라고 합니다.  

"Trigger clicks all the way up."  


## 이벤트 캡쳐(Event Capture)

이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.  

<img src="https://joshua1988.github.io/images/posts/web/javascript/event/event-capture.png" />  

> 클릭 이벤트가 발생한 지점을 찾아내려 가는 그림  


위 그림처럼 특정 이벤트가 발생했을 때, 최상위 요소인  body 태그에서 해당 태그를 찾아 내려갑니다.  
그러면 이벤트 캡쳐는 코드로 어떻게 구현할 수 있을까요?  

``` html
<div class="one">one
  <div class="two">two
    <div class="three">three</div>
  </div>
</div>
```

``` javascript
const divs = document.querySelectorAll('div');
divs.forEach(div => {
	div.addEventListener('click', logEvent, {
		capture: true // default 값은 false입니다.
	});
});

function logEvent(event) {
	console.log(event.currentTarget.className);
}
```

`addEventListener()` API에서 옵션 객체에 `capture:true`를 설정해주면 됩니다. 그러면 해당 이벤트를 감지하기 위해 이벤트 버블링과 반대 방향으로 탐색합니다.  

따라서, 아까와 동일하게 `<div class="three"></div>`를 클릭해도 아래와 같은 결과가 나타납니다.  


<img width="300" src="https://i.imgur.com/Rnw8qyk.png" />  

> three 클래스를 갖는 div 태그를 클릭했을 때의 결과
 


## event.stopPropagation()

"난 이렇게 복잡한 이벤트 전다 방식을 알고 싶지 않고, 그냥 원하는 화면 요소의 이벤트만 신경 쓰고 싶어요!" 라고 생각하시는 분들이 충분히 있을 수 있습니다.  
실제로 마감 기한에 쫓기는 상황에서 이런 동작 방식을 정확히 이해하는 시간보다는 구현에 더 많은 시간을 쏟아야 하기 때문입니다. 그럴 때는 아래처럼 `stopPropagation()` 웹 API를 사용합니다.  

``` javascript
function logEvent(event) {
  event.stopPropagation();
}
```

위 API는 해당 이벤트가 전파되는 것을 막습니다. 따라서, 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해합니다.  
그리고 이벤트 캡쳐의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않습니다.  

위와 같이 `logEvent` 함수에 `stopPropagation()` API를 사용한다면 앞의 '이벤트 버블링 예제'와 '이벤트 캡처 예제'에서 사용한 코드 기준으로 각각 three와 one이 찍히겠네요.  

``` javascript
// 이벤트 버블링 예제
divs.forEach(div => {
  div.addEventListener('click', logEvent);
});

function logEvent(event) {
	event.stopPropagation();
	console.log(event.currentTarget.className); // three
}
```

``` javascript
// 이벤트 캡쳐 예제
divs.forEach(div => {
	div.addEventListener('click', logEvent, {
		capture: true // default 값은 false입니다.
	});
});

function logEvent(event) {
	event.stopPropagation();
	console.log(event.currentTarget.className); // one
}
```

## 이벤트 위임 - Event Delegation


앞에서 살펴본 이벤트 버블링과 캡쳐는 사실 이벤트 위임을 위한 선수 지식이라고 해도 과언이 아닙니다. 이벤트 위임은 실제 바닐라 JS로 웹 앱을 구현할 때 자주 사용하게 되는 코딩 패턴입니다.  

이벤트 위음을 한 문장으로 요약해보면 '하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식'입니다.  

아래의 코드를 살펴보겠습니다.  

``` html
<h1>오늘의 할 일</h1>
<ul class="itemList">
  <li>
	<input type="checkbox" id="item1">
	<label for="item1">이벤트 버블링 학습</label>
  </li>
  <li>
	<input type="checkbox" id="item2">
    <label for="item2">이벤트 캡쳐 학습</label>
  </li>
</ul>
```

``` javascript
const inputs = document.querySelectorAll('input');
inputs.forEach(input => {
  input.addEventListener('click', function(event) {
	alert('clicked');
  });
});
```

> 할 일 목록의 체크 박스를 클릭했을 때 클릭 이벤트 리스너가 동작하는 모습


<img src="https://joshua1988.github.io/images/posts/web/javascript/event/event-delegation-1.gif" />  


자바스크립트 `querySelectorAll()`을 이용해 화면에 존재하는 모든 인풋 박스 요소를 가져온 다음 각 인풋 박스의 요소에 클릭 이벤트 리스너를 추가합니다.  
화면을 실행시키고 각 리스트 아이템의 인풋 박스(체크 박스)를 클릭하면 위와 같이 알람 창이 표시됩니다.  

여기까지는 별다를 것 없는 이상하지 않는 코드였는데요. 만약 여기서 할 일이 더 생겨서 리스트 아이템을 추가하면 어떻게 될까요?  

``` javascript
// ...

// 새 리스트 아이템을 추가하는 코드
const itemList = document.querySelector('.itemList');

const li = document.createElement('li');
const input = document.createElement('input');
const label = document.createElement('label');
const labelText = document.createTextNode('이벤트 위임 학습');

input.setAttribute('type', 'checkbox');
input.setAttribute('id', 'item3');
label.setAttribute('for', 'item3');
label.appendChild(labelText);
li.appendChild(input);
li.appendChild(label);
itemList.appendChild(li);
```

새로 추가한 리스트 아이템에 클릭 이벤트가 정상적으로 동작하는지 확인해보겠습니다.  

<img src="https://joshua1988.github.io/images/posts/web/javascript/event/event-delegation-2.gif" />  

> 새로 추가된 리스트 아이템(이벤트 위임 학습)에서 클릭 이벤트가 동작하지 않는 모습  
 

새로 추가된 리스트 아이템에는 클릭 이벤트 리스너가 동작하지 않는데 왜그럴까요?  

코드를 다시 살펴보면, input 박스에 클릭 이벤트 리스너를 추가하는 시점에서 리스트 아이템은 두 개입니다. 따라서, 새롭게 추가된 리스트 아이템에는 클릭 이벤트 리스너가 등록되지 않았죠.  
이런 식으로 매번 새롭게 추가된 리스트 아이템까지 클릭 이벤트 리스너를 일일이 달아줘야 할까요?  

리스트 아이템이 많아지면 많아질수록 이벤트 리스너를 다는 작업 자체가 매우 번거롭습니다. 이 번거로운 작업을 해결할 수 있는 방법이 바로 이벤트 위임(Event Delegation)입니다.  

앞에서 살펴본 코드를 아래와 같이 변경해보겠습니다.  

``` javascript
// var inputs = document.querySelectorAll('input');
// inputs.forEach(function(input) {
// 	input.addEventListener('click', function() {
// 		alert('clicked');
// 	});
// });

var itemList = document.querySelector('.itemList');
itemList.addEventListener('click', function(event) {
	alert('clicked');
});

// 새 리스트 아이템을 추가하는 코드
// ...
```

화면의 모든 input 박스에 일일이 이벤트 리스너를 추가하는 대신 이제는 인풋 박스의 사우이 요소인 ul 태그, `.itemList`에 이벤트 리스너를 달아 놓고 하위에서 발생한 클릭 이벤트를 감지합니다. 이 부분이 앞에서 배웠던 이벤트 버블링이죠.  

결과는 다음과 같습니다.  

<img src="https://joshua1988.github.io/images/posts/web/javascript/event/event-delegation-3.gif" />  

> 새로 추가된 리스트 아이템에서 클릭 이벤트가 정상적으로 동작하는 모습 
 


이젠 리스트 아이템을 새로 추가할 때마다 클릭 이벤트를 안달아도 되겠네요!!  


```
참고: 위 코드는 현재 input 박스의 이벤트만 다루는 것이 아니라 label 태그의 이벤트도 감지합니다. 
event 객체를 이용하여 인풋 박스의 이벤트만 감지할 수 있도록 구현해보세요.

내 답변: if (클릭되는것이 !== 'input') return; 을 맨 윗줄에 입력해서 input 태그가 아니면 빠져나간다.
```
