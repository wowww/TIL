# startsWith()

`.startsWith()`는 문자열이 특정 문자열로 시작하는지 확인하는 메서드입니다.  
IE는 Edge부터 지원합니다.  

## 문법(Syntax)
``` javascript
string.startsWith(searchString, length)
```
- searchString : 검색할 문자열로 필수 요소입니다. 대소문자를 구분합니다.
- length : 문자열 중 어디까지 검색할지 정합니다. 선택 요소로, 값이 없으면 전체 문자열을 대상으로 합니다.

## 예제(Example)
``` javascript
'abcde'.startsWith('a');
```
`abcde`가 `a`로 시작하는지 검사합니다. `a`로 시작하므로 `true`를 반환합니다.
``` javascript
'abcde'.startsWith('a', 1);
```
`bcde`가 `a`로 시작하는지 검사합니다. `a`로 시작하지 않으므로 `false`를 반환합니다.
``` javascript
'abcde'.startsWith('A');
```
`abcde`가 `A`로 시작하는지 검사합니다. 대소문자를 구분하므로 `false`를 반환합니다.  

출처: https://www.codingfactory.net/10903