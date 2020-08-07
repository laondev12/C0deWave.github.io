---
layout: default
title:  AutoLayout 설정하기
parent: IOS
nav_order: 25
---

# AutoLayout 설정하기

이번에는 안드로이드의 constraint layout과 같은 화면의 크기 비율을 자동으로 지정해주는 방법에 대해 알아보겠습니다.

![스크린샷 2020-08-07 오후 9 47 36](https://user-images.githubusercontent.com/16849874/89646930-a1b90380-d8f7-11ea-92c2-3491a6ffb06f.png)

먼저 그림과 같이 Stack view를 만들어 줍니다.

![스크린샷 2020-08-06 오전 6 13 35](https://user-images.githubusercontent.com/16849874/89646889-8fd76080-d8f7-11ea-997a-ffbd3bb1e32f.png)

그후 해당 요소들을 View로 드래그를 해서 Equal Height/Width를 눌러서 비율을 맞춰줍니다.

![스크린샷 2020-08-07 오후 9 49 17](https://user-images.githubusercontent.com/16849874/89647044-d9c04680-d8f7-11ea-9de4-648569b8ebcb.png)

다음으로 줄자 메뉴의 해당 부분에서 Multiplier을 조작해서 비율을 맞춰 줍니다.

그러면 요소의 비율을 전제 가로 세로의 길이의 비율로 정할 수 있습니다.