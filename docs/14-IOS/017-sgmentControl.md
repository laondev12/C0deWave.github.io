---
layout: default
title:  세그먼트 컨트롤
parent: IOS
nav_order: 17
---

# 세그먼트 컨트롤

세그먼트 컨트롤이란 무엇인가??

라디오 버튼과 비슷하다

아래의 그림을 보면 이해가 빠르다.

<img width="288" alt="스크린샷 2020-08-01 오전 11 59 38" src="https://user-images.githubusercontent.com/16849874/89092691-f851a980-d3ee-11ea-9fe6-2627dafae3fb.png">

이런 버튼이 세그먼트 컨트롤이다.

이를 위해서 먼저 뷰에 세그먼트 컨트롤을 추가한다.

<img width="686" alt="스크린샷 2020-08-01 오후 12 04 40" src="https://user-images.githubusercontent.com/16849874/89092716-2f27bf80-d3ef-11ea-90b0-ed7c010aaa7a.png">

이제 세그먼트 컨트롤의 개수를 조정하고 이름을 정해준다.

<img width="256" alt="스크린샷 2020-08-01 오후 12 07 35" src="https://user-images.githubusercontent.com/16849874/89092783-cdb42080-d3ef-11ea-8eaa-fe75be9e01e4.png">

맨 마지막의 segments의 개수를 조절하면 된다.

이제 아웃렛 변수를 만들고 이를 이용해서 분기문을 만들어 주면된다.

예시

```swift
@IBAction func sgChangeLocation(_ sender: UISegmentedControl) {
    if sender.selectedSegmentIndex == 0 {

    }else if sender.selectedSegmentIndex == 1{

    }else if sender.selectedSegmentIndex == 2{

    }else if sender.selectedSegmentIndex == 3{

    }
}
```