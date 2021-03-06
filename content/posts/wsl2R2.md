---
author: 권세훈
date: "2022-02-11"
linktitle: WSL2-R-install-2 
menu: main
tag:
- R
- RStudio
- RStudio Server
- Ubuntu
- WSL2
- CRAN
title: 우분투에 최신 R 설치(정식 방법)
weight: 10
---


> WSL2 및 일반 우분투(20.04)에서 R 설치하는 방법은 세 가지
>
>  1) 우분투 제공 패키지 설치 >> 편리하나 최신 버전이 아님   
>  2) 공식적인 CRAN 지시대로 설치 >> 최신 버전이나 복잡함
>  3) RStudio 제공 간단법 >> 특히 버전 지정 방식이 편리함
>
> 이전 글의 RStudio 제공 간단법 설명에 이어  
> 이번 글은 CRAN 의 설치법에 대해 설명하겠다.  

- [CRAN: The Comprehensive R Archive Network](https://cran.r-project.org) 

- [RStudio: R Quick Installation](https://github.com/rstudio/r-builds)

- [RStudio 에서 제공하는 간단 설치 설명 글](/posts/wsl2R1) 

# R 설치 준비

- WSL2 및 우분투 설치 방법은 인터넷에 설명 많으니 참고할 것

R 설치는

- 우분투 패키지 관리에서 제공하는 앱을 설치하거나

- 다음처럼 바로 설치하면 예전 버전이 설치됨(권장하지 않음)

  `$ sudo apt install r-base`
  
최신 버전 사용하려면 다음 방법들을 따라가야 함

- 우분투 쉘 터미널에서 아래 사항들을 진행함

- 설치 진행되는 과정에서 계속 진행 여부 물으면 y 누름

  `$ sudo apt update && sudo apt upgrade -y`

# [CRAN 제공 최신 R 설치 설명](https://cran.r-project.org)

# 1. 준비

#### 패키지 설치 도우미의 설치

- 미리 설치해 두면 나중에 일부 패키지 설치가 원활함
    ```
    $ sudo apt install gcc
    $ sudo apt install gcc make
    $ sudo apt install g++
    ```
- 도우미 패키지 설치 성공 확인(선택)
    ```
    $ gcc --version
    $ make --version
    $ g++ --version
    ```

#### opencv 작동 위한 준비 설치(선택)  
- 향후 opencv 관련 패키지 설치시 필요할 것이므로 미리 설치  
- 선택사항이며 참고로 설치에 시간이 꽤 오래 걸림  

  `$ sudo apt install libopencv-dev`

# 2. 필요한 도우미 패키지 설치

`$ sudo apt install --no-install-recommends software-properties-common dirmngr`

#### 공개키 받음

`$ wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc`

- 혹시 wget 패키지 미설치 에러나면 설치
  
  `$ sudo apt install wget`

#### 공개키 정상 여부 확인(선택)  
`$ gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc`

- 해쉬 값이 동일한지 확인하는 것임


# 3. R 저장소 설치

- R 패키지를 공인된 저장소로부터 불러와 설치하기 위한 조치임 

```
$ sudo add-apt-repository \
  "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" 
```

- 명령줄 안의 `\` 는 코드 내용이 너무 길어 보기 좋게 줄 바꿈하는 것임 

- 명령줄 안의 `$( ... )` 는 명령 `...` 결과를 입력으로 이용하는 것임

- lsb-release 패키지가 미설치되어 에러가 나는 경우는 설치해주면 됨

  `$ sudo apt install lsb-release && sudo apt clean all`  

- 설치 성공 확인(선택)
    ```
    $ lsb_release -a
    $ lsb_release -cs
    ```

# 4. R 설치

#### R 기본 패키지 설치

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

#### [참고: RStudio 에서 제공하는 간단 설치 설명 글](/posts/wsl2R1) 

#### 이제 R 이 설치되었으므로 RStudio 를 설치하면 됨

- [RStudio 서버 설치 설명](/posts/wsl2Rstudio)