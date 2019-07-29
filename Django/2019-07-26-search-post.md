# Search

1. 검색어
2. filter
- 어떤 항목? : 제목, 내용, 작성자 ...
- 어떤 옵션? : 대소문자 구분 여부, 키워드 시작점, 끝나는 지점 ...


## filter
### .filter를 많이 써도 실제 값을 요청하기 전까지는 쿼리문이 실행되지 않음
- 필드명 = "값" 매칭
- 필드명_exact = "값" 매칭
- 필드명_iexact = "값" 대소문자 구분 없이 매칭
- __startswith, __istartswith 값으로 시작
- __endswith, __iendswith 값으로 끝
- __continas, __icontains 값을 포함하느냐

### ForeignKey 매칭
- 필드명__해당모델의필드명 = 매칭
- 필드명__해당모델의필드명__옵션 = 위와 동일하게 동작

- __gt=값, __gte=값 -> 크다, 크거나 같다.
- ex) created__gt = 오늘 -> 작성일이 오늘보다 크다

- __lt=값, __lte=값 -> 작다, 작거나 같다.
- ex) created__lt = 오늘 -> 작성일이 오늘보다 이전
- ex) 판매시작일__lte = 오늘 -> 판매시작일 설정값이 오늘보다 작거나 같으면 판매 시작

## Q
from django.db.models import Q
- objects.filter() : filter 메서드에 들어가는 매개변수들은 항상 and 연산을 한다.
- or 연산을 하고싶어서 Q 객체를 사용한다. (사용법은 filter 에 들어가는 매개변수의 작성법과 똑같다.)
- Q() | Q() -> or
- Q() & Q() -> and
- ~Q() -> not

### 옵션
- username : Q(author__username__icontains=search_key)
- title : Q(title__icontains=search_key)
- text : Q(text__icontains=search_key)
- q1 | q2 | q3
```python
if search_key and search_type:
    documents = get_list_or_404(Document, q1|q2|q3)
```

## F
from django.db.models import F

###
- title이 text에 포함되어 있는지 확인 : Document.objects.filter(text__icontains=F('title'))
- text에 유저이름이 포함되어 있는지 확인 : Document.objects.filter(text__icontains=F('author__username'))

## exclude() 제외
- Document.objects.exclude(title__icontains='sd') : title에 'sd' 가 들어있는 포스트를 제외하고 출력
- Document.objects.exclude(category__in=Category.objects.filter(name__icontains='dubu')) : 'dubu' 카테고리 제외하고 출력

### exclude 옵션
1. 블로그 제목에 제외 키워드가 있는 경우
2. 블로그 생성일이 한달 이내인 경우
- Document.objects.exclude(blog__in=Blog.objects.filter(title__icontains='key', created__gt=datetime.now()-timedelta(months=1))
3. 나머지 글들만
