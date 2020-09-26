---
layout: default
title: http 모듈
parent: nodeJS
nav_order: 7
---

이번에는 nodeJS를 이용해서 간단한 웹서버를 만들어 볼 생각이다.

먼저 웹서버를 만들고 이벤트를 연결해 보겠다.

```JS
//웹서버의 생성과 실행
//모듈을 추출합니다.
var http = require("http");

//웹 서버를 생성합니다.
//안에 함수를 넣어서 응답을 만듭니다.
var server = http.createServer(function (requset, response) {
    response.writeHead(200,{'Content-Type':'text/html'});
    response.end('<h1>Hello Web Server whid NodeJs</h1>');
});

//10초후 함수를 실행합니다.
// var closeServer = function() {
//     server.close();
// };
// setTimeout(test,100000);

//웹 서버에서 보다 중요한 것은 이벤트 입니다.
//클라이언트가 요청을 할때 이루어지는 이벤트
server.on('request',function (code) {
    console.log("Request On"); 
});
server.on('connection',function (code) {
    console.log("Request On");
});
server.on('close',function (code) {
    console.log("Request On");
});
//웹 서버를 실행합니다.
server.listen(52273,function () {
    console.log("서버를 실행합니다.");
});
```

이렇게 되면 해당 사이트에 접속을 하면 이벤트가 실행되게 된다.

vscode에서 control + option + N을 이용하면 쉽게 JS를 실행할 수 있다.

```js
//모듈을 추출합니다.
var fs = require('fs');
var http = require('http');

// //웹서버를 생성하고 실행합니다.
// http.createServer(function (request, response) {
//     //html파일을 읽습니다.
//     fs.readFile('HTMLPage.html',function (error,data) {
//         response.writeHead(200, {'Content-Type': 'text/html'});
//         response.end(data);
//     });    
// }).listen(52273, function () {
//     console.log("서버가 실행됩니다.");
// });

//52273번 포트에 서버를 생성하고 실행합니다.
http.createServer(function (request, response) {
    //이미지 파일을 읽습니다.
    fs.readFile("오늘의 정연.jpg",function (error,data) {
        response.writeHead(200,{'Content-Type': 'image/jpeg'});
        response.end(data);
    });
}).listen(52273,function(){
    console.log("52273번 포트에 서버를 실행합니다.");
});

//52274 번 포트에 서버를 생성하고 실행합니다.
http.createServer(function (request,response) {
    //오디오 파일을 읽습니다.
    fs.readFile("Four More Weeks - Vans in Japan.mp3",function (error,data) {
        response.writeHead(200,{'Content-Type': 'audio/mp3'});
        response.end(data);
    });
}).listen(52274,function () {
    console.log("52274번 포트에 서버를 실행합니다.")
});
```

이번에는 서버에서 오디오나 이미지를 전송해주는 프로그램이다.

이를 잘 이용하면 랜덤 이미지를 만들어주는 웹서버도 만들수 있을것 같다.

```js
//모듈을 추출합니다.
var http = require('http');

//서버를 생성하고 실행합니다.
http.createServer(function (request,response) {
    //변수를 선언합니다.
    var date = new Date();
    date.setDate(date.getDate()+7);
   //쿠키를 입력합니다.
   response.writeHead(200,{
        'Content-Type':'text/html',
        'Set-Cookie':[
            'breakfast = toast;Expires = '+date.toUTCString(),
            'dinner = chicken'
        ]
   });

   //쿠키를 출력합니다.
   response.end('<h1>'+request.headers.cookie +'</h1>');
}).listen(52273,function () {
    console.log("서버를 실행합니다.");
});
```

이번에는 쿠키를 이용해서 사용자의 정보를 저장하는 방법이다.

처음에 들어가면 아무것도 나오지 않지만 다음에 들어가면 저장된 쿠키가 나오는 것을 알 수 있다.

```js
//이번에는 웹페이지의 강제이동에 관해서 알아보도록 하겠습니다.
//모듈을 추출합니다.
var http = require('http');

//웹 서버를 생성 및 실행합니다.
http.createServer(function (request, response) {
    //302, 404와 같은 Status code로 페이지의 상태를 알릴 수 있습니다.
    response.writeHead(302,{'Location':'https://c0dewave.github.io/'});
    response.end();
}).listen(52273,function () {
    console.log("강제이동 서버를 생성합니다.");
});
```

