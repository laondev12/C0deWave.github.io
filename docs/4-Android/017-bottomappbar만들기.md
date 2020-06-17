---
layout: default
title: "Fab이 들어간 Bottom Action Bar 만들어 보기"
parent: Android
nav_order: 16
---

# Fab이 들어간 Bottom Action Bar 만들어 보기

아래의 유튜브 영상을 참고했다.

[https://youtu.be/db7TQYedZNE](https://youtu.be/db7TQYedZNE)

학교에서 안드로이드 수업을 들으면서 실습을 했는데 에러가 나서 따로 다시 한번 해 보았다.

결과 화면은 다음과 같다.

![스크린샷 2020-06-17 오후 12 50 24](https://user-images.githubusercontent.com/16849874/84878558-f349cc80-b0c4-11ea-848b-8a10fc2f1e07.png)

먼저 의존성을 추가해 주어야 한다.

```kotlin
    //구글 마테리얼 디자인 설정
    implementation 'com.google.android.material:material:1.2.0-alpha02'
```

그 후에 res -> values -> styles.xml을 눌러서 테마의 parent를 바꿔준다.

구글의 마테리얼 디자인으로 바꾸었다.

```
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.MaterialComponents.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
```

그 다음에는 레이아웃을 수정해 주면 된다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true" >

    <com.google.android.material.bottomappbar.BottomAppBar
        android:id="@+id/bottomAppBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:backgroundTint="@color/colorPrimary"
        app:menu="@menu/app_bar_menu"
        app:navigationIcon="@drawable/ic_menu"

        app:hideOnScroll="true"
        app:fabAlignmentMode="end"
        />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_add"
        android:backgroundTint="@color/colorAccent"
        app:layout_anchor="@id/bottomAppBar"
        app:maxImageSize="35dp"
        app:tint="@color/colorWhite"
        />


</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

그럼 화면이 잘 나오는 것을 알 수 있다.