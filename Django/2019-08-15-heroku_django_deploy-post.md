# 헤로쿠로 장고 프로젝트 배포하기

## 프로젝트에 모듈 설치
1. pip install dj-database-url
2. pip install gunicorn
3. pip install whitenoise
4. pip install psycopg2-binary
5. pip freeze > requirements.txt

## 프로젝트 수정
- config/settings.py
```python
import dj_database_url

DEBUG = False

ALLOWED_HOSTS = ['*']

DATABASES = DATABASES

DATABASES['default'].update(dj_database_url.config(conn_max_age=500))

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    
    'whitenoise.middleware.WhiteNoiseMiddleware', # here
]

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

- Procfile 파일 생성
```python
web: gunicorn config.wsgi
```

- runtime.txt 생성
```txt
python-3.7.0
```

## 헤로쿠에 업로드
1. heroku login
```terminal
(venv) ➜  stargram git:(master) ✗ heroku login
heroku: Press any key to open up the browser to login or q to exit: 
Opening browser to https://cli-auth.heroku.com/auth/browser/fa053c08-4a9a-4a70-870f-c4f59bfb2c69
Logging in... done
Logged in as laii28@naver.com
```

2. git add .
3. git commit -m ''
4. heroku create [앱 이름]
```terminal
(venv) ➜  stargram git:(master) heroku create [앱 이름]
Creating ⬢ [앱 이름]... done
https://[앱 이름].herokuapp.com/ | https://git.heroku.com/[앱 이름].git
```
5. git push heroku master
- 혹시 안되면 오타가 있는지 확인하기

6. heroku run python manage.py migrate
7. heroku run python manage.py createsuperuser
8. heroku open

## 소스코드 수정 후
1. git add .
2. git commit -m ''
3. git push heroku master
