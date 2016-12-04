+++
tags = ["Development", "Environment"]
date = "2016-12-04T16:28:04+09:00"
description = "NTFS write setting for mac os sierra"
keywords = ["MacOS", "Sierra", "Development", "Environment"]
topics = ["Development Environment Setup"]
title = "NTFS for mac"
type = "post"
draft = false
author = "Kenny Kang"
+++
MacOS Sierra 에서 외장 하드 / USB 드라이브 사용을 위해 NTFS 쓰기를 활성화 시키는 방법.

## [Mounty for NTFS](http://enjoygineering.com/mounty/)를 이용하는 방법

한참 아래 항목인 ```/etc/fstab```파일로 NTFS 쓰기모드를 활성화 하는 방법을 적고 나니, 동일한 일을 해 주는 Mounty를 발견 했다.

[다운로드](http://enjoygineering.com/mounty/releases/Mounty.dmg) 하고 일반 어플리케이션 설치와 동일하게 진행한 후 실행하면 NTFS 외장 하드/USB를 연결하면 알림영역에 **Re-mount with read-write?**와 같은 메시지가 뜬다. remount를 선택하면 쓰기모드로 사용 가능.

```Homebrew Cask```사용자의 경우 다음과 같이 설치하면 된다.

```bash
$ brew cask install mounty
```

## ```/etc/fstab```을 이용하는 방법

```bash
$ sudo vi /etc/fstab
```

파일을 생성 후 다음과 같은 내용으로 편집 하고 저장한다.

```
LABEL=Seagate\040\Expansion\040Drive none ntfs rw,auto,nobrowse
```

여기서 LABEL은 파인더에 표시된 드라이브 이름을 적어 주면 된다. Escape 되어 있는 \040은 Space를 의미한다. 따라서 위의 설정에서 ```Seagate\040\Expansion\040Drive``` 부분은 파인더에서 ```Seagate Expansion Drive```라고 표시 될 때를 의미한다.

```fstab``` 파일을 이용하는 방법 설명은 많은데 드라이브 명에 스페이스가 있는 경우에 대한 이야기가 별로 없어서 기록으로 남김.

## Paragon NTFS for Mac 14

```/etc/fstab``` 만으로 대부분의 경우에 충분하겠지만, 비용을 들이더라도 편리하고 지원을 받을 수 있는 방법을 찾고 있다면 [Paragon NTFS for Mac 14](https://paragon.cleverbridge.com/80/?affiliate=40962&scope=checkout&cart=164265&coupon=E86-VA8-CP7)쪽을 추천한다. 경쟁 제품으로 Tuxera NTFS 가 있으나, 전체적인 반응으로는 Paragon 쪽이 낫다는 분위기다. 특히 Tuxera NTFS는 ```Tuxera ntfs could not mount```과 관련된 오류들이 많이 보고 되고 있어 시도하지 않았다.

추가로, Seagate의 외장하드 계열을 사용중인 사람이라면, 굳이 비용을 들이지 않고도 Paragon NTFS를 사용할 수 있다. [Mac OS용 Paragon Driver(10.9. 이상)](http://www.seagate.com/kr/ko/support/downloads/item/ntfs-driver-for-mac-os-master-dl/) 을 받아 설치하면, Seagate 전용으로 설정된 Paragon NTFS 사용이 가능하다.
