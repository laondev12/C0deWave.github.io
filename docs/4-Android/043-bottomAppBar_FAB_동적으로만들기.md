---
layout: default
title: "bottomAppBar_FAB 동적으로 움직이기"
parent: Android
nav_order: 43
---

# bottomAppBar_FAB 동적으로 움직이기

이번에는 bottom App Bar 와 fab을 동적으로 움직이는 방법에 대해서 알아보도록 하겠습니다.

방법은 간단했는데 삽질을 너무 많이 했네요 ;;

먼저 1번 방법입니다.

![스크린샷 2021-02-01 오전 10 30 06](https://user-images.githubusercontent.com/16849874/106405372-56804780-6479-11eb-82ef-40ff9ab82c17.png)

    app:hideOnScroll="true"
    app:layout_scrollFlags="scroll|enterAlWays"

이처럼 hideOnScroll 값을 true로 하면 됩니다.

하지만 이 경우에는 FAB이 자동으로 숨겨지지 않습니다.

2. 다음 방법은 NestedScrollView를 이용하는 방법입니다.

먼저 메인 컨텐츠 부분을 NestedScrollView로 한번 감싸줍니다.

![스크린샷 2021-02-01 오전 10 31 58](https://user-images.githubusercontent.com/16849874/106405360-4ec0a300-6479-11eb-90b1-34684781d97e.png)

그 다음 NestedScrollView를 지정해서 스크롤 할때의 리스너를 등록해주면 됩니다.

![스크린샷 2021-02-01 오전 10 30 26](https://user-images.githubusercontent.com/16849874/106405371-55e7b100-6479-11eb-9fa9-0c7888c1c8b5.png)

```kotlin
//스크롤시 fab을 사라지게 합니다.
NestedScrollView.setOnScrollChangeListener { v: NestedScrollView?, scrollX: Int, scrollY: Int, oldScrollX: Int, oldScrollY: Int ->
    val dy = oldScrollY - scrollY
    if (dy < 0) {
        bottomAppBar.performHide().run {
            fab.hide()
            angryFab.hide()
            sadFab.hide()
            smailFab.hide()
        }
    } else if (dy > 0) {
        bottomAppBar.performShow().run {
            fab.show()
            if (clickedFab){
                angryFab.show()
                sadFab.show()
                smailFab.show()
            }
        }
    }
}
```

---

결과 화면 입니다.

잘 작동하는 것을 볼 수 있습니다.

<img width="504" alt="스크린샷 2021-02-01 오전 10 31 14" src="https://user-images.githubusercontent.com/16849874/106405366-51bb9380-6479-11eb-848e-5196340ea653.png">

<img width="490" alt="스크린샷 2021-02-01 오전 10 31 06" src="https://user-images.githubusercontent.com/16849874/106405370-54b68400-6479-11eb-9701-9c5be3209f02.png">