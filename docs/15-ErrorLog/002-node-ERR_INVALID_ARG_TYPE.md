---
layout: default
title: node함수 선언 에러
parent: 에러기록
nav_order: 2
---

# [ERR_INVALID_ARG_TYPE]: The first argument must be of type string or an instance of Buffer, ArrayBuffer, or Array or an Array-like Object. Received function toString

영어를 끝까지 읽어보면 알수 있듯이 toString에서 일어난 에러 입니다.

# 에러 메세지

![스크린샷 2020-10-05 오후 8 18 37](https://user-images.githubusercontent.com/16849874/95073745-52048600-0748-11eb-8ed9-a59e4126dd3f.png)

보면 알수 있듯이 로그에서 제 코드의 16번째 줄에서 에러가 난 것이였습니다.

# 해결 방법

![스크린샷 2020-10-05 오후 8 18 17](https://user-images.githubusercontent.com/16849874/95073821-75c7cc00-0748-11eb-977f-0603db258f2d.png)

보면 알 수 있듯이 toString에 ()을 붙여주지 않아서 생기는 버그였습니다.

로그를 않읽었다가 시간이 많이 지체되었던 에러입니다.
