# includes()

`.includes()`는 문자열이 특정 문자열을 포함하는지 확인하는 메서드입니다.  
IE는 Edge부터 지원합니다.  

## 문법(Syntax)
``` javascript
string.includes(searchString, length)
```
- searchString : 검색할 문자열로 필수 요소입니다. 대소문자를 구분합니다.
- length : 검색을 시작할 위치입니다. 선택 요소로, 값이 없으면 전체 문자열을 대상으로 합니다.

## 예제(Example)
``` javascript
'abzcd'.includes( 'z' );
```
`abzcd`가 `z`를 포함하는지 검사합니다. `z`를 포함하므로 `true`를 반환합니다.
``` javascript
'abzcd'.includes( 'z', 2 );
```
`zcd`가 `z`를 포함하는지 검사합니다. `z`를 포함하므로 `true`를 반환합니다.
``` javascript
'abZcd'.includes( 'z', 3 );
```
`abZcd`가 `z`를 포함하는지 검사합니다. 대소문자를 구분하므로 `false`를 반환합니다.  

출처: https://www.codingfactory.net/10899