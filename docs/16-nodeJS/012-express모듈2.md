---
layout: default
title: express 모듈2
parent: nodeJS
nav_order: 12
---

이전번에 배우던 express의 미들웨어에 대해서 좀더 알아보도록 하겠습니다.

---

## cookie parser 미들웨어

해당 미들웨어는 요청 쿠키를 추출하는 미들웨어 입니다.

역시 외부 미들웨어이기 때문에 따로 설치를 해주셔야 합니다.

    npm install cookie-parser

이제 이를 이용해서 쉽게 쿠키를 지정하고 해제하겠습니다.

```js
var express = require('express');
const cookieParser = require('cookie-parser');

var app = express();

//미들웨어를 생성합니다.
app.use(cookieParser());

//라우터를 설정합니다.
app.get('/setCookie',function (req,res) {
    res.cookie('string','cookie');
    res.cookie('json',{
        name: 'cookie',
        proper:'delicious'
    });
    res.send("OKOK");
});

app.get('/getCookie',function (req,res) {
    res.send(req.cookies);
});

app.listen(52273, function () {
    console.log("서버가 실행되었습니다.");
});
```

cookies를 이용해서 쿠키를 지정하고 해제를 할수 있습니다.

setCookies에 접속을 한 다음에 getCookies에 접속을 하면 쿠키가 지정이 된 것을 볼 수 있습니다.

