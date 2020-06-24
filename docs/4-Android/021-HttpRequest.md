---
layout: default
title: "Volley를 이용한 Http요청하기"
parent: Android
nav_order: 21
---

# Volley를 이용한 Http요청하기

이번에는 Volley를 이용한 Http호출을 해보도록 하겠다.

개발자 문서를 거의 그대로 실습해본거라 그리 다른점은 없다.

[https://developer.android.com/training/volley/simple](https://developer.android.com/training/volley/simple)

내용은 간단하다.

textView를 하나 만들고 Http 요청을 한 결과를 반환해주는 역할을 한다.

그전에 gradle를 먼저 수정해 주면 된다.

```kotlin
//간편한 http request를 위한 volley
implementation 'com.android.volley:volley:1.1.1'
```

나는 변수에 값을 넣어주는 방향으로 했다.

```kotlin
//volley로 합격률 api값 가져오기
var result : String? = null

// Instantiate the RequestQueue.
val queue = Volley.newRequestQueue(context)
val url = "여기에 호출하고자 하는 url을 적으면 된다."

// Request a string response from the provided URL.
val stringRequest = StringRequest(
    Request.Method.GET, url,
    Response.Listener<String> { response ->
        // Display the first 500 characters of the response string.
        result = response
        Toast.makeText(context,"Volley잘 작동함",Toast.LENGTH_LONG).show()
    },
    Response.ErrorListener { Toast.makeText(context,"Volley작동 안함",Toast.LENGTH_LONG).show() })

// Add the request to the RequestQueue.
queue.add(stringRequest)
```

![스크린샷 2020-06-24 오후 9 21 42](https://user-images.githubusercontent.com/16849874/85555295-bc455f00-b660-11ea-9630-29e5c6ee2160.png)

결과가 Toast로 잘 호출 받았음을 알 수 있다.