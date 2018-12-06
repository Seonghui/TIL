프로젝트 시작하기
===

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
파이썬은 `sqlite3`이 기본으로 설치되어 있음. 데이터베이스를 생성하려면 아래 코드를 실행해야 함.
```
$ python manage.py migrate
```

# 서버 실행
```
$ python manage.py runserver
```

# 애플리케이션 만들기
```
$ python manage.py startapp <파일명>
```