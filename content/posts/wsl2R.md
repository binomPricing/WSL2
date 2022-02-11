---
author: 권세훈
date: "2022-02-11"
linktitle: WSL2-R-install 
menu:
  main:
    parent: tutorials
next: /tutorials/github-pages-blog
prev: /tutorials/automated-deployments
title: WSL2 에서 R 설치하기
weight: 10
---


# 설치 준비

- Ubuntu:20.04.3 을 WSL2 에 설치함

  - 설치 방법은 인터넷에 설명이 많으니 참고할 것

- 우분투 쉘 터미널에서 아래 사항들을 진행함

  - 설치 진행되는 과정에서 계속 진행 여부 물으면 y 누름

`$ sudo apt update && sudo apt upgrade -y`

#### 패키지 설치 도우미의 설치

- 미리 설치해 두지 않으면 나중에 일부 패키지는 설치가 안됨
```
$ sudo apt install gcc
$ sudo apt install gcc make
$ sudo apt install g++
```
#### 도우미 설치 성공 확인(선택)
```
$ gcc --version
$ make --version
$ g++ --version
```

#### opencv 작동 위한 준비 설치(선택)  
- 향후 opencv 관련 패키지 설치시 필요할 것이므로 미리 설치  
- 선택사항이며 참고로 설치에 시간이 꽤 오래 걸림  

`$ sudo apt install libopencv-dev`

# 필요한 도우미 패키지 설치

`$ sudo apt install --no-install-recommends software-properties-common dirmngr`

#### 공개키 받음

`$ wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc`

- 혹시 wget 패키지가 설치되지 않은 경우 에러 날 수 있음

  - 이 경우도 그냥 설치해주면 됨
  
    `sudo apt install wget`

#### 공개키 정상 여부 확인(선택)  
`$ gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc`

- 해쉬 값이 동일한지 확인하는 것임


# R 저장소 설치

- R 패키지를 공인된 저장소로부터 불러와 설치하기 위한 조치임 

`$ sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" `

- 참고로 명령줄 안의 `$( ... )` 표현은 명령 `...` 결과를 이용하는 것임

- lsb-release 패키지가 미설치되어 에러가 나는 경우는 설치해주면 됨

`$ sudo apt install lsb-release && sudo apt clean all`  

- 설치 성공 확인(선택)
```
$ lsb_release -a
$ lsb_release -cs
```

# R 기본 패키지 설치

`$ sudo apt install --no-install-recommends r-base`

#### R 4.0 이상용 주요 패키지 설치를 위한 저장소 추가 설치

`$ sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+`

#### R 주요 패키지들은 단체로 한꺼번에 미리 설치해 주면 편하다

- 여기 포함되지 않은 패키지들은 추후 사용하면서 필요할 때 설치

```
$ sudo apt install --no-install-recommends r-cran-rstan
$ sudo apt install --no-install-recommends r-cran-tidyverse
```

#### R 이 명령줄에서도 잘 실행되는지 확인(선택) `$ R`

- R 에서 빠져나오는 방법은 화면에 설명되어 있는대로 `> q()`

#### 이제 R 이 설치되었으므로 RStudio 를 설치하면 됨

- [RStudio 설치 설명 링크]()