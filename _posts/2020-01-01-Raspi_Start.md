---
title:  "[Raspberry] 라즈베리 파이 시작"
excerpt: "Raspi Setting"
toc: true
toc_sticky: true
header:
  teaser: 

categories:
  - Study
tags:
  - Raspberry
last_modified_at: 2020-01-01T08:18:20

---

처음으로 라즈베리파이를 만져보고 세팅해보았다.  
임베디드 수업이 시간이 안맞아서 미처 들어보지를 못했던지라 처음부터 난관에 빠지기 일수였고 
스트레스 많이 받아가면서 헛짓거리도 많이 했던 것 같다.

라즈베리파이3 b+ 모델을 사용하였다. OS부터 설치하고 연결 후 부팅해보았다.
[라즈베리파이 부팅](https://yeollog.github.io/assets/images/200101_raspi/raspi1.jpg)

실제 윈도우 포맷하듯이 하였고 윈도우와 다른 점이 있다면 라즈베리파이용 이미지파일은 플래싱을 한 즉시 이미 모두 설치되어 있다는 것이다.
그래서 바로 세팅부터 하였다.  

구글링을 하면서 세팅 방법에 대해서 많이 알았기에 자신 있게 하던 도중 시작부터 문제가 발생했다.
[사이트연결문제]](https://yeollog.github.io/assets/images/200101_raspi/raspi2.jpg)

와이파이 연결도 되어 있는데 apt-get 패키지를 얻기위한 사이트에 접속을 하지 못하고 있는 것이다.
이 문제를 해결하기 위해서 역시 구글링을 한 결과 해당 사이트가 먹통이었기에 다른 사이트로 접속하여
apt-get update를 하면 된다고 한다. 

```
$ cd /etc/apt/
$ sudo vi sources.list
```
sources.list 안에 기본적으로 적혀있는 사이트를 다른 ftp나 사이트의 주소로 바꾸게 되면
패키지를 업데이트 할 수 있을 것이라고 하여 한국의 kaist ftp사이트를 주소로 정하여 apt-get을 하였다.

그런데...  

그래도 마찬가지의 결과를 얻게 되었다..  
알고보니, 내가 연결한 와이파이의 문제였던 것이다...  
다른 와이파이를 설정한 후 패키지를 업데이트 및 업그레이드할 수 있었다.  
그리고 vim을 설치하고자 하였으나...

[패키지깨짐]](https://yeollog.github.io/assets/images/200101_raspi/raspi3.jpg)

이번에는 ~held broken packages 문제가 생겼다. 이 문제도 역시 많이 구글링을 해본 결과  
<https://askubuntu.com/questions/78406/cannot-install-vim> 이 사이트에서 답을 찾았다.  

vim이 해결되는건 좋았으나 ibus를 비롯한 다른 것들을 install 하려고 하니 또 ~held broken packages 문제가 발병했기 때문에
결국 포맷하고 다시 처음부터 세팅하게 되었다...

처음부터 네트워크를 잡고 apt-get 패키지도 무난하게 업데이트 및 다운로드 하였고, vim 설치 ibus 까지 설치를 마친 후 vim 세팅을 하였다.  
vim을 세팅하는 이유는 vim을 통해 코딩을 하는데 있어서 좋은 옵션들을 얻기 위해서이다.  
예를 들면, 들여쓰기라던가 줄번호 등 유용한 옵션들이 있으므로 vim으로 코딩을 하고자 하는 사람들은 미리 세팅하기를 권한다.

```
set number            " line 표시를 해줍니다.
set ai                    " auto indent
set si                    " smart indent
set cindent            " c style indent
set shiftwidth=4      " shift를 4칸으로 ( >, >>, <, << 등의 명령어)
set tabstop=4         " tab을 4칸으로
set ignorecase      " 검색시 대소문자 구별하지않음
set hlsearch         " 검색시 하이라이트(색상 강조)
set expandtab       " tab 대신 띄어쓰기로
set background=dark  " 검정배경을 사용할 때, (이 색상에 맞춰 문법 하이라이트 색상이 달라집니다.)
set nocompatible   " 방향키로 이동가능
set fileencodings=utf-8,euc-kr    " 파일인코딩 형식 지정
set bs=indent,eol,start    " backspace 키 사용 가능
set history=1000    " 명령어에 대한 히스토리를 1000개까지
set ruler              " 상태표시줄에 커서의 위치 표시
set nobackup      " 백업파일을 만들지 않음
set title               " 제목을 표시
set showmatch    " 매칭되는 괄호를 보여줌
set nowrap         " 자동 줄바꿈 하지 않음
set wmnu           " tab 자동완성시 가능한 목록을 보여줌

syntax on        " 문법 하이라이트 킴"
```

출처 : <https://norus.tistory.com/13>


라즈베리파이에 대해서 잘 모르지만 일단 첫 걸음을 뗐다는 것도 상당히 중요하다. 다른 공부보다도 유난히 재밌게 했던 것 같다. 그만큼 아쉽고 버려진 것 같은 시간도 많았지만...