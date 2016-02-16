+++
author = "Kenny"
date = "2016-02-15T16:45:16+09:00"
description = "Hugo Hyde-y Theme"
draft = false
keywords = ["hugo", "hyde-y", "theme"]
tags = ["hugo", "theme"]
title = "Hugo Hyde-Y Theme"
topics = ["Hugo", "Hyde-Y"]
type = "post"
+++
이 블로그에 적용한 Hyde-Y Theme에 대해 알아보자.

시작은 야심차게 테마설정법으로 시작했는데, 결국은 Hyde-Y 공식 Readme.md 파일을 일부 번역한게 되어 버렸다. OTL

## Hugo?

Hugo는 Go로 작성한 정적 웹사이트 생성기이다. 자세한 사항은 [Made by HUGO and GitHub Pages](http://brannpark.github.io/blog/posts/20151201_hugo_with_github_pages/)를 참고하자.

## Hyde-Y?

이 블로그는 Hyde-Y 테마를 사용하고 있다. Hugo에서 사용 가능한 테마들은 [Theme Showcase](http://themes.gohugo.io)에서 확인해 볼 수 있다.

많은 테마들 중에 굳이 Hyde-Y를 선택한 이유는 다음과 같다.

- 원래 Jekyll로 블로깅 할 예정이었다. Jekyll에 있던 테마 중 Hyde도 고려 대상 중 하나였다.
- Hyde, Hyde-X, Hyde-Y로 세 가지 포팅이 있는데, 가장 최근에 Github 커밋 내역이 존재한다.
- `data/Menu.toml`파일로 메뉴 관리가 가능하다.
- Hyde 계열 테마들은 색상 테마가 제공되어 기분에 따라 바꿔 쓸 수 있다.

이 글에서는 Hugo에 Hyde-Y Theme를 적용하는데에 집중할 예정이다.

## Theme 설치

Hugo로 생성한 블로그 프로젝트 안에서 다음과 같이 설치한다.

```bash
$ cd your_site_repo/
$ mkdir themes
$ cd themes
$ git clone https://github.com/enten/hyde-y
```

설치에 대해 더 자세한 사항은 [official Hugo themes documentation](http://gohugo.io/themes/installing)을 참고한다.

## 디렉토리 구조

Hyde-Y 테마는 다음과 같은 디렉토리 구조를 가진다.

```
.
└── content
    ├── post
    |   ├── post1.md
    |   └── post2.md
    ├── code
    |   ├── project1.md
    |   ├── project2.md
    ├── license.md // Footer에 있는 license 링크로 연결될 파일
    └── other_page.md
```

`hugo --theme=hyde-y`로 적용하거나, `config.toml`파일 내에 `theme = "hyde-y"`로 적용할 수 있다.

## 설정하기

### Hugo

Hugo 설정을 담당하고 있는 config.toml (기본 값을 사용하는 경우) 파일은 테마에 따라 다른 내용이 들어간다. Hyde-Y 테마의 경우 기본 설정은 다음과 같다.

```toml
# hostname (and path) to the root eg. http://spf13.com/
baseurl = "http://www.example.com"

# Site title
title = "sitename"

# Copyright
copyright = "(c) 2016 yourname."

# Language
languageCode = "ko-kr"

# Metadata format
# "yaml", "toml", "json"
metaDataFormat = "toml"

# Theme to use (located in /themes/THEMENAME/)
theme = "hyde-y"

# Pagination
paginate = 10
paginatePath = "page"

# Enable Disqus integration
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

#
# All parameters below here are optional and can be mixed and matched.
#
[params]
  # You can use markdown here.
  brand = "foobar"
  topline = "few words about your site"
  footline = "code with <i class='fa fa-heart'></i>"

  # Sidebar position
  # false, true, "left", "right"
  sidebar = "left"

  # Text for the top menu link, which goes the root URL for this site.
  # Default (if omitted) is "Home".
  home = "home"

  # Select a syntax highlight.
  # Check the static/css/highlight directory for options.
  highlight = "default"

  # Google Analytics.
  googleAnalytics = "Your Google Analytics tracking code."

  # Sidebar social links.
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

### 메뉴 설정

`data/Menu.toml` 파일에 사이드바 링크를 설정한다. 파일이 없으면 생성한다.

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

### Footer 메뉴 설정

`data/FootMenu.toml` 파일에 푸터 링크를 설정한다. 파일이 없으면 생성하면 된다.

```toml
[license]
  Name = "license"
  URL = "/license"
```

## 내장 색상 테마

스타일시트를 새로 빌드 하여 색상 테마를 바꿔 적용할 수 있다. [npm](http://npmjs.com)을 이용해서 변경할 수 있고, `build:css` 태스크를 사용하면 된다.

```bash
$ vi scss/_00-config.less
# edit configuration

$ npm install
$ npm run build:css

> hyde-y@0.0.4 build:css /home/kenny/workspaces/github/blog/themes/hyde-y
> grunt build:css

Running "less:development" (less) task
File static/css/style.css created

Running "cssmin:target" (cssmin) task
>> 1 file created. 27.04 kb → 14.38 kB

Done, without errors.
```

`watch` 태스크를 사용하면 `scss/*.less` 파일에 변경이 생겼을 때 자동으로 리빌드 하도록 할 수 있다.
