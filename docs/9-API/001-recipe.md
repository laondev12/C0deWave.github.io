---
layout: default
title: 농수축산물 안심레시피 정보
parent: API
nav_order: 1
---

# 농수축산물 안심레시피 정보 API

모바일 프로그래밍 팀 프로젝트 기말 대체 시험에서 요리 레시피를 검색하는 어플을 만들자는 제안이 나와서 처음에 찾은 API이다.

하지만 레시피 이름으로만 검색이 가능하고 정확한 철자가 맞지 않으면 제대로된 결과를 반환하지 않는다는 점에서 아마도 제외되지 않을까 싶다.

요청변수

![스크린샷 2020-06-03 오후 10 49 15](https://user-images.githubusercontent.com/16849874/83645270-3ef07680-a5ed-11ea-919d-48b960156175.png)

예제 URI

* [http://211.237.50.150:7080/openapi/sample/xml/Grid_20150827000000000226_1/1/5/?RECIPE_NM_KO=%EC%95%BD%EC%8B%9D](http://211.237.50.150:7080/openapi/sample/xml/Grid_20150827000000000226_1/1/5/?RECIPE_NM_KO=%EC%95%BD%EC%8B%9D)

결과 화면
![스크린샷 2020-06-03 오후 10 49 32](https://user-images.githubusercontent.com/16849874/83645235-34ce7800-a5ed-11ea-9f85-64487334b51a.png)

에러 메세지의 종류
![스크린샷 2020-06-03 오후 10 49 55](https://user-images.githubusercontent.com/16849874/83645219-2ed89700-a5ed-11ea-9f92-ed534b1843e2.png)

---

한순간 API에 선택 변수를 어떻게 넣는지 고민했는데 잘 복습하는 시간이였다.