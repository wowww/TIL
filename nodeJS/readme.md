# Express 프레임 워크 사용해보기

NodeJS의 웹 프레임 워크를 사용하면 간편하게 웹 서버를 구축할 수 있다.  
웹 프레임 워크 종류는 대표적으로 express, Koa, Hapi 등이 있다.  
**Express**를 사용해서 웹 서버를 구축해 보았습니다.  

## 1. 디렉토리 구조 이해하기

```
express_tutorial/
├── package.json
├── public
│   └── css
│   └── style.css
├── router
│   └── main.js
├── server.js
└── views
 ├── about.html
 └── index.html
```

디렉토리는 아래의 항목(2번부터)에 잘 설명되어 있습니다.  

## 2. package.json 파일 생성

`프로젝트의 이름`, `버전`, `의존 패키지 리스트` 등 정보들에 대한 정보를 담고 있는 파일입니다.  

``` json
{
  "name": "express-tutorial",
  "version": "1.0.0",
  "dependencies": 
  {
    "express": "~4.13.1",
    "ejs": "~2.4.1"    
  }
}
```

## 2.1 NPM으로 Dependency(의존 패키지) 설치

package.json을 생성했다면 다음 명령어로 의존 패키지들을 설치하세요.  

``` 
$ npm install
```

## 3. Express 서버 생성  

package.json 파일을 생성했고 의존 패키지들도 모두 설치했습니다.  
이제 서버를 만들 차례입니다.  
`server.js` 파일을 생성하고 아래의 코드를 입력하세요.  

``` javascript
var express = require('express');
var app = express();
var server = app.listen(3000, function(){
    console.log("Express server has started on port 3000")
});
```
아무것도 하지 않은 웹서버 입니다.  

```
$ node server.js
```
를 입력하면 포트 3000으로 웹서버를 열고, 페이지에 들어가면(localhost:3000) `Cannot Get /` 이라는 텍스트가 나타납니다.  
그 이유는 Router를 아직 정의하지 않아서 입니다.  

## 4. Router로 Request 처리하기  
현재 우리는 서버를 돌리기 위해 피요한 것을 모두 갖췄습니다.  
이제 브라우저에서 Request가 왔을 때 서버에서 어떤 작업을 할 지 Router를 통해 설정해줘야 합니다.  
간단한 Router를 작성해봅시다.  

``` javascript
app.get('/', function(req, res){
    res.send('Hello World');
});
```
이 코드를 추가해주고 server.js를 재실행하면 http://localhost:3000/ 으로 접속했을 때, Hello World를 반환합니다.  

이제 진짜 Router를 작성해볼 차례입니다.  

<img width="500" src="https://velopert.com/wp-content/uploads/2016/02/Untitled-10.png">  

라우터 코드와 서버 코드는 다른 파일에 작성하는 것이 좋은 코딩습관입니다.  
router라는 폴더를 만들고 그 안에 main.js를 생성해주세요.  

``` javascript
module.exports = function(app)
{
     app.get('/',function(req,res){
        res.render('index.html')
     });
     app.get('/about',function(req,res){
        res.render('about.html');
    });
}
```

파일을 저장하고 아직 코드를 실행하지는 마세요.  
module.exports는 우리가 Router 코드를 따로 작성했기에 server.js 에서 모듈로서 불러올 수 있도록 사용된답니다.  


## 5. HTML 페이지를 띄우기

HTML 페이지를 띄우기 위해서는 우선 html 파일이 있어야 합니다.  

views/ 디렉토리를 만들고 그 안에 index.html 과 about.html을 생성해주세요.  

``` html
<html>
  <head>
    <title>Main</title>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    Hey, this is index page
  </body>
</html>
/* index.html */
```

``` html
<html>
  <head>
    <title>About</title>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    About... what?
  </body>
</html>
/* about.html */
```

> style.css 는 아직 만들지 않았지만, 이 포스트의 아랫 부분에서 다루게 됩니다.  

그 후, 다시 server.js를 업데이트 해봅시다.  

``` javascript
var express = require('express');
var app = express();
var router = require('./router/main')(app);

app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.engine('html', require('ejs').renderFile);

var server = app.listen(3000, function(){
    console.log("Express server has started on port 3000")
});
```

**3번째 줄**: 라우터 모듈인 main.js를 불러와서 app에 전달해줍니다.  
**5번째 줄**: 서버가 읽을 수 있도록 HTML의 위치를 정의해줍니다.  
**6,7번째 줄**: 서버가 HTML 렌더링을 할 때, EJS 엔진을 사용하도록 설정합니다.  

## 정적 파일(Static files) 다루기  
정적 파일이란?  
HTML에서 사용되는 .js 파일, css 파일, image 파일 등을 가리킵니다.  
서버에서 정적 파일을 다루기 위해선, express.sratic() 메소드를 사용하면 됩니다.  
public/css 디렉토리를 만들고, 그 안에 style.css 파일을 생성해주세요.  

``` css
body{
  background-color: black;
  color: white;
}
```

그 후, server.js의 11번째 줄 아래에 해당 코드를 추가해주세요.  

``` javascript
app.use(express.static('public'));
```

이제 서버를 실행하고

```
$ node server.js
```
http://localhost:3000/ 에 접속했을 때 css 가 적용된 페이지가 나타나면 성공입니다.


출처: https://velopert.com/294
