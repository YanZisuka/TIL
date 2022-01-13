# git 기초

---

## 저장소 초기화

### dir를 repo로

```
$ git init
```

### repo를 dir로

```
$ rm -rf .git/
```



## 서명 등록

### Username

```
$ git config --global user.name "name"
```

### Email

```
$ git config --global user.email "email"
```



## Staging

```
$ git add 'file'
```



## Commit

```
$ git commit -m 'message'
```



## 상태 확인

```
$ git log
```

```
$ git status
```



## 원격저장소 연결

```
$ git remote add origin https://github.com/ID
```



## 로컬 커밋 원격으로 보내기

```
$ git push origin master
```

