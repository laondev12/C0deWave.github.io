---
layout: default
title: 간단한 노트 어플리케이션 만들기
parent: nodeJS
nav_order: 3
---

# 간단한 노트 어플리케이션 만들기

이제는 속성 공략이 아닌 한번 nodeJS를 정독하는 시간을 가져 보고자 한다.

먼저 node가 설치되어있는 폴더를 열고 node.basic.js를 만들고 다음과 같이 적어보자.

```
console.log("Hello World!!!");
```

그후 node node.basic.js를 눌러서 작동을 해본다.

![스크린샷 2020-09-08 오후 9 18 45](https://user-images.githubusercontent.com/16849874/92475763-e1d21580-f218-11ea-8ed2-1252b7c5cd2d.png)

다음과 같이 잘 작동이 되는 것을 볼 수 있다.

그 다음으로는 웹 서버를 만들어 본다.

```js
//모듈을 추출합니다.
//모듈을 추출합니다.
var http = require('http');

//웹 서버를 만들고 실행 합니다.
http.createServer(function (request,response){
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.end('<h1>Hello World...!</h1>');
}).listen(52273, function(){
        console.log('Server running at http://127.0.0.1:52273/');
});
~
```

그후 

    node node.server을 눌러서 서버를 실행해 봅니다.

그후 127.0.0.1:52273을 접속을 하면 웹 서버가 잘 나오는 것을 알 수 있습니다.

![스크린샷 2020-09-08 오후 9 24 16](https://user-images.githubusercontent.com/16849874/92476454-f236c000-f219-11ea-8af1-0f151c3678a7.png)
