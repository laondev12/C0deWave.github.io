---
layout: default
title: "Beacket Pair_코드 블록 구분해서 보기"
parent: Visual Studio Code
nav_order: 5
---

# Beacket Pair_코드 블록 구분해서 보기

이번에는 vscode에서 코드를 구분해서 보기 위해서 이용하는 확장 프로그램인 Bracket Pair 입니다.

![스크린샷 2020-09-11 오후 4 29 01](https://user-images.githubusercontent.com/16849874/92883526-e7fc0800-f44b-11ea-953f-0344e5ad155f.png)

이렇게 현재 보고 있는 블록을 표시해 주고 블록의 괄호색을 다르게 해줘서 가독성을 늘려줍니다.

Extension 탭에서 Bracket Pair Colorizer 2를 검색하거나

[링크](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)에 들어가서 받으면 됩니다.

## 색 조정방법

commend + P 를 누른 다음에 setting.json을 검색해서 열어줍니다.

    "bracket-pair-colorizer-2.colors": [
        "#8A46BF",
        "#BF568B",
        "Green"
    ],

위와 같이 넣어주면 원하는 괄호의 색을 변경할 수 있습니다.

code - preference - Keyboard shortcut에 들어가서 아래와 같이 설정을 합니다.

단축키가 겹치면 다른 원하는 단축키로 해도 됩니다.

![스크린샷 2020-09-11 오후 4 38 08](https://user-images.githubusercontent.com/16849874/92884833-2f36c880-f44d-11ea-851b-710a0c7f5341.png)

다음과 같이 키맵을 지정하면 현재 위치하는 블록을 전체선택하거나 해제할 수 있습니다.
