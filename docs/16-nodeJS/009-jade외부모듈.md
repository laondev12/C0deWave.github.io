---
layout: default
title: jade외부모듈
parent: nodeJS
nav_order: 9
---

이번에는 jade모듈을 살펴보는 시간을 가져 보도록 하겠습니다.

먼저 jade.basic.js 파일과 JadePage.jade 파일을 만들어 봅니다.

jade.basic.js

jade는 함수를 이용해서 데이터를 호출한다는 점이 ejs와 다른 점입니다.

```js
var http = require('http');
var fs = require('fs');
var jade = require('jade');

//서버를 생성하고 실행합니다.
http.createServer(function (request,response) {
    //JadePage.jade파일을 읽습니다.
    fs.readFile('JadePage.jade','utf8',function (error,data) {
        //jade는 문자열을 HTML문자열로 바꿀수 있는 함수를 생성합니다.
        //jade모듈을 사용합니다.
        //이렇게 하면 jade페이지를 HTML페이지로 변환 할 수 있습니다.
        var fn = jade.compile(data);
        //출력을 합니다.
        response.writeHead(200,{'Content-Type':'text/html'});
        //함수의 형태로 사용합니다.
        response.end(fn())
    });
}).listen(52274,function () {
    console.log("서버를 실행합니다.");
})
```

JadePage.jade

jade파일은 아래와 같이 이용하고 들여쓰기가 중요합니다.

특히 띄어쓰기나 탭중 한가지로 통일을 해야합니다.

동시에 쓰면 오류가 발생합니다.

```js
html
    head
        title   Index Page
    body
        h1  Hello jade
        h2  JuMyepong
        hr
        a(href='https://c0dewave.github.io/')   Go To My Blog
```

정상적으로 잘 작동하는 것을 볼 수 있습니다.

<img width="385" alt="스크린샷 2020-09-26 오전 11 50 41" src="https://user-images.githubusercontent.com/16849874/94328555-82cd1880-ffee-11ea-9cd8-d83150ce62db.png">

아래와 같이 #을 이용하면 div id값을 추가할 수 있고 

.을 이용하면 class를 이용할 수 있습니다.

```js
doctype html
html
    head
        title   Index Page
    body
        #header
            h1  Hello jade
            h2  JuMyepong
        hr
        .article
            a(href='https://c0dewave.github.io/')   Go To My Blog
```

<img width="393" alt="스크린샷 2020-09-26 오전 11 58 16" src="https://user-images.githubusercontent.com/16849874/94328702-92992c80-ffef-11ea-89b1-6a6f1a9d7af5.png">

jade에서 데이터를 전달할 때에는 fn()안에 매개변수를 {}로 감싸서 전달해주면 됩니다.

예제를 실행해 보겠습니다.

```js
var http = require('http');
var fs = require('fs');
var jade = require('jade');

//서버를 생성하고 실행합니다.
http.createServer(function (request,response) {
    //JadePage.jade파일을 읽습니다.
    fs.readFile('JadePage.jade','utf8',function (error,data) {
        //jade는 문자열을 HTML문자열로 바꿀수 있는 함수를 생성합니다.
        //jade모듈을 사용합니다.
        //이렇게 하면 jade페이지를 HTML페이지로 변환 할 수 있습니다.
        var fn = jade.compile(data);
        //출력을 합니다.
        response.writeHead(200,{'Content-Type':'text/html'});
        //함수의 형태로 사용합니다.
        response.end(fn({
            name: 'Jumyeong',
            desc: 'Hello jade'
        }));
    });
}).listen(52274,function () {
    console.log("서버를 실행합니다.");
})
```

#을 이용한 데이터의 출력은 뒤에 문자열을 더할수 있고

=을 이용한 데이터의 출력에는 데이터만을 출력할 수 있습니다.

```jade
doctype html
html
    head
        title   Index Page
    body
        #header
            h1  #{name} .. !
            h2= desc
        hr
        -   for(var i = 0; i<10 ; i++ ){
                p
                    a(href="https://c0dewave.github.io")    My blog
        -   }
```

<img width="347" alt="스크린샷 2020-09-26 오후 12 05 47" src="https://user-images.githubusercontent.com/16849874/94328819-9ed1b980-fff0-11ea-915c-091f0034329f.png">