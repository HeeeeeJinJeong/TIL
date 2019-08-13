# EC2에 장고 배포하기
(디버그모드 False로 변경, ALLOWED_HOSTS 설정)

1. EC2 -> 인스턴스 시작
2. 다운로드 받은 키 페어 파일의 권한을 400으로 변경
- chmod 400 [키페어 파일 이름].pem
3. 키 페어 파일을 .ssh 폴더로 이동
- mv [키페어 파일 이름].pem ~/.ssh/

<hr>

1. EC2 인스턴스에 ssh 접속
- ssh -i ~/.ssh/[키페어 파일 이름].pem ubuntu@[퍼블릭 DNS]
2. sudo apt-get update
3. sudo apt-get install nginx
4. sudo apt-get install vim
5. sudo apt-get install python3-dev python3-venv python3-pip
6. 어플리케이션 구동용 계정 설정
- sudo useradd -b /home -m -s/bin/bash django
7. www-data 그룹에 유저 추가
- sudo usermod -a -G www-data django
- sudo usermod -a -G www-data ubuntu
8. 소스코드 업로드 폴더
sudo mkdir -p /var/www/django
9. UWSGI 폴더
- sudo mkdir /var/www/django/run
- sudo mkdir /var/www/django/logs
- sudo mkdir /var/www/django/ini
10. uwsgi 설정파일 생성
- sudo vim /var/www/django/ini/uwsgi.ini
```vim
[uwsgi]
uid = django
base = /var/www/django

home = %(base)/venv
chdir = %(base)
module = config.wsgi:application
env = DJANGO_SETTINGS_MODULE=config.settings

master = true
processes = 5

socket = %(base)/run/uwsgi.sock
logto = %(base)/logs/uwsgi.log
chown-socket = %(uid):www-data
chmod-socket = 660
vacuum = true
```
11. sudo python3 -m venv /var/www/django/venv
12. 폴더의 소유자와 사용권한 변경
- sudo chown -R django:www-data /var/www/django
- sudo chmod -R g+w /var/www/django

<hr>

1. 파일질라에 소스코드 업로드
- 프로토콜 : SFTP
- 호스트 : 퍼블릭 DNS주소
- 로그온 유형 : 키파일
- 사용자 : ubuntu
- 고급/기본 리모트 디렉토리 : /var/www/django

<hr>

1. cd /var/www/django/
2. sudo -s
3. source venv/bin/activate
4. pip install -r requirements.txt
5. pip install uwsgi
6. python manage.py runserver 0:8000 (프로젝트가 구동되는지 확인)
7. uwsgi --http :8000 --home /var/www/django/venv/ --chdir /var/www/django/ --module config.wsgi (uwsgi 모듈로 구동되는지 확인)
8. vim /etc/systemd/system/uwsgi.service
```vim
[Unit]
Description=uWSGI Emperor service

[Service]
ExecStart=/var/www/django/venv/bin/uwsgi --emperor /var/www/django/ini
User=django
Group=www-data
Restart=on-failure
KillSignal=SIGQUIT
Type=notify
NotifyAccess=all
standardError=syslog

[Install]
WantedBy=multi-user.target
```
9. systemctl start uwsgi
10. systemctl enable uwsgi
11. systemctl status uwsgi
12. vim /etc/nginx/sites-available/default
```vim
upstream django {
        server unix:/var/www/django/run/uwsgi.sock;
}

server {
        listen 80 default_server;
        listen [::]:80 default_server;
        charset utf-8;
        access_log /var/www/django/logs/access.log;
        error_log /var/www/django/logs/error.log;


        server_name _;

        location = /favicon.ico { access_log off; log_not_found off; }

        location / {
                include /etc/nginx/uwsgi_params;
                uwsgi_pass django;
        }

}
```
13. systemctl restart nginx

<hr>

### 소스코드 수정시
sudo chmod -R g+w /var/www/django

1. cd /var/www/django/
2. sudo -s
3. source venv/bin/activate
4. systemctl start nginx
5. systemctl start uwsgi
6. systemctl enable uwsgi
7. systemctl restart uwsgi
