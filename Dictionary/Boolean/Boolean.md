# Boolean

## 요약
원시 데이터형인 Boolean Value를 객체화(wrapper) 시킴

## 문법
``` javascript
new Boolean(value)
```


## 인자
|인자명|데이터형|필수/옵션|설명|
|:--:|:----:|:-----:|:--|
|value|boolean value|필수|Boolean Object의 기본 값. <br /> value의 값을 생략하거나, `0, -0, null, false,  NaN, undefined, 빈문자열`을 전달하면 `false`로 변환되서 전달됨. <br /> 그 외 데이터는 `false`조차도 `true`가 된다. |

## 설명
`Boolean Object`와 `Boolean Value`를 구분해야 한다.  
`value`의 값으로 `false`가 전달된 `Boolean Object`라고 해도 조건문에서는 `true`가 된다.  

## 예제
``` javascript
const x = new Boolean(false);
if(x){
  alert('짜잔!'); // 실행, Boolean의 값으로 false를 주어도 boolena 객체는 조건문에서 true로 형변환됨
}

const y = false;
if(y){
  alert('짜자잔!'); // 실행 안됨
}
```