이번에는 페이지를 강제이동 하는 방법이다.

---

## Request 객체

이번에는 request객체에 대해서 알아보도록 하겠다.

서버에서 받는 request객체에는 method, url, header, trallers, httpVersion의 속성을 가지고 있다.

이번에는 이를 이용해서 request의 url을 이용해서 받은경로를 가지고 각각의 다른 페이지를 반환하는 서버를 구현해 보았다.

먼저 2개의 html 파일을 준비한다.

index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Index</title>
    </head>
    <body>
        <h1>Hello Node.js .. Index</h1>
    </body>
</html>
```

OtherPage.html
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Other Page</title>
    </head>
    <body>
        <h1>Hello Node.js .. OtherPage</h1>
    </body>
</html>
```

app.js
```js
//모듈을 추출합니다.
var http = require('http');
var fs = require('fs');
var url = require('url');

//서버를 생성및 실행을 합니다.
http.createServer(function (request, response) {
    //변수를 선언합니다.
    var pathname = url.parse(request.url).pathname;

    //페이지를 구분합니다.
    if (pathname == '/') {
        //Index.html 파일을 읽습니다.
        fs.readFile('Index.html',function (error,data) {
            //응답합니다.
            response.writeHead(200,{'Content-Type':'text/html'});
            response.end(data);
        })
    } else if(pathname == '/OtherPage'){
        //OtherPage.html 파일을 읽습니다.
        fs.readFile('OtherPage.html',function (error,data) {
            //응답합니다.
            response.writeHead(200,{'Content-Type':'text/html'});
            response.end(data);
        })
    }
}).listen(52273, function () {
    console.log("서버가 실행 되었습니다.");
});
```

이렇게 하면 url에 따라서 다른 페이지를 반환할 수 있다.

<img width="509" alt="스크린샷 2020-09-25 오후 9 15 13" src="https://user-images.githubusercontent.com/16849874/94265915-4a88f400-ff74-11ea-9a24-071059712e5c.png">

<img width="456" alt="스크린샷 2020-09-25 오후 9 15 21" src="https://user-images.githubusercontent.com/16849874/94265921-4c52b780-ff74-11ea-9f31-0bfffa6d1238.png">


이번에는 method를 이용한 호출방식을 출력하는 코드를 짜보겠습니다.

```js
//모듈을 추출합니다.
var http = require('http');

//모듈을 사용합니다.
http.createServer(function (request,response) {
    if(request.method == 'GET'){
        console.log("GET 요청입니다.");
    }else if(request.method == 'POST'){
        console.log("POST요청입니다.");
    }
}).listen(52273,function () {
    console.log("서버가 실행 되었습니다.");
})
```

<img width="766" alt="스크린샷 2020-09-25 오후 9 29 37" src="https://user-images.githubusercontent.com/16849874/94267246-3940e700-ff76-11ea-9ccd-5fb853d9b219.png">

일반적인 웹사이트의 접속으로는 GET을 호출한다는 것을 알 수 있었습니다.

그러면 GET요청에서 매개변수를 추출하는 방법은 무엇일까요?

밑의 코드는 GET에서 매개변수를 추출한 다음 이를 JSON의 방식으로 추출하는 방법입니다.

```js
//모듈을 추출합니다.
var http = require('http');
var url = require('url');

//모듈을 사용합니다.
http.createServer(function (request,response) {
    //요청 매개변수를 추출합니다.
    var query = url.parse(request.url, true).query;

    //get요청 매개변수 추출
    response.writeHead(200,{'Content-Type' : 'text/html'});
    response.end('<h1>'+JSON.stringify(query)+'</h1>');
}).listen(52273,function () {
    console.log("서버가 실행 되었습니다.");
})
```

그후 접속url에 GET의 매개변수를 전달하면 됩니다.

http://localhost:52273/?name=rint&age=18

이번에는 POST요청을 하고 해당 매개변수를 추출하는 방법에 대해서 알아보도록 하겠습니다.

