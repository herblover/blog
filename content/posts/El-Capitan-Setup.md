+++
author = "Kenny Kang"
date = "2016-02-15T15:32:55+09:00"
description = "Setup new OS X El Capitan from scratch"
draft = false
tags = ["Development", "Environment"]
title = "El Capitan Setup"
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

2016-02-15 기준은 다음과 같다.

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
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
$ rbenv install 2.3.0
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

## Brew Cask Upgrade Function

Brew Cask를 통해서 설치한 프로그램들은 업그레이드 관련하여 이슈가 있다. 관련하여 다음을 참고하자.

* [brew cask upgrade](https://github.com/caskroom/homebrew-cask/issues/309)
* [Draft Roadmap: `brew cask upgrade`](https://github.com/caskroom/homebrew-cask/issues/4678)
* [Changes to homebrew-cask installation behavior](https://github.com/caskroom/homebrew-cask/issues/13201)

사람마다 추천하는 업그레이드 방법이 다르지만, 개인적으로는 brew-cask-upgrade 라는 shell function을 이용 중이다.

```bash
brew-cask-upgrade() {
  if [ "$1" != '--continue' ]; then
    echo "Removing brew cache"
    rm -rf "$(brew --cache)"
    echo "Running brew update"
    brew update
  fi
  for c in $(brew cask list); do
    echo -e "\n\nInstalled versions of $c: "
    ls /opt/homebrew-cask/Caskroom/$c
    echo "Cask info for $c"
    brew cask info $c
    select ynx in "Yes" "No" "Exit"; do
      case $ynx in
        "Yes") echo "Uninstalling $c"; brew cask uninstall --force "$c"; echo "Re-installing $c"; brew cask install "$c"; break;;
        "No") echo "Skipping $c"; break;;
        "Exit") echo "Exiting brew-cask-upgrade"; return;;
      esac
    done
  done
}
```

위 코드를 ~/.zshrc 마지막에 넣고, 새 shell을 실행 시킨 후, brew-cask-upgrade 명령을 내리면 업그레이드 절차가 진행된다. 다만, Version이 latest일 경우에는 버전 비교가 안되니 이 경우에는 수동으로 uninstall 후 install 해 줘야 한다.

이런 절차가 복잡하고 귀찮거나, 루비 개발자의 경우에는 [brew-cask-upgrade gem](https://github.com/buo/brew-cask-upgrade)을 이용하는 방법을 선택할 수 있다.
