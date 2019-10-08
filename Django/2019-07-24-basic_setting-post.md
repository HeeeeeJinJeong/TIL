### 장고 설치
- pip install django
- django-admin startproject config .

### 데이터베이스 설정
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME':'',
        'USER':'',
        'PASSWORD':'',
        'HOST':'', # 엔드포인트
        'PORT':'5432',
    }
}
```
- pip install psycopg2-binary
- python manage.py migrate
- python manage.py createsuperuser

### S3 static
- 버킷 생성
- IAM 생성
- config/settings.py
```python
AWS_ACCESS_KEY_ID = ''
AWS_SECRET_ACCESS_KEY = ''

AWS_REGION = 'ap-northeast-2'
AWS_STORAGE_BUCKET_NAME = 'static 버킷 이름'
AWS_S3_CUSTOM_DOMAIN = '%s' % (AWS_STORAGE_BUCKET_NAME)
AWS_S3_SECURE_URLS = False
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_DEFAULT_ACL = 'public-read'
AWS_LOCATION = 'static'

STATIC_URL = 'http://%s/%s/' % (AWS_S3_CUSTOM_DOMAIN, AWS_LOCATION)
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```
- pip install django-storages
- pip install boto3
- python manage.py collectstatic

### media
- config/settings.py
```python
DEFAULT_FILE_STORAGE = 'config.asset_storage.MediaStorage'
```
- config/asset_storage.py 생성
```python
from storages.backends.s3boto3 import S3Boto3Storage

class MediaStorage(S3Boto3Storage):
    location = 'media'
    bucket_name = 'monoground.media.jjjinnn.com'
    region_name = 'ap-northeast-2'
    custom_domain = 's3.%s.amazonaws.com/%s' % (region_name, bucket_name)
    file_overwrite = False
```

### S3에 도메인 연결하기
- 버킷 속성에 '정적 웹 사이트 호스팅' 활성화
- Route53에 연결
- config/asset_storage.py
```python
from storages.backends.s3boto3 import S3Boto3Storage

class MediaStorage(S3Boto3Storage):
    location = 'media'
    bucket_name = 'monoground.media.jjjinnn.com'
    region_name = 'ap-northeast-2'
    custom_domain = bucket_name # 수정
    file_overwrite = False
```
- config/settings.py
```python
AWS_ACCESS_KEY_ID = 'AWS_ACCESS_KEY_ID'
AWS_SECRET_ACCESS_KEY = 'AWS_SECRET_ACCESS_KEY'

AWS_REGION = 'ap-northeast-2'
AWS_STORAGE_BUCKET_NAME = 'monoground.static.jjjinnn.com'
AWS_S3_CUSTOM_DOMAIN = AWS_STORAGE_BUCKET_NAME # 수정
AWS_S3_SECURE_URLS = False
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_DEFAULT_ACL = 'public-read'
AWS_LOCATION = 'static'

STATIC_URL = 'http://%s/%s/' % (AWS_S3_CUSTOM_DOMAIN, AWS_LOCATION)
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

### 의존성 목록 만들기, 설치하기
- pip freeze > requirements.txt
- pip freeze -r requirements.txt

## S3 없이
### media
- settings.py
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
- config/urls.py
```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

# live 상태
from django.views.static import serve
from django.urls import re_path

urlpatterns = [
    # ...
    re_path(r'^media/(?P<path>.*)$', serve, {'document_root':settings.MEDIA_ROOT}),
]
```
- settings.py
```python
DEBUG = False

ALLOWED_HOSTS = ['*']
```

### static
- pip install whitenoise

- settings.py
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware', # here
]

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

- python manage.py collectstatic
