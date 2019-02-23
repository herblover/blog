+++
date = "2016-12-27T15:55:09+09:00"
title = "Fix windows 7 update issue"
tags = ["Development", "Environment", "Windows 7"]
topics = ["Development Environment Setup", "Windows 7"]
keywords = ["Windows7","Development", "Environment"]
author = "Kenny Kang"
draft = false
description = "Fix windows 7 Windows Update Issue"
+++
온라인 뱅킹과 ActiveX 만 지원하는 사이트를 위해 Hyper-V로 Windows 7 SP1을 설치했더니 왠걸?! 업데이트가 되지 않는다. 윈도우 업데이트에서는 업데이트 대상을 찾는다고 시간만 보내다가 업데이트를 찾을 수 없다는 오류만 뜬다.
관련하여 검색을 해 보다가, [WINDOWS 7 WON’T UPDATE? HERE’S WHAT TO DO](http://plugable.com/2016/06/08/windows-7-wont-update-what-to-do/)라는 좋은 글을 찾아 요약 및 향후 설치에 적용하기 위해 정리해 본다.
추가로, reddit windows에 있는 [Windows 7 Slow/Stuck 'Checking For Updates' FIX (as of July 2016)](https://www.reddit.com/r/windows/comments/4tx4s9/windows_7_slowstuck_checking_for_updates_fix_as/)의 내용까지 합쳐서 정리했다.

## 배경지식

윈도우 7은 일반지원은 모두 종료되었다. (SP1 적용 전 버전은 2013년 4월 9일로 종료, SP1은 2015년 1월 13일로 종료) 다만 이는 기능 업데이트나 전화지원등이 종료된다는 의미이고, 보안 패치를 제공하는 연장지원은 2020년 1월 14일 까지 제공된다. (SP1만 가능)
아직도 몇몇 은행과 정부 사이트를 이용할 때 Windows 10으로는 불편함이 있는 관계로, Windows 10 Pro에 내장된 Hyper-V 를 이용하여 Windows 7 SP1을 설치해서 사용하려고 하니 윈도우 업데이트에서 계속 에러가 떠서 원인을 찾아보게 됐다.

## 시작 전에

당연하게도 사용중인 Windows 7(앞으로 SP1은 생략한다. 이후로 나오는 모든 Windows 7은 Windows 7 SP1을 지칭한다.)이 32bit(x86)인지 64bit(x64)인지 확인부터 해야 한다. 보통 ```컴퓨터```의 ```등록정보```에 가 보면 확인할 수 있다. 해당 창에서 SP1 설치 여부도 확인한다.

![시스템](/images/fix_windows_7_update_issue/win7_control_panel_1.png)

노트북의 경우에는 ```전원 옵션```에서 ```고성능```을 선택하여 업데이트 도중 잠자기 모드에 들어가지 않도록 한다.

![전원 옵션](/images/fix_windows_7_update_issue/win7_control_panel_2.png)

## [간편 롤업](https://support.microsoft.com/ko-kr/kb/3125574)업데이트 설치

[간편 롤업](https://support.microsoft.com/ko-kr/kb/3125574) 업데이트로 SP1 이후 2016년 4월까지 나온 업데이트 중 중요한 항목들을 한번에 설치하자. 다만 해당 업데이트 적용 전 다음 업데이트를 먼저 설치해야 한다.

* [KB3020369](https://support.microsoft.com/ko-kr/kb/3020369)

[KB3020369](https://support.microsoft.com/ko-kr/kb/3020369)를 설치하고 나면 윈도우 재부팅 후 [간편 롤업](https://support.microsoft.com/ko-kr/kb/3125574) 업데이트를 설치한다.

* [KB3125574](https://support.microsoft.com/ko-kr/kb/3125574)

설치가 완료되면 재부팅.

## Windows Update Client 패치

기본 설치가 완료되면 Windows Update Client 패치를 설치한다.
앞으로 모든 패치 설치 후 재부팅 한다.

* [KB3138612](https://support.microsoft.com/ko-kr/kb/3138612)

## [MS16-039: Windows 그래픽 구성요소용 보안 업데이트](https://support.microsoft.com/ko-kr/kb/3145739) 설치

* [KB3145739](https://support.microsoft.com/ko-kr/kb/3145739)

## [월별 보안 업데이트 최신 버전](https://support.microsoft.com/ko-kr/kb/3207752) 설치

처음 참고했던 문서에서는 이 패치를 월별로 설치하도록 가이드 했다. Reddit 문서 및 MS 공식 문서 확인 결과 최종 버전만 설치하면 된다.

* [KB3207752](https://support.microsoft.com/ko-kr/kb/3207752)

## 맺음말

여기까지 설치 후 재부팅 하면 Windows Update가 정상 동작 한다.

문서 내에 있는 링크들은 전부 MS의 Support 페이지로 링크가 되어 있으나, 이 링크들이 자주 깨진다는 이야기가 Reddit에 있었다. 또한, 최근 MS의 Support 페이지는 IE8 (Windows 7 SP1 기본 브라우저)을 지원하지 않아 방금 설치한 Windows 7에서 패치 파일을 다운로드 받는데 어려움이 있다. 따라서 아래에 Windows Update Catalog 링크로 별도 정리한다.

* [KB3020369](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3020369)
* [KB3125574](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3125574)
* [KB3138612](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3138612)
* [KB3145739](https://www.catalog.update.microsoft.com/Search.aspx?q=KB3145739)
* [KB3207752](https://www.catalog.update.microsoft.com/Search.aspx?q=kb3207752)
