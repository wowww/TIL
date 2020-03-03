# endsWith()

`.endsWith()`는 문자열이 특정 문자열로 끝나는지 확인하는 메서드입니다.  
IE는 Edge부터 지원합니다.  

## 문법(Syntax)
``` javascript
string.endsWith(searchString, length)
```
- searchString : 검색할 문자열로 필수 요소입니다. 대소문자를 구분합니다.
- length : 문자열 중 어디까지 검색할지 정합니다. 선택 요소로, 값이 없으면 전체 문자열을 대상으로 합니다.

## 예제(Example)
``` javascript
'abcde'.endsWith('e');
```
`abcde`가 `e`로 끝나는지 검사합니다. `e`로 끝나므로 `true`를 반환합니다.
``` javascript
'abcde'.endsWith('a', 3);
```
`abc`가 `e`로 끝나는지 검사합니다. `e`로 끝나지 않으므로 `false`를 반환합니다.
``` javascript
'abcdE'.endsWith('de');
```
`abcdE`가 `de`로 끝나는지 검사합니다. 대소문자를 구분하므로 `false`를 반환합니다.  

출처: https://www.codingfactory.net/10897