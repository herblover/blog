+++
author = "Kenny Kang"
date = "2016-02-11T15:39:55+09:00"
description = "Setup new os x el capitan machine from scratch"
draft = false
keywords = ["OS X", "El Captian", "Development", "Environment"]
tags = ["Development", "Environment"]
title = "El Capitan Setup"
topics = ["Development Environment Setup"]
type = "post"
+++
## XCode 설치

App Store에서 xcode 설치

> 느릴 경우 [Xcode Download](https://developer.apple.com/kr/xcode/downloads/) 에서 다운로드.

XCode Command line tools 설치

```bash
$ xcode-select --install
```

## Homebrew 설치 및 Homebrew Cask Tap

Homebrew 설치

> [Homebrew](brew.sh) 에서 Install Homebrew 참고하여 Terminal에서 설치.

2015-11-24 기준은 다음과 같다.

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Homebrew cask 설정

```bash
$ brew tap caskroom/cask # homebrew cask enable
$ brew tap caskroom/versions # homebrew cask alternative versions enable (firefoxdeveloperedition)
$ brew tap caskroom/fonts # homebrew cask for fonts
$ brew tap homebrew/dupes # homebrew for system duplicate formulae
```

iTerm2 설치

```bash
$ brew cask install iterm2
```

## 기본 쉘을 ZSH로 변경하기

zsh를 homebrew 버전으로 설치 후 oh-my-zsh 설치. dot files 관리용 yadm 설치 및 설정

```bash
$ brew install zsh
$ echo "/usr/local/bin/zsh" | sudo tee -a /etc/shells
$ chsh -s $(which zsh) # 이후 터미널 리스타드
$ sh -c "$(curl -fsSL https://raw.github.c om/robbyrussell/oh-my-zsh/master/tools/install.sh)"
$ brew tap TheLocehiliosan/yadm && brew install yadm
$ yadm init
$ yadm add .zshrc
$ yadm commit
```

## GNU 버전 시스템 유틸리티 설치

기본 System Utility를 GNU 버전으로 변경

```bash
$ brew install findutils --with-default-names
$ brew install gnu-sed --with-default-names
$ brew install gnu-tar --with-default-names
$ brew install gnutls --with-default-names
$ brew install grep --with-default-names
$ brew install coreutils
$ brew install binutils
$ brew install diffutils
$ brew install gzip
$ brew install watch
$ brew install tmux
$ brew install wget
$ brew install nmap
$ brew install gpg
$ brew install htop
$ brew install macvim --with-override-system-vim
$ brew linkapps macvim
```

## 기본 OS X 어플리케이션 설치

기본적으로 사용할 OS X 어플리케이션 설치

```bash
$ brew cask install iterm2
$ brew cask install google-chrome
$ brew cask install firefoxdeveloperedition
$ brew cask install java
```

## Homebrew cask로 폰트 설치하기

여기서부터 **iterm2** 로 작업

```bash
$ brew cask install font-nanumgothic
$ brew cask install font-nanumgothiccoding
$ brew cask install font-d2coding
```

## 자바 개발용 프로그램 설치

```bash
$ brew cask install intellij-idea # intellij-idea 설치. ce 버전을 원하면 brew cask install inteillj-idea-ce
```

## 루비 개발용 프로그램 설치

```bash
$ brew install rbenv ruby-build
```

.zshrc 파일에서 **plugin** 부분을 찾아 **rbenv** 추가 후 iterm 재시작.

```bash
$ yadm add .zshrc
$ yadm commit
$ rbenv install 2.2.3
$ brew cask install rubymine
```

## 파이썬 개발용 프로그램 설치

```bash
$ brew install python
$ pip install virtualenv virtualenvwrapper autoenv
$ mkdir -p ~/workspaces/virtualenvs
$ echo "export WORKON_HOME=$HOME/workspaces/virtualenvs" >> ~/.zshenv
$ echo "export PIP_VIRTUALENV_BASE=\$WORKON_HOME" >> ~/.zshenv
$ yadm add .zshenv
$ yadm commit
```

.zshrc 파일에서 **plugin** 부분을 찾아 **virtualenv virtualenvwrapper autoenv** 추가 후 iterm 재시작.

```bash
$ yadm add .zshrc
$ yadm commit
```
