# npm

## npm\(Node Package Manager\)

npm은 node.js 기반의 모듈을 모아놓은 모듈저장소 npm init 명령어로 프로젝트 관리를 위한 package.json 파일 생성

## 명령어 모음

```text
# package.json에 있는 모든 디펜던시 설치하기
npm install

# 개발용 디펜던시 설치
npm install <패키지 이름> --save-dev

# 특정 버전 설치
npm install <패키지 이름>@버전

# 전역으로 설치
npm install <패키지 이름> -g

# 중복 패키지 정리
npm dedupe

# 설치한 패키지 조회 (매우 많으니 그냥 package.json 파일 확인하기)
npm ls

# 오래된 패키지 조회
npm outdated
```

