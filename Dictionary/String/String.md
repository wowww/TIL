# String

## 요약
문자열 객체.  
문자를 제어하는 다양한 메소드와 속성을 가지고 있다.  

## 문법
``` javascript
String([stringText])
new String([stringText])
```
`String`은 자주 사용하는 객체이므로 다음과 같은 형식을 사용하면 암시적으로 `String` 객체가 된다.  
위와 아래는 똑같이 `String` 객체를 생성한다.  

``` javascript
'stringText'
"stringText"
```

## 인자
stringText - 문자  

## 설명
`String(문자열) Object(객체)`는 자바스크립트의 원시 데이터 형이다.  
Javascript에서는 `String Object`를 생성하는 다양한 방법이 있는데 문법과 같이 `new`를 이용하는 것과 `'`,`"`를 이용하는 방법이 있다.  

## 예제
``` javascript
const a = new String('문자열');
const b = String('문자열');
const c = '문자열';

alert(a == b && b == c); // true
alert(a === b && b === c); // false
```