---
layout: default
title: "피카소 라이브러리를 이용한 이미지 불러오기"
parent: Android
nav_order: 7
---

# 피카소 라이브러리를 이용한 이미지 불러오기

먼저 gradle 수정하기
```
//Picasso	 	 
def picassoVersion = "2.71828"	 	 
implementation "com.squareup.picasso:picasso:$picassoVersion"	
```

1. 먼저 이미지 리소스를 지정해 줍니다.

```kotlin
//  배경리스트 데이터
// res/drawable디렉토리에 있는 배경 이미지를 url주소로 이용한다.
// url 주소를 사용하면 추후 웹에 있는 이미지 URL도 바로 사용이 가능하다.
val bgList = mutableListOf(
    "android.resource://com.example.anonymoussns/drawable/gg",
    "https://imgtenasia.hankyung.com/webwp_kr/wp-content/uploads/2018/12/2018120622312089134-540x757.jpg",
    "https://img.icons8.com/material/4ac144/256/user-male.png"
)
```

2. 피카소 라이브러리를 이용하여 이미지를 특정 뷰에 적용해 줍니다.

```kotlin
//이미지 로딩 라이브러리인 피카소 객체로 뷰홀더에 존재하는 글쓰기 배경이미지뷰에 이미지 로딩
Picasso.get()
    .load(Uri.parse(bgList[position]))
    .fit()
    .centerCrop()
    .into(writeBackground)
```

---

결과 화면 

![스크린샷 2020-06-06 오후 11 00 11](https://user-images.githubusercontent.com/16849874/83946463-fda9d200-a84b-11ea-9a32-506230bf1a52.png)

위와 같이 이미지를 적용할 수 있습니다.

---

궁금점

밑의 아이콘은 .fit와 .centerCrop 함수가 적용되 있지 않을 때만 잘 나왔는데 왜 그렇게 되는지 아직 알아내지 못했습니다.