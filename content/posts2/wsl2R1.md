---
author: 권세훈
date: "2022-02-11"
linktitle: WSL2-R-install-1 
menu: main
tag:
- R
- RStudio
- RStudio Server
- Ubuntu
- WSL2
- CRAN
title: 우분투에 최신 R 설치(간단 방법)
weight: 10
---

> WSL2 및 일반 우분투(20.04)에서 R 설치하는 방법은 세 가지
>
>  1) 우분투 제공 패키지 설치 >> 편리하나 최신 버전이 아님   
>  2) 공식적인 CRAN 지시대로 설치 >> 최신 버전이나 복잡함
>  3) RStudio 제공 간단법 >> 특히 버전 지정 방식이 편리함
>
> 여기서는 RStudio 제공 간단법에 대해 설명하고  
> 다음 글에서  CRAN 의 설치법에 대해 설명하겠다.  


- [CRAN: The Comprehensive R Archive Network](https://cran.r-project.org) 

- [RStudio: R Quick Installation](https://github.com/rstudio/r-builds)

- [CRAN 방법에 대한 설명 글](/posts/wsl2R2) 

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

# [RStudio 제공 R 간단 설치법](https://github.com/rstudio/r-builds)

- 아래 내용은 우분투(20.04) 기준임

- 다른 리눅스 버전은 [R Quick Installation](https://github.com/rstudio/r-builds) 참고할 것

# 1. 자동 설치법

- 정해진 R 버전을 자동 설치함

- `$ bash -c "$(curl -L https://rstd.io/r-install)"`

  - `curl` 미설치로 에러 난다면 설치

    `$ sudo apt install curl`
    
  - 참고로 `curl` 명령 사용법은 [이 곳 정리](https://www.lesstif.com/software-architect/curl-http-get-post-rest-api-14745703.html)가 잘 됨

# 2. 수동 설치법

- 본인이 원하는 버전을 지정하여 설치

- 원하는 버전을 먼저 입력

  - 예: 2021년말 기준 최신버전은 4.1.2.

    `$ R_VERSION=4.1.2`

- 웹에서 설치파일 다운로드

  `$ wget https://cdn.rstudio.com/r/ubuntu-2004/pkgs/r-${R_VERSION}_1_amd64.deb`
  
  - `wget` 미설치로 에러 난다면 설치

    `$ sudo apt install wget`

- 설치

  ```
  $ sudo apt-get install gdebi-core
  $ sudo gdebi r-${R_VERSION}_1_amd64.deb
  ```
- 정상 설치 확인(버전 확인)

  `$ /opt/R/${R_VERSION}/bin/R --version`

- 경로 환경변수 설정(링크 파일 생성)

  ```
  $ sudo ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R 
  $ sudo ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript
  ```

#### R 이 명령줄에서도 잘 실행되는지 확인(선택) `$ R`

- R 에서 빠져나오는 방법은 화면에 설명되어 있는대로 `> q()`

#### [참고: CRAN 방법에 대한 설명 글](/posts/wsl2R2)

#### 이제 R 이 설치되었으므로 RStudio 를 설치하면 됨

- [RStudio 서버 설치 설명](/posts/wsl2Rstudio)