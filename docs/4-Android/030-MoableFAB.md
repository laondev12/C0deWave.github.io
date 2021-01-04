---
layout: default
title: "MoableFAB"
parent: Android
nav_order: 30
---

# MoableFAB

이번에는 이전에 만든 FAB울 드래그하거나 움직일 수 있게 만들 생각이다.

![스크린샷 2021-01-03 오후 11 45 11](https://user-images.githubusercontent.com/16849874/103530851-887aaa00-4ecb-11eb-940a-b275149903a3.png)

방법은 정말 간단하다.

먼저 안드로이드 프로젝트 내부에 자바 파일을 만들어 준다.

아래의 내용을 복사해서 넣어준다.

패키지와 FAB의 상속은 상황에 맞게 처리한다.

```java
package com.example.howtoday;

import android.content.Context;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewGroup;

import com.google.android.material.floatingactionbutton.FloatingActionButton;

public class MovableFloatingActionButton extends FloatingActionButton implements View.OnTouchListener {

    private final static float CLICK_DRAG_TOLERANCE = 10; // Often, there will be a slight, unintentional, drag when the user taps the FAB, so we need to account for this.

    private float downRawX, downRawY;
    private float dX, dY;

    public MovableFloatingActionButton(Context context) {
        super(context);
        init();
    }

    public MovableFloatingActionButton(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public MovableFloatingActionButton(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init() {
        setOnTouchListener(this);
    }

    @Override
    public boolean onTouch(View view, MotionEvent motionEvent){

        ViewGroup.MarginLayoutParams layoutParams = (ViewGroup.MarginLayoutParams)view.getLayoutParams();

        int action = motionEvent.getAction();
        if (action == MotionEvent.ACTION_DOWN) {

            downRawX = motionEvent.getRawX();
            downRawY = motionEvent.getRawY();
            dX = view.getX() - downRawX;
            dY = view.getY() - downRawY;

            return true; // Consumed

        }
        else if (action == MotionEvent.ACTION_MOVE) {

            int viewWidth = view.getWidth();
            int viewHeight = view.getHeight();

            View viewParent = (View)view.getParent();
            int parentWidth = viewParent.getWidth();
            int parentHeight = viewParent.getHeight();

            float newX = motionEvent.getRawX() + dX;
            newX = Math.max(layoutParams.leftMargin, newX); // Don't allow the FAB past the left hand side of the parent
            newX = Math.min(parentWidth - viewWidth, newX); // Don't allow the FAB past the right hand side of the parent

            float newY = motionEvent.getRawY() + dY;
            newY = Math.max(layoutParams.topMargin, newY); // Don't allow the FAB past the top of the parent
            newY = Math.min(parentHeight - viewHeight, newY); // Don't allow the FAB past the bottom of the parent

            view.animate()
                    .x(newX)
                    .y(newY)
                    .setDuration(0)
                    .start();

            return true; // Consumed

        }
        else if (action == MotionEvent.ACTION_UP) {

            float upRawX = motionEvent.getRawX();
            float upRawY = motionEvent.getRawY();

            float upDX = upRawX - downRawX;
            float upDY = upRawY - downRawY;

            if (Math.abs(upDX) < CLICK_DRAG_TOLERANCE && Math.abs(upDY) < CLICK_DRAG_TOLERANCE) { // A click
                return performClick();
            }
            else { // A drag
                return true; // Consumed
            }

        }
        else {
            return super.onTouchEvent(motionEvent);
        }

    }
}
```

그 다음으로는 기존에 만든 xml파일에서의 fab을 MovableFloatingActionButton으로 바꾸어주면 끝이다.

```xml
<!--    <com.google.android.material.floatingactionbutton.FloatingActionButton-->
<!--        android:id="@+id/fab"-->
<!--        android:layout_width="wrap_content"-->
<!--        android:layout_height="wrap_content"-->
<!--        android:contentDescription="OrderDiary"-->
<!--        app:layout_anchor="@id/bottomAppBar"-->
<!--        android:src="@drawable/ic_android_black_24dp" />-->
    <com.example.howtoday.MovableFloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:contentDescription="OrderDiary"
        app:layout_anchor="@id/bottomAppBar"
        android:src="@drawable/ic_android_black_24dp" />
```

그러면 fab을 드래그 할수 있게 된다.

이를 이용해서 나중에는 드래그 했다가 놓으면 원래위치로 돌아오는 애니메이션도 만들어볼 계획이다.