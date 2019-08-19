# WYSIWYG
- pip install django-ckeditor
- config/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    'ckeditor',
    'ckeditor_uploader',
]

# ckeditor
CKEDITOR_RESTRICT_BY_USER = True
CKEDITOR_UPLOAD_PATH = 'wysiwyg/'
```
- config/urls.py
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('ckeditor', include('ckeditor_uploader.urls')), # here
]
```
- python manage.py collectstatic

### model 에 적용
```python
from ckeditor.fields import RichTextField

class Photo(models.Model):
    content = RichTextField()
```

- python manage.py makemigrations
- python manage.py migrate
