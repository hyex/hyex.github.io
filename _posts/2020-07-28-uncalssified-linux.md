---
layout: post
title:  "Start Ubuntu 18.04.4 in VirtualBox"
date:   2020-07-28 01:17:00 +0900
categories: unclassified
---


## Downloads

1. VirtualBox v6.1.12

먼저, 무료 가상환경을 제공하는 virtualBox를 먼저 설치해준다.

![https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads

위 사이트에서 자신의 운영체제에 알맞는 버전을 다운받는다. 나는 Mac을 사용하기 때문에 `OS X hosts`를 다운받았다.

사실 예전에 VirtualBox를 사용해본 경험이 있어, 이미 노트북에 깔려있었는데 너무 오랜만에 키다보니까 오류가 발생했다. 버전 업데이트하면 해결될 것 같아 최신 버전으로 업데이트했고, 문제 없이 잘 동작했다.

2. Ubuntu v18.04.4 LTS

![http://mirror.kakao.com/ubuntu-releases/bionic/](http://mirror.kakao.com/ubuntu-releases/bionic/)

위 페이지에서 `62-bit PC (AMD64) desktop image` 버튼을 클릭하여 Desktop image를 다운받는다. 

## Ubuntu 설치

이 과정은 보통 Default를 따라가면 되고, Google에 관련 블로그 글이 많이 올라와있다. 기본 설정은 

![https://haloaround.tistory.com/15](https://haloaround.tistory.com/15)

위 페이지를 참고했고, 2048/15GB 로 설정했다. 
계정 정보는 기억해두는게 좋은데, 난 간단하게 kimhyeji/1234로 설정했다.

이런 바탕화면이 보인다면, 설치는 완료한 것이다. 

<img src="/assets/img/1_install_complete.png" />

## ssh 연결

### 설치

`sudo apt install net-tools`
`sudo apt-get install ssh`

ssh 연결을 위해 필요한 ssh 모듈을 다운받는다. 네트워크 설정 확인이 필요할텐데, 나같은 경우는 `net-tools` 설치가 안되어 있어 별도로 설치해주었다.

### ssh 켜기

#### ubuntu

1. sudo service ssh start
2. ps aux | grep ssh    // 현재 돌아가고 있는 서버를 보여줌
3. ip addr      // 사용할 ip 주소 

#### mac (본인 OS)

1. ssh hyex@172.20.10.10 

위 hyex는 새로 만든 계정 이름이다. 간단하게 hyex/asdd1212 로 설정했고, ubuntu GUI 설정에서 사용자 추가로 만들었다.

> 여기까지 하면 내 OS에서 ubuntu를 마음대로 조종할 수 있다 !

