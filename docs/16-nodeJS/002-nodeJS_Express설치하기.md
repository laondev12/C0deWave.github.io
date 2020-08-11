---
layout: default
title: nodeJS_Express
parent: nodeJS
nav_order: 2
---

# nodeJS 웹서버 Express 설치하기

이번에는 nodeJS로 간단한 API를 만들기 위해서 nodeJS의 모듈중 하나인 Express를 설치할 것이다.

먼저 node가 설치된 경로를 가지는 터미널 창을 연다.

그 후 해당 커맨드를 입력한다.

```
npm install express
```

그러면 설치가 잘 진행이 되는 것을 알 수 있다.

<img width="961" alt="스크린샷 2020-08-11 오후 6 55 22" src="https://user-images.githubusercontent.com/16849874/89884234-38413980-dc04-11ea-9930-ea50c0cc97f1.png">

다음으로는 해당 폴더 안에 server.js 파일을 하나 만든다.

그 후 해당 내용을 입력한다.

```java
var http = require("http");

var express = require("express");

var handler = express();

handler.get("/",function(req,res){
    res.send("test");
});

var server = http.createServer(handler);
server.listen("5000");
```

다음으로 터미널에서 다음과 같이 명령을 해서 서버를 실행한다.

```
node server.js
```

<img width="942" alt="스크린샷 2020-08-11 오후 7 13 25" src="https://user-images.githubusercontent.com/16849874/89885903-bd2d5280-dc06-11ea-81ad-104a0aafd042.png">

그러면 localhost:5000 으로 접속하면 잘 작동하는 것을 볼 수 있습니다.

<img width="927" alt="스크린샷 2020-08-11 오후 7 29 32" src="https://user-images.githubusercontent.com/16849874/89887310-fd8dd000-dc08-11ea-909a-ce1b32fd4c87.png">
