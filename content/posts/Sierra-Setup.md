+++
author = "Kenny Kang"
description = "Setup new MacOS Sierra from scratch"
keywords = ["MacOS", "Sierra", "Development", "Environment"]
topics = ["Development Environment Setup"]
date = "2016-12-09T15:02:51+09:00"
title = "Sierra Setup"
draft = false
tags = ["Development", "Environment"]
+++
OS X가 MacOS로 이름을 바꾸고 [Homebrew](http://brew.sh) 버전이 올라가면서 관련 설치 및 설정이 많이 간소화 되었다. 따라서 새로운 방법에 맞춰 처음 MacBook등을 받았을 때 개발환경 구축과정을 블로그에 남긴다.

## [Homebrew](http://brew.sh) 설치

[Homebrew 공식 페이지](http://brew.sh)를 참고하여 Terminal에서 설치한다.

2016-12-09 기준은 다음과 같다.

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Brewfile Clone 및 Brew Bundle로 전체 설치

이전에는 [Caskroom](https://caskroom.github.io)등을 Command Line에서 설치했으나, [Homebrew Bundle](https://github.com/Homebrew/homebrew-bundle)을 통해 쉽게 설치할 수 있도록 변경 되었다.
이 문서에 필요한 Brewfile은 [Bootstrap files for setup new Mac](https://github.com/herblover/macos_bootstrap) 리포지트리에 올려 놓았다.

```bash
$ git clone https://github.com/herblover/macos_bootstrap
$ cd macos_bootstrap
$ brew bundle
```

## 기본 쉘을 ZSH로 변경하기

Brewfile을 통해 최신 버전 zsh가 설치 되어 있다. 기본 쉘을 ZSH로 변경하고 oh-my-zsh를 설치한다.

```bash
$ echo "/usr/local/bin/zsh" | sudo tee -a /etc/shells
$ chsh -s $(which zsh) # 이후 터미널 재시작
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Ruby 개발 환경 설정

필요한 brew들은 모두 Brewfile을 통해 설치 완료 되어 있는 상태이다.

```.zshrc``` 파일에서 **plugin** 부분을 찾아 **rbenv** 추가 후 iterm 재시작.

```bash
$ rbenv install 2.3.3
```

## Python 개발 환경 설정

```bash
$ pip install virtualenv virtualenvwrapper
```

```.zshrc``` 파일에서 **plugin** 부분을 찾아 **virtualenv virtualenvwrapper** 추가 후 iterm 재시작.

## Brew Cask Upgrade

[El Capitan Setup](http://blog.funspaces.org/2016/02/15/el-capitan-setup/) 에서는 ```.zshrc```파일에 별도 함수를 작성하여 업데이트 하였으나, 지금은 ```Brewfile```안에 ```brew cu``` Command를 설치하도록 했다. 따라서 이후 Cask들을 업데이트 하고 싶을 때는 다음과 같이 하면 된다.

```bash
$ brew update && brew cu
```

## 마무리

```Brewfile``` 덕에 많은 작업이 줄어들었다. 자동 설치되는 각 프로그램은 [macos_bootstrap 리포지트리](https://github.com/herblover/macos_bootstrap)에서 확인할 수 있다.
