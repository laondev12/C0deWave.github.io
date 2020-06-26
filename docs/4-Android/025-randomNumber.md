---
layout: default
title: "랜덤수 얻기"
parent: Android
nav_order: 25
---

# 랜덤수 얻기

원래는 Random.nextInt() 라고 적으면 될 것 같지만 이러면 랜덤 함수를 실행할 때마다 같은 값을 주기 때문에 

이러한 문제를 해결하기 위해서 seed값으로 오늘의 일자, 시간을 주면 된다.

쉬워 보였는데 은근히 삽질을 했었다.

```kotlin
var predictResult = Random(LocalDateTime.now().hashCode()).nextInt(100)+1
```

1 ~ 100 까지의 랜덤한 수를 나타내는 코드이다.