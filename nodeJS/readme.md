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
서버에서 정적 파일을 다루기 위해선, express.static() 메소드를 사용하면 됩니다.  
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


--- 

# EJS - Express 프레임 워크 응용하기

## 0. 디렉토리 구조
```
express_tutorial/
├── data
│   └── user.json
├── node_modules
├── package.json
├── public
│   └── css
│       └── style.css
├── router
│   └── main.js
├── server.js
└── views
    ├── body.ejs
    ├── header.ejs
    └── index.ejs
```

이번 강좌에선 `data/user.json` 이 추가되었고 `view/ 내부 파일들`이 변경되었습니다.  

## 1. 의존 모듈 추가
저번 강좌에서는 그저 페이지 라우팅만 다뤘지만, 강좌 10편에서는 EJS 엔진과 추가적으로 RESTful API, 그리고 세션을 다룰 것이므로 넣어줘야 할 의존 모듈들이 있습니다.  
- **body-parser** - POST 데이터 처리
- **express-session** - 세션 관리 

우선 전 강좌에서 작성했던 pakage.json 을 업데이트 합니다.  

``` javascript
{
  "name": "express-tutorial",
  "version": "1.0.0",
  "dependencies":
  {
    "express": "~4.13.1",
    "ejs": "~2.4.1"    ,
    "body-parser": "~1.14.2",
    "express-session": "~1.13.0"
  }
}
```

그 후 다음 명령어를 입력해 모듈을 설치합니다.  

```
$ npm install
```

추가한 모듈들을 server.js 에서 불러오겠습니다.  

``` javascript
var express = require('express');
var app = express();
var bodyParser = require('body-parser');
var session = require('express-session');
var fs = require("fs")

app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.engine('html', require('ejs').renderFile);


var server = app.listen(3000, function(){
 console.log("Express server has started on port 3000")
});

app.use(express.static('public'));

app.use(bodyParser.json());
app.use(bodyParser.urlencoded());
app.use(session({
 secret: '@#@$MYSIGN#@$#$',
 resave: false,
 saveUninitialized: true
}));


var router = require('./router/main')(app, fs);
// ... 생략
```

Express 의 이전 버전에서는 `cookie-parser` 모듈도 불러와야 했지만, 이젠 `express-session` 모듈이 직접 쿠리에 접근하므로 `cookie-parser` 를 더이상 사용할 필요가 없습니다.  

추가적으로 `Node.js` 에 내장되어 있는 `fs`모듈도 불러왔는데, 이는 나중에 파일을 열기 위함입니다. 그리고 원래 상단에 있던 `Router` 코드를 아래로 내려주세요. (Line 27) 이 코드가 `bodyParser` 설정 아래 부분에 있다면 제대로 작동하지 않습니다. 그리고 Router에서 `fs` 모듈을 사용할 수 있도록 인자로 추가해 줍니다.  
router/main.js 에서 첫번 째 줄도 업데이트 해주세요.

``` javascript
module.exports = function(app, fs)
// ... 생략
```

section 부분에서의 값에 대해서 알아보겠습니다.  
- **secret** - 쿠키를 임의로 변조하는 것을 방지하기 위한 sign 값 입니다. 원하는 값을 넣으면 됩니다.  
- **resave** - 세션을 언제나 저장할 지(변경하지 않아도) 정하는 값입니다. `express-session documentation` 에서는 이 값을 `false`로 하는 것을 권장하고 필요에 따라 `true`로 설정합니다.  
- **saveUninitialized** - uninitialized 세션이란 새로 생겼지만 변경되지 않은 세션을 의미합니다. Documentation에서 이 값을 true로 설정하는 것을 권장합니다.  


## 2. EJS 템플릿 엔진
템플릿 엔진이란, 템플릿을 읽어 엔진의 문법과 설정에 따라서 파일을 HTML형식으로 변환시키는 모듈입니다. Express에서 사용하는 인기있는 `Jade 템플릿 엔진`은 기존의 HTML에 비해 작성법이 완전히 다른데, 그에 비해 `EJS`는 똑같은 HTML에서 `<% %>`를 사용하여 서버의 데이터를 사용하거나 코드를 실행 할 수 있습니다.  

