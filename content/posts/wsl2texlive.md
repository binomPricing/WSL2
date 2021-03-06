---
author: 권세훈
date: "2022-02-12"
linktitle: WSL2-RStudio-texlive 
menu: main
tag:
- R
- RStudio
- RStudio Server
- R profile
- Ubuntu
- WSL2
- TeX Live
- TeXmaker
- pdf
- KTUG
- VS Code
title: RStudio 와 TeX Live
weight: 10
---

> 한글 pdf 파일 생성을 위해 TeX 조판 프로그램이 필요한데  
> 윈도우, WLS2, 리눅스 등에서 TeX Live 가 대표적이다  
> 이글은 [KTUG 의 WLS 의 TeX Live 설치법](http://wiki.ktug.org/wiki/wiki.php/%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0Windows/WSLtexlive)을 보충 설명하고  
> 특히 RStudio, VSCode, TeXmaker 등을 위한 환경설정에 대해 다룬다  



# TeX Live 를 WSL2 에 설치하는 법

- [KTUG 의 WLS 의 TeX Live 설치법](http://wiki.ktug.org/wiki/wiki.php/%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0Windows/WSLtexlive)에 잘 설명됨 >> 따라 할 것

  - 다만 년도 부분 경로를 2020 대신 최신용인 2021 로 대체

- 이렇게 하면 윈도우에서 `TeXmaker` 등 텍에디터 사용 가능

- 문제는 `RStudio` 와 `VSCode` 가 `LATEX` 인식을 못하는 경우가 있음

  - 기본적으로 `PATH` 환경변수 설정 문제로 추가 조치 필요

# 먼저 윈도우에서 TeXmaker 등 사용하기

- KTUG 설치법에서  `c:\texscripts` 경로의 명령파일들이 중요

- 모든 명령파일들이 명령어를 이름으로 하고 내용은 동일함

    ```
    @echo off
    bash --login -c "%~n0 %*"
    ```

  - 내용의 개요는 윈도우에서 명령파일을 실행하면  
  - 배쉬셀(우분투) 명령창으로 들어가서  
  - 실행된 파일의 이름을 명령으로 실행한다는 뜻임

- 이를 통해 윈도우에 `TeXmaker` 등 설치해 `TeX Live` 이용 가능

  - 즉 `TeX` 편집 프로그램의 명령 빌드 설정에
  - `c:\texscripts` 경로의 해당 명령파일을 지정하면 됨

# VS Code 가 TeX Live 인식 못하는 경우 

- WSL2 우분투에 설치된 텍을 윈도우에서 `VS Code` 로 사용하려면

  - 해당 우분투 폴더에서 다음 명령 실행
  
    `$ code . `
  
- 그런데 텍 명령이 안될 수 있음 >> 경로설정 필요

    `$ PATH=$PATH://usr/local/texlive/2021/bin/x86_64-linux`
    
- 이렇게 하는 것은 임시적 >> 영구적 변경 필요

  - 다음 명령을 `~/.bashrc` 파일 마지막에 추가
  
    `export PATH=$PATH:/usr/local/texlive/2021/bin/x86_64-linux`
    
  - 쉬운 명령법 예시
  
  `$ echo export PATH=$PATH:/usr/local/texlive/2021/bin/x86_64-linux >> ~/.bashrc`

#### 참고사항

- `~/.bashrc` : 해당 사용자에만 적용되는 환경설정

- `/etc/bash.bashrc` : 전체에 적용되는 환경설정


# 다음으로 RStudio 가 LATEX 인식하도록 하기

- 특히 WSL2 우분투에 설치된 `RStudio Server`의 경우

- 콘솔에서 `> Sys.getenv()` 명령을 통해 환경변수를 확인하면

- PATH 에 TeX Live 명령 경로가 포함되지 않은 것을 알 수 있음

#### 해결책 두 가지

(1) 환경변수 PATH 하나를 골라(예: `/usr/bin`) 심볼릭 링크 파일 생성

```
$ sudo ln -s /usr/local/texlive/2021/bin/x86_64-linux/pdflatex /usr/bin/pdflatex
$ sudo ln -s /usr/local/texlive/2021/bin/x86_64-linux/xelatex /usr/bin/xelatex
$ sudo ln -s /usr/local/texlive/2021/bin/x86_64-linux/lualatex /usr/bin/lualatex
```

  - 만약 `/usr/local/bin/` 경로에 tex 명령어 링크파일들이 있다면 재링크 파일 생성

```
$ sudo ln -s /usr/local/bin/pdflatex /usr/bin/pdflatex
$ sudo ln -s /usr/local/bin/xelatex /usr/bin/xelatex
$ sudo ln -s /usr/local/bin/lualatex /usr/bin/lualatex
```

  - 이 외에 추가 명령 필요하면 링크파일도 추가 생성

(2) RStudio 환경변수에 해당 경로를 추가

  - tex 명령어 링크파일들이 있는 경로를 환경설정에 추가해 줌
  
    - 만약 `/usr/local/bin` 경로에 tex 명령어 링크 있다면

    ```
    > add_path <- "/usr/local/bin"
    > old_path <- Sys.getenv("PATH")
    > Sys.setenv(PATH = paste(old_path, add_path, sep = ":"))
    ```
  
  - 그런데 이러한 명령은 일시적이므로 계속 적용되도록

    - .Rprofile 파일 내용 변경 저장 필요(아래 추가 사항 참고)


#### 참고로 추가적인 환경설정은 다음 글들 참고

- [pdf 사용 위한 TinyTeX 설정](/posts/wsl2Rpdf)

- [Hugo theme 이용한 웹페이지 구축]()

- [.Rprofile 변경에 대하여]()