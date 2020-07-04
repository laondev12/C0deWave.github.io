---
layout: default
title: "파이어베이스 Storage사용해 보기"
parent: Android
nav_order: 27
---

# 파이어베이스 Storage사용해 보기

참조

[https://firebase.google.com/docs/storage/android/start](https://firebase.google.com/docs/storage/android/start)

또한 파이어베이스 콘솔에서 Storage를 활성화 해야한다.

이번에는 파이어베이스의 많은 다양한 기능중 하나인 Storage를 사용하는 방법을 알아 보고자 한다.

나는 이번에 앨범에서 사진을 끌어와서 이를 파이어베이스의 Storage 서버에 올리고자 한다.

먼저 나는 아래와 같은 형태의 글 작성 페이지를 이용할 것이다.

<img width="371" alt="스크린샷 2020-07-04 오후 1 13 13" src="https://user-images.githubusercontent.com/16849874/86505028-b25af300-bdfa-11ea-9768-fcbd08b22634.png">

여기서 이미지 추가를 했을때의 기능을 알아보자.

```kotlin
galleryButton.setOnClickListener {
    var intent = Intent(Intent.ACTION_PICK)
    intent.setDataAndType(
        android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
        "image/*"
    )
    startActivityForResult(intent, GET_GALLERY_IMAGE)
}
```

앨범에서 사진 이미지를 고르게 된다.

여기서 사진의 URI를 onActivityResult로 받아서 이를 파이어 베이스 서버에 올릴 것이다.

먼저 전역변수를 지정해준다.

```kotlin
val GET_GALLERY_IMAGE = 200;

//선택된 이미지의 Uri
var selectImageUri : Uri? = null
```

결과를 받았을 때의 코드이다.

```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)
    if (requestCode == GET_GALLERY_IMAGE && resultCode == RESULT_OK && data != null && data.data != null){
        selectImageUri = data.data!!
        imageView6.setImageURI(selectImageUri)
    }
}
```

받아온 사진의 Uri를 전역변수 selectImageUri에 지정해 주었다.

이제 이 Uri 사진 이미지를 파이어베이스 서버에 올리면 된다.

게시글 올리기 버튼을 눌렀을 때

```kotlin
/*게시글 올리기 버튼*/
sendButton.setOnClickListener {
    // 내용이 적혀 있는지 확인하는 변수입니다.
    if (contentEditText.text.isEmpty() || titleEditText.text.isEmpty()) {
        Toast.makeText(requireContext(), "제목과 내용을 입력해 주세요.", Toast.LENGTH_LONG).show()
        return@setOnClickListener
    }
    
    //Post객체생성
    val post = Post()
    
    //참조값을 할당합니다.
    val newRef = FirebaseDatabase.getInstance().getReference("Posts").push()

    //데이터를 할당합니다.
    //이미지를 업로드할 경로를 지정해 줍니다.
    val storage = FirebaseStorage.getInstance().getReference("/image/${post.postId}")

    //이미지의 Uri로 Storage에 파일을 업로드합니다.
    val upload = selectImageUri?.let { it1 -> storage.putFile(it1) }

    //업로드가 완료 되었을 때의 로그를 표시하게 합니다.
    upload?.addOnFailureListener{
        Log.d("image","이미지 업로드 실패")
    }?.addOnSuccessListener {
        Log.d("image","이미지 업로드 성공")
    }

    //파이어베이스에 올린 이미지의 Uri를 받아옵니다.
    //Uri를 받으면 이를 파이어베이스 실시간 데이터베이스에 넣어소 올려줍니다.
    val urlTask = upload?.continueWithTask{ task ->
        if (!task.isSuccessful){
            task.exception?.let {
                throw it
            }
        }
        storage.downloadUrl
    }?.addOnCompleteListener { task ->
        if (task.isSuccessful){
            post.bgUri = task.result.toString()

            newRef.setValue(post)
            Toast.makeText(requireContext(),"아마자 추가해서 저장 성공!!!",Toast.LENGTH_LONG).show()

            (activity as MainActivity).setFragment(noticeBoardFragment.newInstance())
        }
    }

    //이미지가 지정되지 않았을 때 그냥 업로드를 합니다.
    if(urlTask == null) {
        Log.d("게시글 업로드", "게시글 업로")
        newRef.setValue(post)
        Toast.makeText(requireContext(), "이미지 없이 저장 성공!!!", Toast.LENGTH_LONG).show()

        (activity as MainActivity).setFragment(noticeBoardFragment.newInstance())
    }

}
```

---

이렇게 되면 파이어베이스의 Storage에 이미지가 올라가게 됩니다.

![스크린샷 2020-07-04 오후 1 03 40](https://user-images.githubusercontent.com/16849874/86505169-66a94900-bdfc-11ea-971d-9b60436823e7.png)

이제 이 링크를 파이어베이스의 실시간 데이터베이스에 링크만 첨부하여 같이 넣어주면 됩니다.

![스크린샷 2020-07-04 오후 1 07 54](https://user-images.githubusercontent.com/16849874/86505184-8b9dbc00-bdfc-11ea-9861-1853f9276c4e.png)