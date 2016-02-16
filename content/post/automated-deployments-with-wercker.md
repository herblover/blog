+++
author = "Kenny"
date = "2016-02-16T14:13:53+09:00"
description = "Automated Deployments with Wercker"
draft = true
keywords = ["Hugo", "Wercker", "Github-Pages"]
tags = ["Hugo", "Wercker", "Github-Pages"]
title = "Automated Deployments with Wercker"
topics = ["Hugo", "Wercker", "Github-Pages"]
type = "post"
+++

기본 Hugo 프로젝트를 만들고 Wercker를 이용해 자동으로 Github-Pages에 배포하는 작업을 해 보자. 역시나 이 글도 Hugo 공식 문서에 있는 [Automated deployments with Wercker](http://gohugo.io/tutorials/automated-deployments/)의 일부 내용 번역과, 직접 해 보면서 느낀 부분들을 첨언하는 것에 중점을 두려고 한다.

## Hugo Site 생성

Github-Pages, Wercker, Hugo Site Builder를 이용하려면 일단 Hugo Site를 만들어야 한다. Hugo 설치 및 기본 사용법들은 [Made by HUGO and Github Pages](http://brannpark.github.io/blog/posts/20151201_hugo_with_github_pages/)에 상세하게 설명 되어 있으니 기본적인 커맨드들로 진행한다.

```bash
$ hugo new site hugo-sample
$ cd hugo-sample
```

사이트 전체에 사용될 테마를 선택하여 적용한다. 여기서는 [Hyde-Y](https://github.com/enten/hyde-y) 테마를 골랐다.

```bash
$ mkdir themes
$ cd themes
$ git clone https://github.com/enten/hyde-y.git
```

Git에서 충돌나는 것을 방지하기 위해 hyde-y 테마 안의 git repository는 제거해 주자. 색상 테마 변경시에도 제거해 주는 쪽이 편리하다.

```bash
$ rm -rf hyde-y/.git
```

새 페이지를 추가 하기 전에, Hyde-Y 테마에 맞는 기본 설정들을 추가하자

```bash
$ cd ..
# config.toml 수정하기
```

```toml
baseurl = "http://www.example.com" # 원하는 주소로 변경
title = "sitename" # 원하는 사이트 명으로 변경
copyright = "(c) 2016 yourname." # 저작권 표시
languageCode = "ko-kr" # 한국어 사용
metaDataFormat = "toml" # 메타데이터 파일 형식
theme = "hyde-y" # 사용할 테마명

# 페이징 설정
paginate = 10
paginatePath = "page"

# Disqus 연동 설정. (댓글 기능 제공용)
disqusShortname = "your_disqus_shortname"

[permalinks]
  post = "/:year/:month/:day/:slug/"
  code = "/:slug/"

[taxonomies]
  tag = "tags"
  topic = "topics"

[author]
  name = "yourname"
  email = "yourname@example.com"

# 이 아래로는 옵션들.
[params]
  # 여긴 Markdown 문법 사용 가능
  brand = "foobar"
  topline = "few words about your site"
  footline = "code with <i class='fa fa-heart'></i>"

  # 사이드 바 위치 설정
  # false, true, "left", "right"
  sidebar = "left"

  # 사이트 루트로 가는 링크 이름 정하기.
  # 생략시 기본값은 "Home".
  home = "home"

  # 코드 하일라이트 테마 선택
  # static/css/highlight 디렉토리에서 옵션 확인.
  highlight = "default"

  # Google Analytics.
  googleAnalytics = "Your Google Analytics tracking code."

  # 사이드바 소셜 링크
  github = "enten/hugo-boilerplate" # Your Github profile ID
  bitbucket = "" # Your Bitbucket profile ID
  linkedin = "" # Your LinkedIn profile ID (from public URL)
  googleplus = "" # Your Google+ profile ID
  facebook = "" # Your Facebook profile ID
  twitter = "" # Your Twitter profile ID
  youtube = "" # Your Youtube channel ID
  flattr = "" # populate with your flattr uid

[blackfriday]
  angledQuotes = true
  fractions = false
  hrefTargetBlank = false
  latexDashes = true
  plainIdAnchors = true
  extensions = []
  extensionmask = []
```

이제 사이드바에 표시될 링크를 설정한다. 사이드바에 나타날 메뉴는 `data/Menu.toml`에서 설정할 수 있다.

```toml
[about]
  Name = "About"
  URL = "/about"
[posts]
  Name = "Posts"
  Title = "Show list of posts"
  URL = "/post"
[tags]
  Name = "Tags"
  Title = "Show list of tags"
  URL = "/tags"
```

마지막으로 Footer 에 나올 메뉴를 설정한다. Footer 메뉴는 `data/FootMenu.toml`에서 설정할 수 있다.

```toml
[license]
  Name = "license"
  URL = "/license"
```

설정이 완료되면 간단하게 about 페이지를 추가 해 보자.

```bash
$ hugo new about.md
```

About 페이지를 임시상태에서 해제하고, 몇 마디 넣어 보자.

```bash
$ hugo undraft content/about.md
```

다 끝나면, hugo 서버를 띄워서 브라우저에서 확인해 볼 수 있다.

```bash
$ hugo server
```

별 문제없이 모든 설정이 완료되면, localhost:1313 으로 접속했을 때 다음과 같은 페이지가 나온다.

![기본 홈페이지](/images/automated_deployments_with_wercker/screenshot01.png)

Hugo 커맨드를 실행해서 서버에 올라갈 파일들을 생성 해 보자.

```bash
$ hugo
```

## 버전 컨트롤 세팅

기본 작업이 끝난 Hugo 사이트를 Git에 추가하자.

```bash
$ git init
```

`git status`라고 입력하면 **config.toml** 파일, **content/** 디렉토리, **data/** 디렉토리, **themes/** 디렉토리, **public/** 디렉토리가 표시된다.
**public** 디렉토리는 hugo가 생성한 컨텐츠가 들어가는 곳이니, 버전 컨트롤에 넣을 필요는 없다. 제외해 주자.

```bash
$ echo "/public" >> .gitignore
```

아직 static 디렉토리에 파일이 없어서 버전 컨트롤에 등록이 안된다. static 디렉토리가 없으면 이후 작업에서 에러가 나면서 정상 빌드가 안된다.
구글 등 검색엔진이 확인할 수 있는 robots.txt 파일을 추가하자.

```bash
$ echo "User-agent: *\nDisallow:" > static/robots.txt
```

다 끝나면, 리포지트리에 커밋하자.

```bash
$ git add .
$ git commit -a -m "Initial Commit"
```

## Github 연동하기

Github에 새 프로젝트를 만들고, 방금 까지 작업한 Hugo Site와 연결하자. Github 프로젝트 생성 후 clone url을 복사 한 후에 연결하면 된다.

```bash
$ git remote add origin git@github.com:YourUsername/hugo-sample.git
$ git push -u origin master
```

