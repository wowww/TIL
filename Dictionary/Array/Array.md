# Array

## 요약

연관되어 있는 복수의 값을 하나의 컨테이너로 관리

## 문법
``` javascript
const arr1 = new Array(arrayLength);
const arr2 = new Array(element0, element1, element2, ..., elementN);
``` 

``` javascript
// Array literals
const literals = [element0, element1, element2, ..., elementN];
```

## 인자

|인자명|데이터형|필수/옵션|설명|
|:---:|:---:|:-----:|:--|
|arrayLength|number|옵션|배열의 원소수(length)를 지정한다. 생략하면 배열의 원소수는 1이된다. |
|elementN|number|옵션|배열에 포함될 원소의 값|

## 반환값(Return)

Array

## 설명

배열은 연관된 데이터들을 하나의 그룹으로 묶어서 효율적으로 데이터들을 관리하기 위해 사용된다.    
배열에 저장되는 데이터는 순차적으로 저장되고 고유한 index 값을 가지고 있다.    
하나의 배열을 구성하는 단위 데이터들을 `element`, `원소`라고 부른다.    
배열에 특정 원소에 접근하기 위해서는 `arrayValue[index]`의 형식으로 접근한다.   
배열의 원소를 식별하기 위해서 사용하는 `index`는 숫자가 아니라 문자를 사용할 수도 있다.  
> `Associative Array 연관 배열`이라고 부른다.



## 예제
``` javascript
// Array를 선언하는 방법
const a = new Array(1, 2, 3);
const b = [1, 2, 3];
const c = new Array();
c.push(1);
c.push(2);
c.push(3);
```

``` javascript
// Array와 반복문
const a = new Array(1, 2, 3);
for(let i = 0; i < a.length; i++) {
  alert(a[i]);
}
```

``` javascript
const a = new Array(5);
console.log(a); // 결과 값: [undefined, undefined, undefined, undefined, undefined]

// 값이 없는 5개의 원소를 포함한 array를 생성한다.
```

``` javascript
const b = new Array('5');
console.log(b); // ["5"]

// String Object 5를 원소로 하는 array를 생성한다.
```

``` javascript
var c = new Array(new Number(5));
alert(c); // 5

// Number Object 5를 원소로 하는 array를 생성한다.
```

``` javascript
// 배열의 원소에 엑세스하는 방법
var a = new Array(1,2,3);
console.log(a[0]); // 1
console.log(a[1]); // 2
console.log(a[2]); // 3
```

> ### 참고
> ``` javascript
> // Associative Array 연관 배열
> const codingeverybody = new Array();
>  
>codingeverybody['html'] = '웹문서를 만든다';
>codingeverybody['css'] = 'html을 꾸며준다';
>codingeverybody['javascript'] = 'html을 동적으로 제어한다';
>codingeverybody['php'] = 'html을 서버측에서 동적으로 생성한다';
> 
> alert(codingeverybody['html']); // string, 웹문서를 만든다
>alert(codingeverybody['css']); // string, html을 꾸며준다
 >
>codingeverybody.push(1);
>codingeverybody.push(2);
> 
>alert(codingeverybody); // >array, [1,2]
>console.log(codingeverybody.length); // number 2
> 
>// Associative array를 자바스크립트에서는 배열이 아니라 object로 처리한다.
>// codingeverybody는 array object이지만, 동시에 object이기도 하기 때문에 
>// html, css, javascript, php는 object codingeverybody의 원소가 아니라 property(속성)으로 사용된다.
>// 그래서 위의 코드에서 codingeverybody.length 가 2가 된다.   
>```