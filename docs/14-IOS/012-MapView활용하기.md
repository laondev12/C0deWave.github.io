---
layout: default
title: MapView 활용하기
parent: IOS
nav_order: 12
---

# MapView 활용하기

MapView는 애플에서 제공하는 지도이다.

이를 이용하기 위해서는 먼저 설정을 몇개 해야한다.

먼저 위치 설정을 하도록 권한을 얻어야 한다.

<img width="1537" alt="스크린샷 2020-07-29 오후 2 48 28" src="https://user-images.githubusercontent.com/16849874/88878838-c102c180-d263-11ea-8f45-cdc034ac984c.png">

info.Plist를 들어간다.

Privacy - Location When In Use Usage Description 를 추가하고 인수로

App needs location servers for stuff.를 적어 넣는다.

또한 예물레이터에서 위치를 커스텀 하기 위해서는 

<img width="1565" alt="스크린샷 2020-07-29 오후 3 21 37" src="https://user-images.githubusercontent.com/16849874/88878976-1939c380-d264-11ea-89a2-b0b30f6f1f72.png">

Features - Location - Custom Location을 이용해서 위도와 경도를 누르면 된다.

이제 프로젝트로 들어가 보자.

---

맵뷰를 이용하기 위해서는 먼저 헤더를 정의해야 한다.

```swift
import MapKit
```

그 후 맵뷰를 아웃렛 함수로 가져온다.

로케이션 매니저도 만들어 준다.

```swift
@IBOutlet weak var myMap: MKMapView!

let locationManager = CLLocationManager()
```

뷰를 처음 그릴 때 초기화를 한다.

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        //상수 locationManager의 델리게이트를 self로 합니다.
        locationManager.delegate = self
        //정확도를 최고로 설정합니다.
        locationManager.desiredAccuracy = kCLLocationAccuracyBest
        //위치데이터를 추적하기 위해 사용자에게 승인을 요구합니다.
        locationManager.requestWhenInUseAuthorization()
        //위치 업데이트를 시작합니다.
        locationManager.startUpdatingLocation()
        //위치보기값을 true로 합니다.
        myMap.showsUserLocation = true
    }
```

그 다음은 위치를 업데이트 하는 함수 입니다.

```swift
    //지도의 위치를 업데이트 합니다.
    func goLocation(latitudeValue:CLLocationDegrees,
                    longitudeValue:CLLocationDegrees,
                    delta span : Double)->CLLocationCoordinate2D{
        let pLocation = CLLocationCoordinate2DMake(latitudeValue, longitudeValue)
        let spanValue = MKCoordinateSpan(latitudeDelta: span, longitudeDelta: span)
        let pRegion = MKCoordinateRegion(center: pLocation, span: spanValue)
        myMap.setRegion(pRegion, animated: true)
        
        return pLocation
    }
    
    //위치가 업데이트 되었을때 지도에 위치를 나타내기 위해서 사용하는 함수입니다.
    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let pLocation = locations.last
        _ = goLocation(latitudeValue: (pLocation?.coordinate.latitude)!, longitudeValue: (pLocation?.coordinate.longitude)!, delta: 0.01)
        CLGeocoder().reverseGeocodeLocation(pLocation!, completionHandler: {
            (placemarks, error) -> Void in
            //placemarks의 첫 부분만 pm상수로 대입합니다.
            //인터넷이 없으면 에러가 나타납니다.
            let pm = placemarks!.first
            //pm상수에서 나라값을 country에 대입합니다.
            let country = pm!.country
            //문자열 address에 country상수값을 대입합니다.
            var address:String = country!
            //pm상수에서 지역 값이 존재하면 address문자열에 더해줍니다.
            if pm!.locality != nil{
                address += " "
                address += pm!.locality!
            }
            //pm문자열에서 도로값이 존재하면 address값에 추가합니다.
            if pm!.thoroughfare != nil{
                address += " "
                address += pm!.thoroughfare!
            }
            //레이블에 주소를 표시하게 합니다.
            self.lblLocationInfo2.text = address
        })
        //마지막으로 위치가 업데이트 되는 것을 멈추게 합니다.
        locationManager.stopUpdatingLocation()
    }
```

나중에 내 위치를 찾는 방법은 함수를 다음과 같이 호출하면 됩니다.

```swift
locationManager.startUpdatingLocation()
```

또한 아래의 함수를 이용하면 지도에 마크를 찍을 수 있습니다.

```swift
func setAnnotation(latitudeValue:CLLocationDegrees,
                    longitudeValue:CLLocationDegrees,
                    delta span : Double,
                    title strTitle : String,
                    subtitle strSubtitle:String){
    let annotation = MKPointAnnotation()
    annotation.coordinate = goLocation(latitudeValue: latitudeValue, longitudeValue: longitudeValue, delta: span)
    
    annotation.title = strTitle
    annotation.subtitle = strSubtitle
    myMap.addAnnotation(annotation)
}
```

다음과 같이 함수를 호출하면 됩니다.

```swift
setAnnotation(latitudeValue: 37.1458, longitudeValue: 127.0671, delta: 1, title: "오산역", subtitle: "오산에 있음")
```

마크를 찍으면 다음과 같이 나타나게 됩니다.

<img width="470" alt="스크린샷 2020-07-30 오후 1 08 31" src="https://user-images.githubusercontent.com/16849874/88879635-c6f9a200-d265-11ea-93ef-65ac909254c5.png">