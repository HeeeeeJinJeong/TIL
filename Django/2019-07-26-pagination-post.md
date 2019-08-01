# pagination

### class based view
- view
```python
class Reservation_List(ListView):
    model = Reservation
    template_name = 'reservation/reservation_list.html'
    paginate_by = 3 
```

- template
```html
{% if is_paginated %}
<nav aria-label="Page navigation">
    <ul class="pagination justify-content-center">
        {% if page_obj.has_previous %}
        <li class="page-item"><a class="page-link" href="?page={{page_obj.previous_page_number}}">Previous</a></li>
        {% else %}
        <li class="page-item disabled"><a class="page-link" href="#">Previous</a></li>
        {% endif %}
        
        {% for page in paginator.page_range %}
        <li class="page-item"><a class="page-link" href="?page={{page}}">{{page}}</a></li>
        {% endfor %}
        
        {% if page_obj.has_next %}
        <li class="page-item"><a class="page-link" href="?page={{page_obj.next_page_number}}">Next</a></li>
        {% else %}
        <li class="page-item disabled"><a class="page-link" href="#">Next</a></li>
        {% endif %}
    </ul>
</nav>
{% endif %}
```
### function based view
- view
```python
from django.core.paginator import Paginator

def board_list(request):
    all_boards = Board.objects.all().order_by('-id')
    page = int(request.GET.get('p', 1))
    paginator = Paginator(all_boards, 3)
    boards = paginator.get_page(page)
    return render(request, 'board_list.html', {'boards': boards})
```

- templates
```html
<nav>
    <ul class="pagination justify-content-center">
        {% if boards.has_previous %}
        <li class="page-item">
            <a class="page-link" href="?p={{ boards.previous_page_number }}">이전으로</a>
        </li>
        {% else %}
        <li class="page-item disabled">
            <a class="page-link" href="#">이전으로</a>
        </li>
        {% endif %}
        
        <li class="page-item active">
            <a class="page-link" href="#">{{ boards.number }} / {{ boards.paginator.num_pages }}</a>
        </li>
        
        {% if boards.has_next %}
        <li class="page-item">
            <a class="page-link" href="?p={{ boards.next_page_number }}">다음으로</a>
        </li>
        {% else %}
        <li class="page-item disabled">
            <a class="page-link disabled" href="#">다음으로</a>
        </li>
        {% endif %}
    </ul>
</nav>
```
