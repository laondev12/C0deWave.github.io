---
layout: default
title: ColorTheme 지정하기
parent: Android
nav_order: 2
---

# 안드로이드에서 컬러테마를 만들고 지정하는 방법

우선은 values - style 를 열고 원하는 스타일을 추가해 줍니다.

저는 AppTheme.Result라는 style를 추가했습니다.

```xml
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.Result">
        <item name="android:textColor">@android:color/black</item>
        <item name="colorPrimary">#CDDC39</item>
        <item name="android:buttonStyle">@style/ButtonColor</item>
        <item name="colorButtonNormal">@color/colorPrimary</item>
    </style>

    <style name="ButtonColor" parent="@style/Widget.AppCompat.Button">
        <item name="android:textColor">@android:color/white</item>
    </style>

</resources>
```

그 후에 AndroidManifest.xml 을 들어가서 테마를 지정해 줍니다.

```xml
  <activity android:name=".ResultActivity" android:theme="@style/AppTheme.Result"></activity>

```

위와 같이 테마를 지정해 주면 됩니다.

![]()

![]()

그러면 위의 이미지와 같이 앱 테마의 색상이 변하는 것을 볼 수 있습니다.