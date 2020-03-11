# 나만의 커밋 메세지 만들기
> 참고 
> 1. [Git-커밋 메시지 컨벤션](https://doublesprogramming.tistory.com/256)
> 2. [좋은 커밋 메세지를 작성하기 위한 커밋 템플릿 만들어보기](https://junwoo45.github.io/2020-02-06-commit_template/)  
> 3. [Git맞춤 - Git 설정하기](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)  


페어 프로그래밍 중 git commit message를 통일시키기 위함과 나의 지저분한 commit message를 보완하기 위해 위의 [1](https://doublesprogramming.tistory.com/256), [2](https://junwoo45.github.io/2020-02-06-commit_template/), [3](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)를 참고하여 규칙을 만들었습니다.  

## 1. Commit Message Structure

커밋 메시지는 아래와 같이 제목/본문/꼬리말으로 구성한다.  
꼬리말은 생략가능.  
``` commit
제목: type. 내용에 대한 요약

본문: 전체적인 내용

꼬리말
```

## 2. Commit Type
- feat: 새로운 기능 추가
- fix: 버그 수정
- docs: 문서 수정(markdown..)
- style: 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
- refactor: 코드 리펙토링
- test: 테스트 코드, 리펙토링 테스트 코드 추가
- chore: 빌드 업무 수정, 패키지 매니저 수정

## 3. Subject
- 제목은 50자를 넘기지 않고, 제목 첫 글자는 대문자로 작성한다.   
- 제목은 마침표를 붙이지 않는다.  
- 과거 시제를 사용하지 않고 명령어로 작성한다. 
  - "Fixed"(X), "Fix"(O)
  - "Added"(X), "Add"(O)

## 4. Body
- 부연 설명이 필요하거나 커밋의 이류를 설명할 경우 작성한다.  
- 72자를 넘기지 않고 제목과 구분되기 위해 한칸을 띄워 작성한다.  

## Footer
- 선택사항이기 때문에 모든 커밋에 꼬리말을 자성할 필요는 없다.
- Issue tracker id를 작성할 때 사용한다.  

## Example
``` commit
fix. Fixed bug with Y

- MainView.js: 메인에서 보여지는 것 버그 고쳤음.
- ...
- ...
```


### 참고:
``` commit
# <타입>: <제목>

##### 제목은 최대 50 글자까지만 입력 ############## -> |


# 본문은 위에 작성
######## 본문은 한 줄에 최대 72 글자까지만 입력 ########################### -> |

# 꼬릿말은 아래에 작성: ex) #이슈 번호

# --- COMMIT END ---
# <타입> 리스트
#   feat    : 기능 (새로운 기능)
#   fix     : 버그 (버그 수정)
#   refactor: 리팩토링
#   style   : 스타일 (코드 형식, 세미콜론 추가: 비즈니스 로직에 변경 없음)
#   docs    : 문서 (문서 추가, 수정, 삭제)
#   test    : 테스트 (테스트 코드 추가, 수정, 삭제: 비즈니스 로직에 변경 없음)
#   chore   : 기타 변경사항 (빌드 스크립트 수정 등)
# ------------------
#     제목 첫 글자를 대문자로
#     제목은 명령문으로
#     제목 끝에 마침표(.) 금지
#     제목과 본문을 한 줄 띄워 분리하기
#     본문은 "어떻게" 보다 "무엇을", "왜"를 설명한다.
#     본문에 여러줄의 메시지를 작성할 땐 "-"로 구분
# ------------------
```