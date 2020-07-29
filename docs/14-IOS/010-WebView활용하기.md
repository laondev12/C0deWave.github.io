---
layout: default
title: WebView 활용하기
parent: IOS
nav_order: 10
---

# WebView 활용하기

이번에는 어플안에 웹 페이지를 띄우는 WebView에 대해서 알아볼것이다.

이를 이용하기 위해서는 사전적으로 Xcode의 프로젝트 안에 프레임 워크를 추가해야 한다.

<img width="1531" alt="스크린샷 2020-07-29 오후 1 02 03" src="https://user-images.githubusercontent.com/16849874/88773809-93633d00-d1bd-11ea-91d8-877bbfac8c9b.png">

먼저 프로젝트의 설정 부분에서 Build Phases - Link Binary With Library로 가서 + 를 누른 다음에 Web Framework를 추가해 준다.

그리고 나서 Info.plist에 들어가서 

<img width="1527" alt="스크린샷 2020-07-29 오후 1 06 51" src="https://user-images.githubusercontent.com/16849874/88774047-eccb6c00-d1bd-11ea-9680-ee504c10475d.png">

위의 설정을 추가해 준다음 NO로 만들어 준다.

먼저 WebView를 이용하기 위해서는 해당 라이브러리를 import해주어야 한다.

```
import WebKit
```

다음으로는 웹 사이트에 접속하는 함수를 만들어 주어야 한다.

메인 스레드에서는 이 작업을 수행할 수 없다.

```
    func loadWebPage(_ url:String){
        let myUrl = URL(string: url)
        let myRequest = URLRequest(url: myUrl!)
        myWebView.load(myRequest)
        
        //웹 뷰에 옵저버를 등록합니다.
        myWebView.addObserver(self, forKeyPath: #keyPath(WKWebView.isLoading), options: .new, context: nil)
    }
```

그 후 해당 함수를 호출해서 페이지로 넘어가면 된다.

또한 아래의 메뉴바는 툴바(toolbar)와 바 버튼 아이템, 플렉서블 스페이스바의 결합으로 만들어 졌다.

여기에는 각각에 맞는 함수를 추가해 주면된다.

```
    //Stop Button
    @IBAction func btnStop(_ sender: UIBarButtonItem) {
        myWebView.stopLoading()
    }
    //새로고침 버튼
    @IBAction func btnReload(_ sender: UIBarButtonItem) {
        myWebView.reload()
    }
    //이전 버튼
    @IBAction func btnGoBack(_ sender: UIBarButtonItem) {
        myWebView.goBack()
    }
    //앞으로 가기 버튼
    @IBAction func btnGoForword(_ sender: UIBarButtonItem) {
        myWebView.goForward()
    }
```

이것으로 간단하게 webView를 이용하는 방법에 대해 알아보았다.

---

최종 결과물

<img width="477" alt="스크린샷 2020-07-29 오후 2 13 58" src="https://user-images.githubusercontent.com/16849874/88774168-0ff61b80-d1be-11ea-868a-fd329c53e385.png">
