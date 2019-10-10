# 이메일 로그인

1. 앱 안에 backends.py 파일을 생성 후 코드 입력
```python
from django.contrib.auth.backends import ModelBackend
from django.contrib.auth import get_user_model

class CustomUserBackend(ModelBackend):
    def authenticate(self, request, username=None, password=None, **kwargs):
        user = super().authenticate(request, username, password, **kwargs)

        if user:
            return user

        # id login fail
        # email login
        UserModel = get_user_model()

        # id 로그인 처리시 username 이 넘어왔을 경우, email 변수에 새로 값을 할당하기 위해
        email = username

        if email is None:
            email = kwargs.get(UserModel.EMAIL_FIELD, kwargs.get('email'))
        try:
            user = UserModel._default_manager.get(email=email)

        except UserModel.DoesNotExist:
            UserModel().set_password(password)

        else:
            if user.check_password(password) and self.user_can_authenticate(user):
                return user
```

2. settings.py 에 코드 추가
```python
AUTHENTICATION_BACKENDS = [
    '[앱 이름].backends.CustomUserBackend',
]
```

### 권한 체크 메서드
- Mehote : 기능
- has_perm : 사용자가 활성화 상태이고 권한이 있는지 체크
- has_module_pers : 특정 앱의 사용권한이 있는지 체크
- get_all_permissions : 해당 사용자가 가진 모든 권한 목록 반환
- get_group_permissions : 해당 사용자가 속한 그룹의 권한 목록 
- get_user_permissions : 해당 사용자가 가진 권한 목록 반환
