---
title: Chirpy 깃블로그 만들기 & 오류 솔루션
date: 2023-01-03 09:00:00 +0900
categories: [Development, GitBlog]
tags: [GitBlog, Error, Solution]
---

## 만났던 오류
```
  Liquid Exception: undefined method `tainted?' for "Text and Typography":String in /home/runner/work/GitBlog/_layouts/post.html
                    ------------------------------------------------
      Jekyll 4.3.1   Please append `--trace` to the `build` command 
                     for any additional information or backtrace. 
                    ------------------------------------------------
```

22년 12월 초 이후로 깃블로그를 만드려고 시도해봤다면 다음과 같은 오류를 만났을 수도 있다.
![Image](https://user-images.githubusercontent.com/52897037/210321849-cdb26ed6-3ca7-4229-bbc2-560f2197f35b.PNG)

검색해도 해결방법도 안나오고 되면서 안되는(?) 상황을 마주하니 멘탈이 살살 녹았다. 그래서 혹시 같은 이유로 `아니 로컬에서 jekyll serve로 돌리면 되는데 왜 깃허브에 올리면 안되지?` 하면서 멘탈이 녹고있을수도 있는 분들을 위해 글을 남겨본다.

### 원인

![Image](https://user-images.githubusercontent.com/52897037/218311338-60cc4c07-d6a2-41b3-8349-cfef618132d4.PNG)

[ruby3.2 변경사항](https://zenn.dev/tmtms/articles/202212-ruby32-5)
<br/>원인은 다음과 같다. 깃블로그 제작에 사용되는 루비가 3.2로 업데이트 되면서 tainted? 라는 메소드가 삭제되었는데 Chirpy로 깃블로그를 생성할때는 저 메소드가 쓰이는 모양 + 깃허브로 업로드하면 루비 3.2버전으로 깃블로그를 생성하려고 시도하기 때문이었다. 심지어 그런 내용을 말해주는 사이트가 구글에 검색해도 저 링크의 일본사이트 하나밖에 검색이 안되더라.. 들어가 보는게 걱정스런 분들을 위해 사진도 남겨본다.

### 해결법
1. GitBlogRepository/.github/workflows/pages-deploy.yml 을 연다.
만약에 pages-deploy.yml.hook 파일이 있으면 뒤에 .hook은 지우면 된다. 그리고 Chirpy를 사용하지 않는데 같은 오류를 겪고 있다면 마찬가지로 깃허브 액션쪽에서 구동되는 파일을 찾으면 된다.
![Image](https://user-images.githubusercontent.com/52897037/210322981-2b1ba111-9eb4-4d17-8b02-2d344dcbc094.PNG)

2. `ruby-version:` 을 찾아준다.
![Image](https://user-images.githubusercontent.com/52897037/210323402-c41728f5-6995-4b0f-b286-aa28e8fa9c05.PNG)

3. 뒤에있는 동그라미를 `'3.1'`로 변경한다.
![Image](https://user-images.githubusercontent.com/52897037/210323686-f6e63f07-b1fe-4508-bb2f-df0b8dd43093.PNG)

느낌상 3이 적혀있으면 3.X 버전 중에 가장 최신 버전으로 다운받는 것 같고 그래서 3.2가 설치되서 오류를 뱉는것으로 보인다. '3.1'로 변경하고 푸쉬해주면 해결된다.

#### 끝나지 않은 시련
![Image](https://user-images.githubusercontent.com/52897037/210324654-d179067c-6caa-47f1-8cbd-114ce812f9c6.PNG)

내용은 대략 23년 5월 31일 이후로는 set-output 이라는 커맨드가 더이상 지원되지 않으니까 환경파일 내용을 고치라는 얘기... 그때가서 해결해야지!

처음에 이 오류를 만났을 때는 깃블로그에 대한 정보가 부족했던 터라<br/>
깃계정마다 깃블로그를 호스팅해주는 서버가 1개씩 존재하고<br/>
그 서버에 되는것도 넣고 안되는것도 넣다가 충돌이 나서 지금 안되고 있구나 라는 생각을 해버렸다.<br/>
그럴만했던게 처음에 Chirpy 아무것도 안건들고 순수하게 쌩으로<br/>
리포지토리에 푸시하니까 페이지가 뜨긴 떴기 때문이다.<br/>
물론 정상적인 Chirpy 블로그는 아니고 내부 파일중에 index.html이 불러와졌다.<br/>

이후에 설정을 내 개인 깃블로그로 고치는 과정에서 리포지토리에 올라가있는건 안되고<br/>
로컬에서 jekyll serve로 돌리면 로컬 접속은 되니까 정말 미치는줄 알았다.<br/>
이게 그 앞에서 말한 `(로컬은)되는데 (깃은)안되는 정말 황당한 경우`에 해당했다.<br/>
따라만 하면 5~10분이면 만들수 있을거라고 생각하고 시작했는데<br/>
Chirpy를 완벽하게 다루는데까지 일주일은 걸린 것 같다. 그래도 결과가 좋으니까 뭐...<br/>
하지만 이제 글을 쓰기 위해서 마크다운을 배워야 한다.. 살려줘!<br/>

그 외 생성하면서 만나볼 수 있는 몇가지 오류들이 더 있긴 한데 대부분 하라는대로 안해서 나오는 오류인것으로 보인다. 뭐를 설치를 안했다던지 설정을 안했다던지 그런 것들은 보통 하라는대로 하면 해결된다.

---

## 깃블로그 생성하기
[Chirpy 제작자의 Getting Started](https://chirpy.cotes.page/posts/getting-started/)
<br/>Chirpy 제작자가 작성한 글을 보고 진행하면 된다.

생성은 몇가지 방법이 있는데 Chirpy Starter를 쓰거나 Chirpy 리포지토리를 fork하거나 zip으로 받아서 내 블로그 리포지토리를 만들고 집어넣고 설정 몇개만 바꿔주면 된다.
<br/>그.런.데<br/>
![Image](https://user-images.githubusercontent.com/52897037/210326659-79b40bbd-1f6d-4bce-b396-bd0041a01a42.PNG)

Starter를 쓰거나 fork를 하게되면 내 리포지토리 밑에 generated from ~ 또는 fork from ~ 이런게 붙게되는데 나는 이게 보기 싫어서 zip으로 받아서 했다.

### How?
[Chirpy 리포지토리](https://github.com/cotes2020/jekyll-theme-chirpy)
<br/>이거를 zip으로 받아서 내 리포지토리에 푸쉬해준다. 물론 푸쉬전에 루비 버전 변경 필수!

탐색기에서 리포지토리 폴더를 우클릭 -> Git Bash Here 클릭<br/>
![Image](https://user-images.githubusercontent.com/52897037/210327302-fdd3b77b-d061-4912-89f0-8c0553b9b1c7.PNG)

`tools/init.sh` 실행<br/>
![Image](https://user-images.githubusercontent.com/52897037/210327550-5ee89da1-6924-4755-9037-981ecfc79139.PNG)

만약 이런 에러가 뜨면서 안되면 커밋할 게 남아있다는 얘긴데 커밋하고 진행해도 되고 아니면 init.sh가 하는게 _post 폴더 내부 지워주고 .github 폴더 내에 pages-deploy.yml.hook 이거 뒤에 .hook 지워주고 pages-deploy.yml이 파일 빼고 나머지 파일 삭제해주고 _config.yml 설정 초기화 해주는거라서 직접 해줘도 된다.

설정이 다 되었다면 깃 배쉬에서 jekyll serve를 입력해서 작동은 되는지 확인해본다.
![Image](https://user-images.githubusercontent.com/52897037/210328697-0da94b0d-4cfb-4a26-8200-38e187d5c578.PNG)

문제가 없다면 위처럼 로컬호스트 주소를 반환해주는데 해당 주소로 접속해서 되는지 확인해보고 깃으로 푸쉬하면 된다. 로컬호스트에서 웹 생성이 안된다면 거의 대부분 뭔가 안깔았다가 원인인것 같다.<br/>
그리고 예전 생성글들을 보면 푸쉬 후에 추가적으로 브랜치를 재설정해줘야되는 과정이 있던데 지금은 안해도 된다. 푸쉬 후에 `안된다면 최우선적으로 깃의 리포지토리에서 액션탭을 열어서 노란불이나 빨간불이 들어와있는지 확인`해야 한다. 만약에 초록불이 떴다, 즉 액션이 제대로 구동했는데 접속이 안되는 것이라면 _config.yml 설정이 되어있지 않아서일 확률이 가장 높다.<br/>

만약 둘 다 문제가 없는데 안된다면.. 저도 잘 모르겠습니다..