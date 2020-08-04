---
layout: default
title:  Audio 재생하기
parent: IOS
nav_order: 21
---

# Audio 재생하기

음악을 재생하는 핵심적인 부분만 다루어 보았습니다.

먼저 음악을 재생하기 위해서는 헤더파일이 필요합니다.

```swift
import AVFoundation
```

또한 AudioPlayerDelegate를 상속 받아야 합니다.

```swift
class ViewController: UIViewController ,AVAudioPlayerDelegate
```

이제는 음악을 재생하는 방법을 알아보겠습니다.

먼저 변수의 선언부터 하겠습니다.

```swift
//Avplayer인스턴스 변수
var audioPlayer: AVAudioPlayer!
//재생할 오디오의 파일명 변수
var audioFile: URL!
//최대 볼륨 실수형 상수
let MAX_VOLUME : Float = 10.0
//타이머를 위한 변수
var progressTimer: Timer!
//타이머 변수
let timePlayerSelector:Selector = #selector(ViewController.updatePlayTime)
```

그 후 Audio재생을 위한 초기화함수를 만들어 줍니다.

```swift
//재생을 하기 위한 설정을 초기화 한다.
func initPlay(){
    do{
        audioPlayer = try AVAudioPlayer(contentsOf: audioFile)
    } catch let error as NSError{
        print("Error-initPlay : \(error)")
    }
    //슬라이더의 볼륨 조절의 최대치를 설정합니다.
    slVolume.maximumValue  = MAX_VOLUME
    slVolume.value = 1.0
    pvProgressView.progress = 0
    
    audioPlayer.delegate = self
    audioPlayer.prepareToPlay()
    audioPlayer.volume = slVolume.value
}
```

또한 음원을 지정하는 방법은 다음과 같이 이용합니다.

```swift
audioFile =  Bundle.main.url(forResource: "Sicilian_Breeze", withExtension: "mp3")
```

오디오 재생이 끝나면 처음 상태로 돌아가는 함수입니다.

```swift
//오디오 재생이 끝나면 맨 처음상태로 돌아가는 함수를 만듭니다.
func audioPlayerDidFinishPlaying(_ player: AVAudioPlayer, successfully flag: Bool) {
    progressTimer.invalidate()
}
```

또한 음악 재생시간은 String값이 아닙니다.

따라서 String값으로 바꾸어주는 함수를 만들어 주어야 합니다.

```swift
//TimeInterval 값을 받아서 문자열로 돌려보내는 함수이다.
func convertNSTimeInterval12String(_ time:TimeInterval) -> String{
    let min = Int(time/60)
    //60으로 남은 나머지를 몫에 대입합니다.
    let sec = Int(time.truncatingRemainder(dividingBy: 60))
    let strTime = String(format: "%02:%02", min,sec)
    return strTime
}
```

음악을  재생하고 멈추고 초기화 하는 방법은 다음과 같습니다.

```swift
audioPlayer.play()
audioPlayer.pause()
audioPlayer.stop()
```

이러한 과정을 거치면 Audio를 성공적으로 재생할 수 있습니다.

![음악재생이미지](https://user-images.githubusercontent.com/16849874/89302170-7939cb00-d6a5-11ea-8d06-42d815470d24.png)