---
layout: default
title: express프레임워크
parent: nodeJS
nav_order: 14
---

express와 express-generator이라는 전역모듈을 합쳐서 express 프레임 워크라고 부릅니다.

이러한 express 프레임워크에 대해서 알아봅시다.

먼저 express-generator을 설치해 봅니다.

    npm install express-generator

을 입력해서 설치를 진행합니다.

그후 다음 명령어를 입력합니다.

    express HelloExpress

    cd HelloExpress

    npm install

그후 다음 명령어를 이용해서 서버를 실행할 수 있습니다.

    DEBUG=HelloExpress:* & npm start

express 프레임 워크는 기본 설정으로 3000번 포트를 사용합니다.

우선은 express --help를 눌러서 도움말을 출력해 봅니다.

express 모듈로 프로젝트를 만들어 사용하면 페이지 렌더링 기능을 사용할 수 있습니다.

express프레임워크는 자체적으로 fs의 render()기능을 제공합니다.

왠만한 파일의 관리는 app.js에서 관리를 합니다.

먼저 view안에 product.jade파일을 만들어 줍니다.

```jade
doctype html
html
    head
        title= title
        link(rel='stylesheet',href='/stylesheets/style.css')
    body
        h1 #{title}
        p Node.js Programming for
        hr
```

그후 app.js에 해당 내용을 입력하면 됩니다.

```js
//render메서드는 view에 존재하는 파일이름 + 매개변수를 누르면 됩니다.
//jade에서 해당 파일을 받을 때는 #{}안에 매개 변수를 적으면 됩니다.
app.use('/product',function (req, res) {
  res.render('product',{
    title:'Product page'
  });
});
```

---

## 폴더를 사용해서 페이지 분류하기

시간이 지나면 지날수록 views안에 파일이 많아집니다.

이제는 views안에서도 폴더로 페이지를 분류하는 방법을 알아보겠습니다.

기본적으로 render을 이용해서 폴더 이름을 지정하면 안에있는 index를 기본적으로 보여줍니다.

아니면 render안에 폴더부터 경로를 입력해 주면 됩니다.

```js
app.use('/product/insert',function (req, res) {
  res.render('product',{
    title:'insert'
  });
});
```

이런 방식으로 지정을 해주면 됩니다.