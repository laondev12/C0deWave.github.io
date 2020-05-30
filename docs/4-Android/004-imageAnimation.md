---
layout: default
title: "안드로이드 이미지 애니메이션 넣기"
parent: Android
nav_order: 4
---
# 안드로이드 이미지 애니메이션 넣기

## 안귀정 저자님의 안드로이드 with Kotlin책을 보면서 예제를 실습한 내용을 정리했습니다.

---

## 뷰 애니메이션

우선은 안드로이드의 res 폴더안에 anim 폴더를 만들어 주고 안에 xml파일을 만들어서 애니메이션을 만든다.

![스크린샷 2020-05-30 오후 10 10 27](https://user-images.githubusercontent.com/16849874/83330126-253af080-a2c8-11ea-8669-1163645d1eaa.png)

우선은 이동을 하는 애니메이션 xml이다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:duration="300"
        android:repeatMode="reverse"
        android:repeatCount="infinite"
        android:fromYDelta="50%"
        android:toYDelta="-100%"
        ></translate>
</set>
```

아래는 회전을 하는 애니메이션 파일이다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <rotate
        android:fromDegrees="0"
        android:toDegrees="360"
        android:pivotX="50%"
        android:pivotY="50%"
        android:duration="300"
        android:repeatMode="restart"
        android:repeatCount="infinite"/>
</set>
```

아래는 크기및 투명도를 조절하는 애니메이션이다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <scale
        android:duration="300"
        android:fromXScale="1.0"
        android:fromYScale="1.0"
        android:pivotX="50%"
        android:pivotY="50%"
        android:repeatCount="infinite"
        android:repeatMode="reverse"
        android:toXScale="1.5"
        android:toYScale="1.5" />

    <alpha
        android:duration="300"
        android:fromAlpha="1.0"
        android:repeatCount="infinite"
        android:repeatMode="reverse"
        android:toAlpha="0.5" />
</set>
```

그 다음에는 변수를 초기화 하는 공간에 해당 코드를 넣어서 이미지와 xml을 연결시키면 된다.

참고로 startOffset를 넣어주면 애니메이션 시작 대기시간을 지정할 수 있다.

```kotlin
        //애니메이션 시작
        val animation = AnimationUtils.loadAnimation(this@MainActivity,R.anim.alpha_scale)
        //앞은 xml id값이다.
        imageView.startAnimation(animation)

        //애니메이션 리스너 설정
        //특정 기능을 만들때 아래 부분을 이용하면 된다.
        animation.setAnimationListener(object  : Animation.AnimationListener{
            override fun onAnimationRepeat(animation: Animation?) {
            //애니메이션이 반복할 때의 처리 코드를 작성한다.
            }

            override fun onAnimationStart(animation: Animation?) {
            //애니메이션이 시작할때의 코드를 작성한다.
            }

            override fun onAnimationEnd(animation: Animation?) {
            //애니메이션이 종료될 때의 코드를 작성한다.
            }
        })
```

또한 애니메이션을 작동시키지 않고 싶다면 아래와 같은 코드를 이용하면 된다.

```kotlin
 //애니메이션 제거
imageView.clearAnimation()
```

---

## 속성 애니메이션

여기까지는 뷰 애니메이션이고 더욱 다양한 방법을 이용하기 위해서는 속성 애니메이션을 이용해야 한다.

속성 애니메이션은 API 11 이상에서 작동한다.

속성 애니메이션은 animator 폴더를 이용한다.

![스크린샷 2020-05-30 오후 10 51 02](https://user-images.githubusercontent.com/16849874/83330116-181e0180-a2c8-11ea-8f62-3d66b0e8e5fe.png)


마찬가지로 xml을 만들어 본다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:ordering="sequentially">
    <!--만약 valueFrom 속성이 없다면 현재 속성 부터 시작한다. -->
    <!--y축 이동 애니메이션-->
    <objectAnimator
        android:duration="300"
        android:propertyName="y"
        android:repeatCount="1"
        android:repeatMode="reverse"
        android:valueFrom="700"
        android:valueTo="400"
        android:valueType="floatType" />
    <!--x축 이동 애니메이션-->
    <objectAnimator
        android:duration="300"
        android:propertyName="x"
        android:repeatCount="1"
        android:repeatMode="reverse"
        android:valueFrom="400"
        android:valueTo="800"
        android:valueType="floatType" />
    <!--알파 애니메이션-->
    <objectAnimator
        android:duration="300"
        android:propertyName="alpha"
        android:repeatCount="1"
        android:repeatMode="reverse"
        android:valueTo="0.2"
        android:valueType="floatType" />
    <!--rotate 애니메이션-->
    <objectAnimator
        android:duration="300"
        android:propertyName="rotation"
        android:repeatCount="1"
        android:repeatMode="reverse"
        android:valueTo="360"
        android:valueType="floatType" />

    <!--애니메이션 set 내부에 중첩으로 set 사용가능-->
    <!--set으로 묶인 애니메이션은 한꺼번에 실행된다.-->
    <set android:ordering="together">
        <!--        X축 스케일 애니메이션-->

        <objectAnimator
            android:duration="300"
            android:propertyName="scaleX"
            android:repeatCount="1"
            android:repeatMode="reverse"
            android:valueTo="2.0"
            android:valueType="floatType" />

        <!--Y축 스케일 애니메이션-->
        <objectAnimator
            android:duration="300"
            android:propertyName="scaleY"
            android:repeatCount="1"
            android:repeatMode="reverse"
            android:valueTo="2.0"
            android:valueType="floatType" />

    </set>
</set>
```

속성 애니메이션은 이미지와 연결하는 방법도 살짝 다르다.

```kotlin
        //속성 애니메이션 이용하기
        AnimatorInflater.loadAnimator(this@MainActivity,R.animator.property_animator).apply {
            //애니매이션 리스너를 추가
            addListener(object : AnimatorListenerAdapter(){
                override fun onAnimationEnd(animation: Animator?) {
                    start()
                }
            })
            setTarget(imageView)
            start()
        }
```

AnimationInflater을 이용해서 애니메이션을 연결한다.

위의 방법으로 무한 반복을 할 수 있다.