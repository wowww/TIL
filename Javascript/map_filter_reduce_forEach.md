# map, filter, reduce, forEach

## 일반적인 Loop 구문

``` javascript
// for 문
const arr = [1, 2, 3, 4, 5, 6];
const new_arr = [];

for (let i = 0; i < arr.length; i++) {
  if (arr[i] % 2 === 0 ) { // 2의 배수
    new_arr.push(arr[i]);
  }
}

console.log(new_arr);
```


``` javascript
// forEach
const arr = [1, 2, 3, 4, 5, 6];
const new_arr = [];

arr.forEach(n => {
  if(n % 2 === 0) {
    new_arr.push(n);
  }
});

console.log(new_arr);
```
결과 값: [2, 4, 6]  

## map()
인자 값: currentValue, index, array  
요소를 일괄적으로 변경
``` javascript
const arr = ["kwon", "eunjee", "lia", "A형"];
const arr2 = arr.map(v => v.length);
console.log(arr2);
```
결과 값: [4, 6, 3, 2]  

## filter()

요소를 걸러내서 배열로 true/false로 반환, 없으면 빈 배열  
``` javascript
const arr = [4, 15 ,377, 395, 400, 1024, 3000];
const arr2 = arr.filter(v => (v % 5 === 0));

console.log(arr2);
```
결과 값: [15, 395, 400, 3000]  

## find()
단 하나의 요소만 반환, 여러 개 있으면 처음 값만 반환  
``` javascript
const arr = [4, 15, 377, 395, 400, 1024, 3000];
const arr2 = arr.find((n) => (n % 5 === 0));

console.log(arr2);
```
결과 값: 15  

## reduce()
인자 값은 callback [, initivalValue]  
callback은 `previouseValue`, `currentValue`, `currentIndex`, `array [, initivalValue]`는 옵션  

**reduce()** 는 위의 `map`, `find`, `filter` 대체 가능  

``` javascript
const arr = [9, 2, 8, 5, 7];
const sum = arr.reduce((prev, value) => prev + value);

console.log(sum);
```

결과 값: 31  

``` javascript
// map
const arr = ['kwon', 'eunjee', 'lia', 'A형'];
const arr2 = arr.reduce((prev, value) => {
  prev.push(value.length);
  return prev;
}, []);

console.log(arr2);
```
결과 값: [4, 6, 3, 2]  

``` javascript
// filter
const arr = [4, 15, 377, 395, 400, 1024, 3000];
const arr2 = arr.reduce((prev, value) => {
  if (value % 5 === 0) prev.push(value);
  return prev;
}, []);

console.log(arr2);
```
결과 값: [15, 395, 400, 3000]  

``` javascript
// find
const arr = [4, 15, 377, 395, 400, 1024, 3000];
const arr2 = arr.reduce((prev, value) => {
  if (typeof prev === 'undefined' && value % 5 === 0) prev = value;
  return prev;
}, undefined);

console.log(arr2);
```
결과 값: 15  

## 기타 응용
``` javascript
// 합집합
const arrA = [1, 4, 3, 2];
const arrB = [5, 2, 6, 7, 1];
console.log([...new Set([...arrA,...arrB])]);
```
결과 값: 
[1, 4, 3, 2, 5, 6, 7]  
 
``` javascript
// 교집합
const arrA = [1, 4, 3, 2];
const arrB = [5, 2, 6, 7, 1];
console.log(arrA.filter(it => arrB.includes(it)));
```

결과 값: [1, 2]  


참고 사이트:   
https://velog.io/@decody/map-%EC%A0%95%EB%A6%AC  
https://bblog.tistory.com/300  