EJS에서는 두가지만 알면 됩니다.  
1. <% 자바스크립트 코드 %>  
2. <% 출력 할 자바스크립트 객체 %>  

2번에서는 Javascript 객체를 router에서 받아올 수도 있습니다.  

### VIEW로 데이터 넘기기
우선, 전 강좌에서 작성(맨위에 작성)하였던 `views/index.html`과 `views/about.html`을 삭제하고, `router/main.js`를 다음과 같이 수정하세요.  

``` javascript
module.exports = function(app, fs)
{
     app.get('/',function(req,res){
         res.render('index', {
             title: "MY HOMEPAGE",
             length: 5
         })
     });
}
```

JSON 데이터를 render 메소드의 두번째 인자로 전달함으로서 페이지에서 데이터를 사용가능하게 합니다.  


### VIEW에서 데이터 접근 및 루프코드 실행

이제 `views/index.ejs`를 다음과 같이 만들어 주세요.  

``` html
<html>
  <head>
  <title><%= title %></title>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    <h1>Loop it!</h1>
    <ul>
        <% for(var i=0; i<length; i++){ %>
            <li>
                <%= "LOOP" + i %>
            </li>
        <% } %>
    </ul>
  </body>
</html>
```
**Line 3**: 라우터에서 title 받아와서 출력합니다.  
**Line 9~13**: 루프문입니다.  

### 출력

서버를 실행하고 http:/localhost:3000/ 에 접속해보세요.  

```
$ node server.js
```


<img width="400" src="https://i.imgur.com/UYkHo0Z.png">  

성공했나요? 이제 view 코드를 여러 파일로 분리해 봅시다.  

### EJS 분할하기

PHP나 Rails에서 처럼, EJS에서도 코드를 여러 파일로 분리하고 불러와서 사용 할 수 있답니다.  
파일을 불러올땐 다음 코드를 사용합니다.  

```
<% include FILENAME %>
```

`index.ejs` 파일의 head와 body를 따로 파일로 저장해서 불러와보겠습니다.  

header.ejs 파일과 body.ejs 파일:

``` ejs
<title>
     <%= title %>
 </title>
 <link rel="stylesheet" type="text/css" href="css/style.css">
 <script>
    console.log("HelloWorld");
 </script>
 
 // header.ejs
```
``` ejs
<h1>Loop it!</h1>
<ul>
    <% for(var i=0; i<length; i++){ %>
        <li>
            <%= "LOOP" + i %>
        </li>
    <% } %>
</ul>

// body.ejs
```
이렇게 파일이 준비됐다면, `index.ejs `를 다음과 같이 수정하면 됩니다.  

``` ejs
<html>
  <head>
    <% include ./header.ejs %>
  </head>
  <body>
    <% include ./body.ejs %>
  </body>
</html>
// index.ejs
```

출처: https://velopert.com/379  

---

# RESTful API - Express 프레임워크 응용하기

## 3. RESTful API

REST는 Representational State Transfer의 약자로서, 월드 와이드 웹(www)와 같은 하이퍼 미디어 시스템을 위한 소프트웨어 아키텍쳐 중 하나의 형식입니다. REST 서버는 클라이언트로 하여금 HTTP 프로토콜을 사용해 서버의 정보에 접근 및 변경을 가능케 합니다. 여기서 정보는 test, xml, josn 등의 형식으로 제공되는데, 요즘 트렌드는 json이다.

https://trends.google.com/trends/explore?date=all&q=json%20api,xml%20api  

### HTTP 메소드
HTTP/1.1 에서 제공되는 메소드는 여러개가 있는데, REST 기반 아키텍쳐에서 자주 사용되는 4가지 메소드는 다음과 같습니다.   
- **GET**: 조회
- **PUT**: 생성 및 업데이트
- **DELETE**: 제거
- **POST**: 생성

