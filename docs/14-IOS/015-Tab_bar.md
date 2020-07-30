---
layout: default
title: Tab bar 활용하기
parent: IOS
nav_order: 15
---

#  Tab bar 활용하기

탭바는 안드로이드의 bottom app bar와 비슷한 역할을 합니다.

이를 만들기 위해서 기존의 View를 누르고 

Editor - Embed in - Tab bar Controller을 눌러준다.

<img width="964" alt="스크린샷 2020-07-30 오후 3 49 16" src="https://user-images.githubusercontent.com/16849874/88902426-261edd00-d28d-11ea-8cf2-6f77f8c28368.png">

그러면 탭바의 형태가 나오게 된다.

나는 다음으로 다른 프로젝트에서 했던 View를 여기에 넣고자 한다.

그러기 위해서는 다른 프로젝트의 ViewController을 이름을 변경해서 이 프로젝트에 넣어준다.

또한 프로젝트의 스토리 보드에서 레이아웃을 commend + c, commend + v로 합칠 수 있습니다.

<img width="1077" alt="스크린샷 2020-07-30 오후 5 54 10" src="https://user-images.githubusercontent.com/16849874/88902831-ae9d7d80-d28d-11ea-99c9-686016d21fd2.png">

이렇게 레이아웃을 넣으면 기존의 tab bar에서 control을 누르고 드래그를 해서 연결할 수 있습니다.

<img width="846" alt="스크린샷 2020-07-30 오후 4 00 19" src="https://user-images.githubusercontent.com/16849874/88903022-f8866380-d28d-11ea-987e-c60a579206cc.png">

나오는 화면에 View Controller을 눌러 줍니다.

<img width="431" alt="스크린샷 2020-07-30 오후 4 00 53" src="https://user-images.githubusercontent.com/16849874/88903076-0cca6080-d28e-11ea-8f89-0587da0021bf.png">

결과적으로 이렇게 되는 것을 볼 수 있습니다.

이제 코드 부분으로 보겠습니다.

레이아웃의 변환은 따로 코드를 만들지 않아도 됩니다.

특정 기능을 하면 임의적으로 탭을 변환 시킬수 있습니다.

```swift
@IBAction func btnMoveImageView(_ sender: UIButton) {
    tabBarController?.selectedIndex = 1
}

@IBAction func btnMoveDatePickerView(_ sender: UIButton) {
    tabBarController?.selectedIndex = 2
}
```