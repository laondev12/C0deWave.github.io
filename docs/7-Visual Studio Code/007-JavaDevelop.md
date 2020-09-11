---
layout: default
title: "Java 개발환경 만들기"
parent: Visual Studio Code
nav_order: 7
---

# 비주얼 스튜디오 코드를 사용해서 JAVA 개발환경 만들기

mac을 기준으로 작성합니다.

먼저 JDK가 설치되어 있어야 합니다.

![스크린샷 2020-09-11 오후 5 54 54](https://user-images.githubusercontent.com/16849874/92897127-e8020500-f457-11ea-88ab-7374b9ee2f2c.png)

[jdk 다운](https://www.oracle.com/java/technologies/javase-downloads.html) > 여기서 위의 JDK 다운을 누릅니다.

다운을 받은 다음 설치를 하면 됩니다.

이제 vscode에서 자바 확장팩을 설치하면 됩니다.

![스크린샷 2020-09-11 오후 6 33 45](https://user-images.githubusercontent.com/16849874/92903852-5eedcc80-f45d-11ea-91c5-be4c448786f8.png)

Extension에서 Java Extension Pack를 검색해서 설치하거나

[여기](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)를 들어가서 설치를 하면 됩니다.

Java Extension Pack은 

    📦 Language Support for Java™ by Red Hat
    📦 Debugger for Java
    📦 Java Test Runner
    📦 Maven for Java
    📦 Java Dependency Viewer
    📦 Visual Studio IntelliCode

총 6가지로 구성이 되어있습니다.

그 다음으로는 commend + , 를 눌러서 Setting를 엽니다.

Extension -> Java -> Home 설정을 해줍니다.

![스크린샷 2020-09-11 오후 6 39 34](https://user-images.githubusercontent.com/16849874/92904841-25699100-f45e-11ea-9139-cc5da2ce5ac9.png)

그 다음으로 간단한 예제 파일을 만들어 봅니다.

![스크린샷 2020-09-11 오후 6 45 01](https://user-images.githubusercontent.com/16849874/92906233-4a123880-f45f-11ea-82ea-79c879b7ddbb.png)

그후 Control + option + N을 눌러서 java파일을 실행할 수 있습니다.

![스크린샷 2020-09-11 오후 6 45 09](https://user-images.githubusercontent.com/16849874/92906243-4bdbfc00-f45f-11ea-9f69-092f47da2be3.png)

디버그도 따로 할 수 있습니다.

![스크린샷 2020-09-11 오후 6 43 17](https://user-images.githubusercontent.com/16849874/92906429-6ca45180-f45f-11ea-9d96-30ac3ff250d7.png)

![스크린샷 2020-09-11 오후 6 53 00](https://user-images.githubusercontent.com/16849874/92907126-05d36800-f460-11ea-8fe1-fa829ee897a6.png)

단축키가 따로 없어서 control + option + D로 지정했습니다.

이제 vscode로 java개발을 하면 됩니다.