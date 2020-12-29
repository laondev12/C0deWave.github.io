---
layout: default
title: "BottomNavigationBar을 이용한 화면전환"
parent: Android
nav_order: 8
---

1. gradle에 해당 내용을 추가합니다.

```kotlin
 //noinspection GradleCompatible
implementation 'com.android.support:design:29.0.0'
implementation 'com.google.android.material:material:1.2.0'
```

2. 메뉴를 만들어 줍니다.

    res -> New -> Android Resource File

    ![스크린샷 2020-12-29 오후 8 52 07](https://user-images.githubusercontent.com/16849874/103287183-2e8a6800-4a25-11eb-9e09-1cc2952fa8a4.png)

    리소스 타입을 메뉴로 하는 것에 주의합니다. 

    ![스크린샷 2020-12-29 오후 8 52 35](https://user-images.githubusercontent.com/16849874/103287196-36e2a300-4a25-11eb-95fb-dbb170646db6.png)
   
3. 타겟 메인 화면의 레이아웃을 다음과 같이 구성합니다.

![스크린샷 2020-12-29 오후 10 31 15](https://user-images.githubusercontent.com/16849874/103287319-92ad2c00-4a25-11eb-9e31-4d4355a9ddce.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@+id/bottomNavigationView"
        />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottomNavigationView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:menu="@menu/bottom_navigation_menu"
        app:itemBackground="@android:color/white"
        />
</androidx.constraintlayout.widget.ConstraintLayout>
```

4. 메인화면의 액티비티를 다음과 같이 구성합니다.

```kotlin
class MainActivity : AppCompatActivity(),BottomNavigationView.OnNavigationItemSelectedListener {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        bottomNavigationView.setOnNavigationItemSelectedListener(this)
        bottomNavigationView.setSelectedItemId(R.id.view_order_diary)
        supportFragmentManager.beginTransaction().add(R.id.linearLayout, OrderDiaryFragment()).commit()
    }

    override fun onNavigationItemSelected(item: MenuItem): Boolean {

        when(item.itemId){
            R.id.view_my_diary -> {
                supportFragmentManager.beginTransaction().replace(R.id.linearLayout, MyDiaryFragment()).commitAllowingStateLoss()
                return true
            }

            R.id.view_order_diary -> {
                supportFragmentManager.beginTransaction().replace(R.id.linearLayout, OrderDiaryFragment()).commitAllowingStateLoss()
                return true
            }

            R.id.write_diary->{
                supportFragmentManager.beginTransaction().replace(R.id.linearLayout, WriteDiaryFragment()).commitAllowingStateLoss()
                return true
            }
        }
        return false
    }
}
```

4-1. 만약 bottomNavigationView가 나오지 않는다면 kotlin extension이 설치 되지 않은 것으로 이를 설치해야 합니다.

gralde-modulr에 다음의 내용을 추가합니다.

```xml
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'

android {
        ........
```

5. 해당 메뉴에 맞는 Fragment를 만들어서 구성합니다.

![스크린샷 2020-12-29 오후 10 36 00](https://user-images.githubusercontent.com/16849874/103287594-3bf42200-4a26-11eb-8f1d-18ece7873329.png)

Bottom Navigation Bar가 잘 작동하는 것을 볼 수 있습니다.

![스크린샷 2020-12-29 오후 10 37 07](https://user-images.githubusercontent.com/16849874/103287650-6514b280-4a26-11eb-995f-f88c04e27db0.png)