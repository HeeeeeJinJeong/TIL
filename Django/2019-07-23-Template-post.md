# 템플릿 문법

1. 컨텍스트 변수 출력하기<br>
뷰에서 컨텍스트 변수로 값을 전달해줬다면 해당 변수 이름을 출력할 수 있다.

```html
# 해당 변수값 출력
{{변수명}}


# 변수에 html이 들어있는 경우
{{변수명|safe}}

# 대문자로 변환
{{변수명|upper}}

# 소문자로 변환
{{변수명|lower}}

# 일부만 끊어서 출력
{{변수명|truncatewords:'30'}}

# 날짜형식 지정
{{변수명|date:'Y m d'}}

# 기본값 지정하기
{{변수명|default:'없음'}}

# 변수 길이 출력
{{변수명|length}}

# 제목 형식 출력
{{변수명|title}}

# 줄바꿈 태그 출력
{{변수:linebreaks}} # p태그로 감싸서 출력
{{변수:linebreaksbr}} # <br> 태그만 넣어줌
```

2. 폼 출력하기
```html
<form action="" method="post">
    {% csrf_token %} # 보안을 위한 필수 입력 값

    {{form.as_p}} p 태그로 감싸서 출력
    <table>{{form.as_table}}</table> # 테이블 형식으로 출력
    <ul>{{form.as_ul}}</ul> li로 감싸서 출력
</form>
```

3. 판별문<br>
값이 존재하는지 확인하기 위해서 if문 사용
```html
{% if 변수 %}
{% else %}
{% endif %}
```
비교연산자와 and, or, not is, in을 사용할 수 있다.<br>
하지만 and, or 사용시 괄호는 사용불가(괄호가 필요한 경우에는 중첩 비교구문을 만들어야 함)

4. 판별문2<br>
값을 비교하기 위해서는 ifequal과 ifnotequal을 사용
```html
{% ifequal 변수 값 %}
{% else %}
{% endifequal %}

{% ifnotequal을 변수 값 %}
{% else %}
{% endifnotequal %}
```

5. 반복문
```html
{% for object in object_list %}
    {{object}}
    {% cycle '1''0' %}
{% empty %}
    {# 반복할 객체가 비어있다면 출력될 영역 #}
{% endfor %}
```
- 리스트 출력시 활용 기능
  - forloop.counter : 1부터 시작되는 반복 횟수
  - forloop.counter0 : 0부터 시작되는 반복 횟수
  - forloop.revcounter : 반복 횟수 역순
  - forloop.revcounter0 : 반복 횟수 역순 0부터 시작
  - forloop.first : 첫번째 반복 회차인지 여부
  - forloop.last : 마지막 반복 회차인지 여부
  
6. 템플릿 확장
```html
{% extends 'base.html' %}
```

7. 블록
템플릿을 확장해서 사용할 경우, 부모 템블릿과 자식 템플릿 모두 block을 만들어서 사용한다.
```html
{% extends 'base.html' %}

{% block title %}{% endblock %}

{% block content %}
{% endblock %}
```

8. 템플릿 불러오기
```html
{% include 'category.html' %}
```

9. static 파일 불러오기
```html
{% load static %}
<script src="{% static 'js/underscore.js' %}"></script>

{% load staticfiles %}
<script src="{% static 'js/underscore.js' %}"></script>
```

10. 주석
```html
{# 한줄 주석 #}

{% comment %}
    여러줄 주석
{% endcomment %}
```
