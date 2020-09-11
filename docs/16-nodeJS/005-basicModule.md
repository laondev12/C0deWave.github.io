---
layout: default
title: 기본 내장 모듈
parent: nodeJS
nav_order: 5
---

# 기본 내장 모듈

우선 node의 공식 문서의 링크는 [여기](https://nodejs.org/ko/docs/)이다.

이번 시간에 우리는 node문서에서 os모듈을 찾은 다음에 이를 활용해 보고자 한다.

자신의 버전에 맞는 node의 버전의 문서를 들어 갑니다.

그후 해당 문서에서 os를 찾아서 들어갑니다.

![스크린샷 2020-09-09 오후 7 58 44](https://user-images.githubusercontent.com/16849874/92590262-e0672280-f2d6-11ea-9dca-4b22c405749a.png)

그러면 

    const os = require('os');

을 이용해서 접근을 한다고 나와 있습니다.

os모듈을 한번 이용해 보도록 합시다.

```js
const os = require('os');

//운영체제의 호스트 네임을 리턴 합니다.
console.log(os.hostname());
//운영체제의 이름을 리턴합니다.
console.log(os.type());
//운영체제의 플랫폼을 리턴합니다.
console.log(os.platform());
//운영체제의 아키텍쳐를 리턴합니다.
console.log(os.arch());
//운영체제의 버전을 리턴합니다.
console.log(os.release());
//운영체제가 실행된 시간을 리턴합니다.
console.log(os.uptime());
//로드레버리지 정보를 담은 배열을 리턴합니다.
console.log(os.loadavg());
//시스템의 총 메모리를 리턴합니다.
console.log(os.totalmem());
//시스템의 사용가능한 메모리를 리턴합니다.
console.log(os.freemem());
//CPU의 정보를 담은 객체를 리턴합니다.
console.log(os.cpus());
//네트워크 인터페이스와 정보를 담은 배열을 리턴합니다.
console.log(os.networkInterfaces());
```

이러한 방법을 이용해서 우리는 모듈을 불러와서 실행할 수 있습니다.

![스크린샷 2020-09-09 오후 8 36 16](https://user-images.githubusercontent.com/16849874/92593549-3e4a3900-f2dc-11ea-9604-67a836b90246.png)

우리는 이러한 방식을 이용해서 URL 모듈, Query String모듈, Util모듈을 사용할 수 있습니다.

---

## Crypto 모듈

이 모듈은 해시 생성과 암호화를 수행하는 모듈입니다.

```js
var crypto = require('crypto');

//해시를 생성합니다.
var shasum = crypto.createHash('sha256');
shasum.update('암호화하기 위한 문장');
var output = shasum.digest('hex');

//해시값을 출력합니다.
console.log('hash : ',output);
```

![스크린샷 2020-09-09 오후 8 50 43](https://user-images.githubusercontent.com/16849874/92594712-22e02d80-f2de-11ea-8d14-d9ca29fa856d.png)

이러한 해시값은 원래대로 돌릴수 없습니다.

이는 서버에서 비밀번호를 해시화해서 저장함으로써 유출 피해를 최소화 하기 위해서 사용합니다.

반대로 다시 돌릴 수 있는 경우를 암호라고 합니다.

```js
var crypto = require('crypto');

//변수를 선언 합니다.
var key = '나만의 비밀키';
var input = 'PASSWORD';

//암호화
var cipher = crypto.createCipher('aes192',key);
cipher.update(input, 'utf8','base64');
var cipheredOutput = cipher.final('base64');

//암호화 해제
var decipher = crypto.createDecipher('aes192',key);
decipher.update(cipheredOutput,'base64','utf8');
var decipheredOutput = decipher.final('utf8');

//출력합니다.
console.log('원래 문자열', input);
console.log('암호화',cipheredOutput);
console.log('암호화 해제',decipheredOutput);
~
```

이러한 방법을 이용해서 암호화와 해제를 할 수 있습니다.

![스크린샷 2020-09-09 오후 9 24 18](https://user-images.githubusercontent.com/16849874/92597778-d4815d80-f2e2-11ea-9b38-0da069509a11.png)

---

## fs 모듈

이번에는 파일시스템을 읽고 쓰는 작업을 해보도록 하겠습니다.

먼저 읽고 쓸 텍스트 파일을 엽니다.

저는 testfile.txt로 만들었습니다.

    Hello FileSystem!!!!

그 다음은 코드를 보겠습니다.

```js
var fs = require('fs');

var text = fs.readFileSync('testfile.txt','utf-8');
console.log(text);
```

이번에는 이벤트 기반으로 작동하는 readFile을 보도록 하겠습니다.

```js
var fs = require('fs');

var text = fs.readFile('testfile.txt', 'utf-8', function (error, data) {
    if (error) {
        console.log(error);
    } else {
        console.log(data);
    }
});

//파일을 씁니다.
fs.writeFile('testfile.txt','Codewave입니다.','utf8',function(error) {
    if (error) {
        console.log(error);
    } else {
        console.log("저장 완료!!!");
    }
});
```

