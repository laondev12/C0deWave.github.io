---
layout: default
title: "구글 리더보드 연동 시키기"
parent: Android
nav_order: 5
---

# 구글 리더보드 연동하기

먼저 연동을 위해서 구글 플레이 콘솔에 로그인을 해야 합니다.

https://play.google.com/apps/publish/

<img width="1443" alt="스크린샷 2020-05-30 오후 1 48 32" src="https://user-images.githubusercontent.com/16849874/83347934-751ac580-a363-11ea-8bc4-dce2fdfc6d8b.png">

옆의 게임 서비스를 누릅니다.

그후에 구글플레이 게임 서비스 설정을 눌러 줍니다.

<img width="564" alt="스크린샷 2020-05-30 오후 1 49 16" src="https://user-images.githubusercontent.com/16849874/83347932-751ac580-a363-11ea-88fd-86aed34bd2f7.png">

연동하고자 하는 어플의 이름과 종류를 눌러 줍니다.


<img width="1498" alt="스크린샷 2020-05-30 오후 1 53 20" src="https://user-images.githubusercontent.com/16849874/83347926-721fd500-a363-11ea-8e22-366247f358a9.png">

리더보드탭으로 가서 리더보드 추가를 눌러줍니다.

<img width="1470" alt="스크린샷 2020-05-30 오후 1 53 54" src="https://user-images.githubusercontent.com/16849874/83347923-6df3b780-a363-11ea-8ccb-176a9149e04c.png">

빈 내용을 추가해서 넣어줍니다.

아래는 연결된 앱을 눌러서 어플과 리더보드를 연동합니다.

<img width="1474" alt="스크린샷 2020-05-30 오후 1 50 04" src="https://user-images.githubusercontent.com/16849874/83347931-74822f00-a363-11ea-9c0a-7b6868c69792.png">
<img width="1498" alt="스크린샷 2020-05-30 오후 1 51 33" src="https://user-images.githubusercontent.com/16849874/83347928-73510200-a363-11ea-8c69-fa7ba5dccfa3.png">
<img width="1472" alt="스크린샷 2020-05-30 오후 1 51 08" src="https://user-images.githubusercontent.com/16849874/83347929-73e99880-a363-11ea-85d9-70f8565cdf10.png">
<img width="1499" alt="스크린샷 2020-05-30 오후 1 52 50" src="https://user-images.githubusercontent.com/16849874/83347927-72b86b80-a363-11ea-8753-1a93f7daae12.png">

연결된 앱에도 들어가서 클라이언트 앱을 연동시켜 줍니다.

테스트 계정도 추가해 줍니다.

<img width="1479" alt="스크린샷 2020-05-30 오후 1 54 50" src="https://user-images.githubusercontent.com/16849874/83347936-81068780-a363-11ea-8203-26b332ce25d6.png">

이렇게 구한 ID와 앱 이름을 구해서 안드로이드에 연동할 계획입니다.

<img width="1492" alt="스크린샷 2020-05-30 오후 1 54 09" src="https://user-images.githubusercontent.com/16849874/83347939-8a8fef80-a363-11ea-9ffc-c0e3d6118d44.png">
<img width="1344" alt="스크린샷 2020-05-30 오후 1 57 34" src="https://user-images.githubusercontent.com/16849874/83347944-8d8ae000-a363-11ea-9387-7b0a68cea8b9.png">

## 안드로이드 스튜디오에서 연동하기

우선은 안드로이드 스튜디오를 열고 

res - values - ids.xml 을 만들어 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--구글 게임 서비스 앱 아이디-->
    <string name="app_id" translatable="false">앱 아이디를 적습니다.</string>

    <!--leaderboard 아이디-->
    <string name="leaderboard_id" translatable="false">리더보드 키값을 적습니다.</string>
</resources>
```

또한 Androidmanifest 안에도 의존성을 추가해 줍니다.

```xml
<!--구글 게임 관련 메타데이터 추가-->
  <meta-data android:name="com.google.android.gms.games.APP_ID" android:value="@string/app_id"/>
  <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version"/>
```
이렇게 하면 리더보드와 연동할 준비는 끝났습니다.

리더보드 연동을 하기 위한 액티비티에서 해당 코드를 적어 줍니다.

```kotlin
class ResultActivity : AppCompatActivity() {

    //구글 리더보드 연동
    val RC_SIGN_IN = 9001
    val RC_LEADERBOARD_UI = 9004
    val signClient: GoogleSignInClient by lazy {
        GoogleSignIn.getClient(this@ResultActivity, GoogleSignInOptions.DEFAULT_GAMES_SIGN_IN)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_result)

        rankingbutton.setOnClickListener {
            //인증된 유저 객체 가져옴
            if (GoogleSignIn.getLastSignedInAccount(this) == null) {
                //sign in 이 필요하다.
                startActivityForResult(signClient.signInIntent, RC_SIGN_IN)
            } else {
                //점수 업로드
                uploadScore()
            }
        }
    }

    fun uploadScore() {
        //인증된 유저 객체를 가져옴
        var user = GoogleSignIn.getLastSignedInAccount(this)
        user?.let {
            val leaderboard = Games.getLeaderboardsClient(this@ResultActivity, user)

//            리더보드 객체에 점수를 즉시 올림
            leaderboard.submitScoreImmediate(getString(R.string.leaderboard_id), power.toLong())
                .addOnSuccessListener { leaderboard.getLeaderboardIntent(getString(R.string.leaderboard_id))
                    .addOnSuccessListener { intent -> startActivityForResult(intent, RC_LEADERBOARD_UI)
                            Toast.makeText(applicationContext,"점수를 올렸습니다."+"${power}",Toast.LENGTH_LONG).show() }
                }
        }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (resultCode == RC_SIGN_IN) {
            val result = Auth.GoogleSignInApi.getSignInResultFromIntent(data)
            //인증 성공인 경우
            if (result.isSuccess) {
                uploadScore()
            } else {
                var message = result.status.statusMessage
                if (message == null || message.isEmpty()) {
                    message = "로그인 오류!!"
                }
                Toast.makeText(applicationContext, message, Toast.LENGTH_LONG).show()
            }

        }
    }
}

```

## 최종 결과 화면은 다음과 같이 구성됩니다.

아래와 같이 구글 리더보드에 점수가 올라가서 다른이드롸 경쟁할 수 있습니다.

<img width="1296" alt="스크린샷 2020-05-30 오후 2 46 00" src="https://user-images.githubusercontent.com/16849874/83347953-a72c2780-a363-11ea-940b-1525a22ff358.png">

연동이 되지 않는 경우 설정 부분 으로 들어가면 구글 플레이를 누르고 업데이트를 해준뒤 안드로이드 스튜디오를 재시동해서 실행해 봅니다.

<img width="1321" alt="스크린샷 2020-05-30 오후 2 42 28" src="https://user-images.githubusercontent.com/16849874/83347947-9085d080-a363-11ea-8f01-b82bd96d3de1.png">