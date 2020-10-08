---
layout: default
title: jade레이아웃 만들기
parent: nodeJS
nav_order: 15
---

## jade레이아웃 만들기

모든 페이지에 상단바와 같이 변하지 않는 메뉴틀을 레이아웃 페이지라고 합니다.

express프레임워크로도 이러한 레이아웃 페이지를 만들수 있습니다.

이는 views폴더 안에 있는 layout파일을 건드리면 됩니다.

아래는 layout파일입니다.

```jade
doctype html
html
  head
    title layout
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    block content
```

아래는 index파일입니다.

```jade
extends layout

block content
  h1= title
  p Welcome to #{title}

```

보면은 layout에서 값을 가져와서 뿌린 다음에 내용을 뿌리는 것을 알 수 있습니다.