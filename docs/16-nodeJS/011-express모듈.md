---
layout: default
title: express 모듈
parent: nodeJS
nav_order: 11
---

이번에는 express 모듈로 웹서버를 생성하는 방법을 배워 보도록 하겠습니다.

```js
var express = require('express');

var app = express();

//require 이벤트 리스너를 등록합니다.
//express모듈로 서버를 생성하면 res,와 req객체에 다양한 기능이 추가됩니다.
app.use(function (req,res) {
    res.writeHead(200,{'Content-Type':'text/html'});
    res.end('<h1>Hello express!!</h1>');
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

웹 서버가 잘 실행되는 것을 알 수 있습니다.

<img width="478" alt="스크린샷 2020-09-26 오후 1 26 37" src="https://user-images.githubusercontent.com/16849874/94330104-ec9fef00-fffb-11ea-83b5-a2c4fd0bece6.png">

이번에는 express의 특별한 기능인 response에서의 send에 대해서 알아보겠습니다.

```js
var express = require('express');
var app = express();

//require 이벤트 리스너를 등록합니다.
app.use(function (req,res) {
    var output = [];
    for (let i = 0; i < 3; i++) {
        output.push({
            count:i,
            name:'name -'+i
        });
    }
    res.send(output);
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

위와 같이 배열을 전달하면 자동으로 json의 방식으로 웹상에 뿌려줍니다.

<img width="567" alt="스크린샷 2020-09-27 오전 11 20 26" src="https://user-images.githubusercontent.com/16849874/94354177-84144900-00b3-11eb-8f31-cdbd9939e075.png">

또한 이러한 send 메서드 앞에 status()메서드를 이용하면 상태코드도 전달을 할 수 있습니다.

```js
var express = require('express');

var app = express();

app.use(function (req,res,next) {
    //응답합니다.
    res.status(404).send("<h1>Sorry Error.</h1>");
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

상태 코드를 잘 전달한것을 알 수 있습니다.

<img width="632" alt="스크린샷 2020-09-27 오전 11 28 33" src="https://user-images.githubusercontent.com/16849874/94354264-95118a00-00b4-11eb-9852-04a50ba4161c.png">

이번에는 express에서의 req의 특별한 기능에 관해서 알아 보겠습니다.

req는 header 메서드를 사용하면 손쉽게 요청 헤더의 속성을 지정하거나 추출할 수 있습니다.

다음은 요청이 왔을때 클라이언트의 요청 헤더에서 User-Agent를 추출하는 방법을 소개합니다.

```js
var express = require('express');

var app = express();

app.use(function (req,res) {
    //User-Agent 속성을 추출합니다.
    var agent = req.header('User-Agent');
    console.log(req.headers);
    console.log(agent);

    //응답합니다.
    //상태 코드만 보면 sendStatus() 메서드를 사용합니다.
    res.sendStatus(200);
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

<img width="909" alt="스크린샷 2020-09-27 오전 11 33 52" src="https://user-images.githubusercontent.com/16849874/94354375-53cdaa00-00b5-11eb-983f-a9644e766250.png">

이를 이용하면 웹 브라우저에 따라서 다른 처리를 할 수 있습니다.

```js
var express = require('express');

var app = express();

app.use(function (req,res) {
    //User-Agent 속성을 추출합니다.
    var agent = req.header('User-Agent');
    console.log(agent);

    if (agent.toLowerCase().match(/chrome/)) {
        //페이지를 출력합니다.
        res.send("<h1>Hello Chrome</h1>");
    }else{
        res.send("<h1>Hello Express</h1>");        
    }
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

보면 크롬 브라우저일때만 다른 처리를 하는 것을 알 수 있습니다.

<img width="402" alt="스크린샷 2020-09-27 오전 11 39 06" src="https://user-images.githubusercontent.com/16849874/94354444-10c00680-00b6-11eb-8ad1-3587f41d036f.png">

또한 query속성을 사용하면 요청 매개변수를 쉽게 추출할 수 있습니다.

```js
var express = require('express');

var app = express();

app.use(function (req,res) {
    //변수를 선언합니다.
    var name = req.query.name;
    var region = req.query.region;

    res.send('<h1>'+name+' - '+region+'</h1>');
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

그후 이번에는 매개변수를 입력해서 사이트에 접속을 하면 이를 추출해서 다시 보여줍니다.

<img width="498" alt="스크린샷 2020-09-27 오전 11 44 41" src="https://user-images.githubusercontent.com/16849874/94354528-d73bcb00-00b6-11eb-99c9-7e0388c225b9.png">

이번에는 express의 미들웨어 관해서 알아보겠습니다.

```js
var express = require('express');
var app = express();

//미들웨어 설정1
app.use(function (req,res,next) {
    console.log('첫번째 미들웨어');
    next();
});

//미들웨어 설정2
app.use(function (req,res,next) {
    console.log('두번째 미들웨어');
    next();
});

//미들웨어 설정3
app.use(function (req,res,next) {
    console.log('세번째 미들웨어');
   
    //응답합니다.
    res.writeHead(200,{'Content-Type':'text/html'});
    res.end('<h1>express basic</h1>');
});

app.listen(52273,function () {
    console.log("서버가 실행되었습니다.");
});
```

이처럼 우리는 요청의 응답을 처리하기 전에 중간중간 여러 일을 처리할 수 있습니다.

<img width="673" alt="스크린샷 2020-09-27 오전 11 50 28" src="https://user-images.githubusercontent.com/16849874/94354604-a5773400-00b7-11eb-9e95-d62ea45460ef.png">

예시를 하나 들어보겠습니다.

```js
var express = require('express');

var app = express();

//미들웨어 설정1
app.use(function (req, res, next) {
    //데이터를 추가합니다.
    req.number = 52;
    res.number = 273;
    next();
});

//미들웨어 설정2
app.use(function (req, res, next) {
    res.send(req.number + ' - ' + res.number);
});

app.listen(52273, function () {
    console.log("서버가 실행되었습니다.");
});
```

이렇게 우리는 역할을 구분해서 프로그램의 구조를 만들 수 있습니다.

<img width="312" alt="스크린샷 2020-09-27 오전 11 54 26" src="https://user-images.githubusercontent.com/16849874/94354641-32ba8880-00b8-11eb-9121-e801992d57c8.png">

이제부터 우리는 이러한 express의 다양한 미들웨어를 알아보고자 합니다.

---

## router 미들웨어

페이지 라우팅은 클라이언트 요청에 적절한 페이지를 제공하는 기능입니다.

예제 코드를 보겠습니다.

```js
var express = require('express');

var app = express();

app.get('/a:id',function (req,res) {
    //변수를 선언합니다.
    var name = req.params.id;
    res.send('<a href="/b">Go to B'+name+'</a>')
});

app.get('/b',function (req,res) {
    res.send('<a href="/a:500">Go to A</a>')
});

app.listen(52273, function () {
    console.log("서버가 실행되었습니다.");
});
```

이러면 페이지별 다양한 기능을 제공해 줄 수 있습니다.

또한 /:id와 같은 방법으로 param을 추가해서 값을 첨부해서 보낼 수 있습니다.

query와는 방법이 살짝 다르니 주의해주시기 바랍니다.

<img width="405" alt="스크린샷 2020-09-27 오후 12 04 31" src="https://user-images.githubusercontent.com/16849874/94354778-9b563500-00b9-11eb-9a75-61307bf99d74.png">

---

## express의 전체 선택자

이번에는 마치 switch문의 default와 같은 등록하지 않은 경로로 접근을 하는 경우 전체 선택자를 이용해서 처리를 할 수 있습니다.

아래 예제를 보면 등록하지 않은 라우터 페이지로 접근을 하는 경우 한곳으로 처리를 하는 것을 알 수 있습니다.

```js
var express = require('express');

var app = express();

app.get('/a:id',function (req,res) {
    //변수를 선언합니다.
    var name = req.params.id;
    res.send('<a href="/b">Go to B'+name+'</a>')
});

//express모듈은 라우터 메서드를 사용한 순서대로 요청을 확인하기 때문에 전체선택자를 사용하는
//라우터는 반드시 마지막에 위치해야 합니다.
app.all('*',function (req,res) {
    res.send("<h1>페이지를 찾을 수 없습니다.</h1>");
})

app.listen(52273, function () {
    console.log("서버가 실행되었습니다.");
});
```

<img width="405" alt="스크린샷 2020-09-27 오후 12 10 10" src="https://user-images.githubusercontent.com/16849874/94354879-65658080-00ba-11eb-880d-1c3c7ba7dcce.png">

이는 나중에 라우터의 모듈화로 페이지별 라우터마다 따로 각각의 파일에서 개발을 할 수 있습니다.

---

## static 미들웨어

웹 서버에서 손쉽게 파일을 제공할 수 있습니다.

이번에는 웹서버에 사진을 올리고 이를 이미지 파일로 보여주는 예제입니다.

먼저 public라는 폴더를 만들고 그 안에 이미지 파일을 넣어주면 됩니다.

```js
var express = require('express');

var app = express();

//미들웨어 서버를 설정합니다.
//static미들웨어를 사용하면 지정한 폴더에 있는 내용을 모두 웹 서버 루트 폴더에 올립니다.
app.use(express.static(__dirname+'/public'));
app.use(function (req,res) {
    res.writeHead(200,{'Content-Type':'text/html'});
    res.end('<img src="aa.jpg" width="100%" />')
});

//서버를 실행합니다.
app.listen(52273,function () {
    console.log("서버를 실행합니다..");
})
```

이미지를 잘 보여주는 것을 알 수 있습니다.

<img width="645" alt="스크린샷 2020-09-27 오후 12 25 45" src="https://user-images.githubusercontent.com/16849874/94355138-934bc480-00bc-11eb-9466-a1f51bca2c63.png">

---

## morgan 미들웨어

morgan미들웨어는 웹 요청이 들어왔을때 로그를 출력하는 미들웨어입니다.

외부 모듈이기 때문에 따로 설치를 해주어야 합니다.

    npm install morgan

이제 예제를 보겠습니다.

```js
var express = require('express');
var morgan = require('morgan');

var app = express();

//미들웨어를 설정합니다.
app.use(morgan('combined'));
app.use(function (req,res) {
    res.send('<h1>express Basic</h1>');
});

//서버를 실행합니다.
app.listen(52273,function () {
    console.log("서버를 실행합니다..");
})
```

아래의 콘솔 이미지와 같이 접속 로그가 나오는 것을 알 수 있습니다.

또한 combind 대신에 원하는 토큰을 조합해서 로그를 원하는 형태로 만들 수 있습니다.

<img width="924" alt="스크린샷 2020-09-27 오후 1 03 40" src="https://user-images.githubusercontent.com/16849874/94355764-dfe5ce80-00c1-11eb-9f7f-5046e2e9ebe8.png">

combind 대신에 :method + :date를 한 경우

<img width="408" alt="스크린샷 2020-09-27 오후 1 06 23" src="https://user-images.githubusercontent.com/16849874/94355801-40750b80-00c2-11eb-8dce-f885dd5470dd.png">