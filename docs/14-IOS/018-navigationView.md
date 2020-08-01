---
layout: default
title:  Navigation View 활용하기
parent: IOS
nav_order: 18
---

# Navigation View 활용하기

이번에는 네비게이션 뷰를 이용해서 화면전환과 화면 전환 사이에서 값을 주고 받는 방법을 알아볼 것이다.

먼저 Navigation Controller를 넣어주어야 한다.

<img width="1156" alt="스크린샷 2020-07-31 오후 3 14 03" src="https://user-images.githubusercontent.com/16849874/89094283-3c4bab00-d3fd-11ea-9e7b-7486f8c4f1c5.png">

위와 같이 메인 View에 Navigation Controller을 넣어준다.

그리고 새로운 뷰를 만들고 화면 전환을 넣어준다.

<img width="1053" alt="스크린샷 2020-08-01 오후 1 48 42" src="https://user-images.githubusercontent.com/16849874/89094324-b8de8980-d3fd-11ea-9b2c-e943155814e7.png">

컨트롤 드래그를 하면 화면전환에 show를 눌러서 설정하면 된다.

그러면 따로 코드로 지정하지 않아도 화면 전환이 이루어진다.

<img width="266" alt="스크린샷 2020-08-01 오후 1 49 59" src="https://user-images.githubusercontent.com/16849874/89094347-e592a100-d3fd-11ea-9627-4fbab5d7daab.png">

새로 만든 뷰는 새로 ViewController을 만들어서 연결해 주도록 하자

기본 UIViewController을 만들면 된다.

이제 코드상에서 값을 전송하고 대답을 받는 방법에 대해 알아보도록 하겠다.

먼저 값을 전송하는 방법을 알아보자.

<img width="908" alt="스크린샷 2020-08-01 오후 1 53 27" src="https://user-images.githubusercontent.com/16849874/89094397-65207000-d3fe-11ea-9b7e-f1f5b52fe609.png">

먼저 화면을 전환하는 전환의 identifier을 만들어 주면 전환별로 구분이 가능하다.

그후에 prepare함수를 오버라이드 해주면 된다.

```swift
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        //가고자 하는 목적지의 변수를 조작한다.
        let editViewController = segue.destination as! EditViewController
        
        if segue.identifier == "editBarButton"{
            //버튼을 클릭한 경우
            editViewController.textWayValue = "segue: use Button"
        }else{
            //바 버튼을 클릭한 경우
            editViewController.textWayValue = "use bar button"
        }
        editViewController.textMessage = txtMessage.text!
        editViewController.delegate = self
    }
```

값을 위와 같이 전달을 하면 이제는 값을 반환 받는 방법에 대해서도 알아보자.

먼저 값을 반환하고자 하는 뷰에서 프로토콜을 만들어주어야 한다.

```swift
protocol EditDelegate {
    func didMessageEditDone(_ controller: EditViewController, message: String)
    func didImageOnOffDoone(_ controller: EditViewController, isOn: Bool)
    func didSizeBig(_ controller: EditViewController, isBig: Bool)
}

함수의 형태로 값을 반환하게 된다.
```

그 후 값을 받는 뷰에서 해당 프로토콜을 임포트 시킨다.

```swift
class ViewController: UIViewController, EditDelegate
```

그후 해당 프로토콜의 함수를 오버라이딩 해주면 된다.

이러면 값을 반환 받을 수 있다.

```swift
    func didMessageEditDone(_ controller: EditViewController, message: String) {
        txtMessage.text = message
    }
```