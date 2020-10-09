---
layout: default
title: SocketIO모듈
parent: nodeJS
nav_order: 16
---

nodejs를 사용하는 대표적인 이유중 하나가 socket.io 가 있기 때문입니다.

먼저 모듈을 설치해야합니다.

    npm install socket.io

그후에는 socket.io.server.js 파일을 하나 만들어 줍니다.

```js
//모듈을 추출합니다.
var http = require('http');
var fs = require('fs');
var socketio = require('socket.io');

//웹서버를 생성합니다.
var server = http.createServer(function (req,res) {
    //Html파일을 가져와서 읽습니다.
    fs.readFile('HTMLPage.html',function (err,data) {
        res.writeHead(200,{'Content-Type':'text/html'});
        res.end(data);
    });
}).listen(9797,function () {
    console.log('서버시작');
});

//마지막 사용자의 id를 저장합니다.
var id = 0;

//소켓 서버를 생성 및 실행합니다.
var io = socketio.listen(server);
io.sockets.on('connection',function (socket) {
    //id를 설정합니다.
    id = socket.id;
    //사용자 정의 이벤트
    socket.on('rint',function (data) {

        //private 통신
        //가장 최근에 접속한 사용자에게 메세지를 전송합니다.
        //to 대신에 in을 이용해도 됩니다.
        io.sockets.to(id).emit('smart',data);
        //클라이언트가 전송한 데이터를 출력합니다.
        console.log('Client Send Data:',data);

        //클라이언트에 smart이벤트를 발생시킵니다.
        //public 통신입니다.
        //io.sockets.emit('smart',data);
    });

    socket.on('broadByClient',function (data) {
        //클라이언트가 전송한 데이터를 출력합니다.
        console.log('Client Send Data:',data);

        //클라이언트에 smart이벤트를 발생시킵니다.
        //public 통신입니다.
        socket.broadcast.emit('broadByServer',data);
    });

    // socket.on('rint',function (data) {
    //     //클라이언트가 전송한 데이터를 출력합니다.
    //     console.log('Client Send Data:',data);

    //     //클라이언트에 smart이벤트를 발생시킵니다.
    //     //public 통신입니다.
    //     io.sockets.emit('smart',data);
    // });
});
```

그후에 HtmlPage.html을 만들어 줍니다.

```html
<!DOCTYPE html>
<html>

<head>
    <script src="/socket.io/socket.io.js"></script>
    <script> window.onload = function () { 
        //클라이언트가 접속을 합니다.
        var socket = io.connect();

        //소켓 이벤트를 연결합니다.
        //서버에서 데이터를 받는 역할
        socket.on('smart',function (data) {
            alert(data);
        });
        socket.on('broadByServer',function (data) {
            alert(data);
        });

        //문서 객체 이벤트를 연결합니다.
        //서버로 데이터를 보내는 역할
        document.getElementById('button').onclick = function () {
            //변수를 선언합니다.
            var text = document.getElementById('text').value;

            //데이터를 전송합니다.
            socket.emit('rint',text);
        };
        //브로드 캐스트를 구현합니다.
        document.getElementById('button22').onclick = function () {
            //변수를 선언합니다.
            var text = document.getElementById('text22').value;

            //데이터를 전송합니다.
            socket.emit('broadByClient',text);
        };
    }; 
    </script>
</head>

<body>
    <input type="text" id="text"/>    
    <input type="button" id="button" value="echo"/>    

    <input type="text" id="text22"/>    
    <input type="button" id="button22" value="broadcast"/>    

</body>

</html>
```

아래의 스크립트는 사용자가 웹소켓에 접속을 하는 코드입니다.

디버그 모드를 하는 키는 다음과 같습니다.

    export DEBUG=socket.io*

그리고 나서 서버를 실행하면 사용자가 접속하면 개발자 모드이기 때문에 로그가 나타납니다.

