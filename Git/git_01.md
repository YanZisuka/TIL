# Git_01

## What

-   개발자들이 버전 관리를 용이하게 하기 위해 사용하는 툴(`분산 버전 관리 시스템`)
-   `CLI, GUI`
-   `Interface`: 서로 다른 것들이 만나는 접점

>   `Git Bash`

-   윈도우에서 Git을 활용하기 위한 기본 도구



## Why

-   관리해야할 파일의 내용이 많은 경우 어디를 수정했는지 확인하기가 곤란함
-   프로젝트 파일의 용량이 너무 큰 경우, 조그마한 수정사항에도 불구하고 파일을 통째로 백업하면 용량 낭비가 심함
-   위와 같은 점을 보완하기 위해 변경사항을 컴퓨터가 따로 관리, 최종 프로젝트 파일만 남겨 효율적으로 관리하기 위함




## How
### 저장소 초기화

```
# 디렉토리를 리포지토리로
$ git init

# 리포지토리를 디렉토리로
$ rm -rf .git/
```

### 서명 등록

```
# Username 등록
$ git config --global user.name "name"

# Email 등록
$ git config --global user.email "email"
```

### 스테이징

```
$ git add filename
```

### 커밋

```
$ git commit -m 'message'
```

### 모니터링

```
# 커밋 내역 확인
$ git log

# 현재 관리 상태
$ git status
```

### 원격저장소 연결

```
$ git remote add origin https://github.com/username
```

### 로컬 커밋 원격으로

```
$ git push origin master
```

### 원격저장소에서 받기

```
# 처음
$ git clone https://github.com/username/repo.git

# 이후
$ git pull origin master
```

