---
layout: default
title: "파이어베이스 연동"
parent: Android
nav_order: 6
---

# 안드로이드 파이어베이스를 이용한 실시간 데이터 베이스 연동하기

먼저 파이어 베이스에서 앱 등록을 진행해야 합니다.

![스크린샷 2020-06-05 오후 6 09 51](https://user-images.githubusercontent.com/16849874/83877773-13e55e80-a776-11ea-81a7-e1a3a3ba5a30.png)

위의 이미지에서 언드로이드 아이콘을 눌러서 안드로이드 앱을 등록합니다.

![스크린샷 2020-06-05 오후 6 10 41](https://user-images.githubusercontent.com/16849874/83877783-18117c00-a776-11ea-8200-7a4af70ac5b7.png)

자신의 패키지 이름을 복사 붙여넣기를 해줍니다.

![스크린샷 2020-06-05 오후 6 11 08](https://user-images.githubusercontent.com/16849874/83877786-18117c00-a776-11ea-931e-fb8730a73618.png)

그 다음으로는 구글의 google-service.json을 받아서

![스크린샷 2020-06-05 오후 6 13 03](https://user-images.githubusercontent.com/16849874/83877789-18aa1280-a776-11ea-8142-3a2b24752ad8.png)

본 이미지의 경로에 넣어 주면 됩니다.

![스크린샷 2020-06-05 오후 6 15 20](https://user-images.githubusercontent.com/16849874/83877790-1942a900-a776-11ea-8b4a-8c5566653823.png)

그 다음은 gradle에 해당 코드를 추가하는데

```kotlin
    //파이어베이스 라이브러리 추가
    implementation 'com.google.firebase:firebase-core:15.0.0'
    implementation 'com.google.firebase:firebase-database:15.0.0'
```
해당 코드도 추가해 줍니다.

![스크린샷 2020-06-05 오후 6 20 05](https://user-images.githubusercontent.com/16849874/83877792-1942a900-a776-11ea-9dcc-961d633567e4.png)

테스트 용으로 실시간 데이터 베이스를 하나 만들어 줍니다.

![스크린샷 2020-06-05 오후 6 32 26](https://user-images.githubusercontent.com/16849874/83877793-19db3f80-a776-11ea-91a8-b8d11cb393b6.png)

키 - value로 이루어진 쌍을 하나 만들어 줍니다.

![스크린샷 2020-06-05 오후 6 21 12](https://user-images.githubusercontent.com/16849874/83877795-1a73d600-a776-11ea-9c53-2917b445a825.png)

그후 MainActivity에 코드를 만들어서 넣어 줍니다.

```kotlin

//파이어베이스의 test키를 가진 데이터의 참조객체를 가져온다.
val ref = FirebaseDatabase.getInstance().getReference("test")

//데이터가 변경이 될 경우 바로 적용이 된다.
//값의 변경이 있는 경우 이벤트 리스너를 추가한다.
ref.addValueEventListener(object : ValueEventListener {
    //데이터 읽기가 취소된 경우 호출 된다.
    //데이터의 권한이 없는 경우
    override fun onCancelled(error: DatabaseError) {
        error.toException().printStackTrace()
    }

    //데이터의 변경이 감지되면 호출된다.
    override fun onDataChange(snapshot: DataSnapshot) {
        //test 키를 가진 데이터 스냅샷에서 값을 읽고 문자열로 변경한다.
        val message = snapshot.value.toString()
        //읽은 문자열을 로깅한다.
        Log.d(TAG, message)
        Toast.makeText(applicationContext, message, Toast.LENGTH_LONG).show()
        //firebase에서 전달받은 메게지로 제목을 변경한다.
        supportActionBar?.title = message
    }
})
```

![스크린샷 2020-06-05 오후 6 32 28](https://user-images.githubusercontent.com/16849874/83877797-1a73d600-a776-11ea-8935-8e8a59e50fcb.png)

데이터 베이스를 고치면 어플안의 값이 바로 바뀌는 것을 볼 수 있습니다.