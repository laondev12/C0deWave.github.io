---
layout: default
title: "meow 네비게이션"
parent: Android
nav_order: 17
---

# meow 네비게이션

원래 만들고자 하는 액션바는 FAB가 움직이는 화면이였다.

결과화면을 보면 무슨 말인지 알 수 있다.

![스크린샷 2020-06-17 오후 6 16 36](https://user-images.githubusercontent.com/16849874/84879841-b67ed500-b0c6-11ea-8bd9-dc580d7bd8f1.png)

![스크린샷 2020-06-17 오후 6 16 31](https://user-images.githubusercontent.com/16849874/84879853-baaaf280-b0c6-11ea-86a3-5dce14b8970e.png)

보면 하단의 액션바가 움직이는 것을 알 수 있다.

이런 액션바를 학교실습에서는 어려웠는데 이를 좀더 쉽게 구현한 meow네비게이션이 있었다.

이를 구현하기 위해서는 먼저 의존성을 추가해 주어야 한다.

```kotlin
    implementation 'com.etebarian:meow-bottom-navigation:1.0.4'
```

그 다음으로는 xml파일에 FrameLayout과 MeowBottomNavigation을 추가해 주면 된다.

```xml
    <FrameLayout
        android:id="@+id/mainFrame"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:background="#8BC34A"
        app:layout_constraintBottom_toTopOf="@+id/meowBottomNavigation"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </FrameLayout>

    <com.etebarian.meowbottomnavigation.MeowBottomNavigation
        android:id="@+id/meowBottomNavigation"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:mbn_backgroundBottomColor="#FFFFFF"
        app:mbn_countBackgroundColor="#dd6d00"
        app:mbn_defaultIconColor="#90a4ae"
        app:mbn_selectedIconColor="#3c415e"/>
```

그후 Activity.kt 파일을 열어보자.

```kotlin
meowBottomNavigation.add(MeowBottomNavigation.Model(1,R.drawable.ic_calendar))
meowBottomNavigation.add(MeowBottomNavigation.Model(2,R.drawable.ic_bulltinboard))
meowBottomNavigation.add(MeowBottomNavigation.Model(3,R.drawable.ic_about))
```

위와 같은 방법으로 버튼을 추가해 준다.

```kotlin
meowBottomNavigation.setOnClickMenuListener {
    when(it.id){
        1 -> {setFragment(calendarFragment.newInstance())}
        2 -> {setFragment(noticeBoardFragment.newInstance())}
        3 -> {setFragment(accountConfigurationFragment.newInstance())}
    }
}
```

버튼을 누르면 특정 작업을 하는 방법은 setOnClickMenuListener이다.

```kotlin
//처음 화면을 캘린더로 지정한다.
setFragment(calendarFragment.newInstance())
meowBottomNavigation.show(1)
```

처음에 버튼을 지정하고 시작하는 방법은 위와 같다.