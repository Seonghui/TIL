가상환경(Virtual environment)
===

* 가상환경(Virtual environment), 줄여서 virtualenv
* 독립적인 python 가상 환경을 새로 설정해 다른 프로젝트를 진행하면서 설치했던 라이브러리들간의 의존성 문제를 해결할 수 있도록 해줌

# 1. 가상환경 만들기
`myvenv`라는 이름의 가상환경 만들기

```
$ python3 -m venv myvenv
```

# 2. 가상환경 실행하기

```
$ source myvenv/bin/activate
```

만약 source를 사용할 수 없는 경우 아래와 같이 입력하기
```
$ . myvenv/bin/activate
```

입력하면 콘솔의 프롬포트 앞에 (myvenv) 접두어가 붙는다.

# 3. 종료하기
```
$ deactivate
```

## References

- [djangogirls](https://tutorial.djangogirls.org/ko/installation/)
