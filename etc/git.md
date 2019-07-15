# Git

## 기본 개념

* 작업 디렉토리\(Working directory\) : 실제 파일들
* 인덱스\(Index\) : 준비 영역\(staging area\)
* HEAD : 최종 확정본\(commit\)

### 작업 흐름

1. **작업 디렉토리\(Working directory\)** 에서 `add` 명령어 실행으로 **인덱스\(Index\)** 변경된 파일 추가
2. `commit` 명령어로 **HEAD**에 반영
3. **HEAD**에 반영된 변경 내용은 `push` 명령어로 발행 가능

## 기본 명령어

### 초기화 \(init\)

새로운 git 저장소 생성

```text
$ git init
```

### 저장소 받아오기 \(clone\)

```text
# 로컬 저장소
git clone /로컬/저장소/경로

# 원격 서버
git clone 사용자명@호스트:/원격/저장소/경로
```

### 상태 확인 \(status\)

```text
$ git status
```

### 변경된 파일 추가 \(add\)

변경된 파일을 인덱스에 추가

```text
# 파일 이름만 추가
$ git add <파일 이름>

# 전부 추가
$ git add *
```

### 확정 \(commit\)

변경 내용을 확정해서 HEAD에 반영

```text
$ git commit -m "커밋 메시지"
```

### 발행 \(push\)

원격 서버로 발행. master 브랜치가 아니면 다른 브랜치로 변경.

```text
$ git push origin master
```

### 원격 서버 연결 \(remote\)

```text
$ git remote add origin <원격 서버 주소>
```

## 브랜치 \(branch\)

안전하게 격리된 상태에서 무언가를 만들 때 사용함. 저장소를 새로 만들면 기본으로 master 브랜치가 만들어짐. 그럼 그 만들어진 이용해서 개발을 진행하고, 나중에 개발이 완료되면 master 가지로 돌아와 병합하면 됨.

### 브랜치 만들기 \(checkout -b\)

"feature\_x"라는 이름의 가지를 만들고 갈아탐

```text
$ git checkout -b feature_x
```

### 다른 브랜치로 돌아오기 \(checkout\)

master 브랜치로 돌아옴

```text
$ git checkout master
```

### 브랜치 삭제 \(branch -d\)

"feature\_x" 브랜치 삭제. 그런데 원격 브랜치도 삭제를 해줘야 됨.

```text
# 로컬 브랜치 삭제
$ git branch -d feature_x

# 원격 브랜치에 반영
$ git push origin feature_x

# 로컬 브랜치 삭제하면서 원격 브랜치까지 삭제
$ git push origin --delete feature_x
```

### 브랜치를 원격 저장소로 전송

```text
$ git push origin <가지 이름>
```

### 저장소 갱신 \(pull\)

원격 저장소의 변경 내용이 로컬 작업 디렉토리에 받아지고\(fetch\), 병합\(merge\)됨

```text
$ git pull
```

### 병합 \(merge\)

충돌이 일어날 경우 일어난 부분을 직접 수정해야 함

```text
$ git merge <가지 이름>
```

### 비교 \(diff\)

변경 내용을 병합하기 전에 어떻게 바뀌었는지 비교

```text
# git diff <원래 가지> <비교 대상 가지>
```

## 되돌리기

### reset

이력을 삭제하고 되돌리기. 옵션에는 hard, soft, mixed가 있음. 옵션의 기본값은 mixed.

* hard: 돌아가려는 이력 이후의 모든 내용 삭제
* sort: 돌아가려는 이력으로 돌아가기는 했지만, stage는 그대로 남아있음. commit 하기 직전의 상태로 돌아감.
* mixed: soft랑 비슷. 하지만 staging area에 추가하기 전의 상태로 돌아감. \(add 하기 전 상태\)

  ```text
  # git reset <옵션> <돌아가고싶은 커밋>
  git revert --soft 2664ce8
  ```

### revert

이력은 그대로 두고 되돌리기. 이력이 남기 때문에 어지간하면 revert를 쓰자.

```text
# git revert <되돌릴 커밋> 
git revert 2664ce8
```

### 참고 자료

* [git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
* [개발바보들](http://www.devpools.kr/2017/01/31/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-1%ED%99%94-git-back-to-the-future/)
* [\[초보용\] Git 되돌리기\( Reset, Revert \)](https://medium.com/nonamedeveloper/%EC%B4%88%EB%B3%B4%EC%9A%A9-git-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-reset-revert-d572b4cb0bd5)

