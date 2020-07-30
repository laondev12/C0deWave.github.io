---
layout: default
title: 반복문 for while
parent: IOS
nav_order: 16
---

# 반복문 for while

스위프트 튜토리얼 가이드를 참조 했습니다.

[공식 가이드의 루프문 소개](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html)

---

for문 예제

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs
```

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```
```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
```

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // render the tick mark every 3 hours (3, 6, 9, 12)
}
```

---

while문 예제

<img width="212" alt="스크린샷 2020-07-30 오후 6 09 50" src="https://user-images.githubusercontent.com/16849874/88904334-eb6a7400-d28f-11ea-82ff-ab6966b8f7b0.png">

<img width="223" alt="스크린샷 2020-07-30 오후 6 09 56" src="https://user-images.githubusercontent.com/16849874/88904327-ea394700-d28f-11ea-953a-718a0d9cf877.png">
