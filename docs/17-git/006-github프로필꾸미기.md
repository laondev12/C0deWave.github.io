---
layout: default
title: "github 프로필 꾸미기"
parent: git
nav_order: 6
---

# github 프로필 꾸미기

먼저 꾸미기 전의 화면부터 봅시다.

![스크린샷 2020-12-25 오후 8 46 55](https://user-images.githubusercontent.com/16849874/103135515-2ae0a380-46fc-11eb-9b54-a812c7e5f388.png)

먼저 꾸미기 위해서 자신의 github handle과 같은 이름으로 저장소를 만듭니다.

Add a README file을 체크해줍니다.

이 README.md 를 수정하면 됩니다.

![스크린샷 2020-12-25 오후 8 50 18](https://user-images.githubusercontent.com/16849874/103135535-48157200-46fc-11eb-9d4e-a984718e7025.png)

자신의 깃헙 status를 보이고 싶다면 해당 부분을 추가하면 됩니다.

    ![{}'s github stats](https://github-readme-stats.vercel.app/api?username=C0deWave&show_icons=true&&theme=dracula&count_private=true)

위의 username에 본인의 깃헙 핸들로 바꾸면 됩니다.

그럼 아래와 같이 나오게 됩니다.

![{}'s github stats](https://github-readme-stats.vercel.app/api?username=C0deWave&show_icons=true&&theme=dracula&count_private=true)

가장 많이 사용한 언어를 나타내고 싶다면???

    ![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=C0deWave&layout=compact&hide=csharp)

![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=C0deWave&layout=compact&hide=csharp)

---

## 나는 언제 커밋을 할까???

1. 먼저 아래의 git저장소를 fork합니다.

    [https://github.com/techinpark/productive-box](https://github.com/techinpark/productive-box)

2. [https://gist.github.com](https://gist.github.com)에서 아무 public gist를 생성해 줍니다.

![스크린샷 2020-12-25 오후 9 25 52](https://user-images.githubusercontent.com/16849874/103135638-edc8e100-46fc-11eb-917e-5b98dd4dbb78.png)

3. github토큰을 repo와 gist두가지를 선택한 채로 생성을 합니다.

    [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new) 여기에서 토큰을 생성합니다.

![스크린샷 2020-12-25 오후 9 28 43](https://user-images.githubusercontent.com/16849874/103135645-f8837600-46fc-11eb-9387-0bce45284908.png)

4. 토큰 값을 따로 저장을 합니다. 지금만 보여줍니다. 그러니 따로 저장합니다.

5. 포크한 레포에 들어가서 Setting -> Secrets를 눌러서 New Secret를 눌러 환경변수를 생성해 줍니다.

6. 생성한 gist 의 https://gist.github.com/jamg123123/이부분 -> 이부분 을 따로 토큰으로 만들어 줍니다.

7. 그후 포크해온 저장소의 .github/workflow에서 환경변수를 지정합니다.

8. 포크해온 저장소의 Action을 enable 해줍니다.

![스크린샷 2020-12-25 오후 9 23 30](https://user-images.githubusercontent.com/16849874/103135626-dc7fd480-46fc-11eb-9671-4d6d28efebcc.png)

9. 그러면 gist에서 자정이나 포크한 레포가 커밋될때마다 커밋수를 통계를 내줍니다.

![스크린샷 2020-12-25 오후 9 51 27](https://user-images.githubusercontent.com/16849874/103135653-0b964600-46fd-11eb-9aef-1551b9d3d846.png)

이제 이것을 public로 권한을 변경한 다음에 이를 Pinned하면 됩니다.

---

최종적으로 아래의 화면으로 바꾸었습니다.

![스크린샷 2020-12-25 오후 10 08 15](https://user-images.githubusercontent.com/16849874/103135736-bd357700-46fd-11eb-81b3-77c10195145b.png)

꾸준히 업데이트 해야 겠습니다.