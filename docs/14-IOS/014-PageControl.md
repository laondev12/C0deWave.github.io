---
layout: default
title: PageControl 활용하기
parent: IOS
nav_order: 14
---

#  PageControl 활용하기

페이지 컨트롤이 무엇이냐??

우리가 이전에 보아온 슬라이드 뷰 이다.

거두절미하고 완성본의 이미지를 보면 알 것이다.

<img width="470" alt="스크린샷 2020-07-30 오후 5 20 24" src="https://user-images.githubusercontent.com/16849874/88901475-ca078900-d28b-11ea-9f5e-6d5d65634d4a.png">

어떻게 구현을 하는 것인가

먼저 스토리보드에 이미지뷰와 page controll를 추가하면 된다.

그후 코드를 작성하자

먼저 이미지와 page control의 아웃렛 변수를 만들어 준다.

```swift
//이미지 출력용 아웃렛 변수
@IBOutlet weak var imgView: UIImageView!
//페이지 컨트롤 용 아웃렛 변수
@IBOutlet weak var pageControl: UIPageControl!
```

그 다음은 페에지 컨트롤의 값을 초기화 해야 한다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    //페이지 컨트롤의 전체 페이지를 images배열의 전체 개수 값으로 설정
    pageControl.numberOfPages = images.count
    
    //페이지 컨트롤의 현재 페이지 설정
    pageControl.currentPage = 0
    
    //페이지 표시 색상을 초록색으로 설정
    pageControl.pageIndicatorTintColor = UIColor.green
    
    //현재 페이지 색상을 빨간색으로 설정
    pageControl.currentPageIndicatorTintColor = UIColor.red
    
    //처음 이미지 뷰 설정
    imgView.image = UIImage(named: images[0])
}
```

마지막으로 page control이 변경되었을 때의 아웃렛 함수를 만들어 준다.

```swift
//페이지가 변하면 호출된다.
@IBAction func pageChange(_ sender: UIPageControl) {
    //images라는 배열에서 pageControl이 가르키는 현재 페이지에 해당하는 imageView를 할당한다.
    imgView.image = UIImage(named: images[pageControl.currentPage])
}
```