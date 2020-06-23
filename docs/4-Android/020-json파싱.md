---
layout: default
title: "Json 파싱하기"
parent: Android
nav_order: 20
---

# Json 파싱하기

원래는 xml을 파싱하는 방법을 알았어야 하는데 이놈의 API가 말을 안들어서 따로 CSV 파일이 있는 것을 보고 이를 JSON으로 파싱해서 이용해야 겠다고 생각했다.

![스크린샷 2020-06-23 오전 8 41 06](https://user-images.githubusercontent.com/16849874/85399497-e1b76780-b591-11ea-8526-963c49cb833b.png)

API가 작동을 안한다.

![스크린샷 2020-06-23 오전 8 41 34](https://user-images.githubusercontent.com/16849874/85399537-f431a100-b591-11ea-8656-adf5a7a45177.png)

그래서 CSV로 올라온 파일이 같은 내용인 것을 알고 이를 이용해서 문제를 해결하기로 한다.

![스크린샷 2020-06-23 오전 8 55 53](https://user-images.githubusercontent.com/16849874/85399630-14616000-b592-11ea-8d39-abcf7a6cbedb.png)

[https://csvjson.com/csv2json](https://csvjson.com/csv2json)

위 사이트에 들어가면 CSV파일을 JSON으로 바꿀수 있다.

그 다음은 JSON 파싱 부분이다.

![스크린샷 2020-06-23 오전 10 08 52](https://user-images.githubusercontent.com/16849874/85399926-8e91e480-b592-11ea-946b-caa50268ea27.png)

그 다음으로는 asset 폴더를 만들어서 안에 JSON파일을 그대로 넣어주자.

![스크린샷 2020-06-23 오후 8 46 55](https://user-images.githubusercontent.com/16849874/85399989-aff2d080-b592-11ea-91d7-73ab23e68c09.png)

다음은 JSON의 형식에 맞게 class 파일을 만들어 준다.

![스크린샷 2020-06-23 오전 9 47 07](https://user-images.githubusercontent.com/16849874/85400049-c567fa80-b592-11ea-9236-c3c0debc6d6b.png)

이제 코드 부분으로 넘어가겠다.

---

나는 스피너 위젯을 쓸 것이기 때문에 스피너 레이아웃을 만들어준다.

![스크린샷 2020-06-23 오후 8 52 41](https://user-images.githubusercontent.com/16849874/85400522-7ec6d000-b593-11ea-91ec-bae756e60965.png)

이제 코틀린 코드를 보도록 하겠다.

먼저 데이터를 넣을 arrayList를 만들어 준다.

```kotlin
class ConfigurationActivity : AppCompatActivity() {
    
    //여기
    var arr = arrayListOf<String>()

    override fun onCreate(savedInstanceState: Bundle?)
    {
```

그 다음은 Json을 읽는 함수를 만들어 보자.

```kotlin
//에셋 폴더에서 JSON을 읽어 옵니다.
fun readJson(): String? {
    var json : String? = null
    try {
        val inputStream : InputStream = assets.open("certification.json")
        json = inputStream.bufferedReader().use { it.readText() }

    }catch (e : IOException){

    }

    return json
}
```

JSON파일을 불러 옵니다.

```kotlin
//Json파일을 불러온다. 
val certificationData = readJson()
val jsonarr = JSONArray(certificationData)
```

그 다음으로는 for를 이용한 반복으로 아이템 하나하나를 arrayList에 추가한다.

```kotlin
for (i in 0 .. jsonarr.length() - 1)
{
    var jsonobj = jsonarr.getJSONObject(i)
    if (jsonobj.getString("SERIESNM") == certificationList[position]){
        arr.add(jsonobj.getString("JMFLDNM"))
    }

    var adpt = ArrayAdapter<String>(this@ConfigurationActivity,android.R.layout.simple_spinner_dropdown_item,arr)
    bottomSpinner.adapter = adpt
}
```


마지막으로 ArrayAdapter을 만든 다음에 이를 연결해주면 된다.

```kotlin
var adpt = ArrayAdapter<String>(this@ConfigurationActivity,android.R.layout.simple_spinner_dropdown_item,arr)
bottomSpinner.adapter = adpt
```

추가적으로 스피너에 Listener을 붙이는 방법은 다음과 같다.

```kotlin
topSpinner.onItemSelectedListener = object : AdapterView.OnItemSelectedListener{
    override fun onNothingSelected(parent: AdapterView<*>?) {
        Toast.makeText(applicationContext,"자격증 종류를 선택해 주세요.",Toast.LENGTH_LONG).show()
    }

    override fun onItemSelected(
        parent: AdapterView<*>?,
        view: View?,
        position: Int,
        id: Long
    ) {
        Toast.makeText(applicationContext,"${certificationList[position]}를 선택했습니다.",Toast.LENGTH_LONG).show()
    }

}
```