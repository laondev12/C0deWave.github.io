---
layout: default
title: "Xml 파싱하기 (kotlin)"
parent: Android
nav_order: 24
---

# Xml파싱하기 (kotlin)

저의 경우에는 먼저 Volley로 xml을 호출받아서 나온 값을 String 변수에 저장하고

이를 파싱하는 방법을 이용했습니다.

먼저 onCreate안에 XmlPullParserFactory변수를 만들어 주고 아래의 내용을 넣어서 

Volley로 받아온 값을 전달해서 넘겨줍니다.

```kotlin
val pullParserFactory : XmlPullParserFactory
    try {
        pullParserFactory = XmlPullParserFactory.newInstance()
        val parser = pullParserFactory.newPullParser()
        val inputStream = ByteArrayInputStream(result?.toByteArray(charset("euc-kr")))
        parser.setFeature(XmlPullParser.FEATURE_PROCESS_NAMESPACES,false)
        parser.setInput(inputStream,null)

        predictXml = parseXml(parser)

    }catch (e : XmlPullParserException){
        e.printStackTrace()
    }catch (e : IOException){
        e.printStackTrace()
    }
```

이제 받아온 값을 파싱하는 단계입니다.

파싱을 하기 위해서는 먼저 받을 데이터의 클래스도 만들어 주어야 합니다.

밑에 내용에서는 item 태그를 만나면 클래스를 생성하고 

statisyy1을 다음 클래스 안에 값으로 넣습니다.

그리고 item태그가 끝나면 클래스를 Array에 넣는 작업을 합니다.

```kotlin
@Throws (XmlPullParserException::class, IOException::class)
    fun parseXml(parser: XmlPullParser?): ArrayList<predictXmlData>? {
        var dataArray : ArrayList<predictXmlData>? = null
        var eventType = parser?.eventType
        var data : predictXmlData? = null

        while (eventType != XmlPullParser.END_DOCUMENT){
            val tagName : String
            when(eventType){
                XmlPullParser.START_DOCUMENT->dataArray = ArrayList()
                XmlPullParser.START_TAG -> {
                    tagName = parser!!.name
                    if (tagName == "item"){
                        data = predictXmlData()
                    }else if (data != null){
                        if (tagName == "statisyy1"){
                            data.starisyy1 = parser.nextText()
                        }
                    }
                }
                XmlPullParser.END_TAG -> {
                    tagName = parser!!.name
                    if (tagName.equals("item", ignoreCase = true) && data != null){
                        dataArray!!.add(data)
                    }
                }
            }
            eventType = parser!!.next()
        }
        return dataArray
    }
```