여기서 POST와 PUT이 둘다 생성이라 헷갈린다면 [PUT vs POST, REST API (1ambda Blog)](https://1ambda.github.io/javascripts/rest-api-put-vs-post/) 이 링크를 확인하세요.  

### 데이터 베이스 생성
JSON 기반의 사용자 데이터 베이스를 만들어보겠습니다.  
Node.js 와 궁함이 잘 맞는 MongoDB를 사용하면 좋지만 여기서는 Express를 사용합니다.  
> **MongoDB 연동하여 RESTful API 만들기**
>  
> [[Node.JS] 강좌 11편: Express와 Mongoose를 통해 MongoDB와 연동하여 RESTful API 만들기](https://velopert.com/594)  

`data` 폴더를 만들고 그 안에 `user.json` 파일을 생성해주세요.  

``` json
{
    "first_user": {
        "password": "first_pass",
        "name": "abet"
    },
    "second_user":{
        "password": "second_pass",
        "name": "betty"
    }
}
```

> 보안을 생각한다면 패스워드는 Encrypt를 하거나 Hash를 쓰는 것이 좋습니다.  
> 예제니까 PASS!

### 첫번째 API: `GET`/list
모든 유저 리스트를 출력하는 GET API를 작성해보겠습니다.   
우선, `user.json`파일을 읽어야 하므로 `fs` 모듈을 사용합니다.  
다음 코드를 `router/main.js`에 추가 작성해주세요.  
``` javascript
module.exports = function (app, fs) {

   app.get('/', function (req, res) {
      res.render('index', {
         title: "MY HOMEPAGE",
         length: 5
      })
   });

   app.get('/list', function (req, res) {
      fs.readFile(__dirname + "/../data/" + "user.json", 'utf8', function (err, data) {
         console.log(data);
         res.end(data);
      });
   })
}
```
`__dirname`은 현재 모듈의 위치를 나타냅니다.  
`router` 모듈은 `router` 폴더에 들어있으니 `data` 폴더에 접근하려면 `/../`를 앞 부분에 입력해서 상위 폴더로 접근해야 합니다.  

서버를 실행해서
```
$ node server.js
```
 http://localhost:3000/list 에 접속해보세요.   
 
> <img width="400" src="https://i.imgur.com/wUmW28S.png">  

### 두번째 API: `GET`/getUser/:username
이번엔 특정 유저 uername의 디테일한 정보를 가져오는 GET API를 작성해보도록 하겠습니다.  

다음 코드를 `router/main.js`의 list API 아래에 작성해주세요.   

``` javascript
   app.get('/getUser/:username', function (req, res) {
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         res.json(users[req.params.username]);
      });
   });
```

파일을 읽은 후, 유저 아이디를 찾아서 출력해줍니다.  
유저를 찾으면 유저 데이터를 출력하고 유저가 없다면 `{}`를 출력하게 됩니다.  

`fs.reaFile()`로 파일을 읽었을 때는 텍스트 형태로 읽어지기 때문에 `JSON.parse()` 를 해야합니다.   

서버를 다시 실행 후  
>`ctrl`+C 하고나서(터미널 종료 후 재실행)
>```
>$ node server.js
>```

http://localhost:3000/getUser/first_user 에 접속해보세요.    
> <img width="400"  src="https://i.imgur.com/dD35Oap.png">  

### 세번째 API: `POST` addUser/:username body: { “password”: “_____”, “name”: “_____” }

이번 API는 첫번째, 두번째와 다르게 POST 메소드를 사용합니다.   

다음 코드를 `router/main.js`의 `getUser API` 하단에 작성해주세요.  

``` javascript
app.post('/addUser/:username', function (req, res) {

      var result = {};
      var username = req.params.username;

      // CHECK REQ VALIDITY
      if (!req.body["password"] || !req.body["name"]) {
         result["success"] = 0;
         result["error"] = "invalid request";
         res.json(result);
         return;
      }

      // LOAD DATA & CHECK DUPLICATION
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         if (users[username]) {
            // DUPLICATION FOUND
            result["success"] = 0;
            result["error"] = "duplicate";
            res.json(result);
            return;
         }

         // ADD TO DATA
         users[username] = req.body;

         // SAVE DATA
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result = {
                  "success": 1
               };
               res.json(result);
            })
      })
   });
```

JSON 형태가 INVALID 하다면 오류를 반환하고, VALID 하다면 파일을 열어서 username의 중복성을 확인 후 JSON 데이터에 추가하여 다시 저장합니다.  
34번줄에서 `stringify(users, null, 2)` 은 `JSON` 의 pretty-print 를 위함 입니다.  

### 네번째 `API`: PUT updateUser/:username body: { “password”: “_____”, “name”: “_____” }

이 API는 위 API와 비슷합니다. 사용자 정보를 업데이트 하는 API 이고, PUT 메소드를 사용합니다.  

PUT API 는 idempotent 해야 합니다, 쉽게말하자면 즉 요청을 몇번 수행하더라도, 같은 결과를 보장해야합니다.  

``` javascript
   app.put('/updateUser/:username', function (req, res) {

      var result = {};
      var username = req.params.username;

      // CHECK REQ VALIDITY
      if (!req.body["password"] || !req.body["name"]) {
         result["success"] = 0;
         result["error"] = "invalid request";
         res.json(result);
         return;
      }

      // LOAD DATA
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         // ADD/MODIFY DATA
         users[username] = req.body;

         // SAVE DATA
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result = {
                  "success": 1
               };
               res.json(result);
            })
      })
   });
```

### 마지막 API: `DELETE` deleteUser/:username

유저를 데이터에서 삭제하는 API 입니다. `DELETE` 메소드를 사용합니다.  
네번째 API 아래에 다음 코드를 작성해주세요.
``` javascript
   app.delete('/deleteUser/:username', function (req, res) {
      var result = {};
      //LOAD DATA
      fs.readFile(__dirname + "/../data/user.json", "utf8", function (err, data) {
         var users = JSON.parse(data);

         // IF NOT FOUND
         if (!users[req.params.username]) {
            result["success"] = 0;
            result["error"] = "not found";
            res.json(result);
            return;
         }

         // DELETE FROM DATA
         delete users[req.params.username];

         // SAVE FILE
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result["success"] = 1;
               res.json(result);
               return;
            })
      })

   });
```

### router/main.js 전체 코드

``` javascript
module.exports = function (app, fs) {

   app.get('/', function (req, res) {
      res.render('index', {
         title: "MY HOMEPAGE",
         length: 5
      })
   });

   app.get('/list', function (req, res) {
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         console.log(data);
         res.end(data);
      });
   });

   app.get('/getUser/:username', function (req, res) {
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         res.json(users[req.params.username]);
      });
   });

   app.post('/addUser/:username', function (req, res) {

      var result = {};
      var username = req.params.username;

      // CHECK REQ VALIDITY
      if (!req.body["password"] || !req.body["name"]) {
         result["success"] = 0;
         result["error"] = "invalid request";
         res.json(result);
         return;
      }

      // LOAD DATA & CHECK DUPLICATION
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         if (users[username]) {
            // DUPLICATION FOUND
            result["success"] = 0;
            result["error"] = "duplicate";
            res.json(result);
            return;
         }

         // ADD TO DATA
         users[username] = req.body;

         // SAVE DATA
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result = {
                  "success": 1
               };
               res.json(result);
            })
      })
   });

   app.put('/updateUser/:username', function (req, res) {

      var result = {};
      var username = req.params.username;

      // CHECK REQ VALIDITY
      if (!req.body["password"] || !req.body["name"]) {
         result["success"] = 0;
         result["error"] = "invalid request";
         res.json(result);
         return;
      }

      // LOAD DATA
      fs.readFile(__dirname + "/../data/user.json", 'utf8', function (err, data) {
         var users = JSON.parse(data);
         // ADD/MODIFY DATA
         users[username] = req.body;

         // SAVE DATA
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result = {
                  "success": 1
               };
               res.json(result);
            })
      })
   });

   app.delete('/deleteUser/:username', function (req, res) {
      var result = {};
      //LOAD DATA
      fs.readFile(__dirname + "/../data/user.json", "utf8", function (err, data) {
         var users = JSON.parse(data);

         // IF NOT FOUND
         if (!users[req.params.username]) {
            result["success"] = 0;
            result["error"] = "not found";
            res.json(result);
            return;
         }

         // DELETE FROM DATA
         delete users[req.params.username];

         // SAVE FILE
         fs.writeFile(__dirname + "/../data/user.json",
            JSON.stringify(users, null, '\t'), "utf8",
            function (err, data) {
               result["success"] = 1;
               res.json(result);
               return;
            })
      })

   });

}
```

출처: https://velopert.com/332

---




