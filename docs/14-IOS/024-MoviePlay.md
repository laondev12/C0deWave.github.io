---
layout: default
title:  동영상 재생하기
parent: IOS
nav_order: 24
---

# 동영상 재생하기

이번에는 동영상을 재생하는 방법에 대해 알아보고자 합니다.

먼저 영상을 재생하기 위해서는 헤더를 지정해 주어야 합니다.

```swift
import AVKit
```

그 다음으로는 영상 파일을 지정해 주어야 하는데 앱 내부의 위치나 링크를 이용해서 지정을 해주면 됩니다.

```swift
        let filePath:String? = Bundle.main.path(forResource: "FastTyping", ofType: "mp4")
        let url = NSURL(fileURLWithPath: filePath!)

        // 또는
        let url = NSURL(string: "https://dl.dropboxusercontent.com/s/e38auz050w2mvud/Fireworks.mp4")!
```

마지막으로는 영상을 재생하는 함수를 만들어서 이용해주면 됩니다.

```swift
private func playVideo(url: NSURL)  {
    let playerController = AVPlayerViewController()
    
    let player = AVPlayer(url: url as URL)
    playerController.player = player
    
    self.present(playerController, animated: true) {
        player.play()
    }
}
```

이제 해당 함수로 url을 호출해서 이용하면 됩니다.

```swift
        playVideo(url: url)
```

![스크린샷 2020-08-06 오전 6 37 50](https://user-images.githubusercontent.com/16849874/89642647-218ea000-d8ef-11ea-910e-134f8a91cfd9.png)
