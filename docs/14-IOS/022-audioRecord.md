---
layout: default
title:  소리 녹음하기
parent: IOS
nav_order: 22
---

# 소리 녹음하기

소리를 녹음하기 위해서는 먼저 헤더를 정의해야 합니다.

```swift
import AVFoundation
```

그후 델리게이트를 상속 받아야 합니다.

```swift
class ViewController: UIViewController ,AVAudioRecorderDelegate{
```

다음으로는 녹음을 위한 각종 변수를 선언합니다.

```swift
//녹음을 위한 변수
var audioRecoder : AVAudioRecorder!
var isRecordMode = false
let timeRecordSelector:Selector = #selector(ViewController.updateRecordTime)

@objc func updateRecordTime(){
    lblRecordTime.text = convertNSTimeInterval12String(audioRecoder.currentTime)
}
```

다음으로는 녹음 준비를 위한 initRecird()함수를 만들고 이를 viewDiaLoad에 추가힙니다.

```swift
func initRecord(){
    let recirdSettings = [
        AVFormatIDKey : NSNumber(value: kAudioFormatAppleLossless as UInt32),
        AVEncoderAudioQualityKey:AVAudioQuality.max.rawValue,
        AVEncoderBitRateKey : 320000,
        AVNumberOfChannelsKey : 2,
    AVSampleRateKey : 44100.0] as [String : Any]
    
    do {
        audioRecoder = try AVAudioRecorder(url: audioFile, settings: recirdSettings)
    } catch let error as NSError {
        print("Error-initRecord : \(error)")
    }
    audioRecoder.delegate = self
    
    //슬라이더의 볼륨과 같게합니다.
    slVolume.value = 1.0
    audioPlayer.volume = slVolume.value
    
    let session = AVAudioSession.sharedInstance()
    do {
        try AVAudioSession.sharedInstance().setCategory(.playback,mode: .default)
        try AVAudioSession.sharedInstance().setActive(true)
    } catch let error as NSError {
        print("Error-setCategory : \(error)")
    }
    do {
        try session.setActive(true)
    } catch let error as NSError {
        print("Error-setActive : \(error)")
    }
}
```

녹음을 하기 위한 파일을 만드는 방법입니다.

```swift
//재생할 오디오의 파일명 변수
var audioFile: URL!
//녹음 모드일 때는 새 파일인 recordFile.m4a가 생성됩니다.
let documentDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
audioFile = documentDirectory.appendingPathComponent("recordFile.m4a")
```

녹음을 시작, 정지하는 방법입니다.

```swift
audioRecoder.record()
audioRecoder.stop()
```

이렇게 하면 녹음이 됩니다.

그후 이전에 배운 Audio재생을이용해서 녹음한 내용을 들어볼 수 있습니다.

![녹음 이미지](https://user-images.githubusercontent.com/16849874/89302170-7939cb00-d6a5-11ea-8d06-42d815470d24.png)