![스크린샷 2020-10-05 오전 8 13 59](https://user-images.githubusercontent.com/16849874/95029372-b987ea80-06e2-11eb-8a58-cbdff4b04a24.png)

cookies의 3번째 속성에는 다양한 옵션을 지정할 수 있습니다.

---

## body parser 미들웨어

이는 Post요청을 추출하는 미들웨어입니다.

이 미들웨어를 이용하면 request객체에 body속성이 부여됩니다.

우선은 설치를 먼저 해 보도록 하겠습니다.

    npm install body-parser

위의 쿠키파서와 바디파서를 이용해서 로그인을 구현할 수 있습니다.

post요청을 하기 위해서는 먼저 입력 양식을 만들어야 합니다.

따라서 login.html을 먼저 만들어 보겠습니다.

```html
<!DOCTYPE html>
<html>

<head>
    <title>Login Page</title>
</head>

<body>
    <h1>Login Page</h1>
    <hr />
    <form method="post">
        <table>
            <tr>
                <td><label>Username</label></td>
                <td><input type="text" name="login" /></td>
            </tr>
            <tr>
                <td><label>Password</label></td>
                <td><input type="password" name="password" /></td>
            </tr>
        </table> <input type="submit" name="" />
    </form>
</body>

</html>
```

그후 app.js파일을 만듭니다.

```js
//모듈을 추출합니다.
var fs = require('fs');
var express = require('express');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

//서버를 생성합니다.
var app = express();

//미들웨어를 설정합니다.
app.use(cookieParser());
app.use(bodyParser.urlencoded({extended:false}));

//라우터를 지정합니다.
app.get('/',function (req,res) {
    if (req.cookies.auth) {
        res.send('<h1>Login Success</h1>');
    } else {
        res.redirect('/login');
    }
});
//get이면 html을 반환하고 post요청이면 auth쿠키를 생성합니다.
app.get('/login',function (req,res) {
    fs.readFile('login.html',function (err, data) {
        res.send(data.toString());
    });
});
app.post('/login',function (req, res) {
    //쿠키를 생성합니다.
    var login = req.body.login;
    var password = req.body.password;

    //출력합니다.
    console.log(login, password);
    console.log(req.body);

    //로그인을 확인 합니다.
    //아이디와 비밀번호를 rint와 1234로 합니다.
    if (login == 'rint' && password == '1234') {
        //로그인 성공
        res.cookie('auth',true);
        res.redirect('/');
    } else {
        //로그인 실패
        res.redirect('login');
    }
});

//서버를 실행합니다.
app.listen(52273, function () {
    console.log("서버가 시작 되었습니다.");
})
```

![스크린샷 2020-10-05 오전 8 46 29](https://user-images.githubusercontent.com/16849874/95029996-43d24d80-06e7-11eb-8f01-411b20f869fd.png)

지금은 하나의 계정이지만 로그인을 하고 쿠키를 만든 것을 볼 수 있습니다.

---

## connect-multiparty 미들웨어

body-parser는 multipart/form-data 인코딩 방식을 지원하지 않습니다.

이번에는 파일 업로드를 구현하면서 connect-multiparty미들웨어를 사용해 봅니다.

먼저 connect-multiparty를 설치해야 합니다.

    npm install connect-multiparty

그후 html 파일을 만들 때 form태그에 enctype를 multipart/form-data로 지정해 주어야 합니다.

이번에는 이미지를 업로드 하는 예제를 해보도록 하겠습니다.

```html
<!DOCTYPE html>
<html>

<head>
    <title>Multipart Upload</title>
</head>

<body>
    <h1>File Upload</h1>
    <form method="post" enctype="multipart/form-data">
        <table>
            <tr>
                <td>Comment: </td>
                <td><input type="text" name="comment" /></td>
            </tr>
            <tr>
                <td>File: </td>
                <td><input type="file" name="image" /></td>
            </tr>
        </table> <input type="submit" />
    </form>
</body>
</html>
```

js파일을 보도록 하겠습니다.

```js
//모듈을 추출합니다.
var fs = require('fs');
var express = require('express');
var multipart = require('connect-multiparty');

//서버를 생성합니다.
var app = express();

//미들웨어를 설정합니다.
//업로드를 할 파일을 지정합니다.
app.use(multipart({ uploadDir: __dirname + '/public' }));

//라우터를 설정합니다.
app.get('/', function (req, res) {
    fs.readFile('HTMLPage.html', function (err, data) {
        res.send(data.toString());
    });
});

app.post('/', function (req, res) {
    console.log(req.body);
    console.log(req.files);

    res.redirect('/');
});

//서버를 실행합니다.
app.listen(52273, function () {
    console.log("서버 시작");
});
```

![스크린샷 2020-10-05 오후 8 25 15](https://user-images.githubusercontent.com/16849874/95074079-e40c8e80-0748-11eb-82e9-7918e12abed2.png)

파일 올리기를 잘 한것을 알 수 있습니다.

이름은 같은 이름을 올리게 되는 경우에는 덮어쓰기가 됩니다.

따라서 적절한 이름을 바꾸는 조치를 해주어야 합니다.

post부분을 아래와 같이 바꾸시면 이미지 파일인지 확인이 가능하고 또한 이미지 파일이 아닌 경우에는 파일을 삭제합니다.

이미지 파일의 이름도 바꿀수 있습니다.

```js
app.post('/', function (req, res) {
    var comment = req.body.comment;
    var imageFile = req.files.image;
    if (imageFile) {
        var name = imageFile.name;
        var path = imageFile.path;
        var type = imageFile.type;

        //이미지 파일인지 확인한다.
        if (type.indexOf('image') != -1) {
            //이미지 파일의 경우에는 이름을 변경합니다.
            var outputPath = __dirname+'/public/'+Date.now()+'_'+name;
            fs.rename(path,outputPath,function (err) {
                res.redirect('/');
            })
        }else{
            //이미지 파일이 아닌 경우에는 삭제를 합니다.
            fs.unlink(path,function(err){
                res.send(400);
            });
        }
    }else{
        //파일이 없는 경우에는
        res.sendStatus(404);
    }
});
```

---

## express-session 미들웨어

세션은 서버에 정보를 저장하는 기술입니다.

먼저 설치를 진행해야합니다.

    npm install express-session

이는 request에 session 속성을 부여합니다.

예제를 보겠습니다.

```js
//모듈을 추출합니다.
var express = require('express');
var session = require('express-session');

//서버를 생성합니다.
var app = express();

//미들웨어를 설정합니다.
app.use(session({
    secret: 'secret key',
    resave: false,
    saveUninitialized: true,
    //쿠키가 사라지는 시간을 브라우저 종료에서 임의로 변경합니다.
    cookie:{
        maxAge: 60 * 1000;
    }
}));

app.use(function (req, res) {
    //세션을 저장합니다.
    req.session.now = (new Date()).toUTCString();

    //응답합니다.
    res.send(req.session);
});

//서버를 실행합니다.
app.listen(9797,function () {
    console.log("서버 시작");
})
```

이를 이용해서 세션을 관리 할 수 있습니다.

아직은 이 부분은 어려운 것 같습니다.

![스크린샷 2020-10-05 오후 9 30 42](https://user-images.githubusercontent.com/16849874/95079762-06ef7080-0752-11eb-886e-a3dbe31d0974.png)

---

## restful api 구현하기

지금까지 배운 내용을 이용해서 restful 앱 서비스를 구축해 보겠습니다.

```js
var fs = require('fs');
var express = require('express');
var bodyParser = require('body-parser');
const { response } = require('express');

//더미 데이터베이스를 구현합니다.
var DummyDB = (function () {
    //변수를 선언합니다.
    var DummyDB = {};
    var storage = [];
    var count = 1;

    //메서드를 구현합니다.
    DummyDB.get = function (id) {
        if (id) {
            //변수를 가공합니다.
            id = (typeof id == 'string') ? Number(id) : id;

            //데이터를 선택합니다.
            for (var i in storage) if (storage[i].id == id) {
                return storage[i];
            }
        } else {
            //id를 넣지 않으면 모든 데이터를 반환합니다.
            return storage;
        }
    };

    DummyDB.insert = function (data) {
        data.id = count++;
        storage.push(data);
        return data;
    };

    DummyDB.remove = function (id) {
        //변수를 가공합니다.
        id = (typeof id == 'string') ? Number(id) : id;

        //제거합니다.
        for (var i in storage) if (storage[i].id == id) {
            //데이터를 제거합니다.
            storage.splice(i, 1);

            //리턴합니다.; 데이터 삭제에 성공했습니다.
            return true;
        }
        //데이터 삭제에 실패했습니다.
        return false;
    }

    return DummyDB;
})();

//서버를 생성합니다.
var app = express();

//미들웨어를 설정합니다.
app.use(bodyParser.urlencoded({
    extended: false
}));

//라우터를 지정합니다.
app.get('/user', function (req, res) { 
    res.send(DummyDB.get());
});

app.get('/user/:id', function (req, res) { 
    res.send(DummyDB.get(req.params.id));
});

app.post('/user', function (req, res) { 
    //변수를 선언합니다.
    var name = req.body.name;
    var region = req.body.region;

    //유효성을 검사합니다.
    if (name && region) {
        res.send(DummyDB.insert({
            name : name,
            region: region
        }));
    } else{
        throw new Error('error');
    }
});

app.put('/user/:id', function (req, res) {
    //변수를 선언합니다.
    //body에 넣어서 보내야 합니다.
    var id = req.params.id;
    var name = req.body.name;
    var region = req.body.region;

    //데이터 베이스를 수정합니다.
    var item = DummyDB.get(id);
    item.name = name || item.name;
    item.region = region || item.region;

    //응답합니다.
    res.send(item);
 });

app.delete('/user/:id', function (req, res) { 
    res.send(DummyDB.remove(req.params.id));
});

//서버를 실행합니다.
app.listen(9797, function () {
    console.log('서버 시작');
})
```

더미 데이터 베이스를 만들어서 restful 서비스를 구축했습니다.

데이터 베이스를 연동하는 방법은 다음 장부터 배워 보도록 하겠습니다.