---
layout: default
title: node의 데이터베이스 연동
parent: nodeJS
nav_order: 13
---

이번에는 node와 mysql의 연동에 관해서 알아보도록 하겠습니다.

우선은 먼저 mysql이 설치되어 있어야 합니다.

없다면 먼저 설치를 해 봅시다.

https://dev.mysql.com/downloads/file/?id=497036 -> 여기서 설치를 진행하면 됩니다.

그 다음으로 mysql 모듈을 이용해서 연동을 해주도록 하겠습니다.

    npm install mysql

node에서 데이터 베이스를 연동하는 예제입니다.

```js
var mysql = require('mysql');

//데이터 베이스와 연결합니다.
var client = mysql.createConnection({
    user: 'root',
    password: '00000000'
});

//데이터 베이스 쿼리를 사용합니다.
// client.query('CREATE DATABASE Company');
client.query('USE Company');
// client.query('CREATE TABLE products (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50) NOT NULL, modelnumber VARCHAR(15) NOT NULL, seties VARCHAR(30) NOT NULL);');
client.query('INSERT INTO products(name, modelnumber, seties ) VALUES ("주명","0000","he is god");')


//쿼리문의 콜백 함수
client.query('SELECT * FROM products',function (err,result,field) {
    if (err) {
        console.log("쿼리 문장에 오류가 있습니다.");
    }
    console.log(result);
});
```

그 다음으로는 이를 이용하여 간단한 CRUD룰 구현해 보겠습니다.

먼저 html파일 3개가 필요합니다.

list.html

```html
<!DOCTYPE html>
<html>

<head>
    <title>List Page</title>
</head>

<body>
    <h1>List Page</h1> <a href="/insert">INSERT DATA</a>
    <hr />
    <table width="100%" border="1">
        <tr>
            <th>DELETE</th>
            <th>EDIT</th>
            <th>ID</th>
            <th>Name</th>
            <th>Model Number</th>
            <th>Series</th>
        </tr> <% data.forEach(function (item, index) { %> <tr>
            <td><a href="/delete/<%= item.id %>">DELETE</a></td>
            <td><a href="/edit/<%= item.id %>">EDIT</a></td>
            <td><%= item.id %></td>
            <td><%= item.name %></td>
            <td><%= item.modelnumber %></td>
            <td><%= item.seties %></td>
        </tr> <%}); %>
    </table>
</body>

</html>
```

insert.html

```html
<!DOCTYPE html>
<html>

<head>
    <title>Insert Page</title>
</head>

<body>
    <h1>Insert Page</h1>
    <hr />
    <form method="post">
        <fieldset>
            <legend>INSERT DATA</legend>
            <table>
                <tr>
                    <td><label>Name</label></td>
                    <td><input type="text" name="name" /></td>
                </tr>
                <tr>
                    <td><label>Model Number</label></td>
                    <td><input type="text" name="modelnumber" /></td>
                </tr>
                <tr>
                    <td><label>Series</label></td>
                    <td><input type="text" name="seties" /></td>
                </tr>
            </table> <input type="submit" />
        </fieldset>
    </form>
</body>

</html>
```

edit.html

```html
<!DOCTYPE html>
<html>

<head>
    <title>Edit Page</title>
</head>

<body>
    <h1>Edit Page</h1>
    <hr />
    <form method="post">
        <fieldset>
            <legend>Edit Data</legend>
            <table>
                <tr>
                    <td><label>Id<label></td>
                    <td><input type="text" name="id" value="<%= data.id %>" disabled /></td>
                </tr>
                <tr>
                    <td><label>Name<label></td>
                    <td><input type="text" name="name" value="<%= data.name %>" /></td>
                </tr>
                <tr>
                    <td><label>Model Number<label></td>
                    <td><input type="text" name="modelnumber" value="<%= data.modelnumber %>" /></td>
                </tr>
                <tr>
                    <td><label>Series<label></td>
                    <td><input type="text" name="seties" value="<%= data.seties %>" /></td>
                </tr>
            </table> <input type="submit" />
        </fieldset>
    </form>
</body>

</html>
```

다음은 node 코드 내용 입니다.

```js
var fs = require('fs');
var ejs = require('ejs');
var mysql = require('mysql');
var express = require('express');
var bodyParser = require('body-parser');

//데이터 베이스와 연결합니다.
var client = mysql.createConnection({
    user:'root',
    password:'00000000',
    database:'Company'
});

//서버를 생성합니다.
var app = express();
app.use(bodyParser.urlencoded({
    extended:false
}));

//서버를 실행합니다.
app.listen(9797,function () {
    console.log("서버 시작");
});

//라우트를 수행합니다.
//모든 제품을 화면에 출력해 줍니다.
app.get('/', function (req,res) {
    //파일을 읽습니다.
    fs.readFile('list.html','utf8',function (err,data) {
        //데이터 베이스 쿼리를 실행합니다.
        client.query('SELECT * FROM products', function (error, results) {
            //응답합니다.
            res.send(ejs.render(data,{
                data: results
            }));
        });
    });
 });

app.get('/delete/:id', function (req,res) {
    //데이터 베이스 쿼리를 실행합니다.
    client.query('DELETE FROM products WHERE id=?', [req.params.id], function () {
        res.redirect('/');
    });
 });
app.get('/insert', function (req,res) {
    //파일을 읽습니다.
    fs.readFile('insert.html','utf8',function (err, data) {
        //응답합니다.
        res.send(data);
    });
 });
app.post('/insert', function (req,res) {
    //변수를 선언합니다.
    var body = req.body;

    //데이터 베이스 쿼리를 실행합니다.
    client.query('INSERT INTO products (name, modelnumber, seties) VALUES (?, ?, ?)',[
        body.name,
        body.modelnumber,
        body.seties
    ],function () {
        //응답합니다.
        res.redirect('/');
    });
 });
app.get('/edit/:id', function (req,res) {
    fs.readFile('edit.html','utf8',function (err data) {
        //데이터 베이스 쿼리를 실행합니다.
        client.query('SELECT * FROM products WHERE id = ?', [
            req.params.id
        ], function (err,result) {
            //응답합니다.
            res.send(ejs.render(data,{
                data: result[0]
            }));
        });
    });
 });
app.post('/edit/:id', function (req,res) {
    //변수를 선언 합니다.
    var body = req.body;

    //데이터베이스 쿼리를 실행합니다.
    client.query('UPDATE products SET name=?, modelnumber=?, seties=? WHERE id=?',[
        body.name, body.modelnumber, body.seties, req.params.id
    ],function () {
        //응답합니다.
        res.redirect('/');
    });
 });
```

![스크린샷 2020-10-08 오전 12 36 30](https://user-images.githubusercontent.com/16849874/95353523-507cbe80-08fe-11eb-8c32-f4eb23ee0d70.png)

데이터 베이스가 정상적으로 작동하는 것을 볼 수 있습니다.