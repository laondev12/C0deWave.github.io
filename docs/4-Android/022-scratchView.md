---
layout: default
title: "ScratchView를 이용한 복권효과"
parent: Android
nav_order: 22
---

# ScratchView를 이용한 복권효과

이번에는 ScratchView를 이용한 복권긁는 효과를 만들어 보고자 한다.

원래 이런 효과를 어떻게 만들어야 할지 찾아보다 절망했는데 이런 효과가 있어서 다행이다.

먼저 gradle를 추가해 주자.

```kotlin
//프로젝트 모듈
allprojects {
    repositories {
        google()
        jcenter()
        maven{ url "https://jitpack.io"}
    }
}

//앱 모듈
//스트래치 기능을 넣기위한 의존성
implementation 'com.github.AnupKumarPanwar:ScratchView:1.2'
```

그다음은 간단하다.

scratchView를 겹쳐서 만들어주면 된다.

```xml
<com.anupkumarpanwar.scratchview.ScratchView
android:id="@+id/scratchView"
android:layout_width="350dp"
android:layout_height="450dp"
app:overlay_height="450dp"
app:overlay_image="@drawable/crystallball"
app:overlay_width="350dp" />
```

overlay_image를 이용해서 이미지를 겹쳐서 넣을 수 있다.

![스크린샷 2020-06-24 오후 9 03 45](https://user-images.githubusercontent.com/16849874/85558819-1398fe80-b664-11ea-9f55-18100cffbf2d.png)

![스크린샷 2020-06-24 오후 9 03 38](https://user-images.githubusercontent.com/16849874/85558797-0e3bb400-b664-11ea-88a2-4c97f5142aaf.png)

잘 작동하는 것을 볼 수 있다.

또한 여기에 리스너를 붙여서 특정 기능을 구현하는 방법으로도 사용할 수 있다.

```kotlin
// 스크래치 기능의 리스너를 넣었습니다.
scratchView.setRevealListener(object : ScratchView.IRevealListener{
    override fun onRevealed(scratchView: ScratchView?) {
        Toast.makeText(context,"축하드립니다.",Toast.LENGTH_LONG).show()
    }

    override fun onRevealPercentChangedListener(scratchView: ScratchView?, percent: Float) {

    }

})
```

일정 부분 이상을 긁으면 기능이 수행된다.

![스크린샷 2020-06-24 오후 9 45 16](https://user-images.githubusercontent.com/16849874/85559129-61ae0200-b664-11ea-9455-2619cb0ccc6e.png)