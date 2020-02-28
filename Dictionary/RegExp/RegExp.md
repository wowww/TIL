# RegExp

## 요약
정규 표현식(Regular Expression)을 사용허기 위한 객체를 생성한다.  

## 문법

``` javascript
new RegExp(pattern [,flags]) // 생성자 방식
/pattern/flags // 정규표현식 리터럴 
```

## 인자

|인자명|데이터형|필수/옵션|설명|
|:---:|:----:|:----:|:--|
|pattern|string|필수|정규표현식|
|flags||옵션| - g: 텍스트 전체에서 일치하는 문자를 찾을 때, 지정하지 않으면 첫번째 일치하는 문자만 검색<br /> - i: 대소문자를 구분하지 않는다. <br /> - m: ^(첫번째 문자)와 $(마지막 문자)가 (\n, \r로 구분되는) 행단위로 일치 |


## 설명

정규 표현식은 다음과 같은 경우 사용한다.  

- 문자열에서 특정 문자열이 존재하는지 확인
- 문자열의 특정 부분을 다른 문자열로 변경

|Character|Meaning|
|:-------|:-------|
| `\` | 이스케이핑(escaping) | 
| `^` | 범위, 시작 지점 |
| `$` | 범위, 끝나는 지점 |
| `*` | 수량, 없거나 더 많다 == {0,} |
| `+` | 수량, 1보다 많다. == {1,} |
| `?` | 수량, 없거나 하나이다. |
| `.` | 일치, 문자 하나와 일치 |
| `(x)` | 일치, x와 일치하는 것을 찾은 후에 이에 접근할수 있도록 함 |
| `x|y` | 일치. x나 y와 일치 |
| `t{n}` | 수량. t와 n번 일치하는 문자열과 일치 |
| `t{n,}` | 수량, t와 n번 이상 일치하는 문자열과 일치 |
| `t{n,m}` | 수량. t와 n번 이상 m번 이하로 일치하는 문자열과 일치 |
| `[xyz]` | 일치. xyz중에 하나라도 일치하는 문자열과 일치 |

## 예제
``` javascript
const re = /(\w+)\s(\w+)/; // 가운데 공백(\s)이 들어가는 단어(\w)와 단어를 찾아서 참조(괄호)할 수 있게 한다.
const str = "coding everybody";
const newstr = str.replace(re, "$2, $1"); // string object의 replace 메소드를 이용해서 첫번째 참조와 두번째 참조의 순서를 바꾸고 그 사이에 ','를 넣는다.
console.log(newstr); // everybody, coding
```

``` javascript
const text = "First line\nsecond line";
const regex = /(\S+) line\n?/y;

const match = regex.exec(text);
console.log(match[1]); // "First"
console.log(regex.lastIndex); // 11

const match2 = regex.exec(text);
console.log(match2[1]); // "Second"
console.log(regex.lastIndex); // "22"

const match3 = regex.exec(text);
console.log(match3 === null); // "true"
```