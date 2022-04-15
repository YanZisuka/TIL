# Git_03

## Undoing things

### 1. 파일 내용을 수정 전으로 되돌리기

>   "Unmodifying a Modified File"

-   Modified 파일 되돌리기
-   Working Directory에서 파일을 수정했다고 가정해 봅시다.
    -   이 파일의 수정사항을 되돌리려면 어떻게 해야 할까?

```
$ git restore <file>
```



### 2. 스테이징 된 파일을 언스테이징 상태로 되돌리기

>   "Unstaging a Staged File"

-   커밋이 없을 때

```
$ git rm --cached <file>
```

-   커밋이 존재할 때
    -   HEAD의 존재로 인해 명령어가 달라진다.

```
$ git restore --staged <file>
```



### 3. 직전 커밋을 수정하기

-   상황별 2가지 기능
    1.   커밋 메시지만 수정
         -   마지막으로 커밋하고 나서 수정한 것이 없을 때 (커밋하자마자 바로 이 명령을 실행하는 경우)
    2.   이전 커밋 덮어쓰기
         -   Staging Area에 새로 올라온 내용이 있을 때
-   커밋을 재정의 한다. (커밋의 해시값이 바뀜)

```
$ git commit --amend
```



## Reset & Revert

-   공통점
    -   "과거로 되돌린다."
-   차이점
    -   "과거로 되돌리겠다는 내용을 기록하는지 (== Commit 이력에 남는가)"

### 1. Reset

-   가끔 앱을 사용하다가 업데이트를 했는데, 이전 버전이 더 좋다고 느낄 때가 있습니다.
    -   예전 버전으로 돌아가고 싶을 때 어떻게 해야할까요?
-   특정 커밋으로 되돌아 갔을 때, 해당 커밋 이후로 쌓아 놨던 커밋들은 전부 사라집니다.

#### option

1.   `--soft`
     -   돌아가려는 커밋으로 되돌아가고,
     -   **이후의 commit된 파일들은 `staging area`로 돌려놓음**
     -   즉, 다시 커밋할 수 있는 상태가 됨
2.   `--mixed`
     -   돌아가려는 커밋으로 되돌아가고,
     -   **이후의 commit된 파일들을 `working directory`로 돌려놓음**
     -   즉, unstaged 상태
     -   기본값
3.   `--hard`
     -   돌아가려는 커밋으로 되돌아가고,
     -   이후의 commit된 파일들(`tracked` 파일들)은 모두 working directory에서 삭제
     -   단, Untracked 파일은 그대로 남아있음

```
$ git reset [option] <commit ID>
```



### 2. Revert

-   `Reset`은 쉽게 과거로 돌아갈 수 있지만, 커밋 내역이 사라진다는 단점이 있습니다.
    -   따라서 협업 시에 커밋 내역의 차이로 충돌이 발생할 수 있습니다.
-   `Revert`는 **이전 커밋을 없었던 일로 만든다는 내용을 가진** 새로운 커밋을 만듭니다.

```
$ git revert <commit ID>
```



### [중요]

-   `git reset --hard 5sd2f42`라고 작성하면 `5sd2f42`라는 커밋`으로` 돌아간다는 의미입니다.
-   `git revert 5sd2f42`라고 작성하면 `5sd2f42`라는 커밋`을` 없앤다는 의미입니다.