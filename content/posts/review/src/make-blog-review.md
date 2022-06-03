---
title: "Hugo로 블로그 만들기"
description: "Hugo + Github Pages를 이용해 나만의 개발 블로그 만들기"
date: 2022-06-03T21:45:40+09:00
hero: /images/hugo.png
theme: Toha
menu:
  sidebar:
    name: "Hugo로 블로그 만들기"
    identifier: make-blog-review
    # 카테고리
    parent: review
    weight: 1
# tags: ["Basic", "Multi-lingual"]
# categories: ["Basic"]
tags: ["개발블로그", "블로그", "Hugo", "Toha", "Github Pages"]
categories: []
draft: false
---

<br>

## 이 글에서는...

<br>

Hugo를 이용해 정적 웹사이트를 만들고, Github Pages를 통해 나만의 웹사이트를 배포합니다.

<br>

---

<br>

## Hugo란?

<br>

[Hugo 공식 웹사이트 바로가기](https://gohugo.io/)

Hugo는 정적 웹사이트를 쉽게 만들게 해주는 툴이다.<br>

<br>

---

<br>

## Hugo를 선택한 이유

<br>

#### 블로그를 만든 이유

<br>

우선 내가 블로그를 만든 이유를 간단히 적어보면 다음과 같다.<br>

1. 개발이나 공부를 할 때 기록할 곳이 필요했다. 공부를 하더라도 몇 개월 있으면 공부했던 내용을 잊어버리는 일이 잦았다.
2. 개인 웹사이트가 있으면 내가 공부한 것이 무엇인지, 내가 관심 있는 것이 무엇인지 효과적으로 사람들에게 알릴 수 있다.
3. 지금까지 여러 개발 블로그들을 통해 문제를 해결한 경험이 많은데, 도움을 받았으니 나도 도움을 주고 싶었다.

이런 이유로 블로그를 만들게 되었고, 위 이유를 충족하기 위해서는 사용자 별로 DB에서 데이터를 나눠준다던지 인증을 한다던지 하는 과정이 필요하지 않았다.<br>
그래서, 원래 계획은 내 개인 웹 서버를 만들어서 서비스하는 것이었지만, 너무 많은 공수가 들어간다는 생각이 들어 동적 웹사이트보다는 정적 웹사이트로 내 블로그를 만들어보기로 했다.<br>

<br>

#### Static web generator

<br>

구글에서 정적 웹사이트를 만들어주는 도구(static web generator)를 찾아보면 다음 친구들이 나온다.<br>
(모두 오픈소스 프로젝트들이다)<br>

<br>

##### Jekyll
- 루비 기반
- 가장 인기 있는 툴. 사람들이 많이 사용하기 때문에 문제가 발생했을 때 커뮤니티에서 쉽게 도움을 받을 수 있음
- 컨텐츠가 많으면 빌드 속도가 느려짐

##### Hexo
- Node.js 기반
- 한국어 documentation이 잘 되어 있음
- 확장성이 뛰어남 ([공식 플러그인 링크](https://hexo.io/plugins/))

##### Hugo
- Go 언어 기반
- 아주 빠른 빌드 속도
- 문서화가 잘 되어 있음
- 플러그인, 테마 등 확장성이 다소 좋지 않음

나는 이 중에서 Hugo를 선택했다.<br>
내가 원하는 기능은 공부한 내용을 markdown 파일에 적어서 업로드만 하면 되는 것이었는데, 따라서 많은 플러그인들이 필요 없었기 때문이다.<br>
또한, 내가 공부한 내용을 블로그에 업로드하는 과정을 스트레스로 만들고 싶지 않았다.<br>
사실 이전에도 내가 공부한 것들을 블로그에 올리려고 시도했으나, 공부한 것에 비해 글을 쓰는 시간이 너무 오버헤드로 다가와서 글을 쓰는 것을 포기한 적이 있다.<br>
따라서 글을 쓰는 것 이외의 귀찮은 일들 (업로드, 디자인, 기타 관리)을 최소화하고 싶었다.<br>
위에 단점으로 '플러그인, 테마 등 확장성이 다소 좋지 않음'이라고 적어놓기는 했으나,<br>

- 마크다운 형식 지원
- 댓글 기능 지원
- 여러 플랫폼에 배포 지원
- 문제 발생 시 찾아볼 공식 사이트 문서화 잘 되어 있음

등등 블로깅을 할 때 기본적인 요소들이 충분히 있다고 생각해 Hugo를 선택했다. <br>

<br>

---

<br>

## Hugo 설치하기

<br>

Hugo 설치는 다음 과정을 따르면 된다. 난이도가 어렵지 않으니 추가적인 설명은 생략하겠다.<br>

[Hugo 공식 사이트 getting-started](https://gohugo.io/getting-started/quick-start/)<br>

Hugo에서는 기본 테마가 없고, [Hugo 공식 사이트](https://themes.gohugo.io/)에서 테마를 선택해 따로 설치해주어야 한다.<br>
이때, 테마 별로 설치 방법이 상이한 경우가 있으므로, 테마 안의 공식 문서를 잘 확인하고 과정을 따라주어야 한다.<br>
모든 과정을 마치고 터미널 창에 `hugo version`을 입력했을 때, (Windows 운영체제 기준으로 설명)<br>

```console
> hugo version
'hugo'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는 배치 파일이 아닙니다.
```

또는 

```console
> hugo version
hugo : 'hugo' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다. 이름이 정확한지 확인하고 경로가 포함된 경우 경로가 올바른지 검증한 다음 다시 시도하십시오.
...
```

와 같은 에러 메시지가 등장한다면, 환경 변수를 설정해주어야 한다.<br>
[환경 변수 설정하는 방법](https://devji.tistory.com/entry/Windows-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)<br>
<br>
그렇지 않고 <br>

```console
> hugo version
hugo v0.100.0-27b077544d8efeb85867cb4cfb941747d104f765 windows/amd64 BuildDate=2022-05-31T08:37:12Z VendorInfo=gohugoio
```

와 같이 버전이 나타난다면 Hugo가 성공적으로 설치된 것이다.<br>

<br>

#### Hugo 기본 명령어<br>

<br>

##### hugo server

<br>

```console
> hugo server
```

작업한 내용을 로컬에서 확인할 수 있도록 로컬 웹 서버를 열어준다.<br>
여기에서 웹페이지에 문제가 있는지 확인한 후 빌드를 할 수 있다.<br>
포트 기본값은 1313이다.<br>
위 명령어를 입력한 뒤 [http://localhost:1313](http://localhost:1313)를 확인해보자.<br>

<br>

##### hugo

<br>

```console
> hugo
```

작업한 내용을 빌드한다. 빌드한 내용은 `<hugo new site를 통해 만든 디렉터리>/public/`에 저장된다.<br> 
참고: `-D` 옵션을 주면 초안(draft)까지 빌드할 수 있다.<br>

<br>

---

<br>

## Toha 테마 설치하기

<br>

> 아래부터는 Toha가 아닌 다른 테마를 선택했다면 도움을 받지 못할 수 있습니다.<br>

> 빠르게 자신만의 웹사이트를 만들어보고 싶다면 아래 과정을 따라도 되지만, 다른 테마 디자인을 원하는 사람들은 [여기](https://themes.gohugo.io/)에서 테마를 선택해 주세요.

<br>

#### Toha 테마를 선택한 이유

<br>

나는 다음 이유 때문에 Toha 테마를 선택했다.<br>

1. 블로그를 포트폴리오 웹사이트로도 사용하고 싶었다.
2. 게시글을 카테고리 별로 잘 정리하고 싶었다.
3. 깔끔한 디자인을 사용하고 싶었다.
4. 한글을 지원해야 한다.

Toha 테마는 위 요구사항들을 만족했고, 그래서 테마로 Toha를 골랐다.<br>

<br>

#### Toha 테마 설치하기

<br>

Toha 공식 사이트의 [Getting Started](https://toha-guides.netlify.app/posts/getting-started/prepare-site/)에서 Toha 테마를 적용하는 방법을 확인할 수 있다.<br>
위 `hugo server` 명령어를 이용해 웹사이트에 깨지는 부분이 있는지 확인하면서, 차근차근 과정을 밟아가는 것을 추천한다.<br>

<br>

##### 자주 발생하는 에러

<br>

웹사이트에 깨지는 부분이 있을 때 (주로 이미지 파일을 정해진 위치에 넣지 않았을 때) 다음 에러 메시지가 발생한다.<br>

```console
C:\yechan\code\blog>hugo server
Start building sites …
hugo v0.100.0-27b077544d8efeb85867cb4cfb941747d104f765 windows/amd64 BuildDate=2022-05-31T08:37:12Z VendorInfo=gohugoio
ERROR 2022/06/03 15:33:26 render of "page" failed: "C:\yechan\code\blog\themes\toha\layouts\_default\single.html:49:46": execute of template failed: template: _default/single.html:49:46: executing "content" at <partial "helpers/get-author-image.html" .>: error calling partial: "C:\yechan\code\blog\themes\toha\layouts\partials\helpers\get-author-image.html:29:22": execute of template failed: template: partials/helpers/get-author-image.html:29:22: executing "partials/helpers/get-author-image.html" at <$authorImage.RelPermalink>: nil pointer evaluating resource.Resource.RelPermalink
ERROR 2022/06/03 15:33:26 render of "home" failed: "C:\yechan\code\blog\themes\toha\layouts\index.html:41:8": execute of template failed: template: index.html:41:8: executing "index.html" at <partial "sections/home.html" .>: error calling partial: "C:\yechan\code\blog\themes\toha\layouts\partials\sections\home.html:111:29": execute of template failed: template: partials/sections/home.html:111:29: executing "partials/sections/home.html" at <$authorImage.RelPermalink>: nil pointer evaluating resource.Resource.RelPermalink
Error: Error building site: failed to render pages: render of "page" failed: "C:\yechan\code\blog\themes\toha\layouts\_default\single.html:49:46": execute of template failed: template: _default/single.html:49:46: executing "content" at <partial "helpers/get-author-image.html" .>: error calling partial: "C:\yechan\code\blog\themes\toha\layouts\partials\helpers\get-author-image.html:29:22": execute of template failed: template: partials/helpers/get-author-image.html:29:22: executing "partials/helpers/get-author-image.html" at <$authorImage.RelPermalink>: nil pointer evaluating resource.Resource.RelPermalink
Built in 283 ms
```

<br>

위 에러 메시지는 author의 이미지가 없어 home이라는 page를 렌더링하는 데에 실패했다는 메시지이다.<br>
이미지를 로드하는 기본 위치는 `<hugo new site를 통해 만든 디렉터리>/assets/images`이다. (`/theme/toha/assets/images` 안에 넣어도 동작은 하나, 권장하지 않음)<br>
따라서, `/data/sections/author.yaml`에 `image: "images/author/author.png"`라고 적혀 있다면 `<site 디렉터리>/assets/images/author/author.png`를 저장해주면 된다.<br>
만약 `<site 디렉터리>/assets/` 디렉터리를 찾을 수 없다면, 새로 만들어주면 된다.<br>

<br>

##### Toha 테마 

<br>

위 Toha 웹사이트의 Getting Started 부분을 완료했고 `hugo server`를 통해 확인을 했을 때 깨지는 부분 없이 웹사이트가 잘 렌더링된다면, 적절히 `/data/`에 있는 yaml 파일들의 내용들과 `/assets/images/`의 이미지 파일들을 변경해주자.<br>

<br>

---

<br>

## Github Pages에 웹사이트 배포하기

<br>

이 과정은 Hugo 공식 웹사이트의 [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/) 문서에 나와 있다.<br>

<br>

정적 웹사이트를 배포하기 위해서 필요한 부분은 `/public/` 뿐이다. <br>
site 디렉터리(`/`), 배포용 디렉터리(`/public/`)를 각각의 repository에 저장하는 방법도 있겠지만, 여러 개의 repository를 만드는 방법보다는 site 디렉터리(`/`)만을 Github의 `<사용자 이름>/github.io` repository에 저장한 뒤, push가 발생하면 `Github Actions`를 이용해 자동으로 `/public/`을 배포용 branch `gh-pages`에 저장하는 방법을 Hugo에서는 권장한다.<br>

**한 줄 요약: github.io repository로 push만 하면 배포됨**<br>

위 문서를 진행하기 전, 다음 중 하나라도 되어있지 않다면 진행해 주자.<br>

1. Git 설치
2. Github 가입
3. Github에서 `<사용자 이름>.github.io`라는 이름의 repository 생성

<br>

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

위 내용을 `/.github/workflows/gh-pages.yml`에 저장해 주고, (디렉터리나 파일이 없으면 새로 만들면 된다) git push를 한 뒤 Github Actions가 잘 작동하는지 확인한다.<br>
(Github의 `<사용자 이름>.github.io` repository의 Actions에서 예쁜 초록색 마크가 뜨는지 확인)<br>
그 후 또다시 Github의 `<사용자 이름>.github.io` repository에서 Settings > Pages > Source를 gh-pages로 바꿔주면 된다.<br>

<br>

#### 자주 발생하는 에러

<br>

##### 1. git push가 안돼요.

만약 `<사용자 이름>.github.io` repository를 처음 만들 때 `README.md`를 추가했다면 원격 저장소와 로컬 저장소의 내용이 달라 푸쉬가 되지 않을 수 있다.<br>
- `<사용자 이름>.github.io` repository에 아무 내용이 없다면, repository를 삭제 후 아무 파일 없는 상태로 새로 만들어 다시 시도해보거나, 또는 `git fetch`를 시도해 보자.<br>

##### 2. Github Actions가 잘 동작하지 않아요.

- `<사용자 이름>.github.io` repository의 Actions 탭에서 실패한 Action을 확인해보고, 어디에서 오류가 났는지 확인해 보자.<br>
- `hugo server` 명령어를 통해 웹페이지를 렌더링하는 과정에서 오류가 없는지 다시 한번 확인해보자.<br>
- git의 submodule 문제라면, `git submodule sync`를 시도해 보자.

<br>

---

<br>

## 마치며

<br>

이 글을 통해 Hugo로 웹사이트를 만드는 여러분들에게 도움이 됐다면 좋겠다. <br>
글을 작성하는 현재는 댓글 기능이 없지만 곧 추가할 예정이다.

<br>