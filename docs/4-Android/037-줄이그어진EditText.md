---
layout: default
title: "일기장 느낌의 메모 레이아웃 구성하기"
parent: Android
nav_order: 37
---

# 일기장 느낌의 메모 레이아웃 구성하기"

이번에는 일기장 느낌의 메모장을 만들 계획입니다.

먼저 결과물을 보도록 하겠습니다.

<img width="507" alt="스크린샷 2021-01-18 오후 8 11 41" src="https://user-images.githubusercontent.com/16849874/104908157-630c9680-59c9-11eb-93cc-09ee0ddb4e6f.png">

핵심 부분은 내용 부분입니다.

아직 글을 입력하지 않았음에도 불구하고 미리 줄이 생겨나 있는것을 볼 수 있습니다.

이를 만들기 위해서는 먼저 클래스를 하나 만들어야 합니다.

```kotlin
package com.example.howtoday.classFile;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.text.Editable;
import android.text.TextWatcher;
import android.util.AttributeSet;
import android.widget.Toast;

//글을 적지 않아도 밑줄이 있는 상태에서 시작할 수 있게 해주는 소스입니다.


public class LinedEditText extends androidx.appcompat.widget.AppCompatEditText {
    private Paint mPaint = new Paint();

    /**
     * Max lines to be present in editable text field
     */
    private int maxLines = 30;

    /**
     * Max characters to be present in editable text field
     */
    private int maxCharacters = 1000;
    /**
     * application context;
     */
    private Context context;

    public int getMaxCharacters() {
        return maxCharacters;
    }

    public void setMaxCharacters(int maxCharacters) {
        this.maxCharacters = maxCharacters;
    }

    @Override
    public int getMaxLines() {
        return maxLines;
    }

    @Override
    public void setMaxLines(int maxLines) {
        this.maxLines = maxLines;
    }

    public LinedEditText(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        this.context = context;
        initPaint();
    }

    public LinedEditText(Context context, AttributeSet attrs) {
        super(context, attrs);
        this.context = context;
        initPaint();
    }

    public LinedEditText(Context context) {
        super(context);
        this.context = context;
        initPaint();
    }

    private void initPaint() {
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setColor(0xFFFFFFFF);
    }

    @Override protected void onDraw(Canvas canvas) {
        int left = getLeft();
        int right = getRight();
        int paddingTop = getPaddingTop();
        int paddingBottom = getPaddingBottom();
        int paddingLeft = getPaddingLeft();
        int paddingRight = getPaddingRight();
        int height = getHeight();
        int lineHeight = getLineHeight();
        int count = (height-paddingTop-paddingBottom) / lineHeight;

        for (int i = 0; i < count+20; i++) {
            int baseline = lineHeight * (i+1) + paddingTop;
            canvas.drawLine(left-20, baseline-15, right, baseline-15, mPaint);
        }

        super.onDraw(canvas);
    }

    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();

        TextWatcher watcher = new TextWatcher() {

            private String text;
            private int beforeCursorPosition = 0;

            @Override
            public void onTextChanged(CharSequence s, int start, int before,
                                      int count) {
                //TODO sth
            }

            @Override
            public void beforeTextChanged(CharSequence s, int start, int count,
                                          int after) {
                text = s.toString();
                beforeCursorPosition = start;
            }

            @Override
            public void afterTextChanged(Editable s) {

                /* turning off listener */
                removeTextChangedListener(this);

                /* handling lines limit exceed */
                if (LinedEditText.this.getLineCount() > maxLines) {
                    LinedEditText.this.setText(text);
                    LinedEditText.this.setSelection(beforeCursorPosition);
                    Toast.makeText(context, "최대 30줄 까지 입니다.", Toast.LENGTH_SHORT)
                            .show();
                }

                /* handling character limit exceed */
                if (s.toString().length() > maxCharacters) {
                    LinedEditText.this.setText(text);
                    LinedEditText.this.setSelection(beforeCursorPosition);
                    Toast.makeText(context, "text too long", Toast.LENGTH_SHORT)
                            .show();
                }

                /* turning on listener */
                addTextChangedListener(this);

            }
        };

        this.addTextChangedListener(watcher);
    }
}
```

텍스트에 미리 밑줄을 치는것 뿐만이 아니라 MultiLine에서 maxline이 적용되지 않는 에러도 해결했습니다.

이를 디자인을 할때 xml에서 선언을 해주면 됩니다.

```xml
<com.example.howtoday.classFile.LinedEditText
                android:id="@+id/contentEditText_writeDiaryView"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"

                android:layout_marginLeft="10dp"
                android:layout_marginTop="10dp"
                android:layout_marginRight="10dp"
                android:layout_marginBottom="10dp"
                android:gravity="top"

                android:hint="내용"
                android:inputType="textMultiLine"
                android:lineSpacingExtra="10dp"
                android:lineSpacingMultiplier="1"
                android:lines="10"
                android:maxLength="1000"
                android:maxLines="10"
                android:minLines="10"
                android:overScrollMode="never"
                android:textSize="20dp"
                app:layout_constraintBottom_toBottomOf="parent"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/titleEditText_writeDiaryView" />
```

아래의 두 문장은 xml에서 줄과 줄 사이의 길이를 조절하는 역할을 합니다.

android:lineSpacingExtra="10dp"
android:lineSpacingMultiplier="1"

한국어를 기준으로 할 경우에는 위의 클래스 파일에서

int lineHeight = getLineHeight() + 10;

으로 바꾸어 주어야 한다.