![스크린샷 2020-10-08 오후 8 52 00](https://user-images.githubusercontent.com/16849874/95454971-1f58c880-09a8-11eb-922e-35fc5f76dbe5.png)

소켓통신의 종류는 3가지가 있습니다.

public -> 자신을 포함한 모든 클라이언트에 데이터를 전달합니다.

broadcast -> 자신을 제외한 모든 클라이언트에 데이터를 전달합니다.

private -> 특정 클라이언트에 데이터를 전달합니다.

---

## 방 만들기

socket.io 모듈은 방을 생성하는 기능을 포함하고 있습니다.

방을 생성할때는 socket.join을 이용합니다.

```js
//소켓 서버 이벤트를 엽니다.
io.sockets.on('connection',function (socket) {
    //방 이름을 저장할 변수
    var roomNamedd = null;
    console.log('접속');

    //join이벤트
    //사용자가 특정한 방에 접속을 하게 도웁니다.
    socket.on('join',function (data) {
        roomNamedd = data;
        socket.join(data);
        console.log('방에 접속했습니다.'+ roomNamedd);
        console.log('JOIN ROOM LIST', socket.rooms);
    });

    //message이벤트
    socket.on('message',function (data) {
        io.sockets.in(roomNamedd).emit('message','test');
        console.log('메세지 잔달'+roomNamedd);
        console.log('JOIN ROOM LIST', socket.rooms);
    });
});
```

위의 코드를 참조하면 될 것 같습니다.

---

## 웹 채팅 프로그램 구현하기

따로 방을 만들지 않고 이루어져 있습니다.

먼저 자바 스크립트 파일 입니다.

```js
var http = require('http');
var fs = require('fs');
var socketio = require('socket.io');

//웹 서버를 만듭니다.
var server = http.createServer(function (req,res) {
    //HTML파일을 읽어옵니다.
    fs.readFile('HTMLPage.html',function (err,data) {
        res.writeHead(200,{'Content-Type':'text/html'});
        res.end(data);
    });
}).listen(9797,function () {
    console.log('서버 시작');
});

//소켓 서버를 만듭니다.
var io = socketio.listen(server);
io.sockets.on('connection',function (socket) {
        //message이벤트
        socket.on('message',function (data) {
            //클라이언트에 message이벤트를 발생 시킵니다.
            io.sockets.emit('message',data);
        });
});
```

다음은 html파일 입니다.

```html
<!DOCTYPE html>
<html>

<head>
    <title>Web Chat</title>
    <script src="https://code.jquery.com/jquery-1.12.4.js"></script>
    <script src="/socket.io/socket.io.js"></script>
    <script>
        //HTML문서가 전부 준비가 되면
        $(document).ready(function () {
            //변수를 선언 합니다.
            var socket = io.connect();

            //이벤트를 연결합니다.
            socket.on('message',function (data) {
                //추가할 문자열을 만듭니다.
                var output = "";
                output += '<li>';
                output += '     <h3>' + data.name + '</h3>';
                output += '     <p>' + data.message + '</p>';
                output += '     <p>' + data.date + '</p>';
                output += '</li>';
                //문서 객체를 추가합니다.
                $(output).prependTo('#content');
            });

            //버튼을 클릭할 때
            $('button').click(function () {
                socket.emit('message',{
                    name : $('#name').val(),
                    message : $('#message').val(),
                    date : new Date().toUTCString()
                });
            });
        });
    </script>
</head>

<body>
    <h1>Socket.io Chat</h1>
    <p>Chat With Node.js</p>
    <hr /> <input id="name" /> <input id="message" /> <button>Button</button>
    <ul id="content"> </ul>
</body>

</html>
```

<img width="437" alt="스크린샷 2020-10-09 오후 9 26 51" src="https://user-images.githubusercontent.com/16849874/95582830-277f3980-0a76-11eb-9816-41adb60ed3cb.png">

정상적으로 작동하는 것을 알 수 있습니다.
