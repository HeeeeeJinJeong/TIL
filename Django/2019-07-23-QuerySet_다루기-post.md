# QuerySet 다루기
QuerySet은 장고에서 데이터베이스를 쉽게 다루기 위해서 사용하는 ORM의 기능이다.<br>
원하는 객체를 불러오거나 검색하는 기능 등이 존재하고 있다.

### 전체 객체 불러오기
Model.object.all()

### 특정 객체 불러오기
Model.object.get(pk=값)<br>
  - get은 불러오려는 객체가 없을 때 오류를 발생시킴
  
### 객체가 존재하는지 확인
Model.object.filter(pk=1).exists()

### 키워드로 검색하기
Model.object.filter(필드명__옵션=값)
  - exact : 값 매치
  - iexact : 대소문자 구분없이 값 매치
  - startswith, istartswith : 해당 값으로 시작하는 결과 찾기
  - endswith, iendswith : 해당 값으로 끝나는 결과 찾기
  - contains, icontains : 값을 포함하는 결과 찾기
  - gt, gte : 값보다 큰 경우, 크거나 같은 경우 찾기
  - lte, lt : 값보다 작은 경우, 작거나 같은 경우 찾기
필터는 연속해서 여러번 사용해도 됨, 이 때는 and 조건으로 연결됨
```python
# ex) pk가 3보다 크고, test필드에 test값을 포함한 결과 찾기
Model.object.filter(pk__gt=3).filter(text__icontains='test')
Model.object.filter(pk__gt=3, text__icontains='test')
```

### 요소 제외하기
Model.object.exclude(조건)

### 개수 끊기
- 처음 10개만 가져오기 : Model.object.all()[:10]

### 정렬하기
Model.object.all().order_by('-created')

### 조건의 or 연결
- 조건 연결 : and(&), or(|), not(~)
QuerySet의 filter에 사용하는 값은 기본적으로 무조건 and 연산이다.<br>
or 조건을 만들기 위에서는 Q 객체를 사용한다.
```python
from django.db.models import Q

# test 값이 a나 b로 시작하는 경우
q = Q(text__startswith='a') | Q(text__startswith='b')
Model.objects.filter(q)

# Q 객체와 필드 조건을 함께 사용할 때는 필드 조건이 항상 뒤에 와야 한다.
Model.objects.filter(q, pk__gt=3)
```

### 필드 참조
일반값만 필터로 사용하는 것이 아니라 특정 필드의 값을 필터 조건에 포함하고 싶을 때 F 객체 사용
```python
from django.db.models import F

# 해당 레코드의 제목이 본문에 포함된 경우
Model.objects.filter(text__icontains=F('title'))
```

### ForeignKey로 연결된 필드를 참조하는 경우
```python
# 글 작성자의 유저이름에 admin이 포함된 경우
Model.objects.filter(author__username__icontain='admin')
```
