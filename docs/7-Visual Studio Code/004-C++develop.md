---
layout: default
title: "C++개발환경 만들기"
parent: Visual Studio Code
nav_order: 4
---

# 비주얼 스튜디오를 사용해서 C++개발환경 만들기

mac을 기준으로 작성합니다.

출처는 웨일님의 [[VSCode] Macbook에서 C/C++ 개발환경 구축하기](https://justdoitproject.tistory.com/31)

를 기반으로 옮겨왔습니다.

먼저 Vscode를 설치를 합니다.

먼저 Xcode를 설치해서 g++과 lldb가 터미널에 설치 되었는지 확인합니다.

    g++ -v
    lldb

를 눌러서 실행이 되고 버전이 나타나면 다음으로 vs플러그인을 설치합니다.

C++ 설치하기

![스크린샷 2020-08-29 오후 1 41 27](https://user-images.githubusercontent.com/64197788/91629211-e705b800-ea01-11ea-839d-929d9f052dc4.png)

llbd설치하기

![스크린샷 2020-08-29 오후 1 41 45](https://user-images.githubusercontent.com/64197788/91629235-292ef980-ea02-11ea-9346-c6af6b41ed09.png)

VSC를 재시작 합니다.

그 다음으로는 간단한 C++코드를 작성해 봅니다.

```C++
#include<iostream>

using namespace std;

int main(void){
    cout<<"Hello";
    return 0;
}
```

그 다음 commend + shift + B를 눌러서 빌드를 해봅니다.

그러면 상단에 다음과 같은 창이 나타나는데 톱니 모양을 눌러 줍니다.

![스크린샷 2020-08-29 오후 1 47 42](https://user-images.githubusercontent.com/64197788/91629292-9cd10680-ea02-11ea-9b00-f97c05f188ed.png)

그 다음으로는 해당 내용을 넣어줍니다.

```json
{ 
	// See https://go.microsoft.com/fwlink/?LinkId=733558 
	// for the documentation about the tasks.json format 
	"version": "2.0.0", 
	"tasks": [ 
		{ 
			"type": "shell", 
			"label": "g++ build active file", 
			"command": "/usr/bin/g++", 
			"args": [ 
				"-g", 
				"${file}", 
				"-o", 
				"${fileDirname}/${fileBasenameNoExtension}.out", 
				// 1. execute .out file 
				
					"&&", 
					//to join building and running of the file 
					"${fileDirname}/${fileBasenameNoExtension}.out", 
				
				
				//2. file input 
				/* 
					"<", 
					"${fileDirname}/sample_input.txt" 
				*/ 
				
				//3. file output 
				/* 
					">", 
					"${fileDirname}/sample_output.txt" 
				*/ 
				
				], 
				"options": { 
					"cwd": "/usr/bin" 
				}, 
				"problemMatcher": [ 
					"$gcc" 
				], 
				"group": { 
					"kind": "build", 
					"isDefault": true 
				} 
		}, 
	] 
}
	
```

1번 주석을 해제한 상태로 2번을 해제하면 파일입력, 3번 주석을 해제하면 파일 출력을 할 수 있습니다.

이제 commend + shift + B를 누르면 빌드가 되는 것을 볼 수 있습니다.

---

디버깅하기

먼저 .vscode 파일 안에 launch.json파일을 만들어 줍니다.

![스크린샷 2020-08-29 오후 2 22 50](https://user-images.githubusercontent.com/64197788/91629334-1f59c600-ea03-11ea-9bc9-eeb3c386405d.png)


```json
{ 
    "version": "0.2.0", 
    "configurations": [ 
        { 
            "type": "lldb", 
            "request": "launch", 
            "name": "Launch", 
            "program": "${fileDirname}/${fileBasenameNoExtension}.out", 
            "args": [], 
            "preLaunchTask": "g++ build active file", 
            "stdio": [null,null,null], 
            "terminal": "integrated" 
        } 
    ] 
}
```

이제 vscode에서 디버깅 탭으로 갑니다.

이제 원하는 C++파일을 열고 상단에 디버깅 버튼을 누르면 됩니다.

중단점을 설정할 수도 있습니다.

![스크린샷 2020-08-29 오후 2 24 00](https://user-images.githubusercontent.com/64197788/91629352-4b754700-ea03-11ea-9eb7-bdf8743da30a.png)