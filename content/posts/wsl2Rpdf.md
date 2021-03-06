---
author: 권세훈
date: "2022-02-12"
linktitle: WSL2-RStudio-pdf 
menu: main
tag:
- R
- RStudio
- RStudio Server
- Ubuntu
- WSL2
- Markdown
- LaTeX
- TinyTeX
- TeX Live
- pdf
- Rmd
- kotex
title: RStudio 에서 한글 pdf 만들기
weight: 10
---

> RStudio 에서 마크다운 등으로 pdf 파일을 만들려면  
> TeX 조판 프로그램이 필요한데,  
> 이번 글에서는 TinyTeX 설치와 사용법을 알아본다

- [Live TeX 풀버전 설치 및 사용법은 다른 글](/posts/wsl2texlive) 참조할 것


# RStudio 에 TinyTeX 설치 및 사용법

- [TinyTeX 소개](https://yihui.org/tinytex/)

  - 우분투 등 리눅스에서 사용하는 Live TeX 은 용량이 매우 큰데
  - 대부분의 내용이 사용되지 않는 경우가 많으므로
  - 필수적이고 자주 사용되는 부분만으로 경량화하고
  - 필요한 부분은 그 때 그 때 추가해서 사용하는 개념임

# 관련 패키지 설치

- 먼저 주로 `Rmarkdown` 파일(확장자 `.Rmd`)과 결합되므로

  - 메뉴 새파일에서 `R makrdown` 클릭해 템플릿 파일을 하나 띄운다
  - 그러면 마크다운 패키지도 설치하라고 함 >> 예스 클릭해 자동 설치

- `TinyTeX` 패키지 설치는 다음 명령으로 실행

  ```
  > install.packages("tinytex")
  > tinytex::install_tinytex( )
  ```

- 새로운 PATH 환경변수 적용을 위해 우분투 쉘에서 재시작 명령 

  - `$ sudo rstudio-server restart`

- 참고로 패키지 관련 파일들은 사용자 홈에 `.TinyTeX/` 경로에 있다.


# R markdown 문서의 pdf 출력

- 영어는 pdf 저장 및 출력이 잘 되나

- 한글은 안되는데 다음 설정이 필요하다

#### 먼저 마크다운 문서 헤더의 디폴트 `YAML` 부분 수정

  ```
  ---
  title: "Untitled"
  author: "Someone"
  date: '2022 2 12 '
  output: pdf_document
  ---
  ```
  - `output: pdf_document` 아래에 다음 내용 추가

    > header-includes:  
    > 　\-　\usepackage{kotex}  
    
  ```
  ---
  title: "제목"
  author: "지은이"
  date: '2022 2 12 '
  output: pdf_document
  header-includes:  
  　- \usepackage{kotex}  
  ---
  ```

- 그러나 처음에는 `kotex.sty` 파일 없다는 에러 나옴

#### 다음 명령으로 해당 패키지 찾아 설치해야 함

`> tinytex::tlmgr_search("kotex.sty")`
  
- 그러면 렌더링 과정에서 다음 내용을 볼 수 있음

  ```
  tlmgr search --file --global 'kotex.sty'  
  tlmgr: package repository   
  https://mirror.kakao.com/CTAN/systems/texlive/tlnet (verified)  
  cjk-ko:	texmf-dist/tex/latex/cjk-ko/kotex.sty  
  ```

- 여기서 첫 줄은 `TeX Live` 명령인데

  - `TeX Live` 없으므로 `tinytex` 함수로 대행한 것임
  
- 둘째 셋째 줄은 TeX 패키지 저장소를 확인

- 마지막 줄은 `cjk-ko` 패키지 필요 알려줌

  - `cjk-ko` 패키지에 `kotex.sty` 파일이 있다

  - 즉 해당 경로에 파일 위치한다는 뜻임
  
    - 현재 우분투에서 경로 검색해 보면 없음
    
  - 따라서 콘솔에서 `cjk-ko` 패키지 설치 필요
  
    ```
    > tinytex::tlmgr_update()
    > tinytex::tlmgr_install("cjk-ko")
    ```
  
  - 이제 한글 pdf 출력 잘됨
  
  - 참고로 `.TinyTeX/` 해당 경로도 존재함
  
  
  
#### 참고로 `TeX Live` 명령은 다음과 같음
  
```
$ tlmgr update --self
$ tlmgr search –file –global ‘kotex.sty’
$ tlmgr info cjk-ko
$ tlmgr install cjk-ko
```

- [TeX Live 설치와 사용법은 다른 글](/posts/wsl2texlive)에서 다룸

# VS Code 가 TinyTeX 인식 못하는 경우 

- WSL2 우분투에 설치된 텍을 윈도우에서 `VS Code` 로 사용하려면

  - 해당 우분투 폴더에서 다음 명령 실행
  
    `$ code . `
  
- 그런데 텍 명령이 안될 수 있음 >> 경로설정 필요

    `$ PATH=$PATH:/home/cjj/.TinyTeX/bin/x86_64-linux`
    
- 이렇게 하는 것은 임시적 >> 영구적 변경 필요

  - 다음 명령을 `~/.bashrc` 파일 마지막에 추가
  
    `export PATH=$PATH:/home/cjj/.TinyTeX/bin/x86_64-linux`
    
  - 쉬운 명령법 예시
  
  `$ echo export PATH=$PATH:/home/cjj/.TinyTeX/bin/x86_64-linux >> ~/.bashrc`

#### 참고사항

- `~/.bashrc` : 해당 사용자에만 적용되는 환경설정

- `/etc/bash.bashrc` : 전체에 적용되는 환경설정