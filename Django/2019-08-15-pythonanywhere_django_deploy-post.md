# pythonanywhere 로 장고 프로젝트 배포하기

### Bash console 설정
1. 깃허브 커밋
2. bash에서 pwd로 현재 경로가 /home/[id] 인지 확인
3. git clone [레포지토리 주소]
4. cd [프로젝트 폴터]
5. virtualenv venv --python=python3.7
6. source venv/bin/activate
7. pip install -r requirements.txt
8. python manage.py migrate
9. python manage.py createsuperuser
10. python manage.py collectstatic

### Web 설정
1. 가상환경 경로(Bash에서 pwd로 확인)
- /home/[id]/[프로젝트명]/venv

2. pythonanywhere_com_wsgi.py

```python
import os
import sys

path = "/home/heejinnnni/fc_stargram"
if path not in sys.path:
    sys.path.append(path)

from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
application = get_wsgi_application()
```

3. static, media 경로 
- /static/	/home/[id]/[프로젝트명]/static/	 
- /media/	/home/[id]/[프로젝트명]/media/

<hr>

### 소스코드 수정 후
1. 깃허브에 커밋
2. pythonanywhere 콘솔에서 git pull
3. reload
