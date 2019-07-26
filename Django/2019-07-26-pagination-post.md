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
