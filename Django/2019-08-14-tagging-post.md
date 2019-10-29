# tagging

- pip install django-tagging

- config/settings.py
```python

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'tagging', # here
]
```

- models.py
```python
from tagging.fields import TagField

tag = TagField(blank=True)
```

- views.py
```python
from django.views.generic import TemplateView
from tagging.views import TaggedObjectList


class PhotoTaggedObjectList(TaggedObjectList):
    model = Photo
    allow_empty = True
    template_name = 'photo/photo_list.html'


class TagList(TemplateView):
    template_name = 'photo/tag_list.html'
```

- urls.py
```python
from django.conf.urls import url
from django.urls import path

from .views import *

app_name = 'blog'

urlpatterns = [
    url('(?P<tag>[^/]+(?u))/$', BlogTaggedList.as_view(), name='taggedlist'),

]
```

- templates/photo/photo_list.html
```html
<small class="text-muted">
    {% load tagging_tags %}
    {% tags_for_object object as tags %}
    {% if tags %}
    {% for tag in tags %}
        <a href="{% url 'photo:taggedlist' tag.name %}" class="badge badge-dark">#{{tag.name}}</a>
    {% endfor %}
    {% endif %}
</small>
```

- templates/photo/tag_list.html
```html
<div class="row mt-5">
    <div class="col"></div>
    <div class="col-6">
        <table class="table table-hover text-center">
            <thead>
            <tr>
                <th scope="col">TagList</th>
            </tr>
            </thead>
            <tbody>
            <tr>
                {% load tagging_tags %}
                {% tag_cloud_for_model photo.Photo as photo_tags with steps=9 min_count=1 distribution=log %}
                {% for tag in photo_tags %}
                <th scope="row"><a href="{% url 'photo:taggedlist' tag.name %}"
                                   style="font-size:{{tag.font_size}}em;" class="badge badge-dark">#{{tag}}</a></th>
                {% endfor %}
            </tr>
            </tbody>
        </table>
    </div>
    <div class="col"></div>
</div>
```
