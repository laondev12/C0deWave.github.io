---
layout: default
title: node 전역객체 console, process, exports
parent: nodeJS
nav_order: 4
---

# node 전역객체 console, process, exports에 대해서 알아보자

이번에는 nodeJS에서 기본적으로 있는 전역객체인 console, process, exports 에 대해서 적어 보고자 한다.

---

## console

콘솔은 우리가 로그창을 많이 사용해서 익숙하다.

로그를 출력하거나 시간을 측정하는 용도로 쓰인다.

먼저 다음과 같이 입력을 해보자

```js
//콘솔객체(console)객체에 대해서 알아볼 것입니다.
//시간 측정을 시작합니다.
console.time('alpha');

//실행중인 코드의 파일 경로를 나타냅니다.
console.log('filename: ', __filename);

//현재 실행중인 코드와 폴더 경로를 나타냅니다.
console.log('dirname: ', __dirname);

//콘솔에 문자열을 넣을 수도 있습니다.
console.log('%d + %d = %d' , 3 , 4 , 3+4 );

//콘솔에 글자 색을 변경합니다.
console.log('\u001b[36m' , 'Hello World!!!');

//콘솔에 글자색을 원래대로 돌립니다.
console.log('\u001b[0m')

//시간 측정을 종료합니다.
console.timeEnd('alpha');
```

## 결과화면

![스크린샷 2020-09-08 오후 10 10 55](https://user-images.githubusercontent.com/16849874/92480652-2c0ac500-f220-11ea-9487-a41e3737398e.png)

---

## process

process는 프로그램과 관련된 정보를 나타내는 객체이며 node 만이 가진 객체입니다.

```js
//이번에는 process객체에 대해서 알아볼 것입니다.
//각 매개변수를 process.argv로 접근합니다.
//process.argv
process.argv.forEach(function (item, index){
        //출력합니다.
        console.log(index + ' : ' + typeof (item) + ' : ', item);

        //실행매개변수에 exit가 있을 경우
        if(item == '--exit'){
                //다음 실행 매개변수를 얻습니다.
                var exitTime = Number(process.argv[index + 1]);

                //일정 시간이후에 프로그램을 종료합니다.
                setTimeout(function (){
                        process.exit();
                }, exitTime);
        }
});
```

## 결과화면

![스크린샷 2020-09-08 오후 10 18 27](https://user-images.githubusercontent.com/16849874/92481367-38dbe880-f221-11ea-81de-2a9bfaaad971.png)

---

## exports

모듈을 만드는데 사용하는 객체입니다.

앞으로의 node는 이러한 모듈을 배워 나가는 방식으로 진행이 될 것입니다.

메인 스크립트에서 module.js의 함수를 가져와서 사용하고 있습니다.

```js
//모듈 파일이다.
//절댓값을 구하는 메서드입니다.
exports.abs = function (number){
        if(0<number){
                return number;
        }else{
                return -number;
        }
};

//원의 넓이를 구하는 공식입니다.
exports.circleArea = function(radious){
        return radious * radious * Math.PI;
};
```

```js
//메인 파일이다.
//모듈을 추출합니다.
var module = require('./module.js');

//모듈을 사용합니다.
console.log('abs(-273) = %d', module.abs(-273));
console.log('circleArea(3) = %d', module.circleArea(3));
```

## 결과화면

![스크린샷 2020-09-08 오후 10 18 55](https://user-images.githubusercontent.com/16849874/92481415-498c5e80-f221-11ea-9a20-28bdb3396fcb.png)