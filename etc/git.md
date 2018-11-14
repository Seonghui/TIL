Git
===

# 기본 개념
* 작업 디렉토리(Working directory) : 실제 파일들
* 인덱스(Index) : 준비 영역(staging area)
* HEAD : 최종 확정본(commit)

# 작업 흐름
1. **작업 디렉토리(Working directory)**에서 `add` 명령어 실행으로 **인덱스(Index)** 변경된 파일 추가
2. `commit` 명령어로 **HEAD**에 반영
3. **HEAD**에 반영된 변경 내용은 `push` 명령어로 발행 가능

# 기본 명령어
## 초기화 (init)
새로운 git 저장소 생성
```
$ git init
```

## 저장소 받아오기 (clone)
```
# 로컬 저장소
git clone /로컬/저장소/경로

# 원격 서버
git clone 사용자명@호스트:/원격/저장소/경로
```

## 상태 확인 (status)
```
$ git status
```

## 변경된 파일 추가 (add)
변경된 파일을 인덱스에 추가
```
# 파일 이름만 추가
$ git add <파일 이름>

# 전부 추가
$ git add *
```

## 확정 (commit)
변경 내용을 확정해서 HEAD에 반영
```
$ git commit -m "커밋 메시지"
```

## 발행 (push)
원격 서버로 발행. master 브랜치가 아니면 다른 브랜치로 변경.
```
$ git push origin master
```

## 원격 서버 연결 (remote)
```
$ git remote add origin <원격 서버 주소>
```

# 브랜치 (branch)
안전하게 격리된 상태에서 무언가를 만들 때 사용함. 저장소를 새로 만들면 기본으로 master 브랜치가 만들어짐. 그럼 그 만들어진 이용해서 개발을 진행하고, 나중에 개발이 완료되면 master 가지로 돌아와 병합하면 됨.

## 브랜치 만들기 (checkout -b)
"feature_x"라는 이름의 가지를 만들고 갈아탐
```
$ git checkout -b feature_x
```

## 다른 브랜치로 돌아오기 (checkout)
master 브랜치로 돌아옴
```
$ git checkout master
```

## 브랜치 삭제 (branch -d)
"feature_x" 브랜치 삭제
```
$ git branch -d feature_x
```

## 브랜치를 원격 저장소로 전송
```
$ git push origin <가지 이름>
```

## 저장소 갱신 (pull)
원격 저장소의 변경 내용이 로컬 작업 디렉토리에 받아지고(fetch), 병합(merge)됨
```
$ git pull
```

## 병합 (merge)
충돌이 일어날 경우 일어난 부분을 직접 수정해야 함
```
$ git merge <가지 이름>
```

## 비교 (diff)
변경 내용을 병합하기 전에 어떻게 바뀌었는지 비교
```
# git diff <원래 가지> <비교 대상 가지>
```


## 참고 자료

[git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)