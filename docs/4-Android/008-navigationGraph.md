---
layout: default
title: "네비게이션을 이용해서 화면전환하기"
parent: Android
nav_order: 8
---

# 네비게이션을 이용한 화면전환 구성하기

결과 화면

![스크린샷 2020-06-07 오후 5 30 58](https://user-images.githubusercontent.com/16849874/83967526-42417600-a8fd-11ea-997d-5462513bde72.png)

이러한 화면을 구성하기 위해서는 먼저 안드로이드 gradle를 수정해야 한다.

```kotlin
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = "1.8"
    }
```

위의 내용을 android{} 안에 넣어 줍니다.

그 후에는 navigation xml 리소스를 만들어 줍니다. 

![스크린샷 2020-06-07 오후 4 46 22](https://user-images.githubusercontent.com/16849874/83967550-964c5a80-a8fd-11ea-8e6a-bb259d10f378.png)

그러면 리소스가 만들어 집니다.

그 다음으로는 각각의 fragment와 화면의 이동을 정의하는 Action을 만들어 줍니다.

![스크린샷 2020-06-07 오후 4 50 07](https://user-images.githubusercontent.com/16849874/83967576-df9caa00-a8fd-11ea-95a0-438f8dde91dd.png)

그러면 각각의 레리아웃과 kotlin파일이 만들어져 있는것을 확인 할 수 있습니다.

## MainActivity 수정하기

![스크린샷 2020-06-07 오후 4 56 03](https://user-images.githubusercontent.com/16849874/83967595-0fe44880-a8fe-11ea-8230-e29fffd96e75.png)

그후에 Main 레이아웃에는 NavHostFragment를 넣어줍니다.

```kotlin
//네비게이션 그래프에서 정의한 Destination action 을 취하기 위해서는 NavController이 필요하다.
val navController = findNavController((R.id.fragment))

//DestinationChangedChangedListener을 통해서 destination이 바뀔때 로그를 출력한다.
navController.addOnDestinationChangedListener{ controller, destination, arguments ->
    Log.i("NAVI","${destination.label}")
}
```

MainActivity.kt에서 위의 내용을 넣어서 NavController룰 연결해 줍니다.

바뀔때마다 행동을 취할수도 있습니다.

이제는 각 fragment의 내용도 액션을 연결해 주어야 합니다.

버튼을 만들고 해당 아이디를 불러내서 액션을 넣어 줍니다.

```kotlin
override fun onCreateView(
    inflater: LayoutInflater, container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    val root = inflater.inflate(R.layout.fragment_b, container, false)

    //////////////////////////////////////////////////////////////////////////////
    root.button.setOnClickListener {
        //네비게이트 컨트롤러를 이용해서 원하는 목적지로 이동하도록 설정한다.
        findNavController().navigate(R.id.action_BFragment_to_CFragment)
    }

    root.button4.setOnClickListener {
        findNavController().navigate(R.id.action_BFragment_to_DFragment)
    }

    //////////////////////////////////////////////////////////////////////////////

    // Inflate the layout for this fragment
    return root
}
```

onCreateView를 수정해서 위와 같이 액션을 연결해 줍니다.

그러면 잘 작동하는 것을 알 수 있습니다.