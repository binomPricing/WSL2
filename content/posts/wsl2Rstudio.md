---
author: 권세훈
date: "2022-02-12"
linktitle: WSL2-RStudio-install 
menu: main
tag:
- R
- RStudio
- RStudio Server
- Ubuntu
- WSL2
- Sys.setenv
- Sys.getenv
- R environment
title: 우분투에서 RStudio 설치 및 사용
weight: 10
---

> WSL2 및 일반 우분투(20.04)에 R 설치되었다면  
> 이제 코딩환경으로 RStudio 설치하면 되는데  
> 우분투 등 리눅스에서는 이왕이면 데스크탑 버전보다는  
> 서버 기능까지 추가된 서버 버전이 더 좋을 것임 


# RStudio 설치(서버 버전)

- [RStudio 웹페이지 설치 안내](https://www.rstudio.com/products/rstudio/)

```
$ sudo apt-get install gdebi-core
$ wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2021.09.2-382-amd64.deb
$ sudo gdebi rstudio-server-2021.09.2-382-amd64.deb
```

#### RStudio 서버의 구동과 종료

```
$ sudo rstudio-server start
$ sudo rstudio-server stop
```

# RStudio 사용

- RStudio 서버 시작 후에 윈도우에서 브라우저 열고 다음 URL 접속

  - [http://localhost:8787](http://localhost:8787) 또는
  - [http://127.0.0.1:8787](http://127.0.0.1:8787) 
  - 참고로 디폴트 포트는 `8787` 이며, 설정에서 변경 가능

- 시작 화면 나오면 아이디와 비번에 우분투 계정명과 비번 입력

#### RStudio 주요 설정 사항

- 상단 메뉴의 `Tools` 에서 `Global Option` 으로 진입해 설정함

  - 한글 입력은 `UTF-8` 이 디폴트 인코딩이므로 별 문제 없으나
  
    - 만약 설정이 잘 안되어 있다면 
    - `Code` >> `Saving` >> `Default text encoding : UTF-8` 로 설정함 
    
#### RStudio 환경설정변수 확인 및 설정(콘솔 명령임)

- 확인: `> Sys.getenv()`

- 설정변경: `> Sys.setenv(환경변수=변경사항)`

- 기타 관련 명령 보려면 `> Sys.` 입력한 상태에서 `TAB` 키 누르면 관련 명령이 나열됨

#### R 패키지 설치 방법

- R 의 강점은 오픈소스로서 다양한 라이브러리가 있다는 점이다

- 라이브러리 이용을 위해 먼저 패키지를 설치해야 하는데 두 방법이 있다

  - 첫째, RStudio 메뉴 이용  
  
    - 상단 메뉴에서 `Tools` >> `Install Packages ...` 
  
  - 둘째, R 또는 RStudio 콘솔에서 명령줄 입력
  
    - `> install.packages("패키지명")`
    
  - 참고로 패키지 저장 경로가 시스템용 및 개인용 두 가지가 있음

#### 참고로 추가적인 환경설정은 다음 글들 참고

- [pdf 사용 위한 TinyTeX 설정](/posts/wsl2Rpdf)

- [pdf 사용 위한 TeX Live 사용법](/posts/wsl2texlive)

- [Hugo theme 이용한 웹페이지 구축]()