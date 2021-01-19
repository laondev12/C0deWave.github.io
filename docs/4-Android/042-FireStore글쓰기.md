---
layout: default
title: "파이어베이스 firestore를 이용한 게시판 글쓰기(1)"
parent: Android
nav_order: 42
---

# 파이어베이스 firestore를 이용한 게시판 글쓰기(1)

아래 링크를 참조하면 더 많은 도움이 됩니다.

Firebase문서 입니다.

[https://firebase.google.com/docs/firestore/quickstart?hl=ko](https://firebase.google.com/docs/firestore/quickstart?hl=ko)

1. 우선은 글쓰기 폼을 만들어 줍니다.

<img width="502" alt="스크린샷 2021-01-19 오후 8 30 17" src="https://user-images.githubusercontent.com/16849874/105029037-273f0300-5a95-11eb-8ac9-1af1eaa26fd5.png">

사진을 올리는 것은 나중에 구현하기로 하고 우선은 제목과 내용을 입력하려고 합니다.

2. 파이어베이스 콘솔에서 FireStore를 활성화 시켜줍니다.

<img width="1718" alt="스크린샷 2021-01-19 오후 8 36 34" src="https://user-images.githubusercontent.com/16849874/105029701-07f4a580-5a96-11eb-9328-e2620224560a.png">

그리고 나서 위의 xml의 일기 발행을 누르면 아래와 같이 실행이 되도록 만들었습니다.

```kotlin

var isWriteReady = false

saveDiaryButton.setOnClickListener {
        checkDiary()
        saveDiary()
    }

    //제목과 내용이 잘 적혀 있는지 확인을 합니다.
    private fun checkDiary() {
    val title = titleEditText_writeDiaryView.text.toString()
    val content = contentEditText_writeDiaryView.text.toString()

    if (title.isEmpty() || content.isEmpty()) {
        Toast.makeText(applicationContext, "내용을 입력해주세요.", Toast.LENGTH_SHORT).show()
        isWriteReady = false
    } else {
        isWriteReady = true
    }
}

//제목 내용이 적혀 있으면 해당 내용을 파이어베이스에 올립니다.
private fun saveDiary() {
    if (isWriteReady){
        val db = Firebase.firestore
        val user = FirebaseAuth.getInstance().currentUser
        var writeInfo = WriteInfo(
            titleEditText_writeDiaryView.text.toString(),
            contentEditText_writeDiaryView.text.toString()
        )

        val diary = hashMapOf(
            "title" to writeInfo.title,
            "content" to writeInfo.content
        )

        // Add a new document with a generated ID
        db.collection("diary")
            .add(diary)
            .addOnSuccessListener { documentReference ->
                Log.d("", "DocumentSnapshot added with ID: ${documentReference.id}")
                Toast.makeText(applicationContext, "일기를 적었습니다.!!!", Toast.LENGTH_SHORT).show()
                finish()
            }
            .addOnFailureListener { e ->
                Log.w("", "Error adding document", e)
            }
    }
}
```

위의 saveDiary()함수를 주의깊게 보면 됩니다.