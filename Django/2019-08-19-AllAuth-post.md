# 소셜 로그인 (Auth) 구현하기

### 프로젝트 설정
1. pip install django-allauth
2. settings.py
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # allauth
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.naver',  # naver
]

# allauth
AUTHENTICATION_BACKENDS = (
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
)

SITE_ID = 1
```
3. config/urls.py
```python
path('accounts/', include('allauth.urls'))
```
4. python manage.py migrate

### 네이버 개발자 사이트 설정
1. 애플리케이션 등록
- 사용 API : 네아로 로그인
- 환경추가 : PC 웹
- 서비스 URL : http://127.0.0.1:8000/
- Callback URL :  http://127.0.0.1:8000/accounts/naver/login/callback/

### 관리자 페이지 설정
1. Sosial applications Add
2. Provider : Naver
3. Client id, Secret key 입력