일반적인 html 요청은 GET만 하고 POST를 하지 않기 때문에 POST요청을 하는 html페이지를 하나 따로 만들어야 합니다.

post요청을 하는 html파일

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Node.js Example</title>
    </head>
    <body>
        <h1>Send Data With POST Method</h1>
        <form method="POST">
            <table>
                <tr>
                    <td><label>Data A</label></td>
                    <td><input type="text" name="data_a"></td>
                </tr>
                <tr>
                    <td><label>Data B</label></td>
                    <td><input type="text" name="data_b"></td>
                </tr>
            </table>
            <input type="submit"/>
        </form>
    </body>
</html>
```

post인수를 출력하는 js파일

```js
//모듈을 추출합니다.
var http = require('http');
var fs = require('fs');

//모듈을 사용합니다.
http.createServer(function (request,response) {

    if (request.method == "GET") {
        fs.readFile("request.post.html",function (error,data) {
            response.writeHead(200,{'Content-Type':'text/html'});
            response.end(data);
        })
    } else if(request.method == "POST") {
        request.on('data',function (data) {
            response.writeHead(200,{'Content-Type':'text/html'});
            response.end('<h1>'+data+'</h1>');
        })   
    }
}).listen(52273,function () {
    console.log("서버가 실행 되었습니다.");
})
```

<img width="554" alt="스크린샷 2020-09-25 오후 9 53 37" src="https://user-images.githubusercontent.com/16849874/94269395-92f6e080-ff79-11ea-90cf-5eb6777c8a39.png">

<img width="448" alt="스크린샷 2020-09-25 오후 9 53 47" src="https://user-images.githubusercontent.com/16849874/94269417-98ecc180-ff79-11ea-94e2-60036a0bde7e.png">

최종적으로 먼저 GET요청을 해서 값을 입력한 다음에 POST를 호출해서 보낸 인자를 받는 역할을 합니다.

이번에는 node를 이용해서 쿠키를 셋팅하고 이를 response로 반환하는 코드를 보도록 하겠습니다.

```js
//모듈을 추출합니다.
var http = require('http');

//서버를 생성하고 실행합니다.
http.createServer(function (request, response) {
    //쿠키를 가져옵니다.
    var cookie = request.headers.cookie;

    //쿠키를  설정합니다.
    response.writeHead(200, {
        'Content-Type': 'text/html',
        'Set-Cookie': ['name = RintIan', 'region = Seoul']
    });

    //응답합니다.
    response.end('<h1>'+JSON.stringify(cookie)+'</h1>');
    
}).listen(52273, function () {
    console.log("서버를 실행합니다.");
});
```

<img width="706" alt="스크린샷 2020-09-26 오전 10 40 49" src="https://user-images.githubusercontent.com/16849874/94327282-d0dd1e80-ffe4-11ea-8f1e-003930a3313e.png">

---

하지만 이 경우에는 문자열로 이루어져 있기 때문에 우리는 쿠키를 분해해서 배열로 생성을 하는 것이 좋습니다.

아래의 코드는 쿠키를 JSON으로 쪼개서 넣은 구문 입니다.

```js
//모듈을 추출합니다.
var http = require('http');

//서버를 생성하고 실행합니다.
http.createServer(function (request, response) {
    //쿠키가 있는지 확인합니다.
    if (request.headers.cookie) {
        //쿠키를 추추라고 분해합니다.
        var cookie = request.headers.cookie.split(';').map(function (element) {
            var element = element.split('=');
            return {
                key: element[0],
                value: element[1]
            };
        });
        response.writeHead(200,{'Content-Type':'text/html'})
        response.end('<h1>' + JSON.stringify(cookie) + '<h1>');
    } else {
        //쿠키를 생성합니다.
        response.writeHead(200, {
            'Content-Type': 'text/html',
            'Set-Cookie': ['name=주명', 'region=서울']
        });
        //응답합니다.
        response.end("<h1>쿠키를 생성했습니다.</h1>");
    }
}).listen(52273, function () {
    console.log("서버를 실행합니다.");
});
```

<img width="731" alt="스크린샷 2020-09-26 오전 10 52 44" src="https://user-images.githubusercontent.com/16849874/94327500-69c06980-ffe6-11ea-9d93-612b753d66cd.png">