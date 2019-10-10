# Openssl로 인증서 만들기

1. openssl의 버전 확인
```terminal
openssl version
```
2. 키 파일 생성
```terminal
openssl genrsa 1024 > [key 이름].key
```

3. cert 파일 생성
```terminal
openssl req -new -x509 -nodes -days 365 -key [key 이름].key > [key 이름].cert
```

4. 생성한 파일들을 프로젝트 폴더로 복사 붙여넣기

5. django-sslserver 설치<br>
pip install django-sslserver

6. settings.py/INSTALLED_APPS에 추가

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'sslserver', # here
]
```

7. 명령어 실행(둘 중에 무엇으로 시작해도 상관없음)
- python manage.py runsslserver --certificate [key 이름].cert --key [key 이름].key
- python manage.py runsslserver
