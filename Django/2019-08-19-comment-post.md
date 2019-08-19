# 댓글 기능 구현하기

- models.py
```python
from django.db import models

# Create your models here.
from django.urls import reverse
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill
from tagging.fields import TagField

from account.models import User


class Photo(models.Model):
    author = models.ForeignKey(User, on_delete=models.CASCADE, related_name='photos')
    photo = ProcessedImageField(upload_to='timeline_photo/%Y/%m/%d', processors=[ResizeToFill(600, 600)], format='JPEG', options={'quality': 90})
    content = models.CharField(max_length=140, help_text="최대 140자 입력 가능")
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)
    tag = TagField(blank=True)
    like = models.ManyToManyField(User, related_name='like_post', blank=True)

    class Meta:
        ordering = ['-created']


    def get_absolute_url(self):
        return reverse('photo:detail', args=[self.id])


class Comment(models.Model):
    photo = models.ForeignKey(Photo, on_delete=models.CASCADE, related_name='comments')
    author = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True, related_name='comments')
    content = models.CharField(max_length=300)
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['-id']

    def __str__(self):
        return (self.author.username if self.author else "무명") + "의 댓글"

    def get_absolute_url(self):
        return reverse('photo:detail', args=[self.id])
```

- views.py
```python
def photo_detail(request, photo_id):
    photo = Photo.objects.get(pk=photo_id)

    if request.method == "POST":
        comment_form = CommentForm(request.POST)
        comment_form.instance.author_id = request.user.id # 작성자
        comment_form.instance.photo_id = photo_id
        if comment_form.is_valid():
            comment = comment_form.save()

    comment_form = CommentForm()
    comment = photo.comments.all()
    return render(request, 'photo/photo_detail.html',
                  {'object': photo, 'comments': comment, 'comment_form': comment_form})
```

- urls.py
```python
from django.urls import path

from .views import photo_detail

app_name = 'photo'

urlpatterns = [
    path('detail/<int:photo_id>/', photo_detail, name='detail'),
]
```

- photo_detail.html
```html
<form action="" method="post">
    {% csrf_token %}
    {{comment_form.as_p}}
    <input type="submit" value="comment" class="btn btn-outline-secondary">
</form>

<div id="docs_comment_list_area">
    <table class="table table-striped" id="comment_list">
        <thead>
            <tr>
                <th colspan="3" class="align-left">댓글목록</th>
            </tr>
        </thead>

        <tbody>
        {% for comment in comments %}
        <tr>
            <td>{{comment.author.username}}</td>
            <td>{{comment.content}}</td>
            <td>{{comment.created|date:'Y-m-d h:m'}}</td>
        </tr>
        {% endfor %}
        </tbody>
    </table>
</div>
```
