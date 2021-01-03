---
layout: default
title: "fab사용하기"
parent: Android
nav_order: 29
---

# fab사용하기

fab이란 무엇인가??

fab는 Floating Action Button의 줄임말로써 움직이거나 특정 기능을 담당해주는 원형의 버튼이라고 생각하면 편하다. 

![스크린샷 2021-01-03 오후 2 01 28](https://user-images.githubusercontent.com/16849874/103472076-2f781c80-4dcc-11eb-9221-fa71997d0add.png)

fab에 관한 디자인은 다음의 링크를 참조하자.

[https://material.io/components/buttons-floating-action-button#types-of-transitions](https://material.io/components/buttons-floating-action-button#types-of-transitions)

![스크린샷 2021-01-03 오후 2 04 04](https://user-images.githubusercontent.com/16849874/103472101-97c6fe00-4dcc-11eb-8887-7486035a8d5a.png)

---

## 이제는 안드로이드에 fab을 추가하는 방법을 소개한다.

먼저 의존성을 gradle에 추가해야 한다.

```kotlin
implementation 'com.google.android.material:material:1.3.0-alpha03'
```

그리고 나서 테마에 들어가서 style의 parent를 바꿔 주어야 한다.

아래와 같이 하자.

```xml
<style name="Theme.HowToday" parent="Theme.MaterialComponents.NoActionBar">
```

그리고 나서 뷰의 레이아웃 역시도 CoordinatorLayout으로 바꾸어 주어야 한다.

![스크린샷 2021-01-03 오후 2 07 37](https://user-images.githubusercontent.com/16849874/103472136-17ed6380-4dcd-11eb-8e2c-d437be4e2b31.png)

위의 이미지의 xml이다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical" >
    </LinearLayout>

    <com.google.android.material.bottomappbar.BottomAppBar
        android:id="@+id/bottomAppBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:backgroundTint="#FFFFFF"
        app:contentInsetStart="0dp"
        app:fabCradleMargin="5dp"
        app:fabCradleRoundedCornerRadius="10dp"
        app:fabCradleVerticalOffset="0dp">

        <com.google.android.material.bottomnavigation.BottomNavigationView
            android:id="@+id/bottomNavigationView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="@android:color/transparent"

            app:itemRippleColor="#00FFFFFF"
            app:menu="@menu/bottom_navigation_menu" />

    </com.google.android.material.bottomappbar.BottomAppBar>

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:contentDescription="OrderDiary"
        app:layout_anchor="@id/bottomAppBar"
        android:src="@drawable/ic_android_black_24dp" />


</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

보시다시피 액션바에 고정을 시켰다.

이번애는 FAB에 기능을 추가해 보자.

kotlin의 extension을 이용해서 바로 리스너를 등록해 주면 된다.

```kotlin
fab.setOnClickListener {
            supportFragmentManager.beginTransaction().replace(R.id.linearLayout, OrderDiaryFragment()).commitAllowingStateLoss()
}
```