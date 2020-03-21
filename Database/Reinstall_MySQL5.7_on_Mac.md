# Mac에서 mySQL@5.7 재설치

## mySQL 삭제하기

나의 경우 멋 모르고 아래의 두가지 경우를 모두 사용하였기에..(심지어 이전에 하위 버전, 최신 버전도 깔아놓음) 모두 삭제해주었다. 좋은 경험이였다..

### brew install mysql로 설치했을 경우

```
$ sudo rm -rf /usr/local/var/mysql
```

```
$ sudo rm -rf /usr/local/bin/mysql*
```

```
$ sudo rm -rf /usr/local/Cellar/mysql
```

### mySQL 홈페이지에서 DMG파일로 설치했을 경우

```
$ sudo rm -rf /usr/local/mysql*
```

```
$ sudo rm -rf /Library/PreferencePanes/My*
```

```
$ sudo rm -rf /var/db/receipts/com.mysql.*
```

> 주의 사항: 파일을 삭제한다고 해도 이미 실행중인 프로그램은 종료되지 않으므로 파일 삭제 후, **재부팅**해줄 것!

삭제가 완료되었다면 mySQL을 설치해보자!!

## mySQL 5.7 설치하기

### 1. [mySQL 사이트](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)에 들어가서 파일을 다운로드 받는다.

> 링크: https://dev.mysql.com/downloads/mysql/5.7.html#downloads

![](https://images.velog.io/images/wow/post/19810a5c-ccc6-4109-a628-3ec73036354f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.19.42.png)

![](https://images.velog.io/images/wow/post/0e3163b9-6e44-4fec-97fc-efa709190918/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.23.55.png)

![](https://images.velog.io/images/wow/post/2fb9a661-3dd5-4b52-abf4-bfd66bd421bb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.25.56.png)

아이콘을 클릭하여 설치한다.

![](https://images.velog.io/images/wow/post/2e1f8731-4629-4c86-b000-e4c083d1b773/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.27.00.png)

`계속` 클릭

![](https://images.velog.io/images/wow/post/4b07db21-4542-4578-8eb5-cea407e49ceb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.27.27.png)

`계속` 클릭

![](https://images.velog.io/images/wow/post/2f009435-fc8b-4f9a-bbd2-6d16f24cc38b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.28.01.png)

`동의` 클릭

![](https://images.velog.io/images/wow/post/5471c95e-3a71-4ee6-b9d1-32d3ac24c51a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.28.55.png)

`설치` 클릭

![](https://images.velog.io/images/wow/post/a487c356-806a-4e96-8fc0-c4eab31397b7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.29.19.png)

`Mac에 저장된 비밀번호` 입력 후, `소프트웨어 설치` 클릭

![](https://images.velog.io/images/wow/post/79baf422-087d-438f-9f38-e92a793f1d76/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.29.59.png)

`확인` 클릭 후, `계속` 클릭

![](https://images.velog.io/images/wow/post/d060e577-d8bd-4c74-ba7a-587965d43d46/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.31.24.png)

데이터베이스는 보안 기능이 있다. 아무나 볼 수 없는 강력한 기능!  
보려면 비밀번호를 입력해야한다. 루트 사용자의 기본 비밀번호를 알려주는 것이다.

상단에 표시된 **비밀번호**를 카피하여 메모장 같은 곳에 잘 적어둔 후, `OK` 클릭

### 시스템 환경 설정에서 mySQL 실행시키기

![](https://images.velog.io/images/wow/post/64cf1735-4851-40b3-8971-a7cc899a52b2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.41.12.png)

`시스템 환경 설정`에 들어간다.

![](https://images.velog.io/images/wow/post/1aa1b715-afcb-4f70-9f3a-414d39b9278a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.41.52.png)

하단의 `MySQL` 클릭

![](https://images.velog.io/images/wow/post/66d89333-a926-4006-a715-cbd64b5db319/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%204.44.06.png)

`Stop MySQL Server`와 `Automatically Start MySQL Server on Startup`을 클릭하여 MySQL을 실행(running)시킨다.

### 3. 터미널에서 mySQL 실행하기

```
$ cd /usr/local/mysql/bin
```

디렉토리 이동

```
$ bin ./mysql -uroot -p
```

- **bin ./mysql**: 난 지금부터 mysql을 쓰고 싶어라고 컴퓨터에게 알림
- **-u**: user를 의미
- **root**: root라는 아이디를 가진 사용자임을 의미
- **-p**: password. 엔터치고 아까 카피해놓은 비밀번호를 입력한다.

나는 애석하게도 위의 두가지 명령어가 안먹혀서

```
$ cd
```

맨 하위 디렉토리로 돌아온 후,

```
$ /usr/local/mysql/bin/mysql -uroot -p
```

이 명령어를 사용하고, 비밀번호 입력하여 실행시켰다.

![](https://images.velog.io/images/wow/post/2a0786c4-e2b4-43dc-8636-c6eca84ba4b9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-22%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%205.00.08.png)

실행 완료!

> 참고 사이트:
>
> 1. [Mac에서 Mysql 삭제하기](https://ldgeao99.wordpress.com/2017/01/19/mac%EC%97%90%EC%84%9C-mysql-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0/)
> 2. [생활코딩 - MySQL 설치](https://opentutorials.org/course/195/1589)
> 3. [osx - brew 로 mysql 5.7 설치](https://junho85.pe.kr/1018)
