---
layout: default
title: "안드로이드 가속센서 이용하기"
parent: Android
nav_order: 3
---

# 안드로이드 가속센서 실습해보기

## 안귀정 저자님의 안드로이드 with kotlin의 예제를 실습하면서 배웠습니다.

먼저 가속도를 저장, 측정하기 위한 변수를 선언해야 한다.

```kotlin
    //측정된 최대 펀치력
    var maxPower = 0.0

    //펀치력 측정이 시작 되었는지 나타내는 변수
    var isStart = false

    //펀치력 측정이 시작된 시간
    var startTime = 0L

    //Sensor 관리자 객체. lazy로 실제 사용할 때 초기화 한다.
    val sensorManager: SensorManager by lazy {
        getSystemService(Context.SENSOR_SERVICE) as SensorManager
    }

    //센서 이벤트를 처리하는 리스너
    val eventListener: SensorEventListener = object : SensorEventListener {
        override fun onAccuracyChanged(sensor: Sensor?, accuracy: Int) {
        }

        //센서값이 바뀔때 실행된다.
        override fun onSensorChanged(event: SensorEvent?) {
            event?.let {
                //측정된 센서 값이 선형 가속도 타입이 아니면 바로 리턴한다.
                if (event.sensor.type != Sensor.TYPE_LINEAR_ACCELERATION) return@let

                //각 죄표값을 제곱하여 음수값을 없애고 값의 차이를 극대화
                val power =
                    Math.pow(event.values[0].toDouble(), 2.0)
                +Math.pow(event.values[1].toDouble(), 2.0)
                +Math.pow(event.values[2].toDouble(), 2.0)

                // 측정된 펀치력이 20을 넘고 아직 측정이 시작되지 않은 경우
                if (power > 20 && !isStart) {
                    startTime = System.currentTimeMillis()
                    isStart = true
                }

                //측정이 시작된 경우
                if (isStart) {

                    //5초간 최대값을 측정. 현재 측정된 값이 최대 값보다 크면 최대값을 현재값으로 변경
                    if (maxPower < power) maxPower = power

                    //측정중인것을 사용자에게 알려줌
                    startLabel.text = "펀치력을 측정하고 있습니다."

                    //최초 측정후 3초가 지나면 측정을 끝낸다.
                    if (System.currentTimeMillis() - startTime > 3000) {
                        isStart = false
                        puchPowerTestComplete(maxPower)
                    }
                }
            }
        }
    }
```

중간에 puchPowerTestComplete() 함수에는 결과를 받아서 결과 화면으로 넘어가는 부분을 넣는다.
```kotlin
    //펀치력 측정이 완료된 경우의 처리함수
    fun puchPowerTestComplete(power: Double) {
        Log.d("MainActivity","측정 완료 : power : ${String.format("%5f",power)}")
        sensorManager.unregisterListener(eventListener)
        val intent = Intent(this@MainActivity,ResultActivity::class.java)
        intent.putExtra("power",power)
        startActivity(intent)
    }
```

그 후에는 안드로이드 실행주기를 고려하며 onStart()안에 각종 초기화를 담당하는 함수를 넣는다.

```kotlin
override fun onStart() {
     super.onStart()
     initGame()
}
```

아래는 초기화를 담당하는 함수이다.

```kotlin
    fun initGame() {
        maxPower = 0.0
        isStart = false
        startTime = 0L
        startLabel.text = "핸드폰을 손에 쥐고 주먹을 내지르세요"

        //센서의 변화값을 처리할 리스너를 등록한다.
        //TYPE_LINEAR_ACCELERATION은 중력값을 제외하고 x,y,z축에 측정된 가속도만 계산되어 나온다.
        sensorManager.registerListener(
            eventListener,
            sensorManager.getDefaultSensor(Sensor.TYPE_LINEAR_ACCELERATION),
            SensorManager.SENSOR_DELAY_NORMAL
        )
    }
```

또한 안드로이드 실행주기를 생각하면서 onStop()안에 센서를 해제하는 함수를 넣어주어야 한다.

```kotlin
    override fun onStop() {
        super.onStop()
        try {
            sensorManager.unregisterListener(eventListener)
        }catch (e : Exception){}
    }
```

이를 이용하면 아래와 같은 가속 센서의 값을 구할 수 있다.

![스크린샷 2020-05-30 오후 10 04 05](https://user-images.githubusercontent.com/16849874/83328938-988d3400-a2c1-11ea-8721-1b8968121abc.png)
