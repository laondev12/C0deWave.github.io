---
layout: default
title: "fragment 화면전환하기"
parent: Android
nav_order: 19
---

# fragment 화면전환하기

먼저 fragment를 만들어본다.

코틀린파일이 있는 폴더 -> new -> fragment -> blankFragment를 선언해서 만들어준다.

그후 fragment.kt의 파일을 다음과 같이 고친다.

```kotlin
class accountConfigurationFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        val inflater = inflater.inflate(R.layout.fragment_account_configuration, container, false)
        inflater.logoutButton.setOnClickListener {
            (activity as MainActivity).signOut()
        }
        return inflater
    }


    companion object {
        fun newInstance(): accountConfigurationFragment {
            return accountConfigurationFragment()
        }
    }
}
```

그 다음에는 fragment를 표시해 줄 frameLayout읆 만들어준다.

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
```

그 다음으로 frameLayout 부분에 함수를 추가해 준다.

```kotlin
fun setFragment(fragment : Fragment){
    supportFragmentManager
        .beginTransaction()
        .replace(R.id.mainFrame,fragment,"mainActivity")
        .commit()
}
``` 

이제 화면 전환을 할때 다음 함수를 호출하면 된다.

```kotlin
setFragment(accountConfigurationFragment.newInstance())
```

끝