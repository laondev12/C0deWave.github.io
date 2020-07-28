---
layout: default
title: Alert 활용하기
parent: IOS
nav_order: 9
---

# Alert 활용하기

Alert는 알람, 경고의 역할 입니다.

<img width="470" alt="스크린샷 2020-07-29 오전 1 03 31" src="https://user-images.githubusercontent.com/16849874/88691062-548aa400-d137-11ea-90b5-bfe6be7779a2.png">

이러한 경고창을 만들기 위해서는 먼저 AlertController을 만들고 그 안에 AlertAction을 넣어준 다음에 이를 나타나게 해주면 됩니다.


핵심 코드만 소개하겠습니다.

램프의 제거 버튼을 눌렀을 떄의 코드 입니다.

```swift
    @IBAction func btnLampRemove(_ sender: UIButton) {
        let lampRemoveAlert = UIAlertController(title: "램프제거", message: "램프를 제거하시겠습니까??", preferredStyle: UIAlertController.Style.alert)
        
        let offAction = UIAlertAction(title: "아니요 끕니다(off)", style: UIAlertAction.Style.default, handler: {
            ACTION in self.lampImage.image = self.imgOff
            self.isLampOn = false
        })
        
        let onAction = UIAlertAction(title: "아니요 킵니다.(on)", style: UIAlertAction.Style.default, handler: {
            ACTION in self.lampImage.image = self.imgOn
            self.isLampOn = true
        })
        
        let removeAction = UIAlertAction(title: "네 제거합니다.", style: UIAlertAction.Style.default, handler: {
            ACTION in self.lampImage.image = self.imgRemove
            self.isLampOn = false
        })
        
        lampRemoveAlert.addAction(offAction)
        lampRemoveAlert.addAction(onAction)
        lampRemoveAlert.addAction(removeAction)
        
        present(lampRemoveAlert, animated: true, completion: nil)
    }
```