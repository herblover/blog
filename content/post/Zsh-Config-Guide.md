+++
tags = ["Development","Environment","Zsh","Oh My Zsh"]
keywords = ["Zsh","Z Shell","Oh My Zsh","Development","Environment"]
type = "post"
title = "Z shell with Oh My Zsh Config Guide"
topics = ["Development Environment Setup", "Mac OS", "Sierra", "Zsh", "Z shell", "Oh My Zsh"]
description = "Install and Configure Z shell with Oh My Zsh on Mac OS Sierra"
author = "Kenny Kang"
date = "2017-02-13T19:08:26+09:00"
+++

## 시작하며
MacOS로 작업하는 개발자들이 늘어나고, JavaScript/Python/Ruby등을 사용하는 프로그래머들이 늘어나면서 Shell 작업이 다시 수면위로 떠오른 지 몇년이 지났다. [Github](https://github.com) 이후로는 많은 프로젝트가 Git으로 작업하고 있는데, Git를 사용하다 보면 별도 설치해서 사용하는 GUI Client들이 생각보다 불편한 경우가 많아 Shell로 돌아오는 분위기이다. 특히 [Github Flow](https://guides.github.com/introduction/flow/)나 [Git Flow](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/)로 공동작업을 하는 경우에는 빈번한 Branch &amp; Merge 작업이 생기기 때문에 GUI Client로 작업하기 불편한 상황을 자주 만나게 된다.

이런식의 Shell 작업이 늘어나면서 당연히 써야 한다고 생각하던 Bash를 Z shell로 교체하는 사람들도 늘어나고 있다. Bash에 특별한 문제가 있는건 아니지만, 좀 더 편한 작업환경을 찾다 보니 Z shell 사용자가 늘어나게 된 것이다.

## Z shell - 1990년에 나온 Shell이 갑자기 왜?
Z Shell도 아주 오래된 프로젝트이다. [Bash](https://www.gnu.org/software/bash/)가 89년이고 [Z Shell](http://www.zsh.org)이 90년인 걸 보면 겨우 1년 차이일 뿐이다. 근데 갑자기 왜 Z Shell에 대한 이야기가 나오고 있을까?
Z Shell이 좋다고 주장하는 내용을 보면 대충 다음과 같이 정리가 된다.

* Bash 보다 Command Prompt가 이쁘다.
* History Substring Search가 편하다.

[왜 Zsh가 당신이 사용중인 Shell보다 좋을까?](http://www.slideshare.net/brendon_jag/why-zsh-is-cooler-than-your-shell)에서 Zsh가 가진 여러 장점을 소개하고 있다. Z shell 사용자들이 이야기하는 기본적인 장점들은 모두 소개되어 있으니 한번 읽어볼 만 하다.

좀 더 상세한 장점을 알고 싶으면 [My favorite Zsh features](http://code.joejag.com/2014/why-zsh.html)를 참고하도록하자. 이 글에서 설명하고 있는 몇 가지 기능들은 개인적으로 아주 편리하게 쓰고 있다.

기능이 복잡해지면 당연히 설정이 어렵기 마련이다. 기본 설정이 잘 되어 있어야 많은 사용자를 확보할 수 있지만, 개인별 설정이 다양하기로는 Shell도 어디가서 빠지지 않는 법. 이 부분을 잘 해결한 Framework가 [Oh-My-Zsh](http://ohmyz.sh/)다.

## Oh-My-Zsh - Z shell을 살린 Framework
Oh-My-Zsh는 [Robby Russell](https://www.planetargon.com/about/robby-russell)이 동료들을 위해 만든 Framework다. (이 녀석을 Framework라고 부르는게 맞는지는 모르겠다.) 개발 시작부터 인기를 얻게되는 과정은 [d'Oh My Zsh](https://medium.com/@robbyrussell/d-oh-my-zsh-af99ca54212c)를 읽어보자. 오픈소스를 시작하는 개발자를 위한 조언까지 친절하게 나와 있다.

2009년 Oh-My-Zsh가 공개된 이후 많은 개발자들이 기본 설치된 Bash에서 Z shell로 변경하였다. 같은 Shell을 공유하는 Server 작업에서는 여전히 몇 가지 문제로 인해 Bash를 주력으로 사용하고 있지만, 로컬 작업에서는 Z shell이 좀 더 편한 개발환경에 도움이 될 거라 확신한다.

## 주의사항
여기에 장황하게 늘어놔도 어차피 읽지 않는 사람들이 대다수인건 알고 있지만, 그래도 이후에 디버그 하는 사람들을 위해 주의해야 할 부분을 남긴다. 이런 개발환경의 경우 사람마다 많이 다르게 사용중인 경우가 많아 진행 중 문제가 생기면 주의사항을 다시 확인해 보기 바란다.

* 이후로 나오는 모든 설정은 macOS Sierra, Homebrew를 기반으로 하고 있다.
* 이 글을 쓰고 있는 현재 Homebrew Zsh는 5.3.1 버전이다.
* Homebrew 및 Homebrew Cask, Z shell의 설치는 [Sierra Setup](http://blog.funspaces.org/2016/12/09/sierra-setup/)을 참고하도록 하자.
* Rbenv, Pyenv등의 설정에 대한 이야기들이 나오겠지만, 해당 부분 역시 [Sierra Setup](http://blog.funspaces.org/2016/12/09/sierra-setup/)을 참고하도록 하자. 이 글에서는 PROMPT / RPROMPT 등 Z shell &amp; Oh-My-Zsh에 관련된 내용만을 다룬다.
* Terminal Emulator는 [iTerm2](https://iterm2.com)를 사용한다고 가정한다.
* **Z shell은 로컬 작업용으로 추천한다.** Server에 기본 값인 Bash를 변경할 때는 꼭 협업하는 사람들과 상의하자. Server에서 공용 계정으로 작업하는 경우가 꽤 많기 때문에 기본값은 가능하면 건드리지 않아야 한다.

## Oh-My-Zsh 설치 및 기본 설정
[공식 홈페이지](http://ohmyz.sh)에 따르면 다음과 같다. (2017. 02. 13. 기준)

### curl로 설치할 경우
```sh
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### wget으로 설치할 경우
```sh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

위와 같이 설치하면 ```~/.oh-my-zsh```디렉토리가 생기면서 관련 파일들이 들어가게 된다. Oh-My-Zsh에서 중요하게 생각하는 것 중 하나가 자동 업데이트이기 때문에 이후로 업데이트에 관해서는 신경쓰지 않아도 된다.

설치 도중 ```~/.zshrc```이 있으면 자동으로 ```~/.zshrc.pre-oh-my-zsh```파일로 백업하니 걱정할 필요 없다. 마지막에 자동으로 Default Shell을 Z shell로 변경하니 ```chsh```명령어도 특별히 신경 쓸 필요 없다.

기본 ```.zshrc```에서 살펴 볼 부분은 ```ZSH_THEME```부분과 ```plugins```부분 두 군데다.

* ```ZSH_THEME```
	* Z shell에서 사용할 테마를 선택하는 부분이다. 기본 값은 ```robbyrussell```
	* Oh My Zsh 설치 후 ```~/.oh-my-zsh/themes``` 디렉토리에 기본 지원하는 테마가 설치되어 있다.
	* 2017. 02. 13. 기준 141개 테마가 같이 설치된다.
* ```plugins```
	* Z shell에 적용할 plugin들을 선택하는 부분이다. 기본 값은 ```(git)```
	* 2017. 02. 13. 기준 232개 plugin이 같이 설치된다.

Oh My Zsh를 사용하면 기본 설정에서 주로 테마와 plugin을 건드리게 된다. 관련하여 좀 더 알아보자.

## 테마 설정
Z shell을 로컬 환경에서 주로 사용하도록 추천하는 이유 중 하나는 바로 테마다. 사실 Z shell이라는 익숙하지 않은(비록 Bash와 거의 동일해서 특별히 더 적응할 필요는 없다지만.) Shell로 변경하면서까지 사용하는 이유 절반은 테마 때문이다. 나머지 절반은 plugin일 테고.

Oh My Zsh 기본 테마인 ```robbyrussell```은 아래와 같이 생겼다.

![robbyrussell theme](https://cloud.githubusercontent.com/assets/2618447/6316876/710cbb8c-ba03-11e4-90b3-0315d72f270c.jpg)

국내에서 많이 사용중인 ```agnoster``` 테마는 아래와 같이 생겼다.

![agnoster theme](https://cloud.githubusercontent.com/assets/2618447/6316862/70f58fb6-ba03-11e4-82c9-c083bf9a6574.png)

개인적으로는 [Powerlevel9k](https://github.com/bhilburn/powerlevel9k) 테마를 추천한다. 아래와 같은 테마이다.

![Powerlevel9k](https://camo.githubusercontent.com/80ec23fda88d2f445906a3502690f22827336736/687474703a2f2f692e696d6775722e636f6d2f777942565a51792e676966)

### Powerlevel9k
Powerlevel9k를 추천하는 이유는 다음과 같다.

* [Awesome Terminal Fonts](https://github.com/gabrielelana/awesome-terminal-fonts)를 사용할 수 있다.
	* [Awesome Terminal Fonts 설정](https://github.com/bhilburn/powerlevel9k/wiki/Install-Instructions#option-3-install-awesome-powerline-fonts)을 참고하자. 만약 ```Homebrew Cask Fonts```가 활성화 되어 있다면 ```brew cask install font-awesome-terminal-fonts```로 쉽게 설치할 수 있다. 설치 후 iTerm2 Profile에서 꼭 폰트를 바꿔주도록 하자.
	* ```.zshrc```설정은 위의 링크에 나와 있다. ```awesome-patched```로 설정해 주면 된다.
* PROMPT &amp; RPROMPT 개인화가 편하다.
	* [Prompt Customization](https://github.com/bhilburn/powerlevel9k#prompt-customization)을 참고하면 된다.
	* 기본 값으로도 충분하고, 많은 사람들이 [Show Off Your Coding](https://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config)에 설정을 공유하고 있다.

## Plugin
기본값인 [(git)](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugin:git)은 Git 작업을 도와주는 Alias들과 PROMPT용 Z shell Function들을 모아 놓은 plugin이다. 아래와 같은 plugin들이 Shell 작업에 많은 도움이 됐다.

별도로 표시되어 있지 않은 플러그인들은 그냥 ```.zshrc```파일에 추가해서 쓰면 된다.

```sh
$ vim ~/.zshrc
...
plugins=(git rbenv pyenv)
...
```

* [rbenv](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/rbenv/rbenv.plugin.zsh) plugin
	* [rbenv](https://github.com/rbenv/rbenv) 사용 시 도움이 되는 plugin 이다. rbenv plugin을 활성화 시키면 별도로 ```.zshrc```파일에 ```rbenv init``` 명령을 입력하지 않아도 된다. (입력시 충돌로 이상한 현상이 일어나니 꼭 입력하지 말기 바란다.)
* [pyenv](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#pyenv) plugin
	* [pyenv](https://github.com/yyuu/pyenv) 사용 시 도움이 되는 plugin 이다.
* [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) plugin
	* [Fish shell](http://www.fishshell.com/)처럼 자동완성을 해 주는 plugin 이다. 해당 사이트 참고.
	* [![zsh-autosuggestions plugin][2]][1]
* [zsh-syntax-highligting](https://github.com/zsh-users/zsh-syntax-highlighting) plugin
	* Z shell 문법 하이라이트 plugin. 상세한 설치 방법은 해당 사이트를 참고하자.

  [1]: https://asciinema.org/a/37390
  [2]: https://camo.githubusercontent.com/8135e25b744f29e5fd83964eded4bd255aa1da74/68747470733a2f2f61736369696e656d612e6f72672f612f33373339302e706e67

## 정리하며
개인적으로 사용중인 Z shell 화면은 다음과 같이 생겼다.

![Kenny Zsh Prompt](/images/zsh_config/powerlevel9k.png)

Powerlevel9k 설정은 다음과 같다

```sh
POWERLEVEL9K_MODE='awesome-patched'
POWERLEVEL9K_SHORTEN_DIR_LENGTH=3
POWERLEVEL9K_SHORTEN_STRATEGY="truncate_middle"
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(status os_icon dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(pyenv node_version rbenv time)
POWERLEVEL9K_TIME_FORMAT="%D{%H:%M} \uE12E"
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX="%{%F{249}%}\u250f"
POWERLEVEL9K_MULTILINE_SECOND_PROMPT_PREFIX="%{%F{249}%}\u2517%{%F{default}%} "
POWERLEVEL9K_SHOW_CHANGESET=true
POWERLEVEL9K_CHANGESET_HASH_LENGTH=6
```
