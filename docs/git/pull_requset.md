---
layout: default
title: "풀 리퀘스트를 하는 방법 pull request"
parent: git
nav_order: 1
---
# How to use pull requset

---

## 이번장에서는 풀 리퀘스트를 하는 방법에 대해 알아봅니다.

풀 리퀘스트의 순차는 다음과 같습니다.

1. Fork
2. clone
3. branch 생성
4. 수정후 commit -> push
5. pull requset 생성
6. 코드 리뷰, Merge Pull Requset

의 순서로 진행 됩니다.

 1. ## Fork 하는 방법

    가져오고자 하는 프로젝트의 우측 상단에 있는 fork 버튼을 눌러줍니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.14.12.png?raw=true)

2. ## clone

    Fork 해서 내 저장소에 생성된 repo를 clone해서 로컬로 받아줍니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.16.04.png?raw=true)

3. ## branch 생성

    다양한 작업을 할 수 있기 때문에 branch를 만들어서 작업을 해줍니다.  

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.16.33.png?raw=true)


4. ## 수정후 commit -> push

    push를 해서 작업을 올려 줍니다.


5. ## pull requset 생성

    pull request를 해서 작업을 메인 작업과 합치는 요청을 합니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.17.18.png?raw=true)

    위를 누른 다음

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.17.41.png?raw=true)

    를 눌러 줍니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-22%20%EC%98%A4%EC%A0%84%2012.17.58.png?raw=true)

    그러면 pull requset를 보낼수 있습니다.


6. ## 코드 리뷰, Merge Pull Requset

    메인에서는 코드리뷰와 메인 repo에 Merge하는 과정을 가집니다.

    상대방이 pull repuest를 요청하면 아래의 알림이 나타납니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-21%20%EC%98%A4%ED%9B%84%2011.37.17.png?raw=true)    
    
    리퀘스트를 받는 이는 메일로도 알람을 받습니다.
    
    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-21%20%EC%98%A4%ED%9B%84%2011.41.38.png?raw=true)

    여기서 Merge Pull request를 눌러서 메인 repo와 합쳐줍니다.

    ![](https://github.com/C0deWave/C0deWave.github.io/blob/master/image/200522/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-21%20%EC%98%A4%ED%9B%84%2011.38.53.png?raw=true)




참조 : https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/