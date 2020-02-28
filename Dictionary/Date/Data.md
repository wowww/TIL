# Date

## 요약
날짜와 시간에 관한 객체.

## 문법
``` javascript
const date1 = new Date();
const date2 = new Date(milliseconds);
const date3 = new Date(dateString);
const date4 = new Date(year, month, day, hours[, minutes, seconds, milliseconds]);
```


## 인자
|인자명|데이터형|필수/옵션|설명ㅣ
|:--:|:----:|:-----:|:--|
|milliseconds|number|옵션|1970/01/01부터의 밀리세컨드(1/1000초)로 객체의 시간을 지정|
|dateString|string|옵션|시간에 대한 문자열. Date.parse 메소드를 통해서 파싱 가능한 형태여야 함.|
|year|number|옵션/필수|년, 2000년과 같이 년도 전체를 적어야 함. 00과 같이 하면 안됨.|
|month|number|옵션/필수|월, 0-11, 0부터 시작함에 주의|
|day|number|옵션/필수|일, 1-31, 1부터 시작함에 주의|
|hour|number|옵션/옵션|시, 0-23, 0부터 시작함에 주의|
|minute|number|옵션/옵션|분, 0-59, 0부터 시작함에 주의|
|millisecond|number|옵션/옵션|밀리세컨드, 0-999, 0부터 시작함에 주의|

## 설명

시간과 관련된 작업을 할 수 있도록 도와주는 객체.
주로 시간을 가져오고(get), 설정(set)하는 것을 통해서 시간을 제어함.  
생성자에 인자를 전달하지 않으면 현재 시간을 가지고 있는 객체를 리턴함.  


## 예제
``` javascript
const date = new Date(); //  인자가 없으면 현재 시간에 대한 Date객체를 생성
console.log(date); // 현재시간을 리턴

// 결과 값: Fri Feb 28 2020 22:37:58 GMT+0900 (대한민국 표준시)
```

``` javascript
const date = new Date(30*365*24*60*60*1000); // 30년 * 365일 * 24시간 * 60분 * 60초 * 1000밀리세컨드 = 1970부터 30년 후에 시간
console.log(date); 
// 결과 값: Sat Dec 25 1999 09:00:00 GMT+0900 (대한민국 표준시)
```

``` javascript
const date = new Date("january 01, 2000 00:00:00");
console.log(date); 

// 결과 값: Sat Jan 01 2000 00:00:00 GMT+0900 (대한민국 표준시)
```

``` javascript
const date = new Date(2000, 0, 1) // Date 객체에 전달되는 인자는 카운팅을 0부터 하는 것도 있고 1부터 하는 것도 있다.
console.log(date); 
// 결과 값: Sat Jan 01 2000 00:00:00 GMT+0900 (대한민국 표준시)
```

``` javascript
const date = new Date(2000, 1, 1, 0,0,0,0);
console.log(date); 
// 결과 값: Tue Feb 01 2000 00:00:00 GMT+0900 (대한민국 표준시)
```