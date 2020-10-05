---
layout: default
title: How To Make a gitblog
parent: git
nav_order: 1
---

---
# 깃 블로그를 만드는 방법

저는 아래의 블로그 글을 참고했습니다.

[https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html](https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html)

따라서 저는 깃허브에서 지금 블로그 레이아웃을 찾았습니다.

[https://github.com/pmarsceill/just-the-docs](https://github.com/pmarsceill/just-the-docs)

---
## 만드는 방법 정리

1. 새 저장소의 이름을 깃허브닉네임.github.io 로 만들어 줍니다.

2. 마음에 드는 Jekyll테마 찾기

3. 해당 저장소에서 __config.yml 파일을 복사해서 넣습니다.

4. 가져온 파일에 다음의 내용을 추가해 줍니다.
```
remote_theme : 가져온 테마의 저장소

ex.
remote_theme: pmarsceill/just-the-docs
```

5. index.md 파일 만들기

    처음에 들어 왔을떄 보여줄 내용을 지정해 줍니다.

    여기까지 만들었으면 깃닉네임.github.io 로 들어가면 사이트가 생성된 것을 알 수 있습니다.

6.  글쓰기
    
    글을 쓸떄는 
    ```
    ---
    layout: default
    title: How to make blog
    nav_order: 4
    has_children: true
    permalink: docs/form
    ---
    ```
    와 같은 형식으로 Front matter을 만들어 주어야 합니다.

    저장소에 예제가 있을것이니 잘 찾아보면 됩니다.

    저의 경우에는 카테고리 폴더도 위의 형식으로 파일을 하나 만들었습니다.