프로젝트 시작하기
===

# 사전 작업
1. 일단 파이썬 먼저 설치. 그럼 pip도 자동으로 설치가 됨
2. pip으로 장고 설치
3. 뭔가 이상하다 싶으면 (가상 환경에서) 장고 설치 제대로 됐는지 확인. 
```
$ python -m django --version
```

# 프로젝트 생성

```
$ django-admin startproject mysite .
```
만약 `django-admin` 명령어를 찾을 수 없는 경우 가상 환경을 activate하고 Django를 설치한 다음 아래 명령어 입력
```
# 가상환경에 Django 설치
$ python3 -m pip install django

# 프로젝트 생성
$ python3 -m django startproject mysite .
```
Django 1.9부터 `django-admin`대신 `python -m django` 사용 가능.

# 설정 변경
`mysite/settings.py`에서 이것저것 변경 가능.

# 데이터베이스 설정
파이썬은 `sqlite3`이 기본으로 설치되어 있음. 아래는 데이터베이스 변경사항을 반영할 떄 실행. 그런데 디비 테이블을 만들지 않아도 이 명령을 먼저 실행해야 함. 장고는 모든 웹 프로젝트 개발 시, 반드시 사용자와 그룹 테이블 등이 필요하다는 가정 하에 설계되었기 때문. 그래서 테이블을 만들지 않았더라도, 사용자 그룹 및 테이블 등을 만들어주기 위해서 프로젝트 개발 시작 시점에 이 명령을 실행하는 것임.

```
$ python manage.py migrate
```

# 서버 실행
```
# 주소를 지정하지 않으면 디폴트로 127.0.0.1 주소 및 8000번 포트를 사용
$ python manage.py runserver
# 포트번호만 지정하면 디폴트 127.0.0.1 주소 및 8888번 포트 사용
$ python manage.py runserver 8888
# 외부 기기 접속 가능
$ python manage.py runserver 0.0.0.0:8000
```

# 애플리케이션 만들기
```
$ python manage.py startapp <파일명>
```

# 슈퍼유저 만들기
```
$ python manage.py createsuperuser
```