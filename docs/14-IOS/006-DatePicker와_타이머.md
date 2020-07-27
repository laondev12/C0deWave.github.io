---
layout: default
title: DatePicker와 타이머 기능
parent: IOS
nav_order: 6
---

# DatePicker와 타이머 기능

<img width="519" alt="스크린샷 2020-07-27 오후 6 09 20" src="https://user-images.githubusercontent.com/16849874/88524851-dcd45080-d034-11ea-98c2-17d8bd15180b.png">

```swift
import UIKit

class ViewController: UIViewController {
    //타이버가 구동되면 실행할 함수를 지정합니다.
    let timeSelector : Selector = #selector(ViewController.updateTime)
    //타이머의 간격입니다.
    let interval = 1.0
    //타이버가 잘 실행되는지 확인하기 위한 변수입니다.
    var count = 0
    
    @IBOutlet weak var lblCurrentTime: UILabel!
    @IBOutlet weak var lblPickerTime: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do anyadditional setup after loading the view.
        
        //타이머를 만드어서 실행합니다.
        //각 인수의 정보
        //타이머 간격
        //동작될 view
        //타이머가 구동될때 실행할 함수
        //사용자 정보
        //반복 여부
        Timer.scheduledTimer(timeInterval: interval, target: self, selector: timeSelector, userInfo: nil, repeats: true)
    }

    //일정이 선택되면 함수를 실행합니다.
    @IBAction func changeDatePicker(_ sender: UIDatePicker) {
        let datePickerView = sender
        
        let formatter = DateFormatter()
        
        formatter.dateFormat="yyyy-MM-dd HH:mm EEE"
        lblPickerTime.text =
            "선택시간: " + formatter.string(from: datePickerView.date)
    }
    
    //타이머가 실행하는 반복함수입니다.
    @objc func updateTime(){
        //타이머 확인하기
//        lblCurrentTime.text = String(count)
//        count = count + 1
        
        //매 초마다 현재시간 업데이트하기
        let date = NSDate()
        
        let fomatter = DateFormatter()
        fomatter.dateFormat = "yyyy-MM-dd HH:mm:ss EEE"
        lblCurrentTime.text = "현재시간 : "+fomatter.string(from: date as Date)
    